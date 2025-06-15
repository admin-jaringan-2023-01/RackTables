# Topologi Penerapan MATA ELANG NIDS pada Jaringan Intra-Pemerintah

## Diagram Topologi Komprehensif

```mermaid
graph TD
    subgraph Nasional[Data Center Nasional - BSSN]
        DC1[Defense Center 1]
        DC2[Defense Center 2]
        LB[Load Balancer]
        FW_DC[Firewall Cluster]
        IDM[Identity Management]
        HSMC[HSM Cluster]
        
        DC1 --> EL[Elasticsearch Cluster]
        DC2 --> EL
        EL --> KI[Kibana Dashboard]
        LB --> DC1
        LB --> DC2
        FW_DC --> LB
        HSMC --> DC1
        HSMC --> DC2
        IDM -->|Authentication| DC1
        IDM -->|Authentication| DC2
        
        SOC[SOC Nasional] --> KI
    end
    
    subgraph Provinsi[Data Center Provinsi]
        FW_PROV[Firewall Provinsi]
        SW_PROV[Core Switch]
        P_DC[Regional DC]
        P_S1[Prov Sensor 1]
        P_S2[Prov Sensor 2]
        
        FW_PROV --> SW_PROV
        SW_PROV -->|SPAN| P_S1
        SW_PROV -->|TAP| P_S2
        P_S1 -->|gRPC| P_DC
        P_S2 -->|gRPC| P_DC
        P_DC -->|gRPC over IPSec| FW_DC
    end
    
    subgraph Kabupaten[Kabupaten/Kota]
        FW_KAB[Firewall Kabupaten]
        SW_KAB[Core Switch]
        K_S1[Kab Sensor 1]
        K_S2[Kab Sensor 2]
        
        FW_KAB --> SW_KAB
        SW_KAB -->|SPAN| K_S1
        SW_KAB -->|TAP| K_S2
        K_S1 -->|gRPC over MPLS| P_DC
        K_S2 -->|gRPC over MPLS| P_DC
    end
    
    subgraph Instansi[Instansi Pusat - K/L]
        FW_INST[Firewall Instansi]
        SW_INST[Core Switch]
        I_S1[Instansi Sensor 1]
        I_S2[Instansi Sensor 2]
        
        FW_INST --> SW_INST
        SW_INST -->|SPAN| I_S1
        SW_INST -->|TAP| I_S2
        I_S1 -->|gRPC over IPSec| FW_DC
        I_S2 -->|gRPC over IPSec| FW_DC
    end
    
    subgraph Internet[Koneksi Internet]
        EXT_FW[Firewall Internet]
        EXT_FW -->|Threat Intel Feed| DC1
        EXT_FW -->|Threat Intel Feed| DC2
    end
    
    Admin_Pusat[Admin Pusat] -->|VPN| IDM
    Admin_Prov[Admin Provinsi] -->|VPN| P_DC
    Admin_Inst[Admin Instansi] -->|VPN| FW_INST

    classDef gov fill:#1f77b4,stroke:#333,stroke-width:2px,color:#fff;
    classDef sensor fill:#ff7f0e,stroke:#333,stroke-width:1px;
    classDef security fill:#d62728,stroke:#333,stroke-width:2px,color:#fff;
    classDef infra fill:#2ca02c,stroke:#333,stroke-width:1px;
    classDef admin fill:#9467bd,stroke:#333,stroke-width:1px;
    
    class Nasional,Provinsi,Kabupaten,Instansi gov;
    class P_S1,P_S2,K_S1,K_S2,I_S1,I_S2 sensor;
    class FW_DC,FW_PROV,FW_KAB,FW_INST,HSMC security;
    class SW_PROV,SW_KAB,SW_INST,LB,EXT_FW infra;
    class SOC,Admin_Pusat,Admin_Prov,Admin_Inst admin;
```

## Keterangan Topologi

