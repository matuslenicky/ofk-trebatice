# OFK Trebatice – návrh webu

Návrh webu futbalového klubu OFK Trebatice (Obecný futbalový klub STAVOPRAC Trebatice). **Nie je to oficiálna stránka klubu.** Ukážka má v hlavičke pás s týmto upozornením a vyhľadávače ju neindexujú (`noindex`).

Ukážka: https://matuslenicky.github.io/ofk-trebatice/ · administrácia (ukážka bez ukladania): https://matuslenicky.github.io/ofk-trebatice/admin/

## Vzhľad

Farby a tvary z erbu: zeleno-biele zvislé pruhy ako na drese, zlatá koruna z erbu obce aj klubu. Zápas je vstupenka s odtrhávacím ústrižkom (výkop alebo výsledok s pečiatkou). Serifové nadpisy ako v tlačenom zápasovom programe, svetlý papierový podklad. Na telefóne je menu posuvný riadok odkazov pod erbom.

## Obsah a zdroje

- Zápasy, výsledky, strelci, karty, počty divákov, zostavy dospelých, tabuľky a súpisky zo Sportnetu (Futbalnet), verejné API súťaží (`scripts/sportnet.mjs` → `data/sportnet.json`). Vek hráčov sa neukladá, zostavy len pri dospelých. Tabuľky prípraviek bez zapísaných výsledkov sa neukazujú.
- Údaje o klube v `data/klub.json`, každá časť s uvedeným zdrojom: kontakt a fakturačné údaje zo Sportnetu, štadión (kapacita, rozmery, poloha) zo zápisu o stretnutí, postup do IV. ligy 2018 a pohárová výhra nad Petržalkou 2021 z Piešťanského denníka (23. 5. 2018 a 26. 8. 2021), zápas so Zlatými Moravcami zo zápisu na futbalsfz.sk, obec z Wikipédie a webu obce. Rok založenia klubu sa v zdrojoch nenašiel.
- Erb OFK Trebatice a erby súperov zo Sportnetu (`scripts/logos.py`, náš erb s priehľadným pozadím). Patria príslušným klubom.
- Písmo Fraunces a Instrument Sans, licencia SIL Open Font License 1.1 (`scripts/fonts.py`).

## Zostavenie

Statický generátor bez závislostí (Node 20+, Python 3 s Pillow na erby):

```
node scripts/sportnet.mjs        # zápasy, tabuľky a súpisky (FULL=1 stiahne znova aj detaily zápasov)
python -I scripts/logos.py       # erby súperov a náš erb do src/img (FORCE=1 stiahne znova)
node build.js                    # data/ + src/ -> dist/ (TODAY=2026-11-16 prepíše dnešný dátum)
node scripts/serve.mjs dist 5290 # náhľad na http://127.0.0.1:5290/
node scripts/deploy.mjs          # nasadenie dist/ na vetvu gh-pages (GitHub Pages)
```

`config.json`: `sportnet.appSpace` určuje klub, `ukazka` zapína pás „Návrh webu“, `noindex` zakazuje indexovanie, `adminDemo` pridá ukážku administrácie.

Výsledky sa po zápasoch aktualizujú samé cez GitHub Actions (`.github/workflows/sportnet.yml`).
