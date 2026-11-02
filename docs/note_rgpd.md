# Note RGPD — Segmentation des utilisateurs Instagram (UE et Royaume-Uni)

**Rush 4 · Bootcamp Data · Epitech Paris — octobre 2026**
Rédaction : **Kiyani** (R4-17 finalité, base légale, transparence, minimisation · R4-18 mineurs et panel volontaire)
Relecture : Marco · Analyse du risque d'inférence (R4-19) et contrôle de confidentialité (R4-20) : Marco, relus par Kiyani

> Cette note est un exercice pédagogique. Elle identifie les conditions et les limites juridiques d'un projet de segmentation ; elle ne constitue pas un avis juridique. Toute mise en œuvre réelle passe par le DPO d'Instagram et une analyse d'impact (AIPD).

---

## 1. Ce que nous traitons, et pourquoi le RGPD s'applique

| Élément | Constat |
|---|---|
| Données reçues | deux répliques **synthétiques** du panel (200 000 profils chacune) : activité in-app, compte, profil déclaratif, bien-être (stress, bonheur, sommeil), santé et mode de vie |
| Garantie d'anonymat | **aucune** : l'équipe privacy ne garantit pas que le générateur n'a jamais reproduit un vrai utilisateur |
| Destination des résultats | les segments et recommandations **s'appliqueront à de vrais utilisateurs**, mineurs compris, dans l'UE et au Royaume-Uni |

Une donnée n'échappe au RGPD que si la réidentification n'est pas « raisonnablement probable » (considérant 26). Un générateur qui a pu mémoriser des enregistrements réels ne permet pas de l'affirmer. Nous traitons donc la réplique **comme une donnée personnelle** : accès restreint, aucune ligne affichée, aucun fichier de données dans le dépôt Git, et des agrégats de 100 personnes au minimum. Surtout, le déploiement des segments sur les vrais comptes est, lui, un traitement de données personnelles sans ambiguïté, et c'est un **profilage** au sens de l'art. 4(4).

---

## 2. Finalité, base légale, transparence, minimisation (R4-17)

### 2.1 Trois traitements distincts, trois analyses distinctes

| # | Traitement | Données nécessaires | Base légale envisageable | Conditions |
|---|---|---|---|---|
| ① | **Étude bien-être** sur le panel (ce rush) | activité + stress/bonheur/sommeil + santé | **Consentement explicite** (art. 9(2)(a)) ou **recherche scientifique** (art. 9(2)(j) + art. 89), plus une base de l'art. 6 | finalité annoncée aux volontaires ; pseudonymisation ; résultats agrégés ; durée de conservation définie |
| ② | **Segmentation de tous les utilisateurs** pour concevoir le produit | activité seule (3 champs) | **Intérêt légitime** (art. 6(1)(f)) après test de mise en balance documenté | information sur le profilage (art. 13-14) ; droit d'opposition (art. 21) ; pas de décision à effet juridique ou significatif (art. 22) |
| ③ | **Ciblage** de fonctionnalités, de contenus ou de publicités selon le segment | activité seule | intérêt légitime **seulement** pour des fonctionnalités qui servent l'utilisateur (outils de pause, contrôle du temps) ; **consentement** pour tout ciblage publicitaire (CEPD, lignes directrices 8/2020) | **AIPD obligatoire** (art. 35 : profilage systématique à grande échelle incluant des personnes vulnérables) ; interdit pour la publicité aux mineurs (DSA art. 28) |

### 2.2 Limitation de la finalité (art. 5(1)(b) et 6(4))

Les volontaires ont accepté une **étude sur le bien-être**. Réutiliser leurs réponses pour **cibler des fonctionnalités** est une finalité nouvelle. Le test de compatibilité de l'art. 6(4) examine le lien entre les finalités, le contexte de la collecte, la nature des données (ici des données de santé), les conséquences pour les personnes et les garanties. Il ne passe pas : un volontaire ne s'attend pas raisonnablement à ce que son niveau de stress déclaré serve à orienter le produit qu'on lui montre.

**Règle adoptée dans le projet** : les données de bien-être et de santé servent **uniquement à évaluer** la segmentation, de façon agrégée. Elles ne servent **ni à définir, ni à affecter, ni à cibler** un segment.

### 2.3 Minimisation (art. 5(1)(c))

