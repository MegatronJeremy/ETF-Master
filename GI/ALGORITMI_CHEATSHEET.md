# Algoritmi GI (teme 7–9) — cheat-sheet sa rešenim primerima

Tri algoritamske teme koje najčešće zeznu na teorijskom ispitu, svaka sa **rukom rešenim
primerom** koji možeš da ponoviš na tabli. Match/mismatch/gap konvencije su naznačene jer
profesor može zadati druge brojeve — bitan je **postupak**, ne konkretne vrednosti.

## Sadržaj
1. [BWT i FM-index (tema 7)](#1-bwt-i-fm-index)
2. [Edit distance / Needleman-Wunsch (tema 8)](#2-edit-distance--needleman-wunsch)
3. [De Bruijn graf (tema 9)](#3-de-bruijn-graf)

---

## 1. BWT i FM-index

**Čemu služi:** BWT je reverzibilna permutacija stringa koja (a) grupiše slične karaktere →
bolja kompresija, i (b) omogućava **FM-index** — strukturu za munjevito traženje podstringa u
ogromnom genomu (srce alata BWA, Bowtie). `$` je sentinel — leksikografski najmanji, ide na kraj.

### Konstrukcija — primer `banana$`

Napiši svih 7 rotacija, pa ih **sortiraj** leksikografski (`$ < a < b < n`):

```
sve rotacije        sortirane (matrica M)     F = prvo slovo   L = poslednje (=BWT)
banana$             $banana                    $                a
anana$b             a$banan                    a                n
nana$ba             ana$ban                    a                n
ana$ban     --->    anana$b                    a                b
na$bana             banana$                    b                $
a$banan             na$bana                    n                a
$banana             nana$ba                    n                a
```

- **BWT** = poslednja kolona (L) = **`annb$aa`**
- **F** (prva kolona) = `$aaabnn` (uvek sortirano)

### Inverzija — LF-mapping
Ključna osobina: **i-to pojavljivanje slova c u L i u F je isti karakter originala.** Pratећi
`L → F` (LF-mapping) i krećući od `$`, rekonstruišeš string unazad. (Zato je BWT reverzibilna.)

### Pretraga (FM-index, „backward search")
Tražimo podstring obrađujući ga **zdesna nalevo**, sužavajući opseg redova `[sp, ep)`.

Potrebne dve stvari:
- `C[c]` = koliko karaktera u stringu je strogo manje od `c`.
  Za `banana$`: `C[$]=0, C[a]=1, C[b]=4, C[n]=5`.
- `Occ(c, i)` = koliko puta se `c` javlja u `L[0..i-1]` (prefiks BWT-a). `L = a n n b $ a a`.

Formula koraka: `sp = C[c] + Occ(c, sp)`, `ep = C[c] + Occ(c, ep)`.

**Primer: traži `ana`** (obrađuj `a`, pa `n`, pa `a`). Start: `[sp,ep) = [0,7)`.

| korak | c | sp = C[c]+Occ(c,sp) | ep = C[c]+Occ(c,ep) | opseg | tumačenje |
|---|---|---|---|---|---|
| 1 | a | 1+0 = 1 | 1+3 = 4 | [1,4) | suffiksi koji počinju `a` |
| 2 | n | 5+0 = 5 | 5+2 = 7 | [5,7) | `na` |
| 3 | a | 1+1 = 2 | 1+3 = 4 | [2,4) | `ana` |

Veličina finalnog opsega = `4−2 = 2` → **`ana` se javlja 2 puta** (b**ana**n**a**: pozicije 1 i 3).
Ako opseg postane prazan (`sp ≥ ep`) → podstring ne postoji.

---

## 2. Edit distance / Needleman-Wunsch

Obe koriste **dinamičko programiranje** sa istom 2D matricom; razlika je samo u bodovanju:
- **Edit (Levenshtein) distance:** *minimizuješ* broj izmena (match 0, mismatch 1, indel 1).
- **Needleman-Wunsch (global alignment):** *maksimizuješ* skor (npr. match +1, mismatch −1, gap −1).
- **Smith-Waterman (local):** kao NW, ali negativne ćelije sečeš na 0 i traceback krećeš od
  najveće ćelije (za lokalna poklapanja, ne celom dužinom).

**Rekurzija (NW):**
```
M[i][j] = max( M[i-1][j-1] + s(xi, yj)   # dijagonala: poravnaj xi sa yj
               M[i-1][j]   + gap          # gore: gap u Y (xi naspram '-')
               M[i][j-1]   + gap )         # levo: gap u X ('-' naspram yj)
```
Prvi red/kolona = kumulativni gap (0, gap, 2·gap, ...).

### Rešen primer — `X = AGT`, `Y = AGCT` (match +1, mismatch −1, gap −1)

```
         ""    A     G     C     T
   ""     0   -1    -2    -3    -4
   A     -1   +1     0    -1    -2
   G     -2    0    +2    +1     0
   T     -3   -1    +1    +1    +2
```
Primer ćelije `(G,G)=2`: dijagonala `M[A,A]+1 = 1+1 = 2` (pobeđuje). 

**Skor = donja-desna ćelija = +2.**

**Traceback** (od dna-desno ka gore-levo, prati odakle je vrednost došla):
- `(T,T)=2` ← dijagonala (match) → `T–T`
- `(G,C)=1` ← levo → gap u X → `-–C`
- `(G,G)=2` ← dijagonala (match) → `G–G`
- `(A,A)=1` ← dijagonala (match) → `A–A`

Optimalno globalno poravnanje:
```
X:  A G - T
Y:  A G C T
```
(`+1 +1 −1 +1 = +2`, slaže se sa skorom.) Edit distance varijanta bi dala 1 (jedan insert `C`).

> Na ispitu: nacrtaj matricu, popuni je, i **obavezno** uradi traceback + ispiši poravnanje.
> Strelice (odakle je max došao) su pola poena.

---

## 3. De Bruijn graf

**Čemu služi:** sklapanje genoma (*assembly*) iz kratkih reads-a **bez reference**. Trik: ne
tražimo Hamiltonov put (NP-težak), nego **Ojlerov put** (svaku granu tačno jednom — rešivo brzo).

**Konstrukcija (k-meri):**
- Iseci reads na sve **k-mere** (podstringovi dužine k).
- **Čvorovi** = svi `(k−1)`-meri.
- **Grana** za svaki k-mer: od njegovog **prefiksa** `(k−1)` ka njegovom **sufiksu** `(k−1)`.
- Genom = **Ojlerov put** (koristi svaku granu jednom).

### Rešen primer — string `ATGCATG`, k=3

3-meri: `ATG, TGC, GCA, CAT, ATG`  (primeti: `ATG` se ponavlja — to je „repeat").

Čvorovi (2-meri): `AT, TG, GC, CA`. Grane (prefiks → sufiks):
```
ATG: AT → TG      (x2, jer se ATG javlja dvaput)
TGC: TG → GC
GCA: GC → CA
CAT: CA → AT
```

**Nalaženje starta/kraja preko balansa stepena** (in/out):
| čvor | out | in | out−in |
|---|---|---|---|
| AT | 2 | 1 | **+1 → START** |
| TG | 1 | 2 | **−1 → KRAJ** |
| GC | 1 | 1 | 0 |
| CA | 1 | 1 | 0 |

Ojlerov put od `AT` do `TG`:  `AT → TG → GC → CA → AT → TG`

Rekonstrukcija (start = `AT`, svaki sledeći čvor dodaje svoje poslednje slovo):
`AT` +G +C +A +T +G → **`ATGCATG`** ✓ (vraćen original, repeat ispravno obiđen dvaput).

> Pravilo: Ojlerov put postoji ako su svi čvorovi balansirani (in=out), uz tačno jedan čvor
> `out−in=+1` (start) i jedan `−1` (kraj). Manji k → više preklapanja ali više dvosmislenosti
> (repeats); veći k → jednoznačnije ali traži duže/dublje reads-e.

---

### Brzi podsetnik šta gde
| Tema | Struktura | Tipično pitanje |
|---|---|---|
| 7 BWT/FM-index | sortirane rotacije, LF-mapping | konstruiši BWT, pretraži podstring backward search |
| 8 NW/edit distance | DP matrica + traceback | popuni matricu, ispiši poravnanje |
| 9 De Bruijn | (k−1)-meri + Ojlerov put | nacrtaj graf, rekonstruiši string |

Za dublje vežbanje: Ben Langmead „Algorithms for DNA Sequencing" (YouTube) i [Rosalind](https://rosalind.info).
