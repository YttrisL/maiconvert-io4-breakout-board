# BOM DigiKey – maiConvert-IO4-hat (soudure manuelle)

PCB fabriquée nue chez JLCPCB, **composants soudés à la main**, achats **DigiKey**.

- `digikey_upload.csv` .................... à téléverser sur DigiKey (carte complète, 1 ex.)
  Lignes en **référence fabricant** (MPN) — plus stable que le n° DigiKey.
- `maiConvert-IO4-hat_BOM_DigiKey.csv` ..... BOM détaillée (empreintes, alternatives, notes)

Source : `maiConvert-IO4-hat.kicad_sch` (kicad-cli 9.0), 2026-09-08.

> **Schéma à jour.** Tous les composants portent `Manufacturer` / `Mfr. No.` /
> `Digi-Key` (connecteurs, résistances, boutons, LED). Bibliothèques projet dans
> `libraries/` : `LED_RGB_Wurth` (symbole + empreinte + STEP Würth). ERC : 0 erreur
> (3 avertissements *endpoint_off_grid* sur SW7 — cosmétique, à recaler sur la grille).
> **PCB** : refaire *Update PCB from Schematic* (elle contient encore l'ancienne
> empreinte TS10 sur SW6–SW21 et pas R1–R16).

## Commander sur DigiKey

**Mes listes** → **Créer une liste** → **Importer**, charger `digikey_upload.csv`
(colonnes `Quantity, Part Number, Customer Reference`). Ou coller dans **Ajout rapide
au panier**. Vérifier les lignes ⚠️.

## Contenu de `digikey_upload.csv` (86 pièces montées + 16 shunts en stock, 12 lignes)

| Qté | Réf. carte | Réf. (MPN) | DigiKey P/N | Description | ~PU |
|----:|-----------|------------|-------------|-------------|-----|
| 12 | R22–R33 | `RC1206FR-071KL` | `311-1.00KFRCT-ND` | R 1 kΩ 1 % **1/4 W 1206 CMS** (Yageo) — ballast R/G/B des LED de test | 0,03 $ |
| 16 | R1–R16 | `CR2512-JW-510ELF` | `118-CR2512-JW-510ELFCT-ND` | R **51 Ω** (≈ 50 Ω) 5 % **1 W** film épais **2512 CMS** (Bourns) — pull-down lockout | 0,09 $ |
| 4 | D1, D2, D21, D22 | `150352M173300` | `732-11999-1-ND` | LED RGB 6-PLCC SMD, canaux indép. (Würth) — anode commune ⚠️ | 0,52 $ |
| 21 | SW1–SW21 | `TS02-66-60-BK-100-LCR-D` | `2223-TS02-66-60-BK-100-LCR-D-ND` | Poussoir tactile 6 mm THT **SPST-NO**, 100 gf, plunger 6 mm (Same Sky) — empreinte compatible B3F-1000 | 0,12 $ |
| 16 | J4–J19 | `B3B-XH-A` | `455-2248-ND` | Embase JST XH 1×03 verticale THT | 0,13 $ |
| 9 | J20, J21, J27–J33 | `B2B-XH-A` | `455-2247-ND` | Embase JST XH 1×02 verticale THT | 0,11 $ |
| 2 | J25, J26 | `B4B-XH-A` | `455-2249-ND` | Embase JST XH 1×04 verticale THT | 0,15 $ |
| 2 | J23, J24 | `B8B-XH-A` | `455-B8B-XH-A-ND` | Embase JST XH 1×08 verticale THT | 0,28 $ |
| 2 | J1, J3 | `BHR-60-VUA` | `2057-BHR-60-VUA-ND` | Box header IDC 2×30 2,54 mm vert. THT (Adam Tech) | ~1,80 $ |
| 1 | J2 | `BHR-20-VUA` | `2057-BHR-20-VUA-ND` | Box header IDC 2×10 2,54 mm vert. THT (Adam Tech) | ~0,70 $ |
| 1 | J22 | `1935161` | `277-1667-ND` | Bornier à vis 2P pas 5,08 mm THT (Phoenix MKDS) | ~1,40 $ |
| 16 | — (stock) | `QPC02SXGN-RC` | `S9337-ND` | Cavalier (shunt) 2 pos 2,54 mm or — **pas monté sur cette PCB**, stock pour autres usages | 0,10 $ |

