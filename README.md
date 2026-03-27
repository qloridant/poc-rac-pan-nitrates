# catala-pan-nitrates-2023

Codification en langage [**Catala**](https://catala-lang.org) de l'arrêté du 30 janvier 2023 modifiant l'arrêté du 19 décembre 2011 relatif au **programme d'actions national (PAN) dans les zones vulnérables**, afin de réduire la pollution des eaux par les nitrates d'origine agricole.

> **Référence officielle :** NOR : TREL2237332A — JORF n°0034 du 9 février 2023, Texte n°28  
> **Légifrance :** https://www.legifrance.gouv.fr/jorf/id/JORFTEXT000047106562

---

## Qu'est-ce que Catala ?

[Catala](https://catala-lang.org) est un langage de programmation développé par l'[Inria](https://inria.fr) spécifiquement conçu pour la **traduction fidèle des textes législatifs en code exécutable**. Il repose sur deux principes :

- **Literate programming** : le texte légal et le code sont entrelacés dans le même fichier. Chaque bloc de code suit immédiatement le paragraphe qu'il formalise.
- **Default logic** : la logique cas général / exceptions est un concept de première classe du langage, ce qui reflète fidèlement la structure des arrêtés, lois et décrets.

---

## Structure du projet

```
catala-pan-nitrates-2023/
├── clerk.toml                                         ← Fichier de build (Clerk)
├── README.md
├── src/
│   └── arrete_pan_nitrates_2023.catala_en            ← Source principale
└── tests/
    └── arrete_pan_nitrates_2023_tests.catala_en      ← Tests unitaires
```

---

## Contenu de la codification

Le fichier source couvre les éléments computationnels de l'arrêté :

| Élément codifié | Scope Catala |
|---|---|
| Art. 1er — Délais de mise en conformité du stockage | `DelaisStockage` |
| Art. 1er — Dérogations transitoires pendant travaux | `ConformiteEpandage` |
| Art. 1er — Délai reporté au 31/03/2023 (zones 2021) | `DelaisStockage` (exception) |
| Art. 2 — Date d'application des annexes par région | `ApplicationAnnexes` |
| Art. 3 — Entrée en vigueur | `EntreeEnVigueur` |
| Annexe I — Périodes d'interdiction d'épandage (mesure I) | `ConformiteEpandage` |
| Annexe I — Interdiction sur sols non cultivés | `ConformiteEpandage` |
| Annexe I — Interdiction sur sols gelés | `ConformiteEpandage` |

### Types modélisés

- **`TypeFertilisant`** : Type0, TypeIa, TypeIb, TypeII, TypeIII (selon le tableau de l'Annexe I)
- **`OccupationSol`** : 11 cas couvrant l'ensemble du tableau des périodes d'interdiction
- **`StatutStockageElevage`** : capacité suffisante / en cours d'augmentation / nouvellement en zone
- **`Elevage`** : données relatives à un élevage (zone vulnérable, statut, travaux...)
- **`EpandageOperation`** : type de fertilisant, occupation du sol, date, état du sol

---

## Prérequis

- **Catala** ≥ 0.10 — [Instructions d'installation](https://catala-lang.org/en/install)
- **Clerk** (outil de build Catala, fourni avec le compilateur)

### Installation rapide de Catala

**Via opam (OCaml) :**
```bash
opam install catala
```

**Via Nix :**
```bash
nix-shell -p catala
```

**Via les binaires pré-compilés :**  
Télécharger depuis https://github.com/CatalaLang/catala/releases

---

## Utilisation

### Vérification syntaxique et typage

```bash
catala typecheck src/arrete_pan_nitrates_2023.catala_en
```

### Exécution d'un scope

Vérifier si un épandage est conforme (exemple) :

```bash
catala interpret src/arrete_pan_nitrates_2023.catala_en \
  --scope=EntreeEnVigueur
```

### Exécution des tests

```bash
# Avec Clerk (recommandé)
clerk test

# Ou directement
catala test tests/arrete_pan_nitrates_2023_tests.catala_en
```

### Régénérer les résultats attendus des tests

```bash
clerk runtest tests/arrete_pan_nitrates_2023_tests.catala_en
```

### Génération d'un PDF lisible par un juriste

```bash
catala latex src/arrete_pan_nitrates_2023.catala_en -o arrete_pan_nitrates_2023.tex
pdflatex arrete_pan_nitrates_2023.tex
```

### Compilation vers OCaml, Python, C...

```bash
# OCaml
catala ocaml src/arrete_pan_nitrates_2023.catala_en -o arrete_pan_nitrates_2023.ml

# Python
catala python src/arrete_pan_nitrates_2023.catala_en -o arrete_pan_nitrates_2023.py
```

---

## Tests inclus

| N° | Description | Scope testé | Résultat attendu |
|---|---|---|---|
| 1 | Épandage sur sol non cultivé | `ConformiteEpandage` | `interdit = true` |
| 2 | Type II sur blé d'hiver en octobre | `ConformiteEpandage` | `interdit = true` |
| 3 | Type II sur blé d'hiver en septembre | `ConformiteEpandage` | `interdit = false` |
| 4 | Dérogation transitoire (travaux) en octobre | `ConformiteEpandage` | `dérogation = true` |
| 5 | Délai zone désignée 2021 → 31/03/2023 | `DelaisStockage` | `date = 2023-03-31` |
| 6 | PAR régional publié après 01/01/2024 | `ApplicationAnnexes` | `date = 2024-01-01` |
| 7 | PAR régional publié avant 01/01/2024 | `ApplicationAnnexes` | `date = date PAR` |
| 8 | Épandage sur sol gelé (type II) | `ConformiteEpandage` | `interdit = true` |
| 9 | Entrée en vigueur de l'arrêté | `EntreeEnVigueur` | `date = 2023-02-10` |

---

## Périmètre et limites

Cette codification couvre les **dispositions computationnelles** de l'arrêté. Les éléments suivants **ne sont pas encore codifiés** et peuvent faire l'objet d'extensions :

- **Annexe I — Mesure III** : plan de fumure et équilibre de la fertilisation azotée (calcul du bilan azoté, référentiel régional)
- **Annexe I — Mesure IV** : cahier d'enregistrement des pratiques
- **Annexe I — Mesure V** : capacités minimales de stockage des effluents
- **Annexe I — Mesure VII** : couverture végétale pour limiter les fuites d'azote
- **Annexe I — Mesure VIII** : bandes enherbées et couverture permanente le long des cours d'eau
- **Annexes II et III** : zones A, B, C, D et petites régions agricoles
- Flexibilité agro-météorologique (renvoi à un arrêté PAR)
- Zones d'actions renforcées (ZAR)

---

## Contribuer

Les contributions sont bienvenues, notamment pour :
- Étendre la codification aux mesures III à VIII de l'Annexe I
- Ajouter des tests supplémentaires
- Relier ce module au programme d'actions régional (arrêté NOR : TREL2237333A)

Merci de respecter les conventions de nommage Catala (CamelCase pour les types et scopes, snake_case pour les variables).

---

## Références

- [Texte officiel sur Légifrance](https://www.legifrance.gouv.fr/jorf/id/JORFTEXT000047106562)
- [Texte sur AIDA (Ineris)](https://aida.ineris.fr/reglementation/arrete-300123-modifiant-larrete-19-decembre-2011-relatif-programme-dactions-national)
- [Documentation Catala](https://catala-lang.org)
- [Tutoriel Catala](https://book.catala-lang.org)
- [Merigoux, Chataing, Protzenko — *Catala: A Programming Language for the Law* (2021)](https://arxiv.org/abs/2103.03198)
- [Arrêté PAR régional du 30 janvier 2023 (NOR : TREL2237333A)](https://www.legifrance.gouv.fr/jorf/id/JORFTEXT000047106603)

---

## Licence

Les textes légaux reproduits sont des œuvres officielles de l'État français, librement réutilisables (Licence Ouverte / Open Licence v2.0 — Etalab).

Le code Catala de ce dépôt est distribué sous licence **Apache 2.0**.
# catala-pan-nitrates-2023
