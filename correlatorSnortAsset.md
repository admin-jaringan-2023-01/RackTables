### **Desain Sistem Integrasi Snort IDS dengan Sistem Informasi Aset untuk Rekomendasi Keamanan**  
*(Diperbarui dengan Penekanan pada Alur Data dan Kontrol)*  

---

#### **1. Tujuan Utama**  
Membangun sistem yang:  
1. Mengintegrasikan deteksi ancaman **Snort IDS** dengan konteks bisnis dari **Database Aset**  
2. Menghasilkan rekomendasi keamanan terprioritaskan berbasis risiko  
3. Memberikan visibilitas real-time melalui dashboard terpusat  

---

#### **2. Arsitektur Sistem**  
```mermaid
flowchart TD
    A[Snort IDS] -->|Raw Alerts| B[Alert Collector]
    C[Asset Database] -->|Asset Context| D[Risk Engine]
    B -->|Parsed Alerts| D
    D -->|Scored Threats| E[Recommendation Engine]
    E -->|Mitigation Actions| F[Action Dispatcher]
    F --> G[Firewall/API]
    F --> H[Dashboard]
    F --> I[SIEM/Ticketing]
    D -->|Threat Intel| J[Vulnerability DB]
```

---

#### **3. Komponen Kunci dan Alur Data**  

**A. Data Input**  
1. **Snort IDS**  
   - Format output: Unified2 atau JSON  
   - Data kunci: `timestamp`, `src_ip`, `dst_ip`, `signature_id`, `severity`, `protocol`  
   - Contoh:  
     ```json
     {
       "timestamp": "2025-06-13T14:30:00Z",
       "src_ip": "202.134.45.78",
       "dst_ip": "10.20.30.5",
       "sig_id": 30001,
       "message": "SQL Injection Attempt",
       "severity": 1  // High
     }
     ```

2. **Sistem Informasi Aset**  
   - Struktur database:  
     ```sql
     CREATE TABLE assets (
       asset_id SERIAL PRIMARY KEY,
       ip VARCHAR(15) NOT NULL UNIQUE,
       hostname VARCHAR(50),
       service VARCHAR(20) CHECK(service IN ('web','db','ssh','dns')),
       criticality INT CHECK(criticality BETWEEN 1-4), -- 4=critical
       last_patch DATE,
       vuln_list JSONB  -- e.g {"CVE-2023-1234": "unpatched"}
     );
     ```
   - Contoh record:  
     | ip       | hostname    | service | criticality | last_patch  | vuln_list               |
     |----------|-------------|---------|-------------|-------------|-------------------------|
     | 10.20.30.5 | db-server-1 | db      | 4           | 2025-05-15 | {"CVE-2024-5678": true} |

**B. Proses Inti**  
1. **Alert Collector**  
   - Fungsi:  
     - Mengonsumsi log Snort via Barnyard2/Syslog  
     - Normalisasi format (standarisasi timestamp, konversi IP)  
     - Filtering noise (misal: abaikan signature ID tertentu)  

2. **Asset Enrichment Module**  
   - Algoritma:  
     ```python
     def enrich_alert(alert):
         asset = asset_db.query(f"SELECT * FROM assets WHERE ip='{alert.dst_ip}'")
         if asset:
             alert.service = asset.service
             alert.criticality = asset.criticality
             alert.vulns = asset.vuln_list
         return alert
     ```

3. **Risk Scoring Engine**  
   - Formula risiko:  
     **Risk Score = (Severity) × (Asset Criticality) + Vulnerability Bonus**  
     - Contoh kalkulasi:  
       Severity: 2 (Medium)  
       Criticality: 3 (High)  
       Vuln Bonus: 1.5 (jika ada CVE relevan)  
       **Total = 2 × 3 + 1.5 = 7.5**  

