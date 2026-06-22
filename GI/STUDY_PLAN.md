# Genomska informatika 2026 — indeks materijala + plan učenja

Lični vodič kroz kurs: šta postoji, šta pokriva, i **kako da naučiš** za ispit
(40% ispit = teorija + praktično) i odbranu projekta (60%). Pisano za nekoga ko nije pratio
predavanja. Linkovi su relativni i otvaraju se iz `GI/` direktorijuma (materijal je u `gi-2026-etf/`).

---

## Sadržaj

1. [Indeks materijala (sa linkovima)](#1-indeks-materijala)
2. [Pun sylabus (9 tema)](#2-pun-sylabus)
3. [Šta tačno treba da znaš po temi](#3-šta-tačno-treba-da-znaš-po-temi)
4. [Plan učenja (4 faze)](#4-plan-učenja)
5. [Praktične vežbe (hands-on)](#5-praktične-vežbe)
6. [Eksterni resursi](#6-eksterni-resursi)

---

## 1. Indeks materijala

### Predavanja (PDF) — postoje za lekcije 1–5
| Lekcija | Tema | Fajl |
|---|---|---|
| 1 | Uvod: bioinformatika/genomika, osnove molekularne biologije, tehnologije sekvenciranja, „omics" | [Lesson_01](gi-2026-etf/lessons/Lesson_01_Genome_Informatics_2026.pdf) |
| 2 | Secondary DNA analysis: alignment, variant calling, assembly, graph genome (FASTQ→SAM/BAM→VCF) | [Lesson_02](gi-2026-etf/lessons/Lesson_02_Genome_Informatics_2026.pdf) |
| 3 | AI-Assisted bioinformatika, analiza raka (cancer), Git | [Lesson_03](gi-2026-etf/lessons/Lesson_03_Genome_Informatics_2026.pdf) |
| 4 | **Single-cell RNA sekvenciranje** (osnova tvog projekta) | [Lesson_04](gi-2026-etf/lessons/Lesson_04_Genome_Informatics_2026.pdf) |
| 5 | Spatial transcriptomics; AnnData; Scanpy basic processing | [Lesson_05](gi-2026-etf/lessons/Lesson_05_Genome_Informatics_2026.pdf) |

> Lekcije 6–9 iz sylabusa (BWT/FM-index, edit distance/DP, De Bruijn graf) **nemaju PDF u
> repozitorijumu** — to su algoritamske teme; uči ih iz eksternih resursa (sekcija 6) i iz
> repo-a prethodnih godina koji često imaju te slajdove.

### Kod i podaci (hands-on)
| Fajl | Šta je | Veže se za |
|---|---|---|
| [source/aln.py](gi-2026-etf/source/aln.py) | praktičan **alignment** pipeline (indeksiranje, mapiranje reads-a, flagstat, depth, pileup, vizuelizacija) | Lekcija 2 |
| [source/dna.py](gi-2026-etf/source/dna.py) | praktičan **variant calling** pipeline (align → BAM → VCF, parsiranje varijanti) | Lekcija 2 |
| [data/dna/](gi-2026-etf/data/dna/) | ulazni podaci: `*.fastq` (paired-end reads) + `*reference.fasta` | za aln.py/dna.py |
| [notebooks/Clustering_Scanpy.ipynb](gi-2026-etf/notebooks/Clustering_Scanpy.ipynb) | scanpy tutorijal: QC → normalizacija → PCA/UMAP → clustering | Lekcije 4–5 (= tvoj projekat) |

---

## 2. Pun sylabus

Iz Lekcije 1 (9 tema + projekti). Boldovane su one sa najviše „ispitnog mesa":

1. Uvod, definicije, molekularna biologija, tehnologije sekvenciranja
2. **Secondary DNA analysis — alignment & variant calling, graph genome**
3. AI-assisted bioinformatika, cancer analiza, Git
4. **Uvod u single-cell RNA analizu**
5. Uvod u spatial transcriptomics
6. Linux komande u bioinformatici
7. **Burrows-Wheeler Transform (BWT) i FM-index**
8. **Approximate string matching, Edit distance, Dynamic programming, Global alignment**
9. **De-Bruijn graf, scaffolding, error correction**
10–14. Studentski projekti

Ispit ima teorijski + praktični deo. Teme **2, 4, 7, 8, 9** su klasično ispitno gradivo
(algoritmi + workflow). Tvoj projekat pokriva temu 4 (i deo 5) u praksi.

---

## 3. Šta tačno treba da znaš po temi

**Tema 1 — Osnove.** Šta je bioinformatika (presek stats + CS + biologija); centralna dogma
(DNK→RNK→protein); šta su gen, genom, hromozom; „omics" (genomika/transkriptomika/...);
tehnologije sekvenciranja (NGS, short reads); *zlatno pravilo: „Never trust your tools/data"*.

**Tema 2 — Secondary DNA analysis (VAŽNO).** Lanac fajlova: **FASTQ** (sirovi reads) → poravnanje
na referencu → **SAM/BAM** (poravnati reads) → variant calling → **VCF** (varijante/mutacije).
Razlika **alignment vs assembly** (mapiranje na referencu vs sklapanje bez nje). Pojmovi: read,
coverage/depth, paired-end, reference genome. Praktično: `aln.py`/`dna.py`.

**Tema 4 — Single-cell RNA (TVOJ PROJEKAT).** Bulk vs single-cell (prosek vs po ćeliji); zašto
single-cell (skrivena varijabilnost, odgovor na stimulus). Workflow (zapamti redosled!):
QC → normalizacija → HVG (feature selection) → batch correction → PCA → UMAP → clustering →
annotation → composition / differential expression. Ćelija×gen matrica; AnnData (`.X/.obs/.var`).
PCA (linearna redukcija) vs UMAP (nelinearna, za vizuelizaciju).

**Tema 5 — Spatial transcriptomics.** Kao single-cell ali sa **prostornim koordinatama** svake
ćelije (gde u tkivu je gen izražen, ne samo koliko). Tehnologije (npr. Stereo-Seq). AnnData
isto.

**Tema 7 — BWT i FM-index (ALGORITAM).** Burrows-Wheeler transformacija (reverzibilna permutacija
stringa preko sortiranih rotacija); FM-index (LF-mapping, „backward search") — kako se brzo traži
podstring u ogromnom genomu. Ovo je srce alata kao BWA/Bowtie.

**Tema 8 — Edit distance / DP / global alignment (ALGORITAM).** Edit distance (broj izmena da se
jedan string pretvori u drugi); dinamičko programiranje; **Needleman-Wunsch** (globalno) i
**Smith-Waterman** (lokalno) poravnanje; matrica bodovanja, gap penalty, traceback. Klasično
ispitno pitanje + zadatak na tabli.

**Tema 9 — De-Bruijn graf (ALGORITAM).** Sklapanje genoma bez reference: k-meri kao čvorovi,
preklapanja kao grane, Euler-ov put; scaffolding i error correction.

---

## 4. Plan učenja

Realan redosled za nekoga ko kreće od nule. Procene su za fokusiran rad.

### Faza 0 — Postavi temelje (pola dana)
- Pročitaj **Lekciju 1** celu. Cilj: razumeš centralnu dogmu i pojmove genom/gen/RNK/„omics".
- Nauči zlatno pravilo i bioinformatičke principe (dokumentuj, automatizuj, git, ne veruj alatu).

### Faza 1 — Tvoj projekat = tema 4 (1 dan) ← počni odavde, najveća vrednost
- Pročitaj **Lekciju 4** + prođi `VODIC.md` iz projekta (`../ANOA-ANO/VODIC.md`).
- Otvori `notebooks/Clustering_Scanpy.ipynb` i prati QC→UMAP→clustering korake.
- Cilj: možeš da ispričaš ceo single-cell workflow napamet i da odbraniš projekat.
- Ovo pokriva 60% ocene (projekat) + temu 4 na ispitu odjednom.

### Faza 2 — Secondary DNA analysis = tema 2 (1 dan)
- Pročitaj **Lekciju 2**. Nauči FASTQ→BAM→VCF lanac i alignment vs assembly.
- Praktično pokreni `source/dna.py` na `data/dna/` (vidi sekciju 5) — vidiš ceo lanac uživo.
- Cilj: razumeš šta svaki format nosi i kako se varijanta „zove".

### Faza 3 — Algoritmi = teme 7, 8, 9 (2–3 dana, najteže za ispit)
- **Edit distance / Needleman-Wunsch** prvo (tema 8) — najčešći zadatak. Uradi rukom 1–2 male
  matrice (poravnaj npr. "GATTACA" i "GCATGCU"): popuni DP matricu, traceback.
- **BWT/FM-index** (tema 7) — uradi rukom BWT malog stringa (npr. "banana$") i backward search.
- **De-Bruijn** (tema 9) — nacrtaj graf k-mera za kratak string i nađi Euler-ov put.
- Resursi: sekcija 6 (Rosalind zadaci su idealni za vežbu na tabli).

### Faza 4 — Dopuna (pola dana)
- **Lekcija 3** (AI-assisted, cancer, Git) i **Lekcija 5** (spatial) — pročitaj za širinu.
- Ponovi ceo sylabus iz sekcije 3 kao „cheat-sheet" pred ispit.

**Prioritet ako imaš malo vremena:** Faza 1 (projekat/tema 4) → Faza 3 (algoritmi, najviše nose
na teorijskom ispitu) → Faza 2 → Faza 4.

---

## 5. Praktične vežbe

Pokreni hands-on kod da teorija „sedne". Treba ti Python + bioinformatički alati (`bwa`/`minimap2`,
`samtools`, `bcftools`) — na Windows-u najlakše preko WSL/Linux ili conda okruženja.

```bash
# Alignment pipeline (tema 2): poravna reads na referencu, izvuce metrике
python source/aln.py

# Variant calling pipeline (tema 2): align -> BAM -> VCF (varijante)
python source/dna.py
```

Ulaz su fajlovi u `data/dna/` (paired-end FASTQ + reference FASTA). Pogledaj izlaz: koliko se
reads-a poravnalo (flagstat), depth/coverage po poziciji, i koje su varijante pozvane (VCF).

Za single-cell (teme 4–5): `notebooks/Clustering_Scanpy.ipynb` — isti scanpy koraci koje tvoj
projekat radi automatizovano.

---

## 6. Eksterni resursi

- **Repo kursa (sva zvanična info, otvori issue za pitanja):** https://github.com/vladimirkovacevic/gi-2026-etf
- **Prethodne godine** (često imaju slajdove za teme 6–9 i više zadataka): [2025](https://github.com/vladimirkovacevic/gi-2025-etf) · [2024](https://github.com/vladimirkovacevic/gi-2024-etf) · [2023](https://github.com/vladimirkovacevic/gi-2023-etf)
- **Algoritmi (teme 7–9):** Ben Langmead „Algorithms for DNA Sequencing" (YouTube + slajdovi) — najbolji izvor za BWT/FM-index, edit distance, De Bruijn.
- **Zadaci za vežbu:** [Rosalind](https://rosalind.info) — bioinformatički problemi (alignment, assembly) sa automatskom proverom.
- **Single-cell (teme 4–5):** [Scanpy tutorials](https://scanpy.readthedocs.io) · „Single-cell best practices" (sc-best-practices.org).
- **Kontakt:** teorija/lekcije 1–6 → vladimir.kovacevic@etf.rs · profesor → marko.misic@etf.bg.ac.rs · vežbe/tehnička pitanja → pedjao@etf.bg.ac.rs