**Total pièces ≈ 16–19 $** à l'unité (dominé par les 2 box headers 2×30 ≈ 3,6 $,
le bornier Phoenix ≈ 1,4 $ et les 21 poussoirs ≈ 2,5 $) + ~1,6 $ de shunts en
stock. Bien moins en quantité (~6–9 $). *(Poussoirs passés de l'Omron B3F-1000 à
0,40 $ au Same Sky TS02 à 0,12 $ : ~6 $ d'économie.)*

## ⚠️ Points à vérifier

### R1–R16 — 51 Ω 1 W, boîtier 2512 CMS (Bourns `CR2512-JW-510ELF`)
Appuyer sur SWx bascule `J*.3` vers **+5 V** → tant que SWx est maintenu la
résistance dissipe `5 V² / 51 Ω ≈ 0,49 W`. Une **1 W** = **50 % de charge** :
point chaud **~60–80 °C** avec du cuivre sur les pastilles → **OK en continu /
bouton bloqué**.
- Empreinte `R_2512_6332Metric…HandSolder` ≈ **8 × 4 mm, hauteur nulle** — ~moitié
  de la surface du THT vertical, et le CMS 2512 se soude bien au fer. Stock ~23 k.
- ⚠️ **Prévoir du cuivre sur les pastilles** (plan de masse dessous / dessus,
  pistes larges vers GND et vers SWx) — le 2512 évacue sa chaleur par la PCB.
- Valeur schéma « 50 » conservée ; réalisation = **51 Ω ±5 %**, électriquement
  identique ici (signal ≈ 0,63 V à l'appui d'un bouton joueur). 50,0 Ω exact =
  bobinée. **Vrai 2 W en 2512** si tu veux plus de marge = Bourns `CRM2512` (~0,30 $).
- Rail +5 V : ~100 mA par bouton maintenu (16 en même temps → 1,6 A — peu probable).

### LED `150352M173300`
RGB à **canaux indépendants** (6 broches R/G/B séparées). La carte veut une
**anode commune** → `B+`/`R+`/`G+` (broches 2/4/6) sont reliées sur la PCB via
l'empreinte `LED_RGB_Wurth` (fournie). CMS : LED + R1–R16 (2512) + R22–R33 (1206) ;
tout le reste THT.

### R22–R33 (1 kΩ 1206) — ballast des LED de test
Anode LED → +12 V ; cathode R/G/B → R → signal IO4. LED allumée en permanence :
dissipation ≈ **0,07 W** (vert/bleu) à **0,10 W** (rouge, Vf plus bas) sous 12 V.
Une **1/4 W** = **~40 % de charge** → tourne froid, aucun souci en continu.
Boîtier 1206 (facile au fer). 5 % `RC1206JR-071KL` équivalent.

### Fonction lockout (SW6–SW21 + R1–R16) — comment ça marche
```
   J*.3 ──┬── R1..R16 (51 Ω / 1 W, 2512) ── GND
          └── [ SWx, NO ] ───────────────── +5V
```
Repos : `J*.3` tiré à GND par R → le bouton joueur fonctionne.
Appui SWx : `J*.3` forcé à +5 V → la résistance interne du module tire le signal
« … BUTTON n » vers +5 V → l'entrée revient à **HIGH**, même si le bouton externe
est bloqué (le commun est à +5 V). C'est l'option **B** (résistance + poussoir NO),
2 composants/voie. Le 1 W (2512) à 0,5 W = 50 % de charge → **maintien de SWx OK
en continu** avec du cuivre sur les pastilles.

## Notes conception / achat

- **21 poussoirs Same Sky `TS02-66-60-BK-100-LCR-D`** : SW1–SW5 (service) et
  SW6–SW21 (lockout par bouton) sont le même modèle. Empreinte 6 mm 4 broches
  identique à l'Omron B3F-1000 (pas 4,5 × 6,5 mm) → aucune modif PCB.
  Différences vs B3F-1000 : 100 000 cycles (vs 1 M), plunger 6,0 mm (vs 4,3 mm,
  dépasse ~1,7 mm de plus — carte fille, cosmétique) ; force d'appui 100 gf
  identique. Contact 50 mA / 12 V → OK pour le signal +5 V du lockout.
  Alternative premium (1 M cycles, plunger 4,3 mm) : Omron `B3F-1000` /
  `SW400-ND` (~0,40 $).
- **JST XH** : embases carte seulement. Faisceaux = boîtiers `XHP-2/3/4/8` +
  contacts `SXH-001T-P0.6(N)` (hors périmètre).
- **Box headers 2×30** : si les nappes IO4/harness ne sont pas à détrompeur, un
  header nu 2×30 (~0,40 $) suffit.
- **Bornier J22** : Phoenix `1935161` cher ; tout bornier à vis 5,08 mm 2P THT
  convient (Same Sky `TB007-508-02BE`, ~0,40 $).
- Revérifier prix / stock / réf. sur digikey.com avant commande, puis régénérer
  BOM + fichier de position depuis KiCad après *Update PCB*.

## Sources
- JST XH : [`455-2247-ND`](https://www.digikey.com/en/products/detail/B2B-XH-A(LF)(SN)/455-2247-ND/1651045),
  [`455-2248-ND`](https://www.digikey.com/en/products/detail/jst-sales-america-inc/B3B-XH-A-LF-SN/455-2248-ND/1651046),
  [`455-2249-ND`](https://www.digikey.com/product-detail/en/jst-sales-america-inc/B4B-XH-A-LF-SN/455-2249-ND/1651047),
  [`455-B8B-XH-A-ND`](https://www.digikey.com/en/products/detail/jst-sales-america-inc/B8B-XH-A/1651049)
- Résistances : [`311-1.00KFRCT-ND`](https://www.digikey.com/en/products/detail/yageo/RC1206FR-071KL/728131) (1 kΩ, 1206 CMS, R22–R33),
  [`118-CR2512-JW-510ELFCT-ND`](https://www.digikey.com/en/products/detail/bourns-inc/CR2512-JW-510ELF/3786005) (51 Ω, 1 W, 2512 CMS, R1–R16 — stock ~23 k)
- LED RGB 6-PLCC : [Würth 150352M173300 / 732-11999-1-ND](https://www.digikey.com/en/products/detail/w%C3%BCrth-elektronik/150352M173300/8557164)
- Poussoir : [Same Sky TS02-66-60-BK-100-LCR-D / 2223-TS02-66-60-BK-100-LCR-D-ND](https://www.digikey.com/en/products/detail/same-sky-formerly-cui-devices/TS02-66-60-BK-100-LCR-D/15634327)
  (alt. premium : [Omron B3F-1000 / SW400-ND](https://www.digikey.com/en/products/detail/omron-electronics-inc-emc-div/B3F-1000/33150))
- Box headers : [Adam Tech BHR-60-VUA](https://www.digikey.com/en/products/detail/adam-tech/BHR-60-VUA/10414828),
  [BHR-20-VUA](https://www.digikey.com/en/products/detail/adam-tech/BHR-20-VUA/2057-BHR-20-VUA-ND/9829308)
- Bornier : [Phoenix 1935161 / 277-1667-ND](https://www.digikey.com/product-detail/en/phoenix-contact/1935161/277-1667-ND/568614)
