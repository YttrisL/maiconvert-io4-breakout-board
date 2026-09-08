# BOM DigiKey – maiConvert-IO4-hat (soudure manuelle)

PCB fabriquée nue chez JLCPCB, **composants soudés à la main**, achats **DigiKey**.
Objectif : liste d'achat la moins chère possible ; les empreintes de la PCB seront
ensuite adaptées aux composants choisis ici (on part du besoin fonctionnel, pas de
ce qui est dessiné aujourd'hui).

- `digikey_upload.csv` .................... à téléverser sur DigiKey (carte complète, 1 ex.)
  Les lignes utilisent la **référence fabricant** (MPN), plus stable que le n° DigiKey
  qui change parfois (ex. `455-2251-ND` retiré → `455-B8B-XH-A-ND`). L'import DigiKey
  résout le MPN sans souci.
- `maiConvert-IO4-hat_BOM_DigiKey.csv` ..... BOM détaillée (réf., alternatives, notes)

Source : netlist générée depuis `maiConvert-IO4-hat.kicad_sch` (kicad-cli 9.0), 2026-09-08.

> **Schéma déjà mis à jour** avec ces pièces (réf. fabricant + n° DigiKey dans les
> champs des symboles). Nouvelles bibliothèques projet dans `libraries/` :
> `LED_RGB_Wurth` (symbole + empreinte + STEP Würth) et `SameSky_Switch`
> (empreinte TS10 + modèle 3D). ERC : 0 erreur, connexions inchangées.
> **PCB pas encore touchée** — faire *Update PCB from Schematic* pour appliquer
> les nouvelles empreintes.

## Comment lancer la commande sur DigiKey

1. Se connecter → **Mes listes** (*My Lists*) → **Créer une liste** → **Importer / Upload**.
   Charger `digikey_upload.csv` (colonnes `Quantity, Part Number, Customer Reference`,
   déjà au bon format).
   *Alternative rapide :* page **Ajout rapide au panier** (*Quick Add to Cart*),
   coller les colonnes `Part Number` + `Quantity`.
2. DigiKey résout chaque ligne. Vérifier les lignes marquées ⚠️ ci-dessous
   (LED = SMD à câbler en anode commune ; SW6–SW21 = choix du modèle NC).
3. Ajuster les quantités si tu veux des rechanges (passifs, JST, poussoirs vendus
   en *cut tape* → en prendre un peu plus).
4. Ajouter au panier.

## Contenu de `digikey_upload.csv` (70 pièces, 11 lignes)

| Qté | Réf. carte | Réf. (MPN) | DigiKey P/N | Description | ~PU |
|----:|-----------|------------|-------------|-------------|-----|
| 12 | R22–R33 | `MFR-25FBF52-1K` | `1.00KXBK-ND` | R 1 kΩ **axiale** 1 % 1/4 W THT métal (Yageo) | 0,10 $ |
| 4 | D1, D2, D21, D22 | `150352M173300` | `732-11999-1-ND` | LED **RGB 6-PLCC / 5050 SMD**, canaux indépendants (Würth) — ⚠️ | 0,52 $ |
| 5 | SW1–SW5 | `B3F-1000` | `SW400-ND` | Poussoir tactile 6 mm THT **SPST-NO** (Omron) | 0,40 $ |
| 16 | SW6–SW21 | `TS10-63-26-BE-250-SMT-TR` | `2223-…-SMT-TRCT-ND` | **Bouton tactile SPST-NC** (normalement fermé, appui = ouverture), CMS (Same Sky TS10) | 0,65 $ |
| 16 | J4–J19 | `B3B-XH-A` | `455-2248-ND` | Embase JST XH 1×03 verticale THT | 0,13 $ |
| 9 | J20, J21, J27–J33 | `B2B-XH-A` | `455-2247-ND` | Embase JST XH 1×02 verticale THT | 0,11 $ |
| 2 | J25, J26 | `B4B-XH-A` | `455-2249-ND` | Embase JST XH 1×04 verticale THT | 0,15 $ |
| 2 | J23, J24 | `B8B-XH-A` | `455-B8B-XH-A-ND` | Embase JST XH 1×08 verticale THT | 0,28 $ |
| 2 | J1, J3 | `BHR-60-VUA` | `2057-BHR-60-VUA-ND` | Box header IDC 2×30 (60 pts) 2,54 mm vert. THT (Adam Tech) | ~1,80 $ |
| 1 | J2 | `BHR-20-VUA` | `2057-BHR-20-VUA-ND` | Box header IDC 2×10 (20 pts) 2,54 mm vert. THT (Adam Tech) | ~0,70 $ |
| 1 | J22 | `1935161` | `277-1667-ND` | Bornier à vis 2P pas 5,08 mm THT (Phoenix MKDS 1,5/2-5,08) | ~1,40 $ |

**Total pièces ≈ 25–28 $** à l'unité, dont ~10 $ pour les 16 boutons NC.
Bien moins en quantité (~7 $ les 16 boutons NC à qty 100).

### Choix actés

- **Résistances → axiales traversantes.** 1 kΩ 1/4 W, ~0,02 $ en bande de 10+, aucune
  contrainte électrique. Empreinte : choisir l'entraxe (ex. 10,16 mm). Carbone 5 %
  `CFR-25JB-52-1K` (DK `1.0KQBK-ND`) encore un peu moins cher, indifférent ici.
- **LED → on garde la SMD** `150352M173300` (6-PLCC, 0,52 $). C'est le seul composant
  SMD restant (4 pièces, se soude très bien au fer). ⚠️ RGB à **canaux indépendants** :
  la carte veut une **anode commune** → relier les 3 anodes ensemble sur la nouvelle
  empreinte Würth `LED_RGB_Wurth` (déjà dans le projet ; anodes = broches 2/4/6).
- **SW6–SW21 → dans la commande** : bouton tactile **SPST-NC** Same Sky
  `TS10-63-26-BE-250-SMT-TR` (~0,65 $, ~0,46 $/100). Appui = ouverture du circuit,
  exactement la fonction voulue. Seul bémol : **CMS** (2 languettes, se soude très
  bien au fer). Empreinte `SameSky_Switch:SW_Tactile_SameSky_TS10` créée dans le projet.

> Symboles `#PWR*` (+12V, +5V, GND) et `#FLG*` (PWR_FLAG) : virtuels, aucun achat.

## ⚠️ Boutons : NO vs NC — vérifié sur la netlist, à ne pas mélanger

### SW1–SW5 = NORMALEMENT OUVERT (SPST-NO), momentané
Symbole KiCad `Switch:SW_Push`. En **parallèle** de leur connecteur externe
(SW1↔J31 TEST, SW2↔J32 COIN, SW3↔J33 SERVICE, SW4↔J20 1P SELECT, SW5↔J21 2P SELECT) :
appui = signal tiré à la masse. → poussoir tactile 6 mm ordinaire (`B3F-1000`).

### SW6–SW21 = NORMALEMENT FERMÉ (SPST-NC), momentané
Symbole KiCad `Switch:SW_Push_Open` (« push-to-open », mot-clé *normally-closed*).
Chaque SWx est **en série dans le retour de masse** d'un bouton joueur :
`connecteur JST broche 3 → SWx → GND`. Au repos : contact **fermé**, le bouton
joueur fonctionne. Appui sur SWx : **ouvre** le circuit → isole ce bouton (lockout / test).

**Un poussoir NO ici = les 16 boutons joueurs morts en permanence.** Il faut un
contact **NC momentané** (appui = ouverture du circuit).

| Solution | Modèle | Coût 16× | Action | Empreinte |
|----------|--------|---------:|--------|-----------|
| **Retenue** — bouton tactile **SPST-NC** CMS | Same Sky `TS10-63-26-BE-250-SMT-TR` | ~10 $ (0,65 $/pc, 0,46 $ à 100) | momentané, appui = ouverture | `SameSky_Switch` (fournie) |
| Micro-rupteur SPDT subminiature à plongeur, PC pin **(THT)** | CIT `SM3` (ex. `SM3CQF3501L00`), Omron `D2F` | ~20 $ | momentané | micro-rupteur ~9,5×5×6,5 mm, 3 broches PC |
| Mini interrupteur à **glissière** SPST | C&K `JS`, Same Sky | ~5–8 $ | **maintenu** (on/off, pas un appui) | slide ~3 broches |
| Embase 2 broches + **cavalier** (shunt) | — | ~1 $ | on **retire** le cavalier | header 1×2 2,54 mm |
| Rien : **pad à ponter / 0 Ω** | — | ~0 $ | aucune (lockout supprimé) | 0603 / 2 pads |

- `TS10-63-26-BE-250-SMT-TR` : vrai tact-switch NC, 0,05 A / 12 V (large pour du
  courant logique), en stock. **CMS** (le seul composant CMS de la carte avec la
  LED) mais 2 languettes accessibles → soudure au fer sans souci.
- Si tu veux rester 100 % traversant : micro-rupteur `SM3` (SPDT, câbler COM+NC),
  ~2× plus cher et empreinte plus grosse.
- Si tu ne te sers **jamais** du lockout par bouton : retire la ligne du CSV et
  prévois des pads à ponter — la carte reste 100 % fonctionnelle.

## Notes conception / achat

- **JST XH** : ce ne sont que les embases carte. Les faisceaux nécessitent en plus
  les boîtiers `XHP-2/3/4/8` + contacts à sertir `SXH-001T-P0.6(N)` (hors périmètre).
- **Box headers 2×30** : si les nappes côté IO4 / harness ne sont pas des
  connecteurs IDC à détrompeur, un simple **header nu 2×30** (~0,40 $) suffit et
  simplifie la PCB (`Amphenol`, `Sullins`, `Wurth`).
- **Bornier J22** : le Phoenix `1935161` est cher ; tout bornier à vis **5,08 mm
  2P THT** convient (Same Sky `TB007-508-02BE`, On Shore, ~0,40 $).
- Revérifier prix / stock / référence sur **digikey.com** avant commande, puis
  régénérer un BOM + fichier de position propres depuis KiCad une fois les
  empreintes mises à jour.

## Sources
- JST XH : [`455-2247-ND`](https://www.digikey.com/en/products/detail/B2B-XH-A(LF)(SN)/455-2247-ND/1651045),
  [`455-2248-ND`](https://www.digikey.com/en/products/detail/jst-sales-america-inc/B3B-XH-A-LF-SN/455-2248-ND/1651046),
  [`455-2249-ND`](https://www.digikey.com/product-detail/en/jst-sales-america-inc/B4B-XH-A-LF-SN/455-2249-ND/1651047),
  [`455-B8B-XH-A-ND`](https://www.digikey.com/en/products/detail/jst-sales-america-inc/B8B-XH-A/1651049) (ancien `455-2251-ND` retiré)
- Résistance axiale : [Yageo `MFR-25FBF52-1K` / `1.00KXBK-ND`](https://www.digikey.com/en/products/detail/yageo/MFR-25FBF52-1K/13011) (métal 1 % 1/4 W)
- LED RGB 6-PLCC SMD : [Würth 150352M173300](https://www.digikey.com/en/products/detail/w%C3%BCrth-elektronik/150352M173300/8557164)
  ($0,52). Option 100 % THT : [Kingbright WP154A4SEJ3VBDZGW/CA / 754-2029-ND](https://www.digikey.com/en/products/detail/kingbright/WP154A4SEJ3VBDZGW-CA/6569334) ($2,20).
- Poussoir NO : [Omron B3F-1000 / SW400-ND](https://www.digikey.com/en/products/detail/omron-electronics-inc-emc-div/B3F-1000/33150)
- Bouton NC (SW6–SW21) : [Same Sky TS10-63-26-BE-250-SMT-TR](https://www.digikey.com/en/products/detail/same-sky-formerly-cui-devices/TS10-63-26-BE-250-SMT-TR/15839065)
  (SPST-NC, CMS, ~0,65 $). Alt THT : [CIT SM3](https://www.digikey.com/en/product-highlight/c/cit/sm3-series-snap-action-switches) (SPDT plongeur, COM+NC, ~1,3 $).
- Box headers : [Adam Tech BHR-60-VUA](https://www.digikey.com/en/products/detail/adam-tech/BHR-60-VUA/10414828),
  [BHR-20-VUA / 2057-BHR-20-VUA-ND](https://www.digikey.com/en/products/detail/adam-tech/BHR-20-VUA/2057-BHR-20-VUA-ND/9829308)
- Bornier : [Phoenix 1935161 / 277-1667-ND](https://www.digikey.com/product-detail/en/phoenix-contact/1935161/277-1667-ND/568614)
