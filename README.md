Projet 8 — Analysez des indicateurs de l’égalité femmes/hommes (RGPD) avec KNIME (+ export PowerBI)




Contexte — Cabinet de conseil (150+ salariés). À la demande de la DRH (Laura) et du Contrôleur de gestion sociale (Vincent), vous mettez en place un workflow KNIME qui :

pseudonymise les données SIRH (RGPD), 2) calcule au moins 5 indicateurs de l’égalité F/H, 3) génère des graphiques et un export CSV prêt pour Tableau. Un support de 15 slides présente résultats, méthodologie et recommandations.

🧭 Résumé exécutif

Collecte & préparation : ingestion SIRH, dictionnaire, contrôles qualité, pseudonymisation.

Indicateurs : calcul de ≥5 KPIs F/H (ex. rémunération, promotions, embauches, départs, formation, top salaires).

Dataviz : graphiques exportés (PNG/SVG) + CSV agrégé pour Tableau.

Conformité : minimisation, traçabilité, seuils d’agrégation (k‑anonymat), registre de traitement.

📦 Livrables

knime/workflow.knwf — workflow KNIME reproductible (du SIRH à l’export CSV + images).

data/ :

raw/ — extractions SIRH non modifiées (non versionnées / ou chiffrées / LFS)

processed/diagnostic_egalite.csv — export agrégé (Tableau‑ready)

figures/ : graphiques (PNG/SVG) générés par KNIME (ou via Python nodes).

slides/egalite_fh_presentation.pptx — ≤ 15 slides (storyline + score + recommandations).

docs/ : lexique.xlsx, rgpd_mesures.md, data_quality_report.md, indicateurs_definitions.md.

🔍 Données & RGPD

Sources : SIRH (salaires, primes, date entrée/sortie, poste, niveau, service, temps de travail, sexe, âge, formation, etc.).

Base légale : obligation de calculer et publier l’index égalité F/H (< 1er mars), intérêt légitime de pilotage social.

Mesures :

Minimisation : seules variables utiles aux indicateurs sont conservées.

Pseudonymisation : employee_id_hash = SHA256(employee_id + salt) ; suppression noms/prénoms/email/adresse.

K‑anonymat : seuil d’affichage k ≥ 5 par croisement sensible (ex. service×niveau×sexe).

Agrégation : exports Tableau agrégés par mois/année, service, niveau, sexe.

Traçabilité : journal de traitement (dates d’import, version des flux, sources).

📊 Indicateurs (≥ 5 requis)

Adapter aux colonnes disponibles et au document du Ministère du Travail (Outil Diagnostic Égalité).

Écart de rémunération F/H (fixe + variable) — médiane & moyenne, par niveau/emploi.

Taux d’augmentation/promotion F vs H — par ancienneté/niveau.

Écart d’augmentation au retour de congé maternité (si éligible).

Répartition femmes dans les plus hautes rémunérations (Top 10%).

Taux de formation F vs H — jours/h pro rata.

(optionnel) Écart de mobilité (changement de poste/niveau) ; taux de départs ; taux d’embauche.

Score Index F/H (si applicable) : calculer selon la méthodologie officielle et publier le score.

🔧 Workflow KNIME — grandes étapes

Zones du flux (recommandation) : 00_input → 10_clean → 20_rgpd → 30_kpi → 40_viz → 50_export.

00_input : File Reader / Excel Reader ; concaténation multi‑fichiers ; normalisation d’encodage.

10_clean : Column Filter, String Manipulation, Rule Engine, Date&Time, Duplicate Row Filter.

20_rgpd : Hash (SHA‑256) sur identifiants ; Column Resorter pour drop PII ; Column Aggregator (k‑anonymat).

30_kpi : GroupBy (médianes/moyennes), Math Formula (Multi Column) ; calcul Index et sous‑scores.

40_viz : Python Script (Labs) ou Image Output pour boxplots/barres ; titres conclusifs.

50_export : CSV Writer (séparateur ,, UTF‑8) → data/processed/diagnostic_egalite.csv ; export PNG/SVG → figures/.

Un metanode 99_qc peut rassembler les contrôles qualité et produire docs/data_quality_report.md.

🧪 Contrôles qualité (automatisés)

Complétude : taux de NA par variable ; règles de rejet/neutralisation.

Cohérence : bornes (salaires > 0, temps partiel ≤ 100%, âges plausibles), hiérarchie niveau/poste.

Uniformité : libellés (sexe, service) normalisés ; formats de date.

Traçabilité : nb de lignes in/out par étape ; hash des fichiers sources.

▶️ Export pour Tableau

Fichier : data/processed/diagnostic_egalite.csv.

Granularité : année/mois × service × niveau × sexe ; valeurs : rémunération moyenne/médiane, effectifs, promotions, formations, départs.

Bonnes pratiques : champs Date, Hiérarchies (année→mois), Mesures prêtes, libellés FR.

🎙️ Présentation (≤ 15 slides) — Storyline

Contexte & cadre légal (obligation annuelle, publication index).

Données & RGPD (pseudonymisation, minimisation, k‑anonymat, registre).

Méthodologie KNIME (schéma du flux + règles de qualité).

Résultats clés (5+ KPIs) — tendances, écarts notables.

Index F/H (score global) + bench secteur (si disponible).

Recommandations (actions RH mesurables et datées).

👤 Auteur & licence

Auteur : David Fernandes — David.Fernandes.data@gmail.com

Licence : CC BY‑NC‑SA 4.0
