```mermaid
%% Diagram Korelasi Indeks SPBE ke Indeks Pemerintah Digital (PEMDI)
%% Versi Perbaikan - Dibuat oleh Gemini

graph LR
    
    %% DEFINISI SUBGRAPH KIRI (SPBE)
    subgraph Indeks SPBE (47 Indikator Lama)
        direction LR
        S1["1. Kebijakan Internal Tata Kelola"]
        S2["2. Kebijakan Internal Peta Rencana"]
        S3["3. Kebijakan Internal Manajemen Data"]
        S4["4. Kebijakan Internal Pembangunan Aplikasi"]
        S5["5. Kebijakan Internal Keamanan Informasi"]
        S6["6. Kebijakan Internal Audit TIK"]
        S7["7. Kebijakan Internal Tim Koordinasi"]
        S8["8. Kebijakan Internal Pusat Data"]
        S9["9. Kebijakan Internal Jaringan Intra"]
        S10["10. Perencanaan Strategis SPBE"]
        S11["11. Keterpaduan Rencana & Anggaran"]
        S12["12. Inovasi Proses Bisnis"]
        S13["13. Arsitektur SPBE"]
        S14["14. Manajemen Risiko SPBE"]
        S15["15. Manajemen Keamanan Informasi"]
        S16["16. Manajemen Data"]
        S17["17. Manajemen Aset TIK"]
        S18["18. Manajemen SDM SPBE"]
        S19["19. Manajemen Pengetahuan"]
        S20["20. Manajemen Perubahan"]
        S21["21. Manajemen Layanan SPBE"]
        S22["22. Audit Internal Pelaksanaan SPBE"]
        S23["23. Reviu dan Evaluasi Pelaksanaan"]
        S24["24. Layanan Administrasi Pemerintahan"]
        S25["25. Layanan Publik"]
        S26["26. Keterpaduan Layanan SPBE"]
        S27["27. Kualitas Layanan SPBE"]
        S28["28. Jaringan Intra"]
        S29["29. Pusat Data"]
        S30["30. Aplikasi Umum"]
        S31["31. Aplikasi Khusus"]
        S32["32. Keamanan SPBE"]
        S33["33. Interoperabilitas Data"]
        S34_47["34-47. Layanan Adm. & Publik Spesifik"]
    end

    %% DEFINISI SUBGRAPH KANAN (PEMDI)
    subgraph Indeks Pemerintah Digital (35 Indikator Baru)
        direction LR
        P1["1. Strategi Transformasi Digital"]
        P2["2. Inovasi Proses Bisnis Tematik"]
        P3["3. Arsitektur Pemerintah Digital"]
        P4["4. Peta Rencana Pemerintah Digital"]
        P5["5. Keterpaduan Rencana & Anggaran"]
        P6["6. Skalabilitas Koordinasi Internal"]
        P7["7. Kolaborasi Penerapan"]
        P8["8. Man. Risiko & Keberlangsungan"]
        P9["9. Manajemen Layanan"]
        P10["10. Pembangunan/Pengembangan Aplikasi"]
        P11["11. Pemanfaatan Ekosistem PDN"]
        P12["12. Layanan Jaringan Intra"]
        P13["13. Pemanfaatan Teknologi Baru"]
        P14["14. Audit Aplikasi Digital"]
        P15["15. Audit Infrastruktur Digital"]
        P16["16. Audit Keamanan"]
        P17["17. Keamanan Siber"]
        P18["18. Penerapan Kriptografi Nasional"]
        P19["19. Kapabilitas Penanganan Insiden"]
        P20["20. Kapabilitas SDM Digital"]
        P21["21. Kapabilitas Kepemimpinan Digital"]
        P22["22. Penerapan Budaya Digital"]
        P23["23. Penerapan Manajemen Data"]
        P24["24. Pengelolaan Data & Informasi"]
        P25["25. Teknologi Digital Pemanfaatan Data"]
        P26["26. Pelindungan Data Pribadi"]
        P27["27. Pemanfaatan Sistem Penghubung"]
        P28["28. Keterpaduan Layanan Adm. Pem."]
        P29["29. Pemanfaatan Portal Nasional Adm. Pem."]
        P30["30. Keterpaduan Pelayanan Publik"]
        P31["31. Pemanfaatan Portal Nasional Yanblik"]
        P32["32. Pemanfaatan Identitas Digital Nasional"]
        P33["33. Kepuasan Pengguna"]
        P34["34. Pemenuhan Kualitas Layanan"]
        P35["35. Pemanfaatan Layanan Digital"]
    end

    %% DEFINISI HUBUNGAN/KORELASI (VERSI PERBAIKAN)
    
    %% KORELASI BEREVOLUSI (1-ke-1)
    S2 --> P4
    S9 --> P12
    S11 --> P5
    S12 --> P2
    S13 --> P3
    S14 --> P8
    S18 --> P20
    S21 --> P9
    S26 --> P27
    S27 --> P34
    S28 --> P12
    S29 --> P11
    S31 --> P10

    %% KORELASI DIGABUNGKAN (Banyak-ke-1)
    S1 --> P6
    S7 --> P6
    S3 --> P23
    S16 --> P23
    S33 --> P23
    S4 --> P10
    S5 --> P17
    S15 --> P17
    S32 --> P17
    S6 --> P14; S6 --> P15; S6 --> P16
    S22 --> P14; S22 --> P15; S22 --> P16
    S8 --> P11
    S10 --> P1
    S17 --> P9
    S19 --> P9
    S20 --> P9
    S24 --> P28; S24 --> P29
    S25 --> P30; S25 --> P31
    S32 --> P18; S32 --> P19
    S34_47 --> P33
    S34_47 --> P35

    %% STYLING NODE
    classDef spbe_node fill:#cce5ff,stroke:#333,color:#000
    classDef pemdi_node fill:#d4edda,stroke:#333,color:#000
    classDef removed_node fill:#cccccc,stroke:#333,color:#000
    classDef new_node fill:#fff3cd,stroke:#333,color:#000
    
    class S1,S2,S3,S4,S5,S6,S7,S8,S9,S10,S11,S12,S13,S14,S15,S16,S17,S18,S19,S20,S21,S22,S24,S25,S26,S27,S28,S29,S31,S32,S33,S34_47 spbe_node;
    class S23,S30 removed_node;
    
    class P1,P2,P3,P4,P5,P6,P7,P8,P9,P10,P11,P12,P14,P15,P16,P17,P18,P19,P20,P23,P27,P28,P29,P30,P31,P33,P34,P35 pemdi_node;
    class P13,P21,P22,P24,P25,P26,P32 new_node;

    %% LEGENDA
    subgraph Legenda
        L1("Indikator SPBE"):::spbe_node
        L2("Indikator PEMDI Terpetakan"):::pemdi_node
        L3("Indikator SPBE Dihilangkan"):::removed_node
        L4("Indikator PEMDI Baru"):::new_node
    end
