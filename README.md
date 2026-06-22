# ETF-Master

Materijali, domaći i projekti sa master studija na [Elektrotehničkom fakultetu
Univerziteta u Beogradu](https://www.etf.bg.ac.rs/). Svaki predmet je u svom
folderu; pravi projekti (timski / za predaju) i tuđi materijali su uključeni kao
**git submoduli**.

## Predmeti

| Folder | Predmet | Projekat / sadržaj | Repo |
| --- | --- | --- | --- |
| [`GI/`](GI) | Genomska informatika | scRNA-seq analiza imunog odgovora na nanoplastiku (PBMC) | [ANOA-ANO](https://github.com/MegatronJeremy/ANOA-ANO) ⋅ materijal: [gi-2026-etf](https://github.com/vladimirkovacevic/gi-2026-etf) |
| [`PSZ/`](PSZ) | Pronalaženje skrivenog znanja | Scraping + analiza + ML nad podacima o beloj tehnici | [Sea-Of-Sorrow](https://github.com/MegatronJeremy/Sea-Of-Sorrow) |
| [`RG2/`](RG2) | Računarska grafika 2 | Render engine + materijali i domaći | [RG2](https://github.com/MegatronJeremy/RG2) ⋅ projekat: [Snowstorm-Engine](https://github.com/MegatronJeremy/Snowstorm-Engine) |
| [`RIP/`](RIP) | Kurs prof. Veljka Milutinovića (dataflow / Maxeler) | Domaći (Prefix Scan) + materijali | [RIP](https://github.com/MegatronJeremy/RIP) |

## Submoduli

Repo trenutno koristi dva obrasca (videti napomenu o strukturi ispod):

| Putanja | Tip | Upstream |
| --- | --- | --- |
| `GI/gi-2026-etf` | tuđi materijal (prof. V. Kovačević) | https://github.com/vladimirkovacevic/gi-2026-etf |
| `GI/ANOA-ANO` | projekat | https://github.com/MegatronJeremy/ANOA-ANO |
| `PSZ/Sea-Of-Sorrow` | projekat | https://github.com/MegatronJeremy/Sea-Of-Sorrow |
| `RG2` | ceo folder predmeta | https://github.com/MegatronJeremy/RG2 |
| `RG2/Snowstorm-Engine` | projekat (ugnježden u RG2) | https://github.com/MegatronJeremy/Snowstorm-Engine |
| `RIP` | ceo folder predmeta | https://github.com/MegatronJeremy/RIP |

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

## Napomena o strukturi

Organizacija submodula je trenutno nekonzistentna: `GI` i `PSZ` su obične foldere
sa projektom kao submodulom, dok su `RG2` i `RIP` *ceo folder kao submodul*.
Planirano je ujednačavanje na jedan obrazac.