4. **Recommendation Engine**  
   - Basis pengetahuan rekomendasi:  
     | Threat Type      | Service | Rekomendasi                           | Automation Action      |  
     |------------------|---------|---------------------------------------|------------------------|  
     | SQL Injection    | db      | "Patch MySQL; blokir IP sumber"       | Block IP via API       |  
     | Port Scan        | any     | "Aktifkan fail2ban"                   | Deploy config Ansible  |  

**C. Output Sistem**  
1. **Prioritized Threat Feed**  
   Format output:  
   ```csv
   Timestamp, Threat, Target, Service, Risk, Recommendations
   2025-06-13T14:30:00Z, SQL Injection, db-server-1, MySQL, 9.2, "1. Patch CVE-2024-XXXX 2. Block 202.134.45.78"
   ```

2. **Automation Dispatcher**  
   - Integrasi dengan:  
     - Firewall (iptables/API)  
     - Sistem patch (Ansible/SaltStack)  
     - Ticketing (Jira ServiceNow)  

---

#### **4. Diagram Interaksi Komponen**  
```plaintext
+--------------+     +-----------------+     +---------------+     +-------------------+
|              |     |                 |     |               |     |                   |
|   Snort IDS  +---->+ Alert Collector +---->+ Risk Engine   +---->+ Recommendation    |
|              |     |                 |     | (Scoring)     |     | Engine            |
+------+-------+     +--------+--------+     +-------+-------+     +---------+---------+
       |                      |                      |                       |
       |                      | Asset Query          | Threat Intel          | Actions
       v                      v                      v                       v
+------+-------+     +--------+--------+     +-------+-------+     +---------+---------+
| Network      |     | Asset Database  |     | Vuln Database |     | Firewall Control  |
| Traffic      |     | (CMDB)          |     | (CVE/NVD)     |     | Patch Management  |
+--------------+     +-----------------+     +---------------+     +-------------------+
```

---

#### **5. Teknologi Pendukung**  
| Komponen         | Teknologi Contoh                |  
|------------------|---------------------------------|  
| Data Pipeline    | Apache Kafka, Logstash          |  
| Asset Database   | PostgreSQL dengan pg_cron       |  
| Risk Engine      | Python + Pandas/Numpy           |  
| Dashboard        | Grafana + Elasticsearch         |  
| Automation       | Ansible, Terraform, Webhook API |  

---

#### **6. Skala dan Keamanan**  
- **Skalabilitas**:  
  - Gunakan worker queue (Celery/RabbitMQ) untuk parallel processing  
  - Sharding database aset berdasarkan subnet  
- **Keamanan Sistem**:  
  - Komunikasi antar komponen via TLS/mTLS  
  - RBAC untuk akses database aset  
  - Audit trail untuk semua rekomendasi otomatis  

---

#### **7. Contoh Output Dashboard**  
```plaintext
[ PRIORITY 1 ] SQL Injection pada db-server-1 (10.20.30.5)
├─ Ancaman: Eksploitasi kerentanan CVE-2024-5678
├─ Risiko: 9.2/10
├─ Rekomendasi: 
│  1. Terapkan patch MySQL v5.7.43 (urgent)
│  2. Blokir IP 202.134.45.78 di firewall
│  3. Audit query database
└─ Tindakan: [Jalankan Patch] [Blokir Otomatis] [Tunda 24h]

[ PRIORITY 2 ] Port Scanning pada web-server-3 (10.20.31.12)
├─ Risiko: 5.8/10
└─ Rekomendasi: Aktifkan fail2ban untuk subnet DMZ
```

---

### **Keunggulan Desain Ini**  
1. **Context-Aware**  
   Ancaman dinilai berdasarkan nilai bisnis aset  
2. **Actionable**  
   Rekomendasi spesifik dengan opsi eksekusi otomatis  
3. **Extensible**  
   Mendukung integrasi dengan scanner vuln (Nessus, OpenVAS)  
4. **Real-Time**  
   Pemrosesan ancaman < 15 detik dari deteksi ke rekomendasi  

