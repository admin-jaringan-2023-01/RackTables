```mermaid
flowchart LR
  %% ======== SUPPLY: 3 PILAR ========
  subgraph SUPPLY[SUPPLY • 3 Pilar Rekognisi Dosen]
    P1[Pilar 1 • Pendidikan & Pembelajaran]
    P2[Pilar 2 • Penelitian & Inovasi Terapan]
    P3[Pilar 3 • Pengabdian & Hilirisasi]
  end

  %% ======== OUTPUT / LUARAN ========
  CGrad[Kompetensi Lulusan]
  Proto[Prototipe/Tech Siap Uji • TKT]
  Policy[Kebijakan/Standar & Dampak Sosial]

  %% ======== DEMAND SIDE ========
  DUDI[DUDI/Industri]
  PUB[Pemerintah & Publik]
  ACAD[Komunitas Akademik]

  %% ======== MEKANISME REKOGNISI & DAYA DUKUNG ========
  REC[Rekognisi Portofolio 3 Pilar]
  CAREER[Karier/Jafung & Pendanaan]
  CAP[Capacity: Waktu & Sumber Daya]
  ADMIN[Beban Administratif]
  COL[Kinerja Kolaborasi PT–Industri–Komunitas]
  ALIGN[Keselarasan Insentif]

  %% ======== RELASI SUPPLY → OUTPUT ========
  P1 -- "+" --> CGrad
  P2 -- "+" --> Proto
  P3 -- "+" --> Policy

  %% ======== OUTPUT → DEMAND ========
  CGrad -- "+" --> DUDI
  Proto -- "+" --> DUDI
  Proto -- "+" --> ACAD
  Policy -- "+" --> PUB

  %% ======== DEMAND → KOLABORASI ========
  DUDI -- "+" --> COL
  PUB  -- "+" --> COL
  ACAD -- "+" --> COL

  %% ======== KOLABORASI → SUPPLY (loop penguatan R1/R2/R3) ========
  COL -- "+" --> P1
  COL -- "+" --> P2
  COL -- "+" --> P3

  %% ======== REKOGNISI → DAYA DUKUNG ========
  COL -- "+" --> REC
  CGrad -- "+" --> REC
  Proto -- "+" --> REC
  Policy -- "+" --> REC

  REC -- "+" --> CAREER
  CAREER -- "+" --> CAP

  %% Capacity memperkuat semua pilar (reinforcing)
  CAP -- "+" --> P1
  CAP -- "+" --> P2
  CAP -- "+" --> P3

  %% ======== BALANCING: ADMIN BURDEN & ALIGNMENT ========
  ADMIN -- "Decreases" --> CAP
  %% single window / standardisasi bukti mengurangi beban
  REC -- "Reduces" --> ADMIN 
  %% aturan menyelaraskan insentif lintas skema
  REC -- "+" --> ALIGN 
  %% insentif selaras mendorong kolaborasi
  ALIGN -- "+" --> COL 

  %% ======== LABEL LOOP (keterangan) ========
  %% R1: P1 → CGrad → DUDI → COL → P1 (reinforcing)
  %% R2: P2 → Proto → DUDI/ACAD → COL → P2 (reinforcing)
  %% R3: P3 → Policy → PUB → COL → P3 (reinforcing)
  %% R4: REC → CAREER → CAP → (P1,P2,P3) → REC (reinforcing)
  %% B1: REC → (–) ADMIN → ( + ) CAP → ( + ) P1/P2/P3 → ... (balancing hambatan)
