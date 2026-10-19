# Aileron centripète

Étude CFD personnelle d’un **aileron arrière actif à inclinaison latérale
variable** : incliner l’aileron convertit une part de l’appui en effort
centripète directement utilisable en virage.

**Romain André** — Centrale Lille, promotion 2026
romain.andre@centrale.centralelille.fr

→ **[Lire l’étude](https://ROMAIN.github.io/aileron-centripete/)**

---

## Résultat

L’inclinaison fait passer le vecteur force de 0,63° à **21,27°** — une rotation
de 20,6° — en ne lui coûtant que **1,1 % de son module**. Le seuil de
rentabilité, posé avant la mesure, autorisait jusqu’à 14,8 %. À l’inclinaison
retenue de 14°, le module n’est pas perdu du tout : il augmente de 8,5 %, ce
qui est aussi la trace de l’excédent d’appui inexpliqué décrit plus bas.

L’optimum n’est pas à la butée mécanique de 23°, ni aux 20° choisis au départ,
mais **à 14°** — encadré par des points mesurés de part et d’autre, aux deux
angles de dérapage calculés. Un ajustement parabolique exact sur 13-14-15°
place le sommet à 14,16° (β = 0) et 14,21° (β = 15°) ; en ajoutant le point à
16°, qui n’existe qu’à β = 15°, ce second sommet remonte à 14,63°. L’accord
entre les deux séries vaut donc **un demi-degré**, et les deux séries partagent
la même géométrie et le même maillage — seule la direction de l’écoulement
incident change.

| inclinaison | gain à β = 0 | gain à β = 15° |
|---|---|---|
| 1,5° | — | +3,14 % |
| 3° | +8,44 % | +6,08 % |
| 13° | +14,22 % | +15,56 % |
| **14°** | **+17,80 %** | **+19,10 %** |
| 15° | +15,95 % | +17,62 % |
| 16° | — | +16,95 % |
| 20° | +11,53 % | +14,72 % |
| 23° | — | +13,61 % |

Gains d’accélération latérale maximale, modèle de cercle d’adhérence augmenté
par l’aérodynamique, à λ = qS/W = 1 et μ = 1,5. **Le gain dépend fortement de
λ**, donc du carré de la vitesse : il tombe à +5,8 % à λ = 0,20 (170 km/h pour
1 400 kg et 2,00 m²).

Appliqué à la trajectoire à courbure minimale de Barcelona-Catalunya pour une
hypercar de 1 400 kg : **1.17 s/tour** sur 13 virages,
2550 m d’arcs, soit 56 % du tour.

---

## Conditions de calcul

| | |
|---|---|
| solveur | RANS incompressible, schéma SIMPLE (SimScale) |
| turbulence | k-ω SST |
| vitesse | 30,000 m/s, module conservé à tous les dérapages |
| air | ρ = 1,196 kg/m³, ν = 1,529·10⁻⁵ m²/s |
| géométrie | corps d’Ahmed tronqué, 1,723 × 1,556 × 0,414 m |
| surface de référence | 0,644 m² — **qS = 346.6 N** |
| couche limite | 8 couches, croissance 1,2, épaisseur relative 0,4 |
| y⁺ à l’aileron | 85 à 107 (zone logarithmique) |
| maillage | 8 M cellules, réglages de raffinement tenus identiques |
| moyennage | 1000 dernières itérations sur 2000 à 2500 |

**Adimensionnement.** Les coefficients sont rapportés à la masse volumique du
solveur, 1,196 kg/m³, et non à la valeur ISA de 1,225 : c’est l’air de la fiche
matériau qui a produit les forces. L’écart est de 2.43 % sur
chaque coefficient et ne déplace aucune conclusion, les gains étant des
rapports entre deux configurations.

---

## Contenu du dépôt

```
index.html          la page de l’étude (GitHub Pages)
img/                figures : écoulement, maillage, sillage, pression, CAO
data/               exports SimScale, un fichier par calcul
data/brut/          exports non rattachés à un calcul (voir ci-dessous)
scripts/            post-traitement, en Python 3 + numpy/scipy
```

### Calculs de production

Chaque fichier contient l’historique itération par itération des forces de
pression, visqueuses et totales dans les trois axes. Convention : **x
longitudinal, y vertical, z latéral** ; C_L négatif = appui, C_z négatif =
centripète.

| inclinaison | dérapage | efforts | moments |
|---|---|---|---|
| 0° | 0° | `incl00_derap00_efforts.csv` | ✓ |
| 0° | 5° | `incl00_derap05_efforts.csv` | ✓ |
| 0° | 10° | `incl00_derap10_efforts.csv` | ✓ |
| 0° | 15° | `incl00_derap15_efforts.csv` | ✓ |
| 1,5° | 15° | `incl1p5_derap15_efforts.csv` | ✓ |
| 3° | 0° | `incl03_derap00_efforts.csv` | ✓ |
| 3° | 15° | `incl03_derap15_efforts.csv` | ✓ |
| 13° | 0° | `incl13_derap00_efforts.csv` | — |
| 13° | 15° | `incl13_derap15_efforts.csv` | — |
| 14° | 0° | `incl14_derap00_efforts.csv` | — |
| 14° | 15° | `incl14_derap15_efforts.csv` | — |
| 15° | 0° | `incl15_derap00_efforts.csv` | ✓ |
| 15° | 15° | `incl15_derap15_efforts.csv` | ✓ |
| 16° | 15° | `incl16_derap15_efforts.csv` | — |
| 20° | 0° | `incl20_derap00_efforts.csv` | — |
| 20° | 15° | `incl20_derap15_efforts.csv` | — |
| 23° | 15° | `incl23_derap15_efforts.csv` | ✓ |

Le rattachement de chaque fichier à son calcul n’a pas été fait d’après son
nom mais **vérifié numériquement** : `scripts/identify.py` recalcule les
coefficients de chaque export et les apparie au jeu de données publié, avec un
écart maximal de 0,004. Les exports de `data/brut/` sont ceux que cette
procédure ne rattache pas de façon univoque — balayages de dérapage
intermédiaires, calculs de convergence en maillage, et un calcul divergent.

### Scripts

| fichier | rôle |
|---|---|
| `build_data.py` | coefficients corrigés, courbe a_y, dépendance à λ, sensibilité au pneu |
| `build_data2.py` | sommets de parabole et exemple sur circuit |
| `yplus.py` | y⁺ à partir de la force visqueuse et des réglages de couche limite |
| `visc.py` | extraction de la force de frottement des exports |
| `identify.py` | appariement fichier ↔ calcul |
| `wake2.py` | mesure de l’antisymétrie des plans de sillage sur les images |
| `count_bl2.py` | comptage des couches de prismes et taux de croissance |
| `mkcharts.py` | graphiques SVG de la page |

Reproduction : `python3 scripts/build_data.py` depuis la racine du dépôt
(numpy et scipy requis). Les chemins vers les CSV sont à ajuster en tête de
script.

---

## Limites

- **Les inclinaisons entre 4° et 12° n’ont pas pu être maillées**, pour deux
  raisons distinctes (angle de coupe dégénéré avec la grille cartésienne à
  10°, tolérance CAO ailleurs). La branche montante n’est donc pas mesurée.
  Une interpolation construite sans le point à 13° surestime C_z de 7,6 à
  8,7 %.
- **L’optimum intérieur n’existe que parce que l’aileron perd de l’appui.** En
  gelant C_L, a_y redevient monotone croissante jusqu’à la butée.
- **Un excédent d’appui à faible inclinaison reste inexpliqué**, et sa
  correction déplacerait l’optimum vers 18–20°.
- Le modèle est une section arrière tronquée, pas un véhicule complet : le
  passage aux unités physiques est une transposition explicite, pas une mise à
  l’échelle validée.
- **La dispersion de remaillage n’est pas quantifiée.** La répétabilité annoncée
  (0,33 % sur C_L) est mesurée à maillage identique, alors que chaque
  inclinaison a son propre maillage. C’est la principale réserve sur la
  significativité du pic.
- **La transposition au véhicule vaut un facteur 3,1** sur les efforts absolus :
  les coefficients sont rapportés à 0,644 m² puis appliqués à un maître-couple
  de 2,00 m². C’est supposer que le véhicule complet produit le même
  coefficient rapporté à sa propre surface.
- Le modèle d’accélération latérale est un cercle d’adhérence à masse
  ponctuelle : aucune équation de moment de lacet, alors que l’aileron agit
  0,86 m derrière le centre de caisse.
- **Le jeu de données est clos** : les heures de calcul allouées sont épuisées.
- RANS stationnaire : ni lâcher tourbillonnaire, ni hystérésis, ni réponse
  transitoire pendant le débattement.

---

## Sources

1. Milliken & Milliken, *Race Car Vehicle Dynamics*, SAE International, 1995,
   ch. 2 — cercle d’adhérence. La forme augmentée par l’aérodynamique est
   dérivée ici.
2. SAE J670, *Vehicle Dynamics Terminology* — conventions d’axes et de signes.
3. Ahmed, Ramm & Faltin, « Salient Features of the Time-Averaged Ground
   Vehicle Wake », *SAE Technical Paper 840300*, 1984 — géométrie de référence.
4. Celik, Ghia, Roache & Freitas, « Procedure for Estimation and Reporting of
   Uncertainty Due to Discretization in CFD Applications », *ASME Journal of
   Fluids Engineering* — procédure de référence pour la convergence en
   maillage. L’incertitude retenue ici est l’écart 8 M → 11 M majoré, **pas un
   GCI formel**.
5. Heilmeier, Wischnewski, Hermansdorfer, Betz, Lienkamp & Lohmann, « Minimum
   curvature trajectory planning and control for an autonomous race car »,
   *Vehicle System Dynamics* 58(10), 1497–1527, 2020. Trajectoires issues de
   [TUMFTM/racetrack-database](https://github.com/TUMFTM/racetrack-database)
   (LGPL-3.0, état du 18 septembre 2021), non redistribuées ici.