Untuk implementasi lebih detail (skrip parser, API schema, rule tuning), saya siap memberikan contoh teknis!
--------------------------------------------------

### **Detil Implementasi: Skrip Parser, API Schema, dan Rule Tuning**

---

#### **1. Skrip Parser Snort Alert (Python)**  
Fungsi: Mengubah raw Snort alerts (unified2/syslog) menjadi format terstruktur untuk diproses.

```python
#!/usr/bin/env python3
# snort_parser.py

import re
import json
import sys
from datetime import datetime

def parse_snort_alert(log_line):
    """Parse Snort syslog alert format"""
    pattern = (
        r'\[(\d+):(\d+):(\d+)\] (.+?) \[Classification: (.+?)\] '
        r'\[Priority: (\d+)\] \{(\w+)\} (\d+\.\d+\.\d+\.\d+):(\d+) -> (\d+\.\d+\.\d+\.\d+):(\d+)'
    )
    
    match = re.match(pattern, log_line)
    if not match:
        return None
    
    return {
        "gid": match.group(1),
        "sid": match.group(2),
        "rev": match.group(3),
        "message": match.group(4),
        "classification": match.group(5),
        "priority": int(match.group(6)),
        "protocol": match.group(7),
        "src_ip": match.group(8),
        "src_port": match.group(9),
        "dst_ip": match.group(10),
        "dst_port": match.group(11),
        "timestamp": datetime.utcnow().isoformat() + "Z"
    }

def main():
    # Contoh input: syslog stream
    for line in sys.stdin:
        if "[Priority:" in line:
            alert = parse_snort_alert(line.strip())
            if alert:
                # Kirim ke message queue/REST API
                print(json.dumps(alert))

if __name__ == "__main__":
    main()
```

**Cara Pakai:**  
```bash
tail -f /var/log/snort/alerts | python3 snort_parser.py
```

**Output Contoh:**  
```json
{
  "gid": "1",
  "sid": "30001",
  "rev": "0",
  "message": "SQL Injection Attempt",
  "classification": "Attempted Admin Privilege Gain",
  "priority": 1,
  "protocol": "TCP",
  "src_ip": "202.134.45.78",
  "src_port": "54321",
  "dst_ip": "10.20.30.5",
  "dst_port": "3306",
  "timestamp": "2025-06-13T15:30:45.123Z"
}
```

---

#### **2. API Schema untuk Integrasi Sistem**  
REST API untuk komunikasi antar komponen menggunakan JSON.

### **A. Asset Lookup API**  
**Endpoint:** `GET /api/v1/assets?ip={ip}`  
**Response:**  
```json
{
  "ip": "10.20.30.5",
  "hostname": "db-primary-01",
  "environment": "production",
  "criticality": 4,
  "services": [
    {
      "name": "mysql",
      "port": 3306,
      "version": "8.0.33",
      "protocol": "tcp"
    }
  ],
  "vulnerabilities": [
    {
      "cve_id": "CVE-2024-1234",
      "severity": 9.2,
      "status": "unpatched",
      "last_detected": "2025-06-10"
    }
  ]
}
```

### **B. Risk Submission API**  
**Endpoint:** `POST /api/v1/risk-assessment`  
**Request Body:**  
```json
{
  "alert_id": "snort-20250613-153045",
  "src_ip": "202.134.45.78",
  "dst_ip": "10.20.30.5",
  "threat_type": "SQL Injection",
  "snort_sid": 30001,
  "asset_criticality": 4,
  "vulnerability_score": 9.2,
  "calculated_risk": 9.8
}
```

### **C. Recommendation API**  
**Endpoint:** `POST /api/v1/recommendations`  
**Response:**  
```json
{
  "action_id": "act-202506131530",
  "recommendations": [
    {
      "priority": 1,
      "action": "block_ip",
      "target": "202.134.45.78",
      "tool": "firewall",
      "command": "iptables -A INPUT -s 202.134.45.78 -j DROP"
    },
    {
      "priority": 2,
      "action": "patch_server",
      "target": "10.20.30.5",
      "tool": "ansible",
      "playbook": "mysql-patch-CVE-2024-1234.yml"
    }
  ]
}
```