### 1. Lapisan Nasional (BSSN/Kominfo)
- **Defense Center Cluster**: Dua node HA dengan load balancing
- **Elasticsearch Cluster**: Penyimpanan dan analisis data terdistribusi
- **HSM Cluster**: Enkripsi data dan manajemen kriptografi
- **Firewall Cluster**: Perlindungan perimeter dan segmentasi jaringan
- **Identity Management**: Otentikasi terpusat untuk semua pengguna

### 2. Lapisan Provinsi
- **Regional Defense Center**: Node regional untuk agregasi data kabupaten
- **Sensor Provinsi**: Terpasang di core switch jaringan provinsi
- **Network TAP/SPAN**: Metode monitoring lalu lintas jaringan
- **Firewall Provinsi**: Perlindungan perimeter jaringan provinsi

### 3. Lapisan Kabupaten/Kota
- **Sensor Kabupaten**: Terpasang di jaringan inti kabupaten
- **Core Switch**: Perangkat jaringan utama dengan port SPAN
- **Firewall Kabupaten**: Perlindungan perimeter jaringan kabupaten
- **Koneksi MPLS**: Keamanan komunikasi ke pusat provinsi

### 4. Lapisan Instansi Pusat (K/L)
- **Sensor Instansi**: Terpasang di jaringan masing-masing kementerian/lembaga
- **Koneksi IPSec**: Tunnel aman ke Data Center Nasional
- **Management Terpisah**: Admin lokal dengan akses terkontrol

### 5. Komponen Keamanan
- **Mutual TLS**: Autentikasi dua arah antara sensor dan defense center
- **Network Segmentation**: Zona keamanan terpisah untuk komponen kritis
- **Threat Intelligence**: Update signature otomatis dari sumber terpercaya
- **HSM**: Perlindungan kunci kriptografi tingkat perangkat keras

## Diagram Topologi Fisik

```mermaid
graph LR
    subgraph Rack_Nasional[Rack Data Center Nasional]
        RU1[RU 1-3: Server Defense Center]
        RU4[RU 4-6: Elasticsearch Nodes]
        RU7[RU 7: Load Balancers]
        RU8[RU 8: Firewall Cluster]
        RU9[RU 9: HSM Appliances]
        RU10[RU 10: Network Storage]
    end
    
    subgraph Rack_Prov[Rack Data Center Provinsi]
        RUP1[RU 1: Regional DC Server]
        RUP2[RU 2: Firewall]
        RUP3[RU 3: Core Switch]
        RUP4[RU 4: Sensor Nodes]
    end
    
    subgraph Kab_Rack[Rack Kabupaten]
        RUK1[RU 1: Core Switch]
        RUK2[RU 2: Firewall]
        RUK3[RU 3: Sensor Node]
    end
    
    subgraph Instansi_Rack[Rack Instansi]
        RUI1[RU 1: Core Switch]
        RUI2[RU 2: Firewall]
        RUI3[RU 3: Sensor Node]
    end
    
    Rack_Nacional -->|DWDM Fiber| Rack_Prov
    Rack_Prov -->|Fiber MetroE| Kab_Rack
    Rack_Nasional -->|IPSec VPN| Instansi_Rack
    
    classDef rack fill:#8c564b,stroke:#333,stroke-width:1px,color:#fff;
    class Rack_Nasional,Rack_Prov,Kab_Rack,Instansi_Rack rack;
```

## Spesifikasi Konektivitas

### Jalur Komunikasi Utama
| Rute                  | Protokol      | Media         | Bandwidth | Latensi Maks |
|-----------------------|--------------|---------------|-----------|--------------|
| Nasional-Provinsi     | gRPC over IPSec | DWDM Fiber    | 10 Gbps   | <10 ms       |
| Provinsi-Kabupaten    | gRPC over MPLS  | Metro Ethernet| 1 Gbps    | <20 ms       |
| Nasional-Instansi     | gRPC over IPSec | Internet VPN  | 1 Gbps    | <50 ms       |
| Sensor-Defense Center | gRPC          | Jaringan Intra| 1-10 Gbps | <5 ms        |

### Konfigurasi Keamanan Jaringan
1. **Segmentasi Zona**:
   - Zona Publik: Interface internet
   - DMZ: Layanan eksternal
   - Zona Internal: Sistem operasional
   - Zona Manajemen: Akses administratif
   - Zona Sensor: Komunikasi khusus sensor