Le notebook (§ 4) mesure l'apport de chaque variable. Résultat : **3 champs d'activité** (minutes actives par jour, posts par semaine, reels regardés par jour) suffisent. Revenu, éducation, situation familiale, genre, pays n'apportent rien → ils ne doivent **pas** être utilisés, ni collectés pour cet usage.

### 2.4 Transparence (art. 12-14 et 21)

À prévoir avant tout déploiement :
- une mention claire, compréhensible par un adolescent, expliquant que l'activité sert à adapter certaines fonctionnalités ;
- la possibilité de **s'opposer** au profilage (art. 21) et de voir/désactiver les fonctionnalités qui en découlent ;
- pour le panel : information sur l'usage **agrégé** des résultats et sur le droit de retirer son consentement (art. 7(3)).

---

## 3. Mineurs (R4-18)

Dans la réplique, **9,5 % du panel a entre 13 et 17 ans**, et **14 % des comptes** auraient été ouverts avant 13 ans (extrait brut).

| Règle | UE | Royaume-Uni |
|---|---|---|
| Âge du consentement numérique (art. 8) | 16 ans par défaut, abaissable jusqu'à 13 ans par chaque État (ex. 15 ans en France, 16 ans en Allemagne et en Irlande) | 13 ans (UK GDPR / DPA 2018) |
| Publicité fondée sur le profilage | **interdite** dès que la plateforme sait avec une certitude raisonnable que l'utilisateur est mineur (DSA art. 28(2)) | — (encadrée par le Children's Code) |
| Obligation générale | niveau élevé de vie privée, de sûreté et de sécurité (DSA art. 28(1)) ; lignes directrices de la Commission (juillet 2025) | **Age Appropriate Design Code** (ICO) : intérêt supérieur de l'enfant, AIPD, paramètres protecteurs par défaut, **profilage désactivé par défaut** (standard 12), pas de *nudges* nuisibles |

**Ce que montre la segmentation** : les segments *Créateurs-Reels* comptent 25 à 35 % de mineurs, contre moins de 4 % chez les Spectateurs. Segmenter, c'est donc en partie **inférer l'âge**.

**Conditions retenues pour les mineurs** :
1. aucun ciblage publicitaire ni commercial par segment ;
2. seules des utilisations **protectrices** : rappels de pause, limites de temps, fin de flux, activées par défaut et désactivables simplement ;
3. participation au **panel** seulement avec le consentement du titulaire de l'autorité parentale lorsque l'âge du mineur est inférieur au seuil national ;
4. les comptes ouverts avant 13 ans sont un signal pour l'**assurance de l'âge**, pas une variable de segmentation.

---

## 4. Le panel volontaire : participer ≠ être ciblé (R4-18)

| | Participation à l'étude | Utilisation ultérieure pour cibler |
|---|---|---|
| Ce que le volontaire a accepté | questionnaire bien-être + analyse de son activité à des fins d'étude | rien |
| Base légale | consentement explicite / recherche | aucune pour des données de santé, sauf **nouveau consentement** explicite, spécifique, libre (refuser ne doit rien coûter) |
| Droits | retrait à tout moment (art. 7(3)), accès, effacement | — |
| Dans notre projet | évaluation agrégée de la segmentation | les scores ne sont **jamais rattachés** à un segment opérationnel ni à un compte |

---

## 5. Un segment peut-il révéler une information de santé ? (R4-19, avec Marco)

**Oui.** Le stress perçu relève de la santé mentale, donc des données de santé (art. 4(15) et 9). Sur la réplique, appartenir au segment *S5 · Intensifs spectateurs* multiplie par environ **2,6** la probabilité d'un stress élevé (≥ 30/40). Le segment est construit sans donnée de santé, mais il devient un **indicateur probabiliste** de santé dès qu'on connaît ce lien.

La CJUE juge que des données qui permettent de **révéler indirectement**, par recoupement ou déduction, une catégorie particulière relèvent de l'art. 9 (*OT*, C-184/20, 1ᵉʳ août 2022 ; *Meta Platforms c. Bundeskartellamt*, C-252/21, 4 juillet 2023).