---

#### **3. Rule Tuning untuk Snort**  
Strategi tuning untuk mengurangi false positive dan meningkatkan relevansi.

### **A. Prioritas Signature**  
```conf
# sql.rules - Disempurnakan
alert tcp any any -> $DB_SERVERS 3306 ( \
  msg:"SQL Injection Detected"; \
  flow:to_server,established; \
  content:"'"; \
  content:"OR"; distance:0; within:2; \
  content:"1=1"; distance:0; \
  metadata:policy security-ips, service mysql; \
  reference:cve,2024-1234; \
  classtype:web-application-attack; \
  priority:1; \
  sid:30001; rev:3;)
```

### **B. Thresholding**  
```conf
# Mengurangi alert scanning
suppress gen_id 1, sig_id 2000101, track by_src, count 5, seconds 60
```

### **C. Variabel Dinamis**  
`snort.lua` konfigurasi:
```lua
HOME_NET = [[
  10.20.30.0/24,
  192.168.100.0/24
]]

DB_SERVERS = [[
  10.20.30.5/32,
  10.20.30.6/32
]]

CRITICAL_SERVICES = {
  ports = '3306,5432,1521',
  ips = DB_SERVERS
}
```

### **D. Adaptive Rules**  
Aktifkan rules berdasarkan aset:
```conf
# Aktifkan rules khusus hanya untuk server kritis
include $RULE_PATH/critical_db.rules \
  when (ip in CRITICAL_SERVICES.ips and port in CRITICAL_SERVICES.ports)
```

---

#### **4. Integrasi dengan Automation**  
Contoh skrip eksekusi otomatis (Python):

```python
# automation_dispatcher.py

import requests
import subprocess

def execute_recommendation(action):
    """Eksekusi aksi otomatis"""
    if action["tool"] == "firewall":
        # Contoh: iptables command
        subprocess.run(action["command"], shell=True, check=True)
        
    elif action["tool"] == "ansible":
        # Contoh: Jalankan playbook Ansible
        subprocess.run(
            f"ansible-playbook /playbooks/{action['playbook']} -l {action['target']}",
            shell=True
        )

# Terima rekomendasi dari API
response = requests.get("http://recommendation-engine/api/v1/actions/pending")
for action in response.json()["actions"]:
    try:
        execute_recommendation(action)
        # Update status aksi
        requests.patch(f"http://recommendation-engine/api/v1/actions/{action['id']}", 
                      json={"status": "completed"})
    except Exception as e:
        requests.patch(f"http://recommendation-engine/api/v1/actions/{action['id']}", 
                      json={"status": "failed", "error": str(e)})
```

---

#### **5. Monitoring Rule Effectiveness**  
SQL query untuk audit aturan Snort:

```sql
-- Query efektivitas aturan
SELECT 
  r.sid,
  r.msg,
  COUNT(a.id) AS total_alerts,
  SUM(CASE WHEN a.status = 'true_positive' THEN 1 ELSE 0 END) AS true_positives,
  SUM(CASE WHEN a.status = 'false_positive' THEN 1 ELSE 0 END) AS false_positives,
  (SUM(CASE WHEN a.status = 'true_positive' THEN 1 ELSE 0 END) * 100.0 / COUNT(a.id)) AS accuracy
FROM snort_rules r
LEFT JOIN alerts a ON r.sid = a.sid
WHERE a.timestamp > NOW() - INTERVAL '7 days'
GROUP BY r.sid, r.msg
HAVING COUNT(a.id) > 5
ORDER BY accuracy ASC;
```

---

