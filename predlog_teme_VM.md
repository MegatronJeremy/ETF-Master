# Predlog teme master rada

**Kandidat:** [ime, broj indeksa]
**Mentor:** prof. dr Veljko Milutinović
**Studijski program:** Master, Elektrotehnički fakultet, Univerzitet u Beogradu
**Datum:** jun 2026.

---

## Naslov

**Da li DataFlow prednost preživljava matrix-core GPU generaciju?
Revizija energetske efikasnosti aproksimativne pažnje na AMD GPU i Maxeler DFE**

*(eng. Does the DataFlow Advantage Survive the Matrix-Core GPU Generation?
Revisiting Energy Efficiency of Approximate Attention on AMD GPU vs. Maxeler DFE)*

---

## Istraživačko pitanje i hipoteza

Literatura je već utvrdila da FPGA/DataFlow akceleratori pažnje postižu višu
energetsku efikasnost od GPU-a (citirano 5–150× nad RTX 6000, V100, A10G).
**Ključno ograničenje tih radova:** svi mere protiv starih GPU-ova bez modernih
matrix engine-a. Generacija sa nativnim FP8/MXFP matrix core-ovima (AMD MI300/
MI350) je 1–2 reda veličine efikasnija od V100 baš na ovim operacijama — pa je
pitanje da li ta prednost uopšte opstaje **otvoreno i nemereno**.

**Pitanje:** Da li energetska prednost DataFlow paradigme nad GPU-om za
(aproksimativnu) pažnju **preživljava prelazak na modernu matrix-core GPU
generaciju**, i ako da — u kom režimu (veličina problema N, ciljana tačnost ε)?