**Mesures de réduction du risque**
1. Noms de segments **comportementaux** (« Intensifs spectateurs »), jamais « stressés » ou « à risque ».
2. Aucun stockage individuel du lien segment ↔ bien-être ; les statistiques de bien-être par segment restent dans le rapport d'étude.
3. Pas de partage du segment avec des annonceurs, pas de ciblage publicitaire.
4. Les fonctionnalités liées au bien-être sont déclenchées par un **critère transparent et maîtrisable** (temps passé affiché à l'utilisateur), proposées à tous, pas par l'appartenance à un segment inféré.
5. Revue par le DPO avant tout nouvel usage ; AIPD mise à jour.

---

## 6. Variables utilisables si les segments servent au ciblage

| Variables | Construire / affecter les segments | Pourquoi |
|---|---|---|
| Minutes actives, posts créés, reels regardés | ✅ oui | activité détenue pour tous ; nécessaire et suffisante |
| Autres compteurs d'activité, paramètres du compte | ⚠️ possible mais inutile | aucun apport mesuré → minimisation |
| Âge | ⚠️ uniquement pour **protéger** | DSA art. 28 ; Children's Code |
| Genre, pays, revenu, éducation, situation familiale, enfants | ❌ non | aucun apport ; risque de discrimination ; donnée financière |
| Stress, bonheur, sommeil, IMC, tension, tabac, alcool, activité physique | ❌ jamais | données de santé, collectées pour une autre finalité |

---

## 7. Données synthétiques : précautions

- Traiter la réplique comme une donnée personnelle tant que le risque de mémorisation n'est pas mesuré.
- Demander à l'équipe privacy : **distance au plus proche enregistrement réel** (*distance to closest record*) et test d'**inférence d'appartenance** (*membership inference*) avant toute diffusion.
- Dans le dépôt : données exclues (`.gitignore`), notebook sans ligne individuelle, seuil d'affichage de 100 personnes vérifié automatiquement (`check_k`).

---

## 8. Contrôles appliqués dans le notebook (R4-20, Marco)

| Contrôle | Mise en œuvre |
|---|---|
| Aucun enregistrement individuel affiché | aucun `.head()` ni `.sample()` affiché ; exemple d'affectation sur des profils **fictifs** (médianes de segment) |
| Aucun résultat isolant une poignée d'utilisateurs | `check_k()` avant chaque tableau groupé ; masquage des taux correspondant à moins de 100 personnes (§ 10.1) |
| Pas de donnée sensible en entrée | `assert` : aucune variable de bien-être, de santé ou de profil dans `REQUIRED_FIELDS` |
| Exports | paramètres du modèle et tableaux agrégés uniquement |

---

## Sources

- Règlement (UE) 2016/679 (RGPD) : art. 4, 5, 6, 7, 8, 9, 12-14, 21, 22, 35, 89 ; considérant 26.
- Règlement (UE) 2022/2065 (Digital Services Act), art. 28 — texte : [Springlex, DSA art. 28](https://www.springlex.eu/en/packages/dsa/dsa-regulation/article-28/) ; analyse : [Bird & Bird, publicité ciblée et mineurs](https://cm.twobirds.com/en/insights/2023/global/dsa-publicite-ciblee-destinee-aux-mineurs-une-interdiction-a-venir).
- Commission européenne, lignes directrices sur la protection des mineurs (art. 28 DSA), juillet 2025 — présentation : [Law Society Gazette Ireland](https://lawsociety.ie/gazette/top-stories/2025/july/eu-online-safety-guide-major-step-forward).
- UK GDPR et Data Protection Act 2018 ; ICO, [Age appropriate design: a code of practice for online services](https://ico.org.uk/media/for-organisations/uk-gdpr-guidance-and-resources/childrens-information/childrens-code-guidance-and-resources/age-appropriate-design-a-code-of-practice-for-online-services-2-1.pdf).
- Âge du consentement numérique par pays : [GDPR Local](https://gdprlocal.com/digital-age-of-consent-under-the-gdpr/) ; Irlande : [RDJ](https://www.rdj.ie/insights/the-digital-age-of-consent-in-ireland-will-be-16).
- CEPD, lignes directrices 8/2020 sur le ciblage des utilisateurs de médias sociaux ; G29, lignes directrices sur la décision automatisée et le profilage (WP251 rév.01).
- CJUE, 1ᵉʳ août 2022, *OT*, C-184/20 ; CJUE, 4 juillet 2023, *Meta Platforms e.a.*, C-252/21 — commentaire : [DLA Piper](https://privacymatters.dlapiper.com/2023/07/eu-cjeus-landmark-decision-in-meta-vs-bundeskartellamt/).