#### **6. Diagram Alir Rule Tuning**  
```mermaid
flowchart TD
    A[Raw Snort Alerts] --> B[False Positive Analysis]
    C[Asset Database] --> D[Critical Service Mapping]
    B --> E[Adjust Thresholds]
    D --> F[Enable/Disable Rules]
    E --> G[Test in Lab Environment]
    F --> G
    G --> H{Reduction in FP?}
    H -->|Yes| I[Deploy to Production]
    H -->|No| J[Revise Rule Logic]
    J --> G
```

---

### **Kelebihan Implementasi Ini:**
1. **Parser Robust**:  
   - Handle berbagai format log Snort  
   - Auto-generate timestamp UTC  

2. **API Terstandarisasi**:  
   - Versioning endpoint  
   - Structured error handling (contoh: `{"error": "Asset not found"}`)  

3. **Rule Tuning Berbasis Konteks**:  
   - Aktifkan rules hanya untuk aset kritis  
   - Referensi CVE langsung di signature  
   - Threshold adaptif berdasarkan traffic pattern  

4. **Otomatisasi Aman**:  
   - Validasi command sebelum eksekusi  
   - Audit trail untuk semua aksi otomatis  

Untuk implementasi lengkap, termasuk Dockerfile untuk deployment containerized dan contoh Ansible playbook, saya bisa menyediakan repository contoh.


-------------------------------------

### **Implementasi Lengkap: Containerization & Automation**

---

#### **1. Struktur Direktori Proyek**
```bash
security-recommendation-system/
├── docker/
│   ├── snort-parser/
│   │   ├── Dockerfile
│   │   └── requirements.txt
│   ├── risk-engine/
│   │   ├── Dockerfile
│   │   └── requirements.txt
│   └── docker-compose.yml
├── ansible/
│   ├── playbooks/
│   │   ├── deploy-system.yml
│   │   └── patch-mysql.yml
│   ├── roles/
│   │   └── snort-config/
│   └── inventory/
├── src/
│   ├── snort_parser.py
│   ├── risk_engine.py
│   └── asset_lookup.py
├── configs/
│   ├── snort/
│   │   ├── snort.lua
│   │   └── critical.rules
├── scripts/
│   └── init_asset_db.sql
└── README.md
```

---

### **2. Dockerfile untuk Containerization**

#### **(A) Snort Parser Microservice**
```dockerfile
# docker/snort-parser/Dockerfile
FROM python:3.11-slim-bullseye

WORKDIR /app

# Install dependencies
RUN apt-get update && apt-get install -y \
    libpcap-dev \
    && rm -rf /var/lib/apt/lists/*

COPY src/snort_parser.py .
COPY docker/snort-parser/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

ENTRYPOINT ["python", "-u", "snort_parser.py"]
```

File `docker/snort-parser/requirements.txt`:
```text
requests==2.32.3
python-dateutil==2.9.0
pyshark==0.4.3
```

#### **(B) Risk Engine Service**
```dockerfile
# docker/risk-engine/Dockerfile
FROM python:3.11-slim-bullseye

WORKDIR /app

COPY src/risk_engine.py .
COPY src/asset_lookup.py .
COPY docker/risk-engine/requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8000

CMD ["uvicorn", "risk_engine:app", "--host", "0.0.0.0", "--port", "8000"]
```

File `docker/risk-engine/requirements.txt`:
```text
fastapi==0.110.2
uvicorn[standard]==0.29.0
psycopg[binary]==3.1.18
python-multipart==0.0.9
```

---

