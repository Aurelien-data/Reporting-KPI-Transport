# 📊 Reporting KPI Transport — Logistique Premium

Prototype de tableau de bord Power BI pour le pilotage de la qualité de service
dans un réseau de transport premium multi-agences.

---

## Contexte

Ce projet simule le reporting opérationnel d'un réseau logistique de type
transport express premium — 8 agences, 32 000 expéditions, 15 mois de données
(janvier 2025 – mars 2026).

L'objectif : donner aux équipes service client, qualité et direction
une vision claire et immédiate de la performance, sans passer par des
fichiers Excel dispersés.

---

## Structure du rapport

### Page 1 — Vue d'ensemble · Qualité de service
Vue macro destinée à la direction et aux COPIL clients.

- Taux OTD (On Time Delivery) + variation vs mois précédent
- Taux de scan réseau
- Nombre de réclamations
- Score qualité moyen
- Évolution mensuelle OTD & Scan sur la période
- Répartition des incidents par nature

### Page 2 — Colis en Erreur · Multi-agences
Rapport opérationnel quotidien — cœur du dispositif de pilotage.

- Volume de colis en erreur par agence
- Classement automatique avec code couleur 🔴🟠🟢
- Répartition par type d'anomalie (scan manquant, non-conformité, retard hub...)
- Évolution hebdomadaire du volume d'erreurs

### Page 3 — Taux de Scan · Traçabilité réseau
Suivi de la fiabilité de la traçabilité à chaque étape de la chaîne.

- Taux de scan par agence (axe Y calibré pour faire ressortir les écarts)
- Évolution mensuelle sur l'ensemble du réseau
- Répartition des scans manquants par mode de transport
- Répartition des scans manquants par segment client (Grand Compte, ETI, PME)

---

## Modèle de données

Schéma en étoile — 1 table de faits, 10 dimensions.
```
FACT_expeditions
    ├── dimDate
    ├── dimClient
    ├── dimSegmentClient
    ├── dimSecteurClient
    ├── dimTypeService
    ├── dimModeTransport
    ├── dimAgenceOrigine
    ├── dimAgenceDestination
    ├── dimConducteurType
    ├── dimCanalSaisie
    └── dimStatutExpedition
```

Les 10 tables dim sont générées automatiquement depuis la source CSV
via Power Query — aucune saisie manuelle.

---

## Stack technique

| Brique | Détail |
|---|---|
| Visualisation | Power BI Desktop |
| Transformation | Power Query (M) |
| Calculs | DAX — 25+ mesures |
| Source | CSV simulé (Python / ChatGPT) |
| Modèle | Schéma en étoile |

---

## Mesures DAX principales

- `Taux OTD` — part des livraisons dans les délais SLA
- `Taux Scan Calculé` — nb scans réalisés / nb scans attendus
- `Flag Agence Erreur` — classification automatique 🔴🟠🟢 par taux de colis en erreur
- `Nb Agences Sous Seuil Scan` — agences dont le taux agrégé est sous 95%
- `Variation OTD vs Mois Précédent` — delta qualité mensuel
- `Rang Agence par Taux OTD` — classement dynamique inter-agences
- `Période Sélectionnée` / `Agence Sélectionnée` — titres dynamiques contextuels

---

## Logique métier — Boucle qualité

Ce rapport s'inscrit dans une logique d'amélioration continue :
```
1. Analyser     → identifier les agences et anomalies les plus impactantes
2. Cibler       → concentrer les actions là où les écarts sont significatifs
3. Former       → construire des formations calées sur les lacunes identifiées
4. Mesurer      → ré-analyser les mêmes indicateurs à J+30
```

---

## Contenu du repo
```
├── README.md
├── MAQUETTE_REPORTING_KPI_TRANSPORT.pbix   # Fichier Power BI
├── logistique_bi_dataset.csv               # Dataset simulé
└── documentation/
    └── documentation_technique.md          # Architecture, M, DAX, palette
```

---

## Auteur

**Aurélien · APBI**
Consultant BI & Data indépendant — Power BI · DAX · Power Query · SQL · Microsoft Fabric

*Mars 2026*
