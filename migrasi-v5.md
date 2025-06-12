### **Rencana Migrasi Pusat Data: 14 Hari (20 September - 3 Oktober 2025)**  
Berikut adalah alur kerja migrasi terstruktur dalam **3 fase utama**, dilengkapi jadwal harian dan diagram Mermaid.

---

### **Fase 1: Perencanaan dan Persiapan**  
*(20-25 September 2025)*  
**Tujuan:** Memastikan semua aspek teknis, logistik, dan koordinasi siap sebelum pemindahan.  

| Hari | Tanggal       | Aktivitas                                                                                               | Tim Terlibat                          |
|------|---------------|---------------------------------------------------------------------------------------------------------|---------------------------------------|
| 1    | 20 Sep (Sabtu)| **Kick-off Meeting**<br>- Menetapkan tujuan, timeline, dan tanggung jawab pihak terkait.                 | PM, Kepala IT, Admin Jaringan, ISP    |
| 2    | 21 Sep (Minggu)| **Inventarisasi & Dokumentasi**<br>- Mencatat konfigurasi perangkat (server, jaringan), topologi jaringan, dan label kabel. | Admin Jaringan, Teknisi              |
| 3    | 22 Sep (Senin)| **Persiapan Lokasi Baru**<br>- Verifikasi daya listrik, cooling, rack layout (8 rack), dan koneksi ISP. | Teknisi Telekomunikasi, ISP          |
| 4    | 23 Sep (Selasa)| **Backup & Rencana Darurat**<br>- Backup penuh data dan konfigurasi.<br>- Siapkan rencana rollback.      | Admin Jaringan, Teknisi              |
| 5    | 24 Sep (Rabu) | **Koordinasi dengan ISP**<br>- Konfirmasi maintenance window untuk migrasi koneksi internet.            | PM, ISP                              |
| 6    | 25 Sep (Kamis)| **Finalisasi SOP**<br>- SOP pemindahan fisik, pengujian, dan komunikasi darurat.                        | Semua Tim                            |

---

### **Fase 2: Pemindahan dan Pemasangan**  
*(26-28 September 2025)*  
**Tujuan:** Memindahkan perangkat secara fisik dan memasangnya di lokasi baru dengan minimal downtime.  

| Hari | Tanggal       | Aktivitas                                                                                               | Tim Terlibat                          |
|------|---------------|---------------------------------------------------------------------------------------------------------|---------------------------------------|
| 7    | 26 Sep (Jumat)| **Shutdown & Pencabutan** *(18:00-23:59)*<br>- Matikan server/perangkat.<br>- Label & cabut kabel/perpindahan dari 2 rack. | Admin Jaringan, Teknisi              |
| 8    | 27 Sep (Sabtu)| **Transportasi** *(08:00-12:00)*<br>- Pindahkan perangkat ke lokasi baru dengan pengamanan khusus.      | Teknisi, Logistik                    |
|      |               | **Pemasangan di 8 Rack** *(13:00-20:00)*<br>- Instal perangkat di rack baru sesuai layout.<br>- Pasang kabel (power & jaringan). | Teknisi Jaringan, Telekomunikasi     |
| 9    | 28 Sep (Minggu)| **Koneksi ISP & Routing Awal** *(08:00-15:00)*<br>- Aktifkan koneksi internet dasar dan routing statis. | ISP, Admin Jaringan                  |

---

### **Fase 3: Pengujian di Lokasi Baru**  
*(29 September - 3 Oktober 2025)*  
**Tujuan:** Memvalidasi fungsi seluruh sistem sebelum operasional penuh.  

| Hari | Tanggal       | Aktivitas                                                                                               | Tim Terlibat                          |
|------|---------------|---------------------------------------------------------------------------------------------------------|---------------------------------------|
| 10   | 29 Sep (Senin)| **Pengujian Konektivitas Dasar**<br>- Ping, VLAN, dan fisik (LED port).<br>- Verifikasi DHCP dan DNS.   | Admin Jaringan, Teknisi              |
| 11   | 30 Sep (Selasa)| **Pengujian Server & Aplikasi**<br>- Boot server, cek OS, dan aplikasi kritis.<br>- Uji akses storage.   | Admin Jaringan, Teknisi              |
| 12   | 1 Okt (Rabu)  | **Pengujian Jaringan Lanjutan**<br>- Uji routing, firewall, bandwidth, dan QoS.                         | Admin Jaringan, ISP                  |
| 13   | 2 Okt (Kamis) | **Pengujian Beban & Failover**<br>- Simulasi beban tinggi.<br>- Uji redundansi jaringan/server.          | Admin Jaringan, Teknisi              |
| 14   | 3 Okt (Jumat) | **Validasi & Dokumentasi Akhir**<br>- Tandatangan berita acara.<br>- Update dokumentasi topologi baru.  | Kepala IT, PM, Semua Tim             |

---

### **Diagram Alur Migrasi (Mermaid)**  
```mermaid
gantt
    title Jadwal Migrasi Pusat Data (20 Sep - 3 Okt 2025)
    dateFormat  YYYY-MM-DD
    axisFormat  %d/%m

    section Perencanaan dan Persiapan
    Kick-off Meeting           :done,    des1, 2025-09-20, 1d
    Inventarisasi & Dokumentasi :active,  des2, 2025-09-21, 2d
    Persiapan Lokasi Baru       :         des3, 2025-09-22, 2d
    Backup & Rencana Darurat    :         des4, 2025-09-23, 1d
    Koordinasi ISP              :         des5, 2025-09-24, 1d
    Finalisasi SOP              :         des6, 2025-09-25, 1d

    section Pemindahan dan Pemasangan
    Shutdown & Pencabutan       :crit,    a1, 2025-09-26, 1d
    Transportasi Perangkat      :crit,    a2, 2025-09-27, 1d
    Pemasangan di 8 Rack        :crit,    a3, 2025-09-27, 2d
    Koneksi ISP & Routing       :         a4, 2025-09-28, 1d

    section Pengujian di Lokasi Baru
    Pengujian Konektivitas Dasar  :       b1, 2025-09-29, 1d
    Pengujian Server & Aplikasi   :       b2, 2025-09-30, 1d
    Pengujian Jaringan Lanjutan   :       b3, 2025-10-01, 1d
    Pengujian Beban & Failover    :       b4, 2025-10-02, 1d
    Validasi & Dokumentasi Akhir  :       b5, 2025-10-03, 1d
```

---

### **Catatan Penting:**  
1. **Komunikasi:**  
   - Gunakan grup komunikasi real-time (misal: Slack/WhatsApp) untuk laporkan progres/hambatan.  
   - ISP harus tersedia selama fase pemindahan (26-28 Sep) untuk dukungan teknis.  

2. **Manajemen Risiko:**  
   - **Rollback Plan:** Jika pengujian gagal, kembali ke lokasi lama menggunakan backup (max 6 jam).  
   - **Downtime:** Pemadaman hanya terjadi pada 26 Sep (malam) hingga 29 Sep (pagi).  

3. **Kriteria Sukses:**  
   - Semua server/aplikasi berjalan normal di lokasi baru.  
   - Latency jaringan ≤ 5 ms, packet loss 0%.  
   - TTD (Turn Around Document) disetujui Kepala IT.  