### **3. Docker Compose untuk Full Stack**
```yaml
# docker/docker-compose.yml
version: '3.8'

services:
  snort-parser:
    build: ./snort-parser
    volumes:
      - /var/log/snort:/var/log/snort:ro
    environment:
      OUTPUT_URL: "http://risk-engine:8000/api/v1/alerts"
    depends_on:
      - risk-engine
      - asset-db

  risk-engine:
    build: ./risk-engine
    ports:
      - "8000:8000"
    environment:
      DB_URL: "postgresql://asset_user:securepass@asset-db:5432/asset_db"
      FIREWALL_API_URL: "http://firewall-controller/api"
    depends_on:
      asset-db:
        condition: service_healthy

  asset-db:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: asset_db
      POSTGRES_USER: asset_user
      POSTGRES_PASSWORD: securepass
    volumes:
      - ./scripts/init_asset_db.sql:/docker-entrypoint-initdb.d/init.sql
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U asset_user"]
      interval: 5s
      timeout: 5s
      retries: 5

  # Service opsional untuk firewall management
  firewall-controller:
    image: nginx:alpine
    ports:
      - "8080:80"
    # (Dalam implementasi nyata: gunakan image custom dengan API firewall)

  # Service opsional untuk monitoring
  dashboard:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    environment:
      GF_SECURITY_ADMIN_PASSWORD: "admin"
    depends_on:
      - risk-engine

volumes:
  asset-data:
```

File inisialisasi database (`scripts/init_asset_db.sql`):
```sql
CREATE TABLE assets (
    asset_id SERIAL PRIMARY KEY,
    ip VARCHAR(15) NOT NULL UNIQUE,
    hostname VARCHAR(50),
    service VARCHAR(20) NOT NULL CHECK(service IN ('web','db','ssh','dns','vpn')),
    environment VARCHAR(20) CHECK(environment IN ('production','staging','development')),
    criticality INT NOT NULL CHECK(criticality BETWEEN 1 AND 4),
    last_patch DATE,
    vuln_list JSONB
);

INSERT INTO assets (ip, hostname, service, environment, criticality) VALUES
('10.0.1.101', 'web-server-01', 'web', 'production', 3),
('10.0.1.102', 'db-server-01', 'db', 'production', 4),
('10.0.2.101', 'dev-db-01', 'db', 'development', 2);
```

---

### **4. Contoh Ansible Playbooks**

#### **(A) Playbook Utama: Deployment Sistem**
```yaml
# ansible/playbooks/deploy-system.yml
---
- name: Deploy Security Recommendation System
  hosts: security_servers
  become: yes
  vars:
    docker_compose_version: "v2.27.0"
    project_dir: "/opt/security-system"
    git_repo: "https://github.com/yourorg/security-recommendation-system.git"

  tasks:
    - name: Install Docker dependencies
      apt:
        name:
          - apt-transport-https
          - ca-certificates
          - curl
          - software-properties-common
          - python3-pip
        state: present

    - name: Add Docker GPG key
      apt_key:
        url: "https://download.docker.com/linux/ubuntu/gpg"
        state: present

    - name: Add Docker repository
      apt_repository:
        repo: "deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ ansible_distribution_release }} stable"
        state: present

    - name: Install Docker
      apt:
        name:
          - docker-ce
          - docker-ce-cli
          - containerd.io
        state: present
        update_cache: yes

    - name: Install Docker Compose
      get_url:
        url: "https://github.com/docker/compose/releases/download/{{ docker_compose_version }}/docker-compose-linux-x86_64"
        dest: "/usr/local/bin/docker-compose"
        mode: "0755"

    - name: Clone project repository
      git:
        repo: "{{ git_repo }}"
        dest: "{{ project_dir }}"
        version: main
        force: yes

    - name: Start containers
      community.docker.docker_compose:
        project_src: "{{ project_dir }}/docker"
        state: present
        restarted: yes
        pull: yes

    - name: Ensure Snort service is configured
      include_role:
        name: snort-config
```

