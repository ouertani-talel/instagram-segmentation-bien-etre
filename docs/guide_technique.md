# Guide technique — Rush 4 : segmentation des utilisateurs Instagram : usage et bien-être

**Epitech Paris · MBA · Bootcamp Data · 15-26 octobre 2026**
Équipe : **Mehdi · Bradley · Kiyani · Talel · Marco**

La Direction Innovation d'Instagram nous demande si l'usage de la plateforme est lié au stress, au sommeil et au bonheur, et quels types d'utilisateurs les équipes produit doivent distinguer. Nous livrons une segmentation fondée sur l'usage, ses critères d'évaluation, ses limites et son cadre RGPD.

## Résultats en bref

| Question | Réponse (réplique synthétique, panel volontaire) |
|---|---|
| L'usage est-il lié au stress ? | **Oui, fortement** : ρ = 0,84 avec le temps passé ; le temps seul explique 71 % de la variance du stress |
| … au bonheur ? | **Modérément**, négativement : ρ = −0,36 |
| … au sommeil ? | **Non** : aucune association, même avec 20 variables d'activité |
| Que dit la segmentation de plus que le temps passé ? | À temps égal, les « Créateurs-Reels » déclarent 3,2 points de stress en moins (sur 40), mais **0,5 point seulement à âge égal** : au-delà du temps, l'usage dit peu, et surtout via l'âge |
| Segmentation livrée | **3 paliers de temps (< 2 h, 2-4 h, > 4 h) × 2 styles (Spectateurs, Créateurs-Reels) = 6 segments**, de 7,7 % à 25 % des utilisateurs, assignables avec 3 compteurs d'activité, stables (ARI 0,995) |
| Critères | 7 sur 8 atteints, C5 (« au-delà du temps ») partiellement |
| Simulation économique | Un outil de pause n'est rentable que si 2 ciblés sur 3 sont à risque. Règle « plus de 4 h 36 par jour » : +62 k€ / an pour 1 M d'utilisateurs ; segment S5 : +38 k€ ; hasard : −108 k€. Protéger les mineurs par défaut coûte ≈ 120 k€ / an (§ 12) |

## Lancer le projet

```bash
pip install -r requirements.txt
# déposer les deux extraits (ressources étudiantes rush-4/) dans data/ :
#   data/instagram_users_lifestyleSafe.csv
#   data/instagram_usage_lifestyleSafe.csv
jupyter notebook notebooks/rush4_segmentation.ipynb   # Kernel → Restart & Run All (≈ 2 min)
```

Le notebook tourne de haut en bas et donne les mêmes résultats à chaque exécution (`SEED = 42`). La dernière cellule compare ses métriques à `outputs/metrics.json` et affiche `Identique à l'exécution précédente : True`.

## Structure du dépôt

```
notebooks/rush4_segmentation.ipynb   notebook final (livrable principal), exécuté
docs/note_rgpd.md                    note RGPD UE / Royaume-Uni, sourcée
figures/                             graphiques produits par le notebook (utilisés dans les slides)
outputs/                             exports agrégés : profils des segments, comparaison des routes,
                                     critères, paramètres d'affectation (JSON), métriques de contrôle
slides/                              support de la soutenance (.pptx)
data/                                les deux extraits (non versionnés, voir .gitignore)
```

## Choix de données (résumé)

Les deux extraits ont les mêmes 200 000 identifiants et **52 champs sur 58 strictement identiques**, dont toute l'activité et tout le bien-être. L'extrait `users` est la version nettoyée : le nettoyage non documenté n'a touché que 6 champs de profil, surtout chez les 13-17 ans.

| Champ | Version retenue | Raison |
|---|---|---|
| Emploi, heures travaillées, situation de couple, enfants | `users` | le brut attribue aux 13-17 ans 45 % de temps plein et 35 % de mariés |
| Niveau d'études | `users`, libellés harmonisés | les deux versions sont incohérentes : champ jugé peu fiable, utilisé nulle part |
| Année de création du compte | `usage` (brut) | la correction crée 3 724 comptes « créés » après leur dernière connexion |
| Activité, bien-être, santé | identiques | aucun arbitrage |

La segmentation n'utilise aucun champ arbitré : **ses résultats ne dépendent pas du choix**.

## Qui a fait quoi

Répartition issue du plan de travail de l'équipe ; les sections correspondantes du notebook sont indiquées.

| Membre | Responsable de | Relecture | Sections du notebook / livrables |
|---|---|---|---|
| **Marco** | Cadrage et critères de réussite (R4-01) ; risque d'inférence santé (R4-19) ; contrôle de confidentialité (R4-20) ; limites (R4-23) ; vérification finale (R4-29) | RGPD (R4-17, R4-18), simulation (R4-22), slides (R4-26) | § 0, § 9, § 10.1, § 10.5 ; slides « Limites » |
| **Mehdi** | Dépôt et README (R4-02, R4-27) ; inventaire, comparaison et choix des extraits (R4-03 à R4-06) ; affectation d'un nouvel utilisateur (R4-14) ; assemblage et reproductibilité du notebook (R4-24, R4-25) | Exploration, variables, segmentation | § 1, § 6, § 13 ; slide « Choix de données » |
| **Bradley** | Exploration (R4-07) ; bien-être (R4-08, R4-09) ; feature engineering (R4-10) ; critères et méthodologie (R4-11, R4-12) ; segmentation (R4-13) ; évaluation et comparaison au temps passé (R4-15, R4-16) | Qualité des données, affectation | § 2 à § 5, § 7, § 8 ; slides analyse |
| **Kiyani** | RGPD : finalité, base légale, transparence, minimisation (R4-17) ; mineurs et panel volontaire (R4-18) | Risque d'inférence (R4-19), limites (R4-23) | § 10.2 à § 10.4, § 10.6, § 10.7 ; `docs/note_rgpd.md` ; slides « RGPD » et « Mineurs » |
| **Talel** | Recommandations produit (R4-21) ; simulation économique (R4-22, complément) ; slides de soutenance (R4-26) | — | § 11, § 12 ; slides « Recommandations », « Conclusion », annexe |
| **Ensemble** | Répétition de la soutenance (R4-28) | — | chaque membre doit pouvoir répondre sur toutes les parties |


## Protection des données

- Les répliques sont traitées comme des données personnelles (le générateur a pu reproduire de vrais utilisateurs) : **aucun fichier de données n'est versionné**.
- Le notebook n'affiche **aucune ligne individuelle** ; chaque agrégat porte sur au moins 100 personnes (fonction `check_k`), les taux correspondant à moins de 100 personnes sont masqués.
- Les segments reposent uniquement sur 3 compteurs d'activité ; les données de bien-être et de santé ne servent qu'à les **évaluer**, de façon agrégée.
- Analyse détaillée (finalité, bases légales, mineurs, panel, risque d'inférence santé, sources) : `docs/note_rgpd.md`. Ce n'est pas un avis juridique.

## Limites principales

Associations et non causalité ; panel volontaire non représentatif ; réplique synthétique dont la structure n'est cohérente que deux à deux (ex. : l'âge n'est pas lié au stress, sauf à temps passé égal) ; mesures déclaratives. Détail : § 9 du notebook.
