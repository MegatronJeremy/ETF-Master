# ETF-Master

Materijali, domaći i projekti sa master studija na [Elektrotehničkom fakultetu
Univerziteta u Beogradu](https://www.etf.bg.ac.rs/). **Svaki predmet je samostalan,
nezavisno klonabilan git repo**, uključen u ovaj meta-repo kao submodule; pravi
projekti su dalje ugnježdeni submoduli unutar repoa svog predmeta.

## Predmeti

| Folder | Predmet | Projekat / sadržaj | Repo predmeta |
| --- | --- | --- | --- |
| [`GI/`](GI) | Genomska informatika | scRNA-seq analiza imunog odgovora na nanoplastiku (PBMC) | [GI](https://github.com/MegatronJeremy/GI) |
| [`PSZ/`](PSZ) | Pronalaženje skrivenog znanja | Scraping + analiza + ML nad podacima o beloj tehnici | [PSZ](https://github.com/MegatronJeremy/PSZ) |
| [`RG2/`](RG2) | Računarska grafika 2 | Render engine + materijali i domaći | [RG2](https://github.com/MegatronJeremy/RG2) |
| [`RIP/`](RIP) | Razvoj i primena računarskih akceleratora (prof. V. Milutinović; dataflow / Maxeler) | Domaći (Prefix Scan) + materijali | [RIP](https://github.com/MegatronJeremy/RIP) |

## Submoduli

Svaki predmet je submodule meta-repoa; projekti i tuđi materijali su ugnježdeni
submoduli unutar repoa predmeta.

| Putanja | Tip | Upstream |
| --- | --- | --- |
| `GI` | predmet | https://github.com/MegatronJeremy/GI |
| `GI/gi-2026-etf` | tuđi materijal (prof. V. Kovačević) | https://github.com/vladimirkovacevic/gi-2026-etf |
| `GI/ANOA-ANO` | projekat | https://github.com/MegatronJeremy/ANOA-ANO |
| `PSZ` | predmet | https://github.com/MegatronJeremy/PSZ |
| `PSZ/Sea-Of-Sorrow` | projekat | https://github.com/MegatronJeremy/Sea-Of-Sorrow |
| `RG2` | predmet | https://github.com/MegatronJeremy/RG2 |
| `RG2/Snowstorm-Engine` | projekat | https://github.com/MegatronJeremy/Snowstorm-Engine |
| `RIP` | predmet | https://github.com/MegatronJeremy/RIP |

## Kloniranje

Zbog ugnježdenih submodula (npr. `RG2/Snowstorm-Engine`) koristi `--recursive`:

```bash
git clone --recursive https://github.com/MegatronJeremy/ETF-Master
```

Ako si već klonirao bez toga:

```bash
git submodule update --init --recursive
```

Povlačenje najnovijih izmena (super-projekat + svi submoduli):

```bash
git pull
git submodule update --init --recursive
```

## Struktura

Jedinstven obrazac: **svaki predmet = jedan submodule** (`GI`, `PSZ`, `RG2`, `RIP`),
a unutar svakog su projekti i tuđi materijali ugnježdeni submoduli. Tako se svaki
predmet može klonirati i deliti nezavisno, dok meta-repo služi kao kišobran.