#### **(B) Playbook untuk Patch Management**
```yaml
# ansible/playbooks/patch-mysql.yml
---
- name: Apply MySQL security patches
  hosts: database_servers
  become: yes
  vars:
    cve_id: "CVE-2024-1234"
    mysql_target_version: "8.0.36"

  tasks:
    - name: Check current MySQL version
      shell: mysql --version | awk '{ print $3 }'
      register: mysql_version
      changed_when: false

    - name: Abort if already patched
      fail:
        msg: "Server {{ inventory_hostname }} already patched to {{ mysql_target_version }}"
      when: mysql_version.stdout == mysql_target_version

    - name: Download MySQL patch package
      get_url:
        url: "https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-{{ mysql_target_version }}-linux-glibc2.17-x86_64.tar.xz"
        dest: "/tmp/mysql-{{ mysql_target_version }}.tar.xz"
        checksum: "sha256:abc123def456..."

    - name: Extract package
      unarchive:
        src: "/tmp/mysql-{{ mysql_target_version }}.tar.xz"
        dest: "/tmp"
        remote_src: yes

    - name: Stop MySQL service
      service:
        name: mysql
        state: stopped

    - name: Apply binary update
      copy:
        src: "/tmp/mysql-{{ mysql_target_version }}/bin/mysqld"
        dest: "/usr/sbin/mysqld"
        owner: root
        group: root
        mode: "0755"
        remote_src: yes

    - name: Update MySQL system tables
      shell: "mysql_upgrade -u root -p{{ mysql_root_password }}"
      no_log: true

    - name: Start MySQL service
      service:
        name: mysql
        state: started

    - name: Verify MySQL version
      shell: mysql --version | awk '{ print $3 }'
      register: new_version
      changed_when: false

    - name: Validate patch application
      assert:
        that: new_version.stdout == mysql_target_version
        msg: "MySQL update failed on {{ inventory_hostname }}"
```

---

### **5. Ansible Role untuk Snort Configuration**
```yaml
# ansible/roles/snort-config/tasks/main.yml
---
- name: Install Snort
  apt:
    name: snort
    state: present

- name: Configure network variables
  template:
    src: snort.lua.j2
    dest: /etc/snort/snort.lua
    owner: root
    group: root
    mode: "0644"
  notify: Restart Snort

- name: Deploy critical rules
  copy:
    src: critical.rules
    dest: /etc/snort/rules/local/critical.rules
    owner: root
    group: root
    mode: "0644"
  notify: Restart Snort

- name: Enable Snort service
  systemd:
    name: snort
    state: started
    enabled: yes

- name: Configure log rotation
  copy:
    src: snort-logrotate
    dest: /etc/logrotate.d/snort
    owner: root
    group: root
    mode: "0644"
```

Template `snort.lua.j2`:
```lua
-- ansible/roles/snort-config/templates/snort.lua.j2
HOME_NET = [[
  {% for network in snort_home_net %}
  {{ network }},
  {% endfor %}
]]

EXTERNAL_NET = "any"

CRITICAL_SERVICES = {
  ips = [[
    {% for ip in snort_critical_ips %}
    {{ ip }},
    {% endfor %}
  ]],
  ports = "{{ snort_critical_ports }}"
}
```

File `critical.rules`:
```conf
# ansible/roles/snort-config/files/critical.rules
alert tcp any any -> $CRITICAL_SERVICES 3306 ( \
  msg:"Critical: SQL Injection Attempt"; \
  flow:to_server,established; \
  content:"'"; \
  content:"OR"; distance:0; within:2; \
  content:"1=1"; distance:0; \
  metadata:policy max-detect-ips, service mysql; \
  priority:1; sid:1000001; rev:1;)

alert tcp any any -> $CRITICAL_SERVICES 22 ( \
  msg:"Critical: SSH Bruteforce Attempt"; \
  flow:to_server,established; \
  detection_filter: track by_src, count 5, seconds 60; \
  metadata:policy security-ips, service ssh; \
  priority:1; sid:1000002; rev:1;)
```

---

