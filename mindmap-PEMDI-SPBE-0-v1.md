### Diagram Sekuens: Proses Transisi Evaluasi PEMDI untuk Pemerintah Daerah

```mermaid
sequenceDiagram
    actor KemenPANRB
    participant Pemda as Pemerintah Daerah
    participant TimKoordinasi as Tim Koordinasi PEMDI Daerah
    participant OPD as Perangkat Daerah (OPD) Terkait

    %% --- Tahap 1: Inisiasi dan Analisis Internal ---

    KemenPANRB ->> Pemda: Menerbitkan Kebijakan & Pedoman Evaluasi PEMDI
    
    Pemda ->> TimKoordinasi: Membentuk / Merevitalisasi Tim Koordinasi PEMDI
    activate TimKoordinasi

    TimKoordinasi ->> OPD: Melakukan Sosialisasi 35 Indikator PEMDI Baru
    
    OPD -->> TimKoordinasi: Melakukan Identifikasi Awal Kondisi Eksisting
    
    TimKoordinasi ->> TimKoordinasi: Melakukan Evaluasi Mandiri Awal & Analisis Kesenjangan (Gap Analysis)
    note over TimKoordinasi: Membandingkan bukti dukung yang ada dengan kebutuhan 35 indikator baru

    %% --- Tahap 2: Perencanaan dan Perancangan Intervensi ---

    TimKoordinasi ->> OPD: Menggelar Rapat Koordinasi Pembahasan Hasil Analisis
    activate OPD
    
    OPD -->> TimKoordinasi: Memberikan Masukan untuk Program Prioritas
    
    TimKoordinasi ->> TimKoordinasi: Menyusun Draf Peta Rencana (Roadmap) Akselerasi PEMDI
    note over TimKoordinasi: Fokus pada aspek baru berbobot tinggi: <br/>- Kapabilitas & Budaya Digital<br/>- Pengelolaan & Pemanfaatan Data<br/>- Keterpaduan Layanan<br/>- Kepuasan Pengguna
    
    TimKoordinasi ->> Pemda: Mengajukan dan Menetapkan Peta Rencana Akselerasi
    deactivate TimKoordinasi

    %% --- Tahap 3: Implementasi dan Pengumpulan Bukti
