# Changelog

## [1.0.0] — 2024

### Codification initiale

#### Scopes implémentés
- `EntreeEnVigueur` — dates de publication et d'entrée en vigueur (art. 3)
- `ApplicationAnnexes` — date d'application des annexes par région, plafond 01/01/2024 (art. 2)
- `DelaisStockage` — délai de 2 ans, date limite de signalement, exception zones 2021 (art. 1er)
- `ConformiteEpandage` — périodes d'interdiction par type de fertilisant et occupation du sol,
  dérogations transitoires pendant travaux (art. 1er + Annexe I, mesure I)

#### Types et structures déclarés
- `TypeFertilisant` (5 variantes : Type0, TypeIa, TypeIb, TypeII, TypeIII)
- `OccupationSol` (11 variantes)
- `StatutStockageElevage` (3 variantes)
- `Elevage` (structure d'entrée)
- `EpandageOperation` (structure d'entrée)

#### Tests
- 9 tests unitaires couvrant les cas principaux

#### Périmètre non encore codifié
- Mesures III à VIII de l'Annexe I (plan de fumure, cahier d'enregistrement,
  capacités de stockage, couverture végétale, bandes enherbées)
- Annexes II et III (zones A/B/C/D, petites régions agricoles)
- Flexibilité agro-météorologique
- Zones d'actions renforcées (ZAR)