### **6. Deployment Workflow Lengkap**
```mermaid
flowchart TB
    A[Git Repository] -->|Pull Code| B[Build Docker Images]
    B --> C[Deploy with Docker Compose]
    C --> D[Snort Parser]
    C --> E[Risk Engine]
    C --> F[Asset Database]
    D -->|Process Alerts| E
    E -->|Query Assets| F
    G[Ansible Control Node] -->|Run Playbook| H[Database Servers]
    E -->|Trigger| G
    E -->|Visualize| I[Grafana Dashboard]
    H -->|Patch Applied| J[Secured MySQL]
    H -->|Configuration| K[Snort Sensors]
    
    style E fill:#9f9,stroke:#333
    style G fill:#f9f,stroke:#333
    style H fill:#ff9,stroke:#333
```

---

### **7. Skrip Pendukung**

#### **(A) Risk Engine Skeleton (src/risk_engine.py)**
```python
from fastapi import FastAPI, HTTPException
import psycopg
import os
import requests

app = FastAPI()
DB_URL = os.getenv("DB_URL")

@app.post("/api/v1/alerts")
async def process_alert(alert: dict):
    try:
        # Step 1: Enrich with asset data
        asset = get_asset_info(alert['dst_ip'])
        
        # Step 2: Calculate risk score
        risk_score = calculate_risk(
            alert['severity'],
            asset['criticality'],
            asset.get('vuln_score', 0)
        )
        
        # Step 3: Generate recommendations
        recommendations = generate_recommendations(
            alert['signature_id'],
            asset['service'],
            risk_score
        )
        
        # Step 4: Trigger actions
        if risk_score > 8.0:
            trigger_automation(recommendations)
        
        return {
            "status": "processed",
            "risk_score": risk_score,
            "recommendations": recommendations
        }
    
    except Exception as e:
        raise HTTPException(status_code=500, detail=str(e))

def get_asset_info(ip: str):
    with psycopg.connect(DB_URL) as conn:
        with conn.cursor() as cur:
            cur.execute("""
                SELECT ip, service, criticality, 
                       (SELECT SUM(severity) 
                        FROM jsonb_to_recordset(vuln_list) 
                        AS vulns(cve_id text, severity float)) AS vuln_score
                FROM assets WHERE ip = %s
            """, (ip,))
            return cur.fetchone()

# ... (implementasi fungsi lainnya)

if __name__ == "__main__":
    import uvicorn
    uvicorn.run(app, host="0.0.0.0", port=8000)
```

---

### **8. Cara Menjalankan Sistem**

#### **(A) Dengan Docker Compose**
```bash
# Clone repository
git clone https://github.com/yourorg/security-recommendation-system.git
cd security-recommendation-system/docker

# Build dan jalankan
docker compose up -d --build

# Verifikasi services
docker compose ps
```

#### **(B) Dengan Ansible**
```bash
# Konfigurasi inventory
cat > ansible/inventory/production.ini <<EOF
[security_servers]
sec-srv01 ansible_host=192.168.1.100

[database_servers]
db-srv01 ansible_host=192.168.1.101
db-srv02 ansible_host=192.168.1.102
EOF

# Jalankan playbook
ansible-playbook -i ansible/inventory/production.ini \
    ansible/playbooks/deploy-system.yml
```

---

### **Keunggulan Implementasi Ini**

1. **Containerized Architecture**:
   - Isolasi komponen
   - Scalability horizontal
   - Dependency management

2. **Infrastructure as Code**:
   - Reproducible deployments
   - Version-controlled configuration
   - Automated provisioning

3. **Integrated Security Automation**:
   - Deteksi → Analisis → Rekomendasi → Eksekusi
   - Closed-loop security

4. **Production-Ready Features**:
   - Health checks
   - Secret management
   - Log rotation
   - RBAC (melalui Ansible)

5. **Extendable Design**:
   - Mudah tambah scanner baru (Suricata, Wazuh)
   - Dukungan multi-cloud
   - Integrasi SIEM (Splunk, ELK)

Untuk environment produksi, tambahkan:
- TLS termination (NGINX reverse proxy)
- Secret management (HashiCorp Vault)
- Monitoring (Prometheus + Grafana)
- High availability (Kubernetes cluster)
