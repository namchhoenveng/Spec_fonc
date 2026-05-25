# Backlog FUENI MVP — Epics et Roadmap

**Projet :** FUENI MVP
**Client :** Nazounki
**Préparé par :** Namchhoen VENG — Product Owner, Paris Partners
**Version :** 1.0 — Brouillon soumis à validation
**Date :** 2026-05-25
**Statut :** Brouillon — à valider avec Direction Nazounki + équipe technique

**Destinataires :** Direction Nazounki, Direction Paris Partners, équipe technique (Chamrong THOR, Songchhay EAR, Mengty LIM, QA)

---

## 1. Objet du document

Le présent document constitue le **backlog de niveau Epic** du MVP FUENI, accompagné de la **roadmap de livraison** sur 6 mois (12 sprints de 2 semaines). Il sert de :

- **Référence stratégique** pour le pilotage de projet et les comités de pilotage Nazounki
- **Base de planification** pour le découpage en user stories (à venir, propriété équipe technique côté Cambodge)
- **Document contractuel** sur le périmètre engagé du MVP

Le backlog est **vivant** : il sera mis à jour à chaque revue trimestrielle ou en cas d'évolution structurelle du périmètre. Les changements significatifs (ajout/retrait d'epic, décalage de milestone > 1 sprint) requièrent validation conjointe par Nazounki et la Direction de projet Paris Partners.

---

## 2. Vue d'ensemble

| Élément | Détail |
|---|---|
| **Nombre d'epics MVP** | 13 |
| **Nombre de milestones** | 5 (M0 → M4) |
| **Durée totale MVP** | 6 mois |
| **Nombre de sprints** | 12 sprints de 2 semaines |
| **Périmètre géographique** | 9 pays MVP (UEMOA + Cameroun + RDC) |
| **Équipe** | 1 PO (25 %) + 1 Tech Lead (50 % code) + 1 dev backend (100 %) + 1 frontend (100 %) + 1 QA (100 %) |
| **Tout ce qui dépasse le MVP** | **post-MVP** — voir §8 du présent document |

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          ROADMAP MVP — 6 MOIS                            │
└─────────────────────────────────────────────────────────────────────────┘