2. **Kontrol Akses**:
   - RBAC (Role-Based Access Control) terpusat
   - Network Access Control berbasis sertifikat
   - Mikrosegmentasi antar komponen

3. **Enkripsi Data**:
   - Data in-transit: TLS 1.3 dengan cipher suite AEAD
   - Data at-rest: AES-256 dengan kunci di HSM
   - Key rotation otomatis setiap 90 hari

## Diagram Alur Operasional

```mermaid
sequenceDiagram
    participant Sensor as Sensor Node
    participant DC as Defense Center
    participant SOC as SOC Nasional
    participant CSIRT as CSIRT Nasional
    
    Sensor->>DC: Stream alert via gRPC (realtime)
    DC->>DC: Korelasi lintas sensor
    DC->>SOC: Notifikasi ancaman kritis
    SOC->>DC: Verifikasi insiden
    alt Ancaman Terkonfirmasi
        SOC->>CSIRT: Eskalasi insiden
        CSIRT->>DC: Request mitigasi
        DC->>Sensor: Push blocking rule
        Sensor->>Sensor: Terapkan aturan blokir
        Sensor->>DC: Konfirmasi mitigasi
        DC->>CSIRT: Lapor hasil
    else False Positive
        SOC->>DC: Update false positive signature
        DC->>Sensor: Update ruleset
    end
```

## Keuntungan Topologi Terpadu

1. **Pertahanan Berlapis**:
   - Deteksi di edge (sensor)
   - Analisis di regional (provinsi)
   - Korelasi di nasional (pusat)
   
2. **Resiliensi Jaringan**:
   - Multiple path komunikasi
   - Redundansi komponen kritis
   - Failover otomatis

3. **Manajemen Terpusat**:
   - Pembaruan kebijakan terpusat
   - Monitoring kesehatan seluruh sensor
   - Distribusi signature serangan

4. **Kepatuhan Regulasi**:
   - Pemenuhan SPBE (Sistem Pemerintahan Berbasis Elektronik)
   - Sesuai standar SNI ISO/IEC 27001
   - Audit trail terintegrasi

## Strategi Implementasi Topologi

### Fase 1: Penyiapan Infrastruktur Pusat
```mermaid
gantt
    title Penyiapan Infrastruktur Pusat
    dateFormat  YYYY-MM-DD
    section Perangkat Keras
    Pemasangan Server :done, h1, 2024-01-01, 30d
    Penyiapan Jaringan :active, h2, after h1, 30d
    Instalasi HSM : h3, after h2, 15d
    
    section Perangkat Lunak
    Deploy Defense Center : h4, after h1, 45d
    Konfigurasi Cluster : h5, after h4, 15d
    Integrasi Sistem : h6, after h5, 30d
    
    section Pengujian
    Uji Beban : h7, after h6, 15d
    Uji Keamanan : h8, after h7, 15d
    Sertifikasi : h9, after h8, 30d
```

### Fase 2: Implementasi Bertahap di Daerah
1. **Pilot Project**: 1 provinsi + 3 kabupaten
2. **Ekspansi Regional**: 5 provinsi berikutnya
3. **Coverage Nasional**: Seluruh provinsi dan kabupaten
4. **Integrasi Instansi**: K/L strategis terlebih dahulu

Topologi ini dirancang untuk memenuhi kebutuhan khusus jaringan pemerintah dengan memperhatikan:
- **Kedaulatan Data**: Seluruh infrastruktur dalam negeri
- **Keamanan Berlapis**: Pertahanan depth-in-defense
- **Skalabilitas Hierarkis**: Dari pusat hingga daerah terpencil
- **Keterpaduan Sistem**: Integrasi dengan existing infrastructure
- **Kepatuhan Regulasi**: Memenuhi semua persyaratan keamanan pemerintah

Dengan implementasi topologi ini, MATA ELANG NIDS akan membentuk tulang punggung sistem deteksi intrusi nasional yang terpadu, aman, dan responsif terhadap ancaman siber terkini.