**Hipoteza (falsifikabilna):** Velika prednost DataFlow-a iz literature se
**uglavnom gubi** protiv matrix-core GPU-a za gustu, egzaktnu pažnju, a opstaje
samo u uskim režimima — niska latencija pri batch=1, agresivna retkost (gde
matrix core-ovi „štektaju") i custom sub-8-bit preciznost koju GPU nema nativno.
Posledica: **tačka preseka postoji, ali je znatno uža** nego što tvrdi
postojeća literatura.

I potvrda i opovrgavanje su validan, objavljiv rezultat: oba revidiraju
brojku koja se danas citira.

---

## Zašto je ovo doprinos (a ne ponavljanje poznatog)

- Smer „DataFlow je efikasniji" je **već poznat** — zato rad *ne dokazuje to*,
  nego **revidira** poznatu brojku na hardveru koji je niko nije testirao.
- **AMD GPU (ROCm/HIP, MI-klasa) kao baseline je u akademskoj literaturi
  praktično nepostojeći** — sva poređenja koriste NVIDIA. Autorov redak pristup
  i AMD GPU i realnom Maxeler DFE čini ovo merenje izvodljivim.
- **Maxeler DFE kao high-level dataflow apstrakcija** naspram ručnog Verilog/HLS
  iz postojećih radova — drugačija, manje istražena tačka poređenja.
- Rezultat je **prenosiv model tačke preseka**, ne samo brojevi: predviđa koju
  paradigmu i koji nivo aproksimacije izabrati za dato (N, ε, budžet energije).

---

## Eksperimentalni dizajn

**Kernel:** mehanizam pažnje (attention) — jedan kernel, kroz spektar tačnosti:

| Nivo | Varijanta | Tačnost |
|---|---|---|
| Egzaktni | full softmax attention (FlashAttention stil) | referentna |
| Aproks. 1 | niža preciznost (FP16/FP8/MXFP) | visoka |
| Aproks. 2 | retka pažnja (block-sparse / top-k) | srednja |
| Aproks. 3 | linearna / Nyström pažnja | niža |

**Platforme:**
- **GPU:** AMD (ROCm/HIP) — referentna i aproksimativne varijante, optimizovano
  (tiling, deljena memorija, fuzija). Varijante preciznosti se generišu
  **template-based** (jedan kernel, parametarski po tipu podataka), uz oslonac
  na FP16/FP8/MXFP putanje koje cilja MI300/MI350 generacija.
- **DataFlow:** realan Maxeler DFE (MaxCompiler) — iste varijante, protočni graf.
- **CPU:** baseline radi konteksta.

**Industrijski baseline:** sve GPU varijante se porede sa produkcijskim ROCm
bibliotekama (**Composable Kernel / hipBLASLt**) — ne radi nadmašivanja, već
radi poštene kalibracije („gde stojimo i zašto") i validacije merne
infrastrukture.

**Merenja (po varijanti i N):**
propusnost (GFLOP/s), latencija, **energetska efikasnost (perf/W)**,
arithmetic intensity (roofline), greška naspram egzaktnog (relativna/zadatak),
i složenost implementacije. GPU profilisanje se radi standardnim ROCm alatima
(**rocprof, omniperf**); energija na DFE board-level radi fer poređenja.

---

## Plan rada (faze)

| Faza | Aktivnost | Procena |
|---|---|---|
| 1 | Egzaktni attention: GPU (HIP) + DFE, CK/hipBLASLt baseline, rocprof/omniperf infrastruktura | 3 ned. |
| 2 | Aproksimativne varijante na GPU (template-based, FP8/MXFP), Pareto tačnost×perf×W | 2 ned. |
| 3 | Aproksimativne varijante na DFE | 3 ned. |
| 4 | Model tačke preseka (cross-paradigm roofline) + empirijska validacija | 2 ned. |
| 5 | Analiza, grafici, pisanje, odbrana | 2 ned. |

---

## Očekivani doprinosi

1. **Revizija citirane DataFlow-vs-GPU brojke** na matrix-core generaciji:
   prvo pošteno energetsko poređenje pažnje na AMD MI-klasi GPU naspram realnog
   Maxeler DFE.
2. **Empirijski Pareto frontovi** (tačnost × performanse × energija) za attention
   na obe paradigme — reproduktivni, sa objavljenim kodom.
3. **Validiran model tačke preseka** koji predviđa optimalnu paradigmu i nivo
   aproksimacije za dato (N, ε).
4. Konkretan, kontraintuitivan nalaz o tome u kom (suženom) režimu DataFlow i
   dalje dobija, a gde matrix-core GPU briše prednost.

---

## Srodni radovi (pozicioniranje)

Postoji bogata literatura o FPGA/DataFlow akceleraciji pažnje i poređenju sa
GPU-om — što ovaj rad svesno koristi kao polaznu tačku, a ne kao prazan prostor:

- **FPGA attention akceleratori:** FAMOUS (FPT 2024), BETA (binarizovani),
  HARDSEA, A³, ELSA, length-adaptive sparse attention (2022) — pokazuju visoku
  energetsku efikasnost uz retkost / nisku preciznost.
- **Pregledi:** sveobuhvatni survey hardverske akceleracije LLM-ova (2024).

**Praznina koju rad popunjava:** sva ova poređenja koriste *stare NVIDIA GPU-ove*
(RTX 6000, V100, A10G) i *ručni Verilog/HLS*. Niko ne meri protiv (a) **AMD
matrix-core GPU generacije** niti (b) preko **Maxeler high-level dataflow
apstrakcije**. Rad upravo to radi i time *revidira* citiranu brojku.

---

## Rizici i kontrola

- **Negativan rezultat (nema preseka):** i dalje validan i objavljiv; teza se
  preformuliše u „granice DataFlow prednosti".
- **DFE kompleksnost/rok:** ako Faza 3 probije rok, egzaktni + jedan
  aproksimativni nivo na DFE su dovoljni za odbranu; ostatak ide u „budući rad".
- **Fer poređenje:** energija se meri dosledno (board-level), iste veličine
  problema, ista metrika greške — eksplicitno dokumentovano.

---

## Veza sa kursom i industrijom

Direktan nastavak domaćih sa kursa (multicore/manycore/DataFlow nad istim
kernelima) i Veljkove teze da jednostavniji/aproksimativni algoritam na bržoj
tehnologiji može da pobedi. Poklapa se sa autorovim radom u GPU akceleraciji,
što obezbeđuje pristup hardveru i ekspertizi. Ciljani ishod: pored master rada,
i materijal za radionički/konferencijski rad.

Inženjerski, rad demonstrira veštine koje traže AMD-ovi ROCm/AI GPU timovi:
moderni C++ i template-based generisanje kernela, HIP/ROCm razvoj, optimizacija
za memorijsku hijerarhiju i fuziju, niska preciznost (FP8/MXFP), profilisanje
(rocprof/omniperf) i poređenje sa produkcijskim bibliotekama (Composable
Kernel, hipBLASLt).

---

## Literatura (početna)

1. Materijali kursa — VLSI/DataFlow predavanja (V. Milutinović, O. Mencer).
2. Dao et al., *FlashAttention*, 2022.
3. Williams et al., *Roofline: An Insightful Visual Performance Model*, 2009.
4. Choromanski et al., *Performer (linear attention)*, 2021.
5. *Hardware Acceleration of LLMs: A Comprehensive Survey and Comparison*, 2024 — arXiv:2409.03384.
6. Kachris et al. / Hong et al., *FAMOUS: Flexible Accelerator for the Attention Mechanism*, FPT 2024 — arXiv:2409.14023.
7. *A Length Adaptive Algorithm-Hardware Co-design of Transformer on FPGA*, 2022 — arXiv:2208.03646.
8. Ji et al., *BETA: Binarized Energy-Efficient Transformer Accelerator*, 2024 — arXiv:2401.11851.
9. *Accelerating Sparse Transformer Inference on GPU*, 2025 — arXiv:2506.06095.
10. AMD ROCm / HIP i Maxeler MaxCompiler programski vodiči.