M0  M1            M2            M3            M4
│   │             │             │             │
S1  S2 S3 S4     S5 S6 S7      S8 S9 S10     S11 S12
│   │            │              │              │
│   │            │              │              └→ Launch (S12 fin)
│   │            │              └→ Booking loop complète
│   │            └→ Doctor utilisable
│   └→ Patient utilisable
└→ Infra prête
```

---

## 3. Liste des Epics

Les epics sont numérotés selon la **priorité business** (décision client + investissement intellectuel déjà réalisé), et non selon l'ordre de développement. L'ordre de développement est défini par les milestones (cf. §4) et tient compte des dépendances techniques.

### Epic 1 — Patient Registration

| Aspect | Détail |
|---|---|
| **Périmètre** | Inscription patient (téléphone + e-mail + mot de passe), vérification SMS OTP bloquant + e-mail OTP différé, profil de base obligatoire post-OTP, modal de bienvenue, suppression de compte (post-MVP) |
| **Audience** | Patients adultes (≥ 18 ans) résidant dans l'un des 9 pays MVP |
| **Cœur de valeur** | Acquisition B2C — friction minimale à l'inscription pour maximiser la conversion. **Acté par Nazounki comme priorité MVP #1** (décision 2026-05-22). |
| **Dépendances amont** | Epic 3 (Homepage pour le point d'entrée S'inscrire) · Auth (Keycloak) opérationnel |
| **Référence SF** | `MVP_SPRINT_PLANNING/NAZOUNKI_SF_PATIENT_REGISTRATION.md` (v1.0 brouillon) |
| **Critères de succès** | Taux d'inscription terminée ≥ 80 % · Durée moyenne ≤ 2 min · Taux d'abandon SMS OTP < 8 % |
| **Hors périmètre MVP** | Profils dépendants · vérification d'identité plateforme (KYC) · connexion sociale (Google/Apple) · suppression de compte |

### Epic 2 — Authentication & Login

| Aspect | Détail |
|---|---|
| **Périmètre** | Login transverse aux 3 rôles (patient, médecin, super admin), reset mot de passe par OTP différé, 2FA SMS optionnelle, expiration de session par inactivité |
| **Audience** | Tous les utilisateurs inscrits |
| **Cœur de valeur** | **Foundational** — sans cette epic, le patient peut s'inscrire mais ne peut jamais revenir. La fonctionnalité Patient Registration est incomplète sans Auth. Critique pour la continuité d'usage. |
| **Dépendances amont** | Epic 1 (Patient Registration) ou Epic 6 (Doctor Registration) ont créé des comptes Keycloak |
| **Référence SF** | À rédiger — **SF-LOGIN** (à produire avant Sprint 4) |
| **Critères de succès** | Taux d'échec login < 3 % · Délai moyen reset MDP < 5 min · Taux d'activation 2FA ≥ 25 % |
| **Hors périmètre MVP** | Connexion par téléphone + OTP SMS (vs e-mail + MDP) · Single Sign-On · Connexion sociale · Gestion explicite des sessions actives |

### Epic 3 — Public Homepage & Public Annuaire

| Aspect | Détail |
|---|---|
| **Périmètre** | Page d'accueil publique : hero section avec value prop, **audience cards** (Patient / Professionnel de santé), barre de navigation (S'inscrire ▾ + Espace patient + Espace professionnel), footer légal (CGU, Politique de confidentialité, Contact), responsive mobile-first.<br><br>**Annuaire public en lecture seule** : liste paginée des médecins validés avec filtres (spécialité, ville), fiche praticien consultable publiquement (cf. Epic 8). La réservation effective d'un RDV bascule vers `/login` ou `/inscription` (couvert par Epic 11 côté connecté). |
| **Audience** | Visiteurs anonymes (patients potentiels + médecins potentiels) |
| **Cœur de valeur** | Premier point de contact — convertit le trafic en inscriptions. La présence d'un annuaire public augmente la confiance et le SEO, conformément aux standards de l'industrie B2C santé. |
| **Dépendances amont** | Design system validé · Nom de domaine tranché · Hébergement opérationnel · Epic 8 (Doctor Public Profile) pour le contenu de l'annuaire |
| **Critères de succès** | Taux de conversion homepage → page inscription ≥ 5 % · Taux de consultation annuaire ≥ 30 % des visiteurs · Score Lighthouse mobile > 80 · SEO de base (meta tags, sitemap, robots.txt) |
| **Hors périmètre MVP** | Blog · témoignages clients · landing pages dédiées par spécialité · A/B testing infrastructure · vidéos de présentation · Schema.org markup avancé · annuaire post-MVP (sous-spécialités, langues, badges, avis) |

**Note sur la portée "production-ready minimale"** : la homepage MVP doit être **professionnelle et brandée**, mais ne couvre pas tous les éléments d'un site marketing complet. L'objectif est de convertir efficacement le visiteur en utilisateur inscrit, pas de servir de site vitrine institutionnel.

### Epic 4 — Patient Account & Profile

| Aspect | Détail |
|---|---|
| **Périmètre** | Tableau de bord "Mon espace", profil progressif (coordonnées, contact d'urgence, infos médicales : groupe sanguin, allergies, traitements, antécédents), préférences de notification, modification de profil (3 niveaux) |
| **Audience** | Patients inscrits actifs |
| **Cœur de valeur** | Permet au patient d'enrichir son profil sans dépendre d'une prise de RDV — fidélisation |
| **Dépendances amont** | Epic 1 (Patient Registration) · Epic 2 (Login) |
| **Référence SF** | Couvert dans `NAZOUNKI_SF_PATIENT_REGISTRATION.md` §4.5 et §4.7 |
| **Critères de succès** | Taux de complétion profil sous 7 jours ≥ 40 % · Taux d'utilisation paramètres notif ≥ 30 % |
| **Hors périmètre MVP** | Médecin traitant déclaré · partage de profil avec un praticien |

**Note sur l'accès praticien au profil patient (MVP)** : au MVP, le praticien voit le profil du patient **uniquement dans le contexte d'un RDV confirmé** (le RDV joue le rôle de consentement implicite ponctuel). Il n'existe pas de mécanisme de partage persistant ou proactif du profil hors RDV — ce sera traité en post-MVP via les fonctions "médecin traitant déclaré" et "partage de profil".

### Epic 5 — Patient Documents Management

| Aspect | Détail |
|---|---|
| **Périmètre** | Upload de documents médicaux personnels (ordonnances, comptes-rendus, résultats analyses), classement, recherche, suppression |
| **Audience** | Patients inscrits |
| **Cœur de valeur** | Valeur d'usage **indépendante** de la prise de RDV — permet au patient d'utiliser FUENI dès J1 même sans interaction médecin |
| **Dépendances amont** | Epic 1 (Patient Registration) · Epic 2 (Login) · stockage sécurisé OVH Object Storage |
| **Critères de succès** | ≥ 50 % des patients ont importé au moins 1 document à J+30 |
| **Hors périmètre MVP** | Partage avec un praticien · annotation collaborative · OCR automatique · classification IA |

### Epic 6 — Doctor Registration & KYC

| Aspect | Détail |
|---|---|
| **Périmètre** | Inscription médecin individuel, upload pièces justificatives (CNI, diplôme, attestation Ordre, RIB, justificatif domicile), file d'attente Super Admin, validation / rejet manuel avec motifs standardisés, re-soumission illimitée |
| **Audience** | Médecins exerçant en pratique libérale dans les 9 pays MVP |
| **Cœur de valeur** | Côté offre — sans médecins validés, l'annuaire est vide |
| **Dépendances amont** | Epic 11 (Super Admin Console) doit fournir l'UI de validation · stockage sécurisé OVH |
| **Référence SF** | `MVP_SPRINT_PLANNING/NAZOUNKI_SF_DOCTOR_REGISTRATION.md` (v1.0 brouillon, à valider) |
| **Critères de succès** | SLA validation Super Admin ≤ 48 h ouvrées · Taux de rejet au premier passage < 30 % |
| **Hors périmètre MVP** | Inscription pharmacien/infirmier · Inscription cabinet (entité morale) · vérification automatique du numéro d'Ordre via API · API tiers KYC (Smile Identity, etc.) |

### Epic 7 — Doctor Subscription & Payment

| Aspect | Détail |
|---|---|
| **Périmètre** | Choix du plan (Free / Solo), capture du moyen de paiement à l'inscription, période d'essai gratuite 15 jours (démarre à la validation KYC), premier prélèvement Solo, renouvellements automatiques (carte) ou manuels (Mobile Money), bascule auto Free en cas d'échec |
| **Audience** | Médecins en plan Solo (et Free comme niveau d'entrée) |
| **Cœur de valeur** | Modèle économique du MVP — sans cette epic, pas de revenus récurrents |
| **Dépendances amont** | Epic 6 (Doctor Registration) · intégration Flutterwave validée + KYC entité signé · plan B CinetPay si nécessaire |
| **Critères de succès** | Taux de conversion essai Solo → premier prélèvement ≥ 50 % · Taux de réussite premier prélèvement (carte) ≥ 90 % · Taux de bascule Free évitée par mise à jour MDP ≥ 60 % |
| **Hors périmètre MVP** | Stripe (Europe) · Mobile Money recurrence (selon maturité Flutterwave) · Wave / Orange Money / MTN Money en intégration directe |

### Epic 8 — Doctor Public Profile

| Aspect | Détail |
|---|---|
| **Périmètre** | Fiche praticien publique visible aux patients : photo, biographie, spécialité, langues parlées, lieu de consultation principal, horaires, tarifs par type de consultation, badge "Vérifié" |
| **Audience** | Visiteurs anonymes + patients inscrits |
| **Cœur de valeur** | Vitrine du médecin — booste la confiance et la conversion à la prise de RDV |
| **Dépendances amont** | Epic 6 (Doctor Registration + KYC validé) · Epic 3 (Homepage et annuaire public) |
| **Critères de succès** | ≥ 70 % des médecins validés ont complété leur fiche publique à J+7 |
| **Hors périmètre MVP** | Avis patients · témoignages · galerie photos · vidéo de présentation · sous-spécialités (taxonomie post-MVP) |

### Epic 9 — Doctor Calendar & Planning

| Aspect | Détail |
|---|---|
| **Périmètre** | Création de créneaux de consultation, modèles récurrents (semaine type), blocages (congés, RDV externes), vue jour/semaine/mois, gestion de plusieurs types de consultation (cabinet, à domicile post-MVP) |
| **Audience** | Médecins (côté praticien) |
| **Cœur de valeur** | Permet au médecin d'exposer ses disponibilités → condition nécessaire à la prise de RDV |
| **Dépendances amont** | Epic 6 (Doctor Registration validé) |
| **Critères de succès** | ≥ 80 % des médecins ACTIVE ont défini un planning à J+7 post-validation |
| **Hors périmètre MVP** | Synchronisation Google Calendar / iCal · plannings multi-cabinets · gestion d'équipe (secrétariat) · règles avancées (jeunes patients en matinée, etc.) |

### Epic 10 — Doctor Patient Management

| Aspect | Détail |
|---|---|
| **Périmètre** | Liste patients du médecin, fiche patient (vue agrégée : profil, RDV passés, documents partagés), création d'un dossier médical de base (POMR simplifié), upload de documents par le médecin pour un patient (ordonnance, compte-rendu) |
| **Audience** | Médecins (côté praticien) |
| **Cœur de valeur** | Cœur du métier médical — sans cela, le médecin ne peut pas exercer numériquement |
| **Dépendances amont** | Epic 6 (Doctor Registration validé) · Epic 11 (Search & Booking) pour avoir des patients liés |
| **Critères de succès** | ≥ 60 % des médecins ACTIVE ont consulté au moins 1 dossier patient à J+30 post-1ère consultation |
| **Hors périmètre MVP** | POMR complet (problèmes / objectifs / médicaments / résultats avec workflow) · prescription électronique · annotation collaborative · intégration laboratoires |

### Epic 11 — Practitioner Search & Booking

| Aspect | Détail |
|---|---|
| **Périmètre** | **Couche transactionnelle** de prise de RDV au-dessus du moteur de recherche / annuaire. Sélection d'un créneau de consultation, formulaire de prise de RDV (motif, préférences), confirmation de RDV, réception SMS + e-mail de confirmation, annulation par patient ou médecin, gestion des no-shows.<br><br>**Note sur l'annuaire** : la **liste des praticiens** et les **filtres de recherche** (spécialité, ville, nom) sont livrés par l'Epic 3 (Public Homepage & Annuaire Public) et exposés en accès **public**. Epic 11 ajoute la **couche réservation connectée** : à l'action « Prendre RDV », le visiteur anonyme est redirigé vers `/login` ou `/inscription` ; un patient déjà connecté accède directement au formulaire de réservation. |
| **Audience** | Patients connectés (prise + gestion de RDV) + Médecins (réception + gestion) — c'est la **boucle qui crée la valeur de plateforme** |
| **Cœur de valeur** | Connecte l'offre et la demande — sans cela, FUENI est un annuaire sans transaction |
| **Dépendances amont** | Epic 3 (annuaire public et UI de recherche) · Epic 8 (Doctor Public Profile) · Epic 9 (Doctor Calendar) · Epic 13 (Notifications) |
| **Critères de succès** | Taux de réussite prise de RDV ≥ 95 % · Délai moyen sélection créneau → confirmation < 1 min · Taux de no-show < 15 % |
| **Hors périmètre MVP** | Téléconsultation · acompte mobile money à la réservation · liste d'attente automatique · recommandation IA · réservation multi-RDV (suivi) · recherche full-text avancée (couvert en Epic 2 niveau MVP, raffiné post-MVP) |

### Epic 12 — Super Admin Console

| Aspect | Détail |
|---|---|
| **Périmètre** | File d'attente KYC (Doctor Registration), validation/rejet avec motifs, gestion des utilisateurs (recherche, suspension, réactivation), gestion des plans (création, modification, désactivation), pricing matrix par devise (XOF, XAF, CDF), tableau de bord opérationnel (KPIs : inscriptions, validations, paiements, anomalies) |
| **Audience** | Équipe Nazounki opérationnelle (validation KYC, support patient, comptabilité, direction) |
| **Cœur de valeur** | Outils d'exploitation — sans eux, Nazounki ne peut pas faire tourner la plateforme |
| **Dépendances amont** | Tous les epics qui produisent des entités à administrer (Patient, Doctor, Subscription) |
| **Critères de succès** | SLA validation KYC ≤ 48 h ouvrées · ≥ 95 % des KYC validés sans intervention de support externe |
| **Hors périmètre MVP** | Rôles internes Nazounki différenciés (Médecin Nazounki, Secrétaire, Logisticien, Financier) — Super Admin unique en MVP · BI avancé · export comptable automatisé |

### Epic 13 — Notifications & Communications

| Aspect | Détail |
|---|---|
| **Périmètre** | Infrastructure transverse pour SMS (Africa's Talking) et e-mails transactionnels (Brevo), templates des notifications clés (inscription, validation KYC, RDV, paiement, rappels licence, suppression), gestion des préférences patient |
| **Audience** | Tous les utilisateurs (envois automatiques selon flow) |
| **Cœur de valeur** | Cross-cutting — sans cela, aucun flux n'est complet (pas d'OTP, pas de confirmation RDV, pas de factures) |
| **Dépendances amont** | Comptes Brevo + Africa's Talking opérationnels (cf. `NAZOUNKI_SERVICES_PROVIDERS_CHECKLIST.md`) |
| **Critères de succès** | Taux de délivrance SMS ≥ 95 % · Taux d'e-mails non en spam ≥ 95 % · Coût SMS conforme à l'estimation initiale |
| **Hors périmètre MVP** | Notifications WhatsApp Business · notifications push mobile (pas d'app native MVP) · campagnes marketing |

---

## 4. Roadmap — 5 Milestones sur 6 mois

### M0 — Foundations *(Sprint 1)*

**Durée :** 2 semaines · **Objectif :** infrastructure prête, équipe opérationnelle, foundations techniques en place

**Périmètre Sprint 1 :**

- Provisionnement environnements OVH (dev, staging, prod) + Cloudflare DNS + tunnel
- Setup Keycloak self-hosted (realm, clients, password policy, OTP config)
- Setup CI/CD GitLab (build, test, deploy automatisé)
- Setup observabilité (Prometheus + Grafana + Loki + Tempo)
- Création comptes prestataires SaaS : Brevo, Africa's Talking, Flutterwave (KYC en cours)
- Cadrage architectural final (Network-Split Gateway, BFF Next.js, Redis session) — validation CTO Seyha HENG
- Onboarding Mengty LIM (dev backend) — démarrage 2026-05-26, 4 jours d'onboarding stack Spring Boot + repo

**Livrable Sprint 1 :** environnements opérationnels, équipe au complet, premier commit "Hello World" déployé en staging.

**Note :** aucun epic métier n'est livré ce sprint. C'est volontaire — les foundations conditionnent tout le reste.

---

### M1 — Public + Patient Core *(Sprints 2-4)*

**Durée :** 6 semaines · **Objectif :** le patient peut s'inscrire, se connecter, gérer son profil et ses documents

**Epics délivrés :**

- **Epic 1** — Patient Registration
- **Epic 2** — Authentication & Login *(côté patient uniquement à ce stade)*
- **Epic 3** — Public Homepage & Public Annuaire
- **Epic 4** — Patient Account & Profile
- **Epic 5** — Patient Documents Management

**Livrable M1 :** un patient peut s'inscrire en autonomie, se connecter, compléter son profil et importer ses documents. La homepage est en ligne.

---

### M2 — Doctor Core *(Sprints 5-7)*

**Durée :** 6 semaines · **Objectif :** le médecin peut s'inscrire, payer, et apparaître dans l'annuaire (vitrine seulement, pas encore de RDV)

**Epics délivrés :**

- **Epic 2** — Authentication & Login *(extension au rôle Doctor)*
- **Epic 6** — Doctor Registration & KYC
- **Epic 7** — Doctor Subscription & Payment
- **Epic 8** — Doctor Public Profile

**Dépendance critique :** KYC Flutterwave (côté Nazounki) doit être finalisé avant Sprint 6 — sinon décalage de M2 vers le sprint suivant.

**Livrable M2 :** un médecin peut s'inscrire, soumettre ses justificatifs, voir son dossier validé en 48 h, payer son plan Solo (avec essai 15 j), et apparaître dans l'annuaire public.

---

### M3 — Booking Loop *(Sprints 8-10)*

**Durée :** 6 semaines · **Objectif :** la boucle complète patient → médecin est opérationnelle. Un patient peut prendre un RDV avec un médecin.

**Epics délivrés :**

- **Epic 9** — Doctor Calendar & Planning
- **Epic 10** — Doctor Patient Management
- **Epic 11** — Practitioner Search & Booking
- **Epic 13** — Notifications & Communications (infrastructure consolidée)

**Livrable M3 :** la boucle complète fonctionne — un patient peut chercher un médecin, voir un créneau, le réserver, et recevoir SMS + e-mail de confirmation. Le médecin voit le RDV dans son agenda et peut créer/consulter le dossier patient.

---

### M4 — Super Admin & Launch *(Sprints 11-12)*

**Durée :** 4 semaines · **Objectif :** Nazounki dispose de ses outils d'exploitation, et le MVP est commercialisable

**Epics délivrés :**

- **Epic 12** — Super Admin Console (consolidation : user management, plan management, pricing matrix, dashboard opérationnel)
- Stabilisation, tests E2E, optimisations performances, mise en prod soft

**Livrable M4 :** Nazounki peut opérer la plateforme (validation KYC, gestion utilisateurs, monitoring financier). Le MVP est en production avec premiers utilisateurs réels (bêta restreinte → ouverture progressive).

---

## 5. Hypothèses de capacité

Cette roadmap suppose les conditions suivantes côté équipe :

| Rôle | Localisation | Engagement |
|---|---|---|
| **Namchhoen VENG** (Product Owner) | 🇫🇷 Paris | ~25 % FUENI (4 projets en parallèle) — focus SF, scope, comités, contrat |
| **Chamrong THOR** (Tech Lead + PM tactique) | 🇰🇭 Cambodge | 50 % code / 50 % PM tactique (cf. PM Handover) |
| **Mengty LIM** (Dev Backend) | 🇰🇭 Cambodge | 100 % FUENI dès Sprint 1 — démarrage 2026-05-26, onboardé Sprint 0 |
| **Songchhay EAR** (Frontend) | 🇰🇭 Cambodge | 100 % FUENI dès Sprint 1 |
| **QA interne** | 🇰🇭 Cambodge | 100 % FUENI dès Sprint 1, dédié (pas de rotation — cf. procédure QA-Dev-PM) |
| **Seyha HENG** (CTO Paris Partners) | 🇰🇭 Cambodge | Consultation ponctuelle sur architecture structurelle |

**Capacité nominale par sprint** :
- Backend : ~1,5 FTE (Chamrong THOR 0,5 + Mengty LIM 1,0) = 10 j ouvrés / sprint
- Frontend : 1 FTE (Songchhay EAR) = 10 j ouvrés / sprint
- QA : 1 FTE = 10 j ouvrés / sprint
- **Total équipe technique : 3,5 FTE soit ~35 j-h par sprint**

---

## 6. Risques et dépendances critiques

| Risque | Probabilité | Impact | Mitigation |
|---|---|---|---|
| **KYC Flutterwave > 4 sem** | Moyenne | Décale M2 (Doctor Core) | Lancer KYC dès Sprint 0, dev en parallèle avec mock, bascule mock→réel au Sprint 6 |
| **SF restantes non validées à temps** | Moyenne | Démarre dev sans périmètre validé → re-work | Plan de validation SF : Doctor Reg (S3), Subscription (S4), Search & Booking (S6), Super Admin (S9) |
| **Ramp-up Mengty LIM (dev backend) plus lent que prévu sur la stack** | Faible | Vélocité backend réduite S1-S2 | Mengty LIM confirmé, démarrage 2026-05-26 — onboarding 4 jours Sprint 0 + pair programming intensif avec Chamrong THOR S1-S2 |
| **Décision domaine + Brevo DNS** | Moyenne | Blocage e-mails transactionnels | Décision Nazounki avant Sprint 1 obligatoire (Q16.1 backlog questions) |
| **Vélocité réelle < estimation** | Moyenne | Décalage du launch | Buffer 1 sprint dans M4 (Sprint 12 = stabilisation), réduction périmètre si nécessaire |
| **Découpage epic → user stories trop fin** | Moyenne | Surcharge backlog Jira, dilution focus | Chamrong THOR cadre 8-12 user stories max par epic, refinement progressif sprint par sprint |
| **Chamrong THOR sur-sollicité (PM + code + archi)** | Moyenne | Burnout, qualité dégradée | Skip-level mensuel PO ↔ équipe, alerte préventive si ratio 50/50 dérive > 65 % PM |
| **Bug critique en prod fin M4** | Faible | Decalage launch | Tests E2E + audit de sécurité externe avant mise en prod soft |

---

## 7. Gouvernance et révision

### 7.1 Cycle de révision

- **Hebdomadaire** : rapport Chamrong THOR → Namchhoen VENG (rythme procédure QA-Dev-PM Cross-Entités §11.8)
- **Bi-mensuel** : comité de pilotage Nazounki (selon besoin, agenda jalons)
- **À chaque fin de milestone** : bilan formel + ajustement éventuel des sprints suivants
- **À la fin du MVP** : rétrospective complète, base de la roadmap post-MVP

### 7.2 Versioning du backlog

- **v1.0 — Brouillon** : version actuelle, soumise à validation
- **v1.0 — Validée** : à la signature électronique conjointe Nazounki + Paris Partners
- **v1.1, v1.2…** : ajustements en cours de MVP (changement de scope, décalage milestone < 1 sprint)
- **v2.0** : refonte si pivot stratégique majeur

### 7.3 Source unique de vérité

Le présent fichier est la **source unique de vérité** du backlog niveau Epic. Toute représentation différente (Jira, Confluence, présentations) doit faire référence à cette version canonique.

---

## 8. Hors périmètre MVP (post-MVP)

Liste consolidée des fonctionnalités identifiées comme **post-MVP** et donc explicitement hors périmètre des 6 mois engagés. Elles seront priorisées et planifiées lors de la roadmap post-MVP, à élaborer en fin de M4.

| Catégorie | Élément post-MVP |
|---|---|
| **Identité & sécurité patient** | Profils dépendants · Inscription mineurs · Vérification d'identité plateforme (KYC patient) · Connexion sociale (Google/Apple) · Gestion explicite des sessions actives · Suppression de compte |
| **Identité & sécurité médecin** | Inscription cabinet / institution · Inscription pharmacien / infirmier · API vérification Ordre des Médecins · API tiers KYC (Smile Identity) |
| **Téléconsultation** | Vidéo Agora.io · workflow téléconsultation complet · prescription électronique en téléconsultation |
| **Messagerie sécurisée** | Synapse / Matrix · échanges patient ↔ médecin asynchrones · partage de documents en messagerie |
| **Services contextuels** | Trouver une ambulance · trouver un hôpital · trouver un médicament · second avis · RCP (réunion de concertation pluridisciplinaire) · triage IA |
| **Paiement avancé** | Stripe (Europe) · Mobile Money récurrent direct (Wave, Orange Money, MTN Money en API directe) · acompte à la réservation · split de paiement (mutuelle, complémentaire) |
| **Modules métier** | POMR complet · dossier médical avancé · suivi traitement permanent · gestion bilans biologiques · intégration laboratoires · annotation collaborative |
| **Acquisition et expansion** | Référencement entre patients (parrainage) · campagne marketing · A/B testing landing pages · expansion anglophone (Nigeria, Kenya) |
| **Plateforme et organisation** | Rôles internes Nazounki différenciés (au-delà du Super Admin unique) · BI avancé · export comptable automatisé · application mobile native |
| **Conformité avancée** | Certifications additionnelles (ISO 27001, HDS si expansion FR) · audits externes récurrents |

---

## 9. Acceptation

| Rôle | Nom | Statut | Date |
|---|---|---|---|
| Direction Nazounki | [À renseigner] | En attente | — |
| Direction Paris Partners | [À renseigner] | En attente | — |
| Product Owner (Paris Partners) | Namchhoen VENG | Auteur | 2026-05-25 |
| Tech Lead (Paris Partners) | Chamrong THOR | En attente | — |

Validation par **signature électronique** (DocuSign ou Yousign) selon la procédure de collaboration QA-Dev-PM §12.

---

## 10. Annexes et références

- `MVP_SPRINT_PLANNING/NAZOUNKI_SF_PATIENT_REGISTRATION.md` — SF Patient (v1.0 brouillon)
- `MVP_SPRINT_PLANNING/NAZOUNKI_SF_DOCTOR_REGISTRATION.md` — SF Médecin (v1.0 brouillon)
- `MVP_SPRINT_PLANNING/NAZOUNKI_SERVICES_PROVIDERS_CHECKLIST.md` — Comptes prestataires applicatifs
- `MVP_SPRINT_PLANNING/NAZOUNKI_PM_HANDOVER_CHAMRONG.md` — Passation PM tactique
- `PMO Procedure/QA-Dev-PM_Collaboration_Procedure_FR.md` — Standard de collaboration QA/Dev/PM (Paris Partners, cross-entités)
- `Rebuild/NAZOUNKI_ARCH_TECHNIQUE_REBUILD_V2.md` — Architecture technique cible
