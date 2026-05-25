# Spécification Fonctionnelle — Cycle de vie du compte Patient

**Projet :** FUENI MVP
**Client :** Nazounki
**Préparé par :** Namchhoen VENG — Direction de projet, Paris Partners
**Version :** 1.0 — Brouillon soumis à validation
**Date :** 2026-05-22
**Statut :** En attente de validation par Nazounki

**Destinataires :** Direction Nazounki, CTO, parties prenantes métier
**Référence détaillée (équipe technique) :** [`NAZOUNKI_FS_PATIENT_REGISTRATION.md`](./NAZOUNKI_FS_PATIENT_REGISTRATION.md)
**Référence Espace Patient (MVP) :** [`NAZOUNKI_FUENI_PATIENT_SPACE_V1.md`](../NAZOUNKI_FUENI_PATIENT_SPACE_V1.md)

---

## 1. Résumé

Cette spécification couvre **l'intégralité du cycle de vie du compte d'un patient sur FUENI**, de l'inscription initiale à la suppression éventuelle du compte. Elle inclut toutes les étapes intermédiaires : inscription (§4.1) avec téléphone et e-mail tous deux obligatoires, vérification du téléphone par SMS OTP à l'inscription + vérification e-mail par OTP différé (§4.2), complétion du profil de base obligatoire avant accès complet à la plateforme (§4.3), activation et modal de bienvenue (§4.4), complétion ultérieure du profil (§4.5), responsabilité du praticien sur la vérification d'identité en consultation (§4.6), modification des informations personnelles (§4.7), gestion des profils dépendants — post-MVP (§4.8), sécurité du compte avec 2FA (§4.9), et politique de suppression conforme au RGPD (§4.10).

Le document décrit le **comportement complet de la fonctionnalité** indépendamment du planning de livraison. Les éléments explicitement reportés à une phase ultérieure sont signalés par la mention « post-MVP » dans les sections concernées.

Cette approche garantit que toutes les décisions produit sont prises de manière cohérente dès le départ, évitant la fragmentation de la spécification entre plusieurs documents.

---

## 2. Public cible et valeur métier

| Aspect | Détail |
|---|---|
| **Utilisateur final** | Toute personne ≥ 18 ans résidant dans l'un des 9 pays cibles MVP (UEMOA + Cameroun + RDC) souhaitant gérer sa santé en ligne |
| **Profil typique** | Adulte équipé d'un smartphone et d'une adresse e-mail, souhaitant prendre RDV, conserver ses documents médicaux (ordonnances, comptes-rendus) et communiquer avec ses médecins de manière digitale |
| **Valeur pour FUENI / Nazounki** | Acquisition B2C massive : annuaire de praticiens exposé publiquement, signup ouvert, friction minimale pour maximiser la conversion. Le patient nourrit la valeur d'usage de la plateforme côté médecins. |
| **Valeur pour le patient** | Inscription en moins de 2 minutes (sans paiement, sans validation manuelle), accès immédiat à l'annuaire des médecins, conservation sécurisée des ordonnances et résultats d'analyses, prise de RDV en quelques clics |

---

## 3. Cycle de vie complet du compte patient

```
                          ┌──────────────────────────────┐
                          │   Visite du site public       │
                          │   (annuaire, page d'accueil)  │
                          └──────────────┬───────────────┘
                                         │
                                         ▼
                          ┌──────────────────────────────┐
                          │   §4.1 INSCRIPTION            │
                          │   Identité + Téléphone        │
                          │   + Email + Mot de passe      │
                          │   + Consentements             │
                          └──────────────┬───────────────┘
                                         │
                                         ▼
                          ┌──────────────────────────────┐
                          │   §4.2 VÉRIFICATION SMS OTP   │
                          │   Téléphone uniquement        │
                          │   6 chiffres, 5 min           │
                          │   E-mail → OTP différé        │
                          └──────────────┬───────────────┘
                                         │
                                         ▼
                          ┌──────────────────────────────┐
                          │   §4.3 PROFIL DE BASE         │
                          │   Date de naissance, Sexe,    │
                          │   Pays, Ville, Langue         │
                          │   (Adresse optionnel)         │
                          └──────────────┬───────────────┘
                                         │
                                         ▼
                          ┌──────────────────────────────┐
                          │   §4.4 ACTIVATION             │
                          │   Compte ACTIVE               │
                          │   Modal de bienvenue          │
                          └──────────────┬───────────────┘
                                         │
                                         ▼
                          ┌──────────────────────────────┐
                          │   UTILISATION QUOTIDIENNE     │
                          │   - Recherche de médecins     │
                          │   - Gestion des documents     │
                          │   - Consultation profil       │
                          └──────────────┬───────────────┘
                                         │
                ┌────────────────────────┼───────────────────────┐
                │                        │                       │
                ▼                        ▼                       ▼
   ┌─────────────────────┐  ┌──────────────────────┐  ┌─────────────────────┐
   │ §4.5 Complétion      │  │ §4.6 Vérification    │  │ §4.7 Modification    │
   │ ultérieure du        │  │ d'identité           │  │ du profil            │
   │ profil               │  │ = responsabilité     │  │ (3 niveaux selon     │
   │                      │  │ du praticien         │  │ sensibilité)         │
   └─────────────────────┘  └──────────────────────┘  └─────────────────────┘
                                         │
                                         ▼
                          ┌──────────────────────────────┐
                          │   §4.8 Profils dépendants     │
                          │   (post-MVP — enfants mineurs, │
                          │   parents sous tutelle)       │
                          └──────────────┬───────────────┘
                                         │
                                         ▼
                          ┌──────────────────────────────┐
                          │   §4.9 Sécurité du compte     │
                          │   - 2FA optionnelle           │
                          │   - Réinitialisation MDP      │
                          └──────────────┬───────────────┘
                                         │
                                         ▼
                          ┌──────────────────────────────┐
                          │   §4.10 Suppression de compte │
                          │   (volontaire, 2 étapes,      │
                          │   pseudonymisation à J+30,    │
                          │   rétention dossier 20 ans)   │
                          └──────────────────────────────┘
```

---

## 4. Détail par étape

### 4.1 Inscription

Le patient accède au formulaire d'inscription depuis le site public. L'inscription est libre, ouverte au public et entièrement self-service — aucune invitation préalable n'est requise.

#### 4.1.1 Points d'entrée

Le patient peut atteindre la page d'inscription par plusieurs canaux :

| Source | Comportement |
|---|---|
| **Barre de navigation supérieure — bouton « S'inscrire ▾ » → choix « Je suis patient »** | Redirection directe vers `/inscription`. Le choix « Je suis professionnel de santé » dans le même menu mène à `/inscription-medecin`. |

**Note** : les boutons « Espace patient » et « Espace professionnel » présents en barre supérieure du prototype sont des **points d'entrée de connexion** (déjà inscrit) et **non d'inscription**. Ils renvoient vers `/login` avec l'audience pré-sélectionnée.

#### 4.1.2 Structure de la page d'inscription

La page d'inscription est volontairement **sobre et concentrée** sur l'action. Conformément au prototype validé (`13-patient-signup.html`), la structure est la suivante :

- **En-tête de page** : logo FUENI (lien retour accueil) au centre, titre de page « Créer mon compte ».
- **Carte centrale (formulaire)** : un seul bloc visuel rassemblant tous les champs d'inscription (cf. §4.1.4), l'indicateur de force du mot de passe en temps réel avec liste à puces des règles cochées au fur et à mesure (cf. §4.1.4 ligne 7b), les cases de consentement (cf. §4.1.5), et le **bouton principal de soumission « → Créer mon compte »**.
- **Séparateur « ou » sous le bouton**, puis lien de bascule : *« Déjà un compte ? **Connectez-vous** »* (vers `/login`).
- **Pied de carte — lien d'inscription pro** : *« 👤 Vous êtes un professionnel de santé ? **Inscription pro** »* (vers `/inscription-medecin`). Permet à un visiteur arrivé sur la mauvaise page d'inscription de basculer sur l'autre audience.
- **Pas de menu de navigation latéral** : aucune distraction susceptible de faire abandonner l'inscription en cours.
- **Pied de page minimal** : mention de copyright + liens CGU / Politique de confidentialité / Contact (alignés sur le pied de page du site public).

Le **mot « S'inscrire »** est réservé au bouton de la barre de navigation supérieure (point d'entrée), tandis que le **bouton de soumission du formulaire** utilise *« Créer mon compte »* — formulation plus engageante et plus claire sur l'action terminale.

#### 4.1.3 Canaux d'inscription — Téléphone et e-mail tous deux obligatoires

Conformément aux standards de l'industrie médicale numérique (cf. plateformes de référence en Europe et au-delà) et au prototype validé, FUENI exige les **deux canaux** lors de l'inscription patient :

| Canal | Rôle |
|---|---|
| **Numéro de téléphone** | Vérifié par SMS OTP à l'inscription. Sert aux notifications urgentes (rappels RDV, alertes médicales), à la 2FA optionnelle, et à la récupération d'accès. |
| **Adresse e-mail** | Vérifiée par **OTP différé** (cf. §4.2.2), déclenché par le patient lui-même ou avant une action sensible (1ère prise de RDV, reset mot de passe). Non bloquante à l'inscription. Sert à l'identifiant de connexion, à la réception des documents médicaux (ordonnances, comptes-rendus), aux factures, et à la récupération de mot de passe. |
| **Mot de passe** | Obligatoire. Sert à la connexion (e-mail + mot de passe) avec 2FA optionnelle par SMS. |

**Pourquoi exiger les deux ?**

- **Stabilité de l'identifiant** : un numéro de téléphone peut changer (changement d'opérateur, perte du téléphone), une adresse e-mail est plus stable dans le temps
- **Canaux complémentaires** : le téléphone pour l'urgent (SMS court), l'e-mail pour le formel (documents, factures)
- **Récupération mutuelle** : en cas de perte d'accès à un canal, l'autre prend le relais (procédure de récupération via support plus simple)
- **Standard SaaS B2C santé** : cohérent avec les pratiques en vigueur dans l'industrie

#### 4.1.4 Champs collectés à l'inscription

L'inscription est volontairement **minimale** — seules les informations indispensables à la création du compte sont demandées. Les informations personnelles complémentaires (date de naissance, sexe, pays, ville, langue) sont collectées **juste après la vérification OTP** lors du Profil de base (cf. §4.3).

| # | Champ | Format / placeholder | Validation | Messages d'erreur |
|---|---|---|---|---|
| 1 | **Prénom** | Texte libre, placeholder *« Ex : Aïssatou »* | 1 à 80 caractères, lettres + tirets + apostrophes + espaces uniquement | « Le prénom est requis. » / « Caractères non autorisés. » |
| 2 | **Nom** | Texte libre, placeholder *« Ex : Diop »* | 1 à 80 caractères, mêmes règles que prénom | « Le nom est requis. » / « Caractères non autorisés. » |
| 3 | **Adresse e-mail** | Champ e-mail, placeholder *« Ex : aissatou.diop@example.com »* | Format RFC valide, pas de blocklist (Gmail, Yahoo, etc. acceptés) | « Adresse e-mail invalide. » / « Un compte existe déjà avec cet e-mail. Connectez-vous. » |
| 4 | **Confirmation de l'adresse e-mail** | Champ e-mail, placeholder *« Confirmez votre e-mail »* | Doit être identique au champ 3 | « Les adresses e-mail ne correspondent pas. » |
| 5a | **Indicatif pays (téléphone)** | Sélecteur drapeau + code (ex. 🇸🇳 +221), auto-détecté par géolocalisation IP, défaut Sénégal +221 si détection impossible | E.164 | — |
| 5b | **Numéro de téléphone** | Champ numérique uniquement, placeholder *« Ex : 77 123 45 67 »* (espaces décoratifs supprimés à l'envoi) | Format E.164 valide pour le pays sélectionné (libphonenumber) | « Numéro invalide pour le pays sélectionné. » / « Un compte existe déjà avec ce numéro. Connectez-vous. » |
| 6a | **Mot de passe** | Champ password avec bouton afficher/masquer (œil), placeholder *« •••••••••••• »* | 12 à 128 caractères, ≥ 1 majuscule + ≥ 1 minuscule + ≥ 1 chiffre + ≥ 1 caractère spécial, pas d'espace. Vérification HIBP (k-anonymity). | « Le mot de passe ne respecte pas les règles. » / « Ce mot de passe a été compromis dans une fuite de données. Choisissez-en un autre. » |
| 6b | **Indicateur de force du mot de passe** | Barre visuelle en temps réel : 🔴 Faible · 🟡 Moyen · 🟢 Fort · 💚 Très fort + liste à puces des règles à respecter (chaque règle cochée au fur et à mesure) | — | — |
| 6c | **Confirmation du mot de passe** | Champ password, placeholder *« Confirmez votre mot de passe »* | Doit être identique au champ 6a | « Les mots de passe ne correspondent pas. » |

**Total inscription : 6 champs principaux** (avec confirmations e-mail et mot de passe = 8 saisies au total) + consentements. Inscription en ~ 2 minutes.

#### 4.1.5 Mentions légales et consentements

Sous les champs, deux cases à cocher obligatoires :

```
[ ] J'accepte les Conditions Générales d'Utilisation (CGU)
    [lien CGU — ouvre dans un nouvel onglet]

[ ] J'accepte le traitement de mes données personnelles et médicales
    conformément à la Politique de confidentialité et au RGPD.
    [lien Politique de confidentialité — ouvre dans un nouvel onglet]
```

- Les **deux cases sont obligatoires** — un encadré rouge apparaît si elles ne sont pas cochées à la soumission
- Les liens vers les documents juridiques s'ouvrent dans un nouvel onglet pour ne pas casser la saisie en cours
- L'inscription à des communications marketing (newsletter, offres) est **hors périmètre** du formulaire d'inscription. Si Nazounki souhaite la proposer, ce sera via une **opt-in séparée** dans les paramètres du compte après inscription (cohérent avec les bonnes pratiques RGPD : consentement granulaire et révocable).

**Note** : les textes finaux des CGU et de la Politique de confidentialité sont en cours de rédaction par le cabinet juridique de Nazounki (cf. décision D1). Les versions actuelles affichées sont des brouillons (v0.1 — « EN COURS »).

#### 4.1.6 Protection anti-robot

Un **CAPTCHA invisible** (Cloudflare Turnstile) est intégré au formulaire :

- Aucune action n'est demandée au patient dans 95 % des cas — la vérification est silencieuse
- En cas de comportement suspect détecté (ex. inscriptions répétées depuis la même IP), un défi visuel (case « Je ne suis pas un robot ») peut apparaître
- Si la vérification échoue trois fois consécutivement, un message *« Vérification de sécurité échouée. Veuillez réessayer plus tard. »* apparaît et le formulaire est temporairement bloqué pour cette adresse IP (15 minutes)

#### 4.1.7 Bouton « → Créer mon compte » — règles d'activation

Le bouton de soumission **« → Créer mon compte »** est **désactivé visuellement (grisé)** tant que **toutes** les conditions suivantes ne sont pas réunies :

- Tous les champs requis sont remplis et passent la validation
- Les cases de consentement obligatoires (cf. §4.1.5) sont cochées
- Le CAPTCHA anti-robot a été validé

Dès que toutes ces conditions sont remplies, le bouton passe à la couleur principale FUENI et devient cliquable. Au clic :

- Animation de chargement (spinner) sur le bouton
- Désactivation des champs pendant l'envoi pour éviter une double soumission
- En cas d'erreur serveur (cas rare), réactivation du formulaire avec message d'erreur globale

#### 4.1.8 Issue de la soumission

- ✅ **Soumission validée** (champs OK + confirmations correspondantes + consentements cochés + Turnstile passé + téléphone et e-mail non déjà utilisés) → un **code SMS OTP** est envoyé sur le numéro de téléphone renseigné → redirection vers l'**écran de vérification SMS OTP** (§4.2.1). L'adresse e-mail est conservée comme `email_verified = false` et sera vérifiée plus tard via un OTP différé (§4.2.2).
- ❌ **Validation échouée** → encadré d'erreur global en haut du formulaire + erreurs in-line au niveau des champs concernés
- 🔒 **Téléphone déjà utilisé** → erreur ciblée *« Un compte existe déjà avec ce numéro. »* + lien direct vers la page de connexion
- 🔒 **E-mail déjà utilisé** → erreur ciblée *« Un compte existe déjà avec cet e-mail. »* + lien direct vers la page de connexion

---

### 4.2 Vérification des canaux d'inscription

À l'inscription, **seul le téléphone est vérifié par SMS OTP** (gate bloquant). L'e-mail est vérifié **plus tard** par un OTP différé, déclenché par le patient lui-même ou à l'occasion d'une action sensible (typiquement la première prise de rendez-vous).

**Pourquoi cette logique en deux temps ?**

- ✅ **Friction minimale à l'inscription** : un seul gate de vérification, le SMS qui arrive en quelques secondes
- ✅ **Le téléphone est la preuve d'identité primaire** : c'est aussi le canal des notifications urgentes (alertes RDV, 2FA)
- ✅ **L'e-mail n'est pas requis immédiatement** : il sert à la livraison de documents et à la récupération de mot de passe, deux usages qui n'arrivent pas le jour de l'inscription
- ✅ **Aligné sur le standard de l'industrie médicale numérique** B2C

---

#### 4.2.1 Vérification du téléphone — SMS OTP à l'inscription (bloquant)

Après une soumission valide du formulaire d'inscription (§4.1), le patient est dirigé vers un **écran dédié de vérification SMS OTP**. Cette vérification est bloquante : le patient ne peut pas accéder au reste du parcours d'inscription tant qu'elle n'est pas validée.

##### Aperçu de l'écran

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   Vérifiez votre numéro de téléphone                    │
│                                                         │
│   Nous avons envoyé un code à 6 chiffres à              │
│   +XXX *** *** 412                                      │
│   Il expire dans 5 minutes.                             │
│                                                         │
│   ┌───┐┌───┐┌───┐┌───┐┌───┐┌───┐                          │
│   │   ││   ││   ││   ││   ││   │                          │
│   └───┘└───┘└───┘└───┘└───┘└───┘                          │
│                                                         │
│   Renvoyer le code dans 0:59                            │
│                                                         │
│   ← Changer mon numéro                                  │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

##### Caractéristiques du code OTP

| Élément | Détail |
|---|---|
| Format | 6 chiffres numériques |
| Canal | SMS via Africa's Talking |
| Validité | 5 minutes |
| Tentatives | 3 maximum |
| Renvoi | Possible après 60 secondes, max 5 renvois par heure par numéro |

##### Comportements clés de l'écran

- **Numéro masqué partiellement** : seuls les 3 derniers chiffres du téléphone sont visibles
- **6 cases individuelles** : focus automatique sur la première case, passage automatique à la case suivante après saisie, soumission automatique dès que la 6e case est remplie
- **Clavier numérique sur mobile** (input type=number)
- **Bouton « Renvoyer le code »** : désactivé pendant 60 secondes (compte à rebours visible), puis activable. Limite : 5 renvois par heure.
- **Lien « Changer mon numéro »** : retour à la page d'inscription avec les autres champs préservés et seul le numéro à corriger.
- **Animation de validation** : à la saisie correcte du 6e chiffre, indicateur de succès puis transition fluide vers le Profil de base (§4.3).

##### Issues possibles

- ✅ **Code correct** → numéro de téléphone marqué `phone_verified = true` → progression vers **§4.3 Profil de base**
- ❌ **Code incorrect** → message *« Code incorrect. Il vous reste {n} tentatives. »* avec compteur dégressif
- ❌ **3 tentatives incorrectes** → message *« Trop d'essais incorrects. Veuillez recommencer l'inscription. »* + redirection vers `/inscription` (les données saisies à l'étape 4.1 sont perdues, le patient recommence)
- ⏱ **Code expiré (> 5 minutes)** → message *« Votre code a expiré. »* + bouton « Renvoyer le code » immédiatement actif (sans cooldown)
- 🚫 **Trop de renvois (> 5 / heure)** → message *« Vous avez demandé trop de renvois. Réessayez dans une heure. »*

---

#### 4.2.2 Vérification de l'e-mail — OTP différé (non bloquant à l'inscription)

L'adresse e-mail renseignée à l'inscription est conservée dans le compte mais **flaggée comme non vérifiée** (`email_verified = false`). Aucune vérification e-mail n'est demandée pendant le parcours d'inscription. Le patient peut finaliser son inscription et accéder à son tableau de bord avec un e-mail non vérifié.

##### Quand la vérification e-mail est-elle déclenchée ?

L'OTP différé est déclenché dans l'un des cas suivants :

1. **Modal de bienvenue post-inscription** : à la première connexion suivant l'inscription, la modal de bienvenue (cf. §4.4) propose en première position la carte **« Vérifier mon adresse e-mail »** avec un badge [Recommandé]. Le clic sur cette carte lance immédiatement le flux OTP différé. C'est le déclencheur le plus naturel et le plus encouragé.
2. **Action volontaire du patient depuis son profil** : bouton « Vérifier mon e-mail » dans la section Paramètres → Compte, accessible à tout moment.
3. **Soft-gate avant une action sensible** : la première tentative de prise de rendez-vous (le RDV nécessite un canal e-mail vérifié pour l'envoi de la confirmation et des documents médicaux). Un pop-up apparaît : *« Pour finaliser ce RDV, veuillez vérifier votre adresse e-mail. »* avec un bouton qui lance le flux OTP.
4. **Action critique de récupération** : tentative de réinitialisation du mot de passe (un e-mail non vérifié bloque la procédure standard de reset MDP — cf. §4.9.2).

Le déclencheur #1 (modal de bienvenue) est le **chemin nominal recommandé** : tant que le patient suit le parcours d'inscription standard, il est invité à vérifier son e-mail tout de suite après son arrivée sur le tableau de bord, sans avoir à attendre un soft-gate ou à chercher dans les paramètres.

##### Aperçu de l'écran OTP différé

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   Vérifiez votre adresse e-mail                         │
│                                                         │
│   Nous avons envoyé un code à 6 chiffres à              │
│   a***@example.com                                      │
│   Il expire dans 10 minutes.                            │
│                                                         │
│   ┌───┐┌───┐┌───┐┌───┐┌───┐┌───┐                          │
│   │   ││   ││   ││   ││   ││   │                          │
│   └───┘└───┘└───┘└───┘└───┘└───┘                          │
│                                                         │
│   Renvoyer le code dans 1:59                            │
│                                                         │
│   ℹ Pensez à vérifier vos courriers indésirables.       │
│                                                         │
│   ← Plus tard                                            │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

##### Caractéristiques du code OTP différé

| Élément | Détail |
|---|---|
| Format | 6 chiffres numériques |
| Canal | E-mail via Brevo (template `OTP_PATIENT_EMAIL_VERIFICATION_FR` / `..._EN`) |
| Validité | 10 minutes *(plus long que le SMS car l'e-mail peut prendre quelques secondes à arriver ou être filtré en spam)* |
| Tentatives | 3 maximum |
| Renvoi | Possible après 120 secondes, max 5 renvois par heure par adresse |

##### Bannière persistante avant vérification

Tant que `email_verified = false`, **une bannière persistante** apparaît en haut du tableau de bord du patient :

> ⚠️ *Votre adresse e-mail n'est pas encore vérifiée. Vérifiez-la pour pouvoir prendre des rendez-vous et recevoir vos documents médicaux.* **[ Vérifier maintenant ]**

La bannière est non-dismissible mais non bloquante : le patient peut continuer à utiliser la plateforme (recherche médecin, complétion profil, import documents personnels) avec un e-mail non vérifié.

##### Issues possibles de l'OTP différé

- ✅ **Code correct** → e-mail marqué `email_verified = true` → la bannière persistante disparaît → progression de l'action en cours (ex. RDV, reset MDP)
- ❌ **Code incorrect** → message *« Code incorrect. Il vous reste {n} tentatives. »*
- ❌ **3 tentatives incorrectes** → message *« Trop d'essais incorrects. Réessayez plus tard. »* — le patient revient au tableau de bord sans modification, l'e-mail reste `unverified`
- ⏱ **Code expiré** → message + bouton « Renvoyer le code »
- 🚫 **Trop de renvois (> 5 / heure)** → message *« Vous avez demandé trop de renvois. Réessayez dans une heure. »*
- ← **Plus tard** → le patient ferme l'écran, retour à la page précédente (sauf si soft-gate avant RDV, dans ce cas retour à la liste des praticiens sans avoir pris le RDV)

##### Changement d'adresse e-mail avant vérification

Le patient peut modifier son adresse e-mail depuis son profil avant de la vérifier. Cela invalide tout OTP précédemment envoyé et impose un nouveau cycle de vérification.

---

### 4.3 Profil de base *(post-OTP, avant accès au tableau de bord)*

Une fois la vérification OTP réussie, le compte du patient est techniquement créé, mais le patient doit **compléter quelques informations essentielles avant d'accéder pleinement à son espace**. Cette étape est conçue pour rester légère (~ 1 minute) tout en collectant les données nécessaires pour :

- Vérifier l'éligibilité (âge ≥ 18 ans)
- Personnaliser l'expérience (langue, fuseau, format des dates)
- Préparer la prise de rendez-vous (pays + ville pour la recherche locale)

#### 4.3.1 Aperçu de l'écran (référence prototype `14-patient-verification.html` Étape 2)

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   Complétez votre profil                                    │
│                                                             │
│   Informations nécessaires pour prendre votre premier RDV.  │
│   Vous pourrez enrichir votre profil plus tard.             │
│                                                             │
│   ℹ️  Ces informations resteront privées et ne seront       │
│      partagées qu'avec les praticiens que vous autorisez.   │
│                                                             │
│   Date de naissance *      Sexe *                           │
│   [ JJ / MM / AAAA  ]      [ Choisir...           ▾ ]        │
│                                                             │
│   Pays de résidence *      Ville *                          │
│   [ 🇸🇳 Sénégal       ▾ ]   [ Dakar              ▾ ]        │
│                                                             │
│   Langue de communication *                                 │
│   [ Français                                       ▾ ]      │
│   ℹ️  Communications, notifications et interfaces dans       │
│      cette langue                                           │
│                                                             │
│   Adresse  (optionnel à ce stade)                           │
│   [ Ex : Sacré-Cœur 3, Villa 12, face à la pharmacie...  ]   │
│                                                             │
│   [ Finaliser et accéder à mon espace  ✓ ]                   │
│   [ Passer pour le moment (accès limité) ]                   │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

#### 4.3.2 Champs collectés

| # | Champ | Format | Obligatoire | Validation |
|---|---|---|---|---|
| 1 | **Date de naissance** | Calendrier déroulant, format JJ/MM/AAAA (FR) ou MM/DD/YYYY (EN) | ✅ | ≥ 18 ans, ≤ 120 ans |
| 2 | **Sexe** | Liste déroulante : `Féminin` · `Masculin` · `Autre` · `Préfère ne pas répondre` | ✅ | Une option sélectionnée |
| 3 | **Pays de résidence** | Liste déroulante (9 pays MVP en tête, avec drapeau) | ✅ | Une option sélectionnée |
| 4 | **Ville** | Liste déroulante en cascade depuis le pays sélectionné (principales villes du pays + option « Autre » pour saisie libre) | ✅ | Une option sélectionnée |
| 5 | **Langue de communication** | Liste déroulante : `Français` · `English` | ✅ | Une option sélectionnée (défaut : langue détectée par le navigateur) |
| 6 | **Adresse** | Champ texte libre (zone de texte 2 lignes) — *« Ex : Sacré-Cœur 3, Villa 12, face à la pharmacie El Hadji »* | ❌ Optionnel | Aucune validation stricte |

#### 4.3.3 Encadré de réassurance vie privée

Un bandeau d'information explicite est affiché en haut du formulaire :

> ℹ️ *Ces informations resteront privées et ne seront partagées qu'avec les praticiens que vous autorisez explicitement.*

Cet encadré est essentiel pour rassurer le patient sur la confidentialité de ses données médicales personnelles (sexe, date de naissance) et conforme aux exigences de transparence du RGPD.

#### 4.3.4 Issue du Profil de base

Deux actions possibles pour le patient :

##### Option A — Finaliser et accéder à mon espace (recommandée)

- Tous les champs obligatoires sont remplis et validés
- Le patient clique sur **« Finaliser et accéder à mon espace »**
- Le profil est sauvegardé, le compte passe à `account_status = ACTIVE_COMPLETE`
- Redirection vers le tableau de bord avec la modal de bienvenue (§4.4)

##### Option B — Passer pour le moment *(accès limité)*

- Le patient clique sur le lien discret **« Passer pour le moment (accès limité) »** en dessous du bouton principal
- Une confirmation discrète apparaît : *« Sans ces informations, vous ne pourrez pas prendre de rendez-vous. Vous pouvez les ajouter à tout moment depuis votre profil. »*
- Si le patient confirme, le compte passe à `account_status = ACTIVE_PROFILE_INCOMPLETE`
- Redirection vers le tableau de bord avec la modal de bienvenue (§4.4), mais une **bannière persistante** apparaît en haut du dashboard : *« ⚠️ Complétez votre profil de base pour prendre des rendez-vous. »* avec un bouton direct vers l'écran §4.3

**Définition concrète de l'« accès limité »** (`account_status = ACTIVE_PROFILE_INCOMPLETE`)

Tableau récapitulatif des fonctionnalités autorisées et bloquées dans ce statut, pour cadrer précisément ce que le patient peut faire avant d'avoir complété son profil de base :

| Fonctionnalité | Autorisée ? | Justification |
|---|---|---|
| Recherche dans l'annuaire des médecins | ✅ | Pas besoin de DOB/sexe/ville pour rechercher |
| Consultation des fiches praticien (lecture seule) | ✅ | Information publique |
| Vérification de son adresse e-mail (OTP différé, cf. §4.2.2) | ✅ | Action de complétion compte |
| Complétion du profil de base (§4.3 — retour ultérieur) | ✅ | C'est précisément l'invitation portée par la bannière |
| Complétion du profil progressif (§4.5 — coordonnées, contacts d'urgence, infos médicales) | ✅ | Pas de blocage technique |
| Ajout et consultation de ses **propres documents personnels** (ordonnances précédentes scannées par exemple) | ✅ | Documents personnels du patient, pas liés à un praticien FUENI |
| Modification des paramètres de notification | ✅ | Préférences utilisateur |
| Activation du 2FA SMS (§4.9.1) | ✅ | Sécurisation du compte |
| Modification de son profil (Niveau 1, cf. §4.7) | ✅ | Champs libres |
| **Prise d'un rendez-vous** | ❌ Bloquée | Le praticien a besoin de DOB et sexe pour le dossier patient, et de la ville pour le matching local. Soft-gate explicite avec redirection vers §4.3. |
| **Réception de documents médicaux émis par un praticien FUENI** | ❌ N/A | Aucune relation patient↔praticien établie sans RDV pris |
| **Téléconsultation** *(post-MVP)* | ❌ N/A | Suit la même logique que la prise de RDV |

**Comportement du soft-gate à la prise de RDV**

Si le patient tente de prendre un RDV en étant en `ACTIVE_PROFILE_INCOMPLETE`, un pop-up apparaît : *« Pour prendre un rendez-vous, complétez d'abord votre profil de base (1 minute). »* avec un bouton **« Compléter maintenant »** redirigeant vers §4.3. Le RDV envisagé est mémorisé (URL ou paramètre) et la prise de RDV reprend automatiquement après complétion.

##### Cas particulier — Âge < 18 ans

La vérification de l'âge applique une logique en **deux temps** pour éviter les suppressions accidentelles tout en respectant la conformité réglementaire (interdiction de conserver les données d'un mineur sans consentement parental).

**Temps 1 — Validation in-line à la saisie (protection contre les fautes de frappe)**

Dès que le patient quitte le champ « Date de naissance », l'âge est calculé côté navigateur. Si âge < 18 :

- Un message d'erreur in-line apparaît **sous le champ** : *« Vous devez avoir 18 ans ou plus pour vous inscrire. Vérifiez la date saisie. »*
- Le bouton **« Finaliser et accéder à mon espace »** est désactivé visuellement (grisé)
- Le bouton **« Passer pour le moment »** est également désactivé — on n'autorise pas le report d'un compte de mineur, ce serait contraire à la conformité

Le patient peut **corriger la date directement** sans aucune action destructive ; aucun compte n'est supprimé à ce stade. C'est la première barrière contre les fautes de frappe (ex. saisir 2010 au lieu de 1990).

**Temps 2 — Cleanup différé en cas d'abandon (protection RGPD)**

Si le patient quitte la page sans corriger la date ni compléter le Profil de base, le compte créé à l'issue du SMS OTP (§4.2.1) reste en statut **`PENDING_PROFILE_BASE`**. Un job de nettoyage automatique s'exécute :

- À **J+7** sans complétion du Profil de base : suppression automatique du compte (rollback complet Keycloak + base de données) + e-mail explicatif au patient lui invitant à recommencer l'inscription s'il le souhaite (avec une date de naissance correcte)

Cette logique remplace la suppression brutale à la soumission. Le patient garde toujours la main pour corriger.

##### Politique de cleanup différé sur les comptes inactifs

Pour respecter le principe RGPD de **limitation de la durée de conservation**, FUENI applique une politique de cleanup automatique au-delà du cas spécifique du mineur :

| Statut du compte | Déclencheur de cleanup | Délai | Action |
|---|---|---|---|
| `PENDING_PROFILE_BASE` *(SMS OTP validé mais Profil de base jamais finalisé ni explicitement reporté)* | Inactivité depuis la création | **J+7** | Suppression compte + e-mail explicatif. Réinscription possible plus tard. |
| `ACTIVE_PROFILE_INCOMPLETE` *(« Passer pour le moment » choisi)* | Aucune connexion depuis | **J+365** | E-mail d'avertissement à **J+335** (J-30 avant suppression). Suppression si aucune connexion à J+365. |
| `ACTIVE_COMPLETE` | Aucune connexion depuis | **J+365** | Idem : avertissement à J+335 puis suppression à J+365. |

**Important** : le choix volontaire **« Passer pour le moment »** (§4.3.4 Option B) **ne déclenche aucun cleanup à court terme** — le patient ayant fait un choix conscient, son compte reste actif aussi longtemps qu'il s'y connecte régulièrement. Seul l'abandon complet (aucune connexion pendant un an) déclenche la suppression long-terme.

#### 4.3.5 Pourquoi placer ces champs en Profil de base et non à l'inscription ?

Trois raisons :

1. **Réduire la friction d'inscription** : un formulaire à 2-4 champs convertit beaucoup mieux qu'un formulaire à 6+ champs
2. **Séparer création de compte et personnalisation** : la création de compte est faite avant la complétion du profil, ce qui permet de relancer l'utilisateur s'il abandonne en cours de route (« Bonjour Aïssatou, complétez votre profil pour prendre vos premiers RDV »)
3. **Cohérence avec le prototype validé** : le prototype patient (`13-patient-signup.html` + `14-patient-verification.html` Étape 2) suit déjà ce séquencement

---

### 4.4 Activation immédiate et modal de bienvenue

À la fin du Profil de base (§4.3), le patient est **automatiquement connecté** et arrive sur son tableau de bord personnel (« Mon espace »). Une **modal de bienvenue** apparaît par-dessus :

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│   ✅  Bienvenue sur FUENI, {Prénom} !                       │
│                                                             │
│   Votre compte est créé. Voici ce que vous pouvez           │
│   faire maintenant :                                        │
│                                                             │
│   ┌─────────────────────────────────────────────────┐       │
│   │  ✉️  Vérifier mon adresse e-mail   [Recommandé] │       │
│   │      Indispensable pour recevoir vos documents   │       │
│   │      médicaux et prendre des rendez-vous         │       │
│   └─────────────────────────────────────────────────┘       │
│                                                             │
│   ┌─────────────────────────────────────────────────┐       │
│   │  🔍  Trouver un médecin                          │       │
│   │      Recherchez par spécialité, nom ou ville     │       │
│   └─────────────────────────────────────────────────┘       │
│                                                             │
│   ┌─────────────────────────────────────────────────┐       │
│   │  👤  Compléter mon profil                        │       │
│   │      Ajoutez vos coordonnées et informations     │       │
│   │      médicales (groupe sanguin, allergies…)      │       │
│   └─────────────────────────────────────────────────┘       │
│                                                             │
│   ┌─────────────────────────────────────────────────┐       │
│   │  📄  Importer mes documents                      │       │
│   │      Conservez vos ordonnances et résultats      │       │
│   │      d'analyses en toute sécurité                │       │
│   └─────────────────────────────────────────────────┘       │
│                                                             │
│   [ Plus tard ]              [ Commencer →  ]                │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

**Logique de la carte « Vérifier mon adresse e-mail »**

À la 1ère connexion post-inscription, l'e-mail du patient est par construction non vérifié (`email_verified = false`) — il n'a pas encore eu l'opportunité de lancer le flux OTP différé. La carte **« Vérifier mon adresse e-mail »** est donc **systématiquement affichée** dans la modal, **en première position** et avec un badge **[Recommandé]** pour signaler que c'est l'action la plus utile à effectuer en premier.

Le clic sur la carte déclenche le flux OTP différé décrit en §4.2.2 (envoi de l'OTP e-mail, écran de saisie, validation). À la validation réussie, l'attribut `email_verified` passe à `true` et la bannière persistante du dashboard disparaît.

**Comportement**

- La modal apparaît **une seule fois**, à la première connexion post-inscription. Elle est rejetée définitivement après avoir cliqué sur l'une des cartes d'action ou sur « Plus tard ».
- 4 cartes sont affichées en première position : **Vérifier mon adresse e-mail [Recommandé]** · Trouver un médecin · Compléter mon profil · Importer mes documents.
- Chacune des cartes est cliquable et redirige vers le module correspondant.
- Le patient peut fermer la modal (« Plus tard ») et explorer librement le tableau de bord.
- Si le patient a choisi l'**Option B « Passer pour le moment »** à l'étape Profil de base (§4.3.4), la carte *« Compléter mon profil »* est **également marquée [Recommandé]** (deux cartes recommandées : vérifier e-mail + compléter profil), et la bannière persistante du dashboard reste visible jusqu'à complétion.

---

### 4.5 Complétion ultérieure du profil

Au-delà du Profil de base (§4.3) collecté juste après l'OTP, **certaines informations utiles ne sont pas demandées d'emblée** mais peuvent être complétées progressivement par le patient depuis son profil. Cela améliore la qualité du dossier médical sans bloquer l'accès initial à la plateforme.

**Informations complémentaires à ajouter (toutes optionnelles à ce stade)**

| Catégorie | Champs |
|---|---|
| **Coordonnées complémentaires** | Code postal, téléphone secondaire, e-mail secondaire (le champ Adresse renseigné optionnellement en §4.3 peut aussi être enrichi ici) |
| **Contact d'urgence** | Nom du contact, lien de parenté, téléphone d'urgence |
| **Informations médicales de base** | Groupe sanguin (A+/A-/B+/B-/AB+/AB-/O+/O-/Inconnu), allergies connues, traitements en cours, antécédents médicaux |
| **Préférences** | Médecin traitant déclaré (référence à un praticien FUENI — post-MVP), préférences de notification |
| **Photo de profil** | Image optionnelle, stockée chiffrée |

**Pourquoi optionnels ?** Le patient remplit selon ses besoins. Plus son profil est complet, plus la qualité de ses futures consultations est élevée (le médecin disposera des antécédents). Mais aucun de ces champs ne bloque l'usage de la plateforme.

**Encouragements doux** : un indicateur visuel de complétion du profil (« Profil complété à 65 % ») incite le patient à enrichir progressivement, sans culpabilisation.

---

### 4.6 Vérification d'identité — responsabilité du praticien

**Principe — alignement avec le standard de l'industrie médicale numérique**

FUENI **ne procède à aucune vérification renforcée d'identité plateforme pour les patients en MVP** (pas de scan de carte d'identité, pas de selfie biométrique, pas de validation manuelle Super Admin). Le patient est uniquement vérifié par le code OTP envoyé sur son canal d'inscription (preuve qu'il possède bien le téléphone ou l'e-mail).

Ce choix est aligné sur les pratiques en vigueur dans l'industrie médicale numérique grand public : les plateformes de prise de RDV B2C ne réalisent pas elles-mêmes de KYC patient — la vérification d'identité reste la **responsabilité du praticien lors de la consultation** (qu'elle soit en cabinet ou en téléconsultation).

**Pourquoi ce choix ?**

- ✅ **Friction minimale** : un scan d'identité avant la 1ère RDV réduirait significativement la conversion (effet documenté à -40 à -60 % sur les benchmarks industriels)
- ✅ **Cadre déontologique respecté** : le médecin reste légalement responsable de vérifier l'identité du patient avant tout acte médical
- ✅ **Charge opérationnelle évitée** : pas d'équipe Super Admin dédiée à la validation manuelle de milliers de dossiers patients
- ✅ **Standards de l'industrie respectés** : les plateformes B2C santé reposent toutes sur cette logique de responsabilité partagée
- ✅ **Sécurité préservée** : OTP + protection anti-robot + audit log + politique de mots de passe robustes assurent un niveau de sécurité plateforme adapté au B2C santé

**Aide à la vérification praticien**

Pour faciliter la vérification par le médecin lors de la consultation, le patient est invité (sans contrainte) à enrichir son profil avec des informations qui aident la confrontation :

- **Nom de naissance** (si différent du nom d'usage)
- **Lieu de naissance**
- **Adresse complète**

Ces champs sont collectés progressivement via la complétion ultérieure du profil (§4.5) et apparaissent visuellement au médecin sur le dossier patient lors de la prise de RDV.

**Cas d'évolution future (post-MVP)**

Si certaines fonctionnalités sensibles sont introduites en post-MVP (téléconsultation à distance, prescription de médicaments contrôlés, certificats médicaux dématérialisés, intégration assurances), une vérification d'identité plateforme pourra alors être ajoutée **uniquement pour ces flux spécifiques** — pas pour la prise de RDV de base. Cela sera spécifié dans la SF dédiée à chaque fonctionnalité concernée.

---

### 4.7 Modification du profil

Après l'activation du compte, le patient peut modifier son profil. **Trois niveaux de droits** s'appliquent selon la sensibilité des informations :

#### Niveau 1 — Modification libre (sans confirmation)

Le patient édite ces champs à tout moment depuis ses paramètres, sans étape de vérification :

- Photo de profil
- Coordonnées (adresse, ville, code postal)
- Téléphone secondaire / e-mail secondaire
- Contact d'urgence
- Groupe sanguin, allergies, traitements en cours, antécédents médicaux
- Langue de communication
- Préférences de notification (e-mail / SMS, types de rappels)

#### Niveau 2 — Modification avec vérification par OTP

Pour les **canaux d'authentification** (qui servent à se connecter), le patient confirme le changement par un nouvel OTP envoyé sur le nouveau canal :

- Téléphone principal (canal Téléphone) — nouvel OTP SMS
- Adresse e-mail principale (canal E-mail) — nouvel OTP e-mail
- Mot de passe (canal E-mail) — ancien mot de passe demandé + saisie du nouveau

#### Niveau 3 — Modification avec validation Super Admin

Pour les informations qui constituent l'identité légale du patient, la modification requiert l'approbation manuelle d'un Super Admin (logique anti-fraude + audit) :

- Prénom, Nom
- Date de naissance
- Sexe
- Pays de résidence

Le patient soumet sa demande avec un justificatif (par ex. acte de mariage pour changement de nom, ou nouvelle CNI pour rectification). Super Admin examine et approuve ou rejette. Ces changements sont également notés dans le journal d'audit du compte.

---

### 4.8 Profils dépendants *(post-MVP)*

**Principe**

Un patient adulte peut gérer plusieurs **profils dépendants** rattachés à son compte :

- Ses **enfants mineurs** (< âge de consentement médical, variable par pays)
- Un **parent âgé** sous tutelle médicale
- Un **conjoint** sous tutelle

**Création d'un profil dépendant**

| Champ | Obligatoire |
|---|---|
| Prénom, Nom du dépendant | ✅ |
| Lien avec le tuteur (Enfant, Parent à charge, Conjoint sous tutelle, Autre) | ✅ |
| Date de naissance | ✅ |
| Sexe | ✅ |
| Justificatif (acte de naissance, jugement de tutelle…) | ❌ Optionnel en post-MVP, demandable à la demande par Super Admin |

**Expérience utilisateur**

- Sélecteur de profil en haut de l'application : *« Vous consultez le profil de : Moi / Mon enfant 1 / Ma mère »*
- Bascule fluide entre profils
- Notifications regroupées sur le compte adulte (un seul SMS / e-mail par RDV, quel que soit le profil concerné)
- Les RDV sont pris au nom du dépendant, payés depuis le compte adulte
- Les documents médicaux du dépendant sont stockés dans son profil dédié (compartimentation)

**MVP = compte adulte uniquement.** L'ajout de dépendants est entièrement reporté en post-MVP. Les modalités opérationnelles précises (transfert automatique enfant→majeur, tutelle partagée, multi-tuteurs avec délégation, etc.) feront l'objet d'une spécification dédiée au moment de l'implémentation de cette fonctionnalité.

---

### 4.9 Sécurité du compte

#### 4.9.1 Authentification à deux facteurs (2FA) — optionnelle

Le patient peut activer la **2FA par SMS** depuis ses paramètres → Sécurité. Comme tous les patients renseignent un téléphone à l'inscription, la 2FA est toujours disponible.

- Une fois activée, à chaque connexion, après la saisie de l'e-mail et du mot de passe, un code à 6 chiffres est envoyé par SMS au numéro enregistré. Le patient doit saisir ce code pour finaliser la connexion.
- La 2FA peut être désactivée à tout moment depuis les paramètres ; la désactivation requiert la saisie du mot de passe.
- En cas de perte d'accès au téléphone, le patient contacte le support qui propose une procédure de réinitialisation (vérification d'identité par e-mail).

**Recommandation FUENI** : activer la 2FA dès la première connexion pour protéger le compte, particulièrement important compte tenu de la sensibilité des données médicales stockées.

#### 4.9.2 Réinitialisation du mot de passe oublié

Le patient peut réinitialiser son mot de passe à tout moment :

1. Depuis la page de connexion → lien **« Mot de passe oublié »**
2. Saisie de l'adresse e-mail
3. Réception d'un **OTP à 6 chiffres** par e-mail (valable 10 minutes, 3 tentatives, renvoi possible après 120 secondes)
4. Saisie de l'OTP sur la page de réinitialisation
5. Saisie du nouveau mot de passe + confirmation
6. Connexion automatique

Le mot de passe doit respecter la même politique qu'à l'inscription : 12 à 128 caractères, contenant au minimum 1 majuscule + 1 minuscule + 1 chiffre + 1 caractère spécial.

Si l'e-mail du patient n'était pas encore vérifié (`email_verified = false`) au moment de la demande de reset, la validation réussie de l'OTP marque automatiquement l'e-mail comme vérifié (`email_verified = true`).

**Cas particulier : perte d'accès à l'e-mail** → contact du support qui propose une procédure de récupération avec vérification d'identité par SMS OTP sur le téléphone enregistré + question de sécurité (post-MVP).

#### 4.9.3 Gestion des sessions

En MVP, deux mécanismes garantissent la sécurité du compte sans interface dédiée à la gestion des sessions :

- **Invalidation automatique à la modification du mot de passe** : tout changement de mot de passe (via reset ou modification volontaire) déconnecte automatiquement le patient de tous ses appareils. Il doit alors se reconnecter avec le nouveau mot de passe.
- **Expiration de session par inactivité** : une session ouverte expire automatiquement après une période d'inactivité (durée à confirmer par l'équipe technique — typiquement 30 jours).

La **visualisation explicite des sessions actives et la révocation à distance d'un appareil suspect** sont reportées en post-MVP — la 2FA SMS optionnelle (§4.9.1) couvre l'essentiel des cas d'usage en MVP.

---

### 4.10 Suppression de compte *(post-MVP)*

#### Politique en deux étapes

Le patient peut demander la suppression de son compte depuis ses paramètres → *Sécurité → Supprimer mon compte*. Le processus se déroule en **deux étapes** pour éviter les suppressions accidentelles et respecter le RGPD tout en honorant les obligations légales de rétention des dossiers médicaux.

**Étape 1 — Demande de suppression (J0)**

- Connexion désactivée immédiatement
- Profil masqué de l'annuaire pour les médecins (le patient n'apparaît plus dans aucune recherche médicale)
- RDV à venir annulés et les praticiens concernés notifiés
- E-mail / SMS de confirmation au patient avec un lien de rétractation

**Étape 2 — Pseudonymisation (J+30, fin du délai de rétractation)**

Pendant les **30 jours** suivant la demande, le patient peut annuler sa suppression depuis le lien envoyé par e-mail / SMS. Passé ce délai, la suppression devient irréversible :

- **Données personnelles supprimées** : nom remplacé par « Patient anonyme {identifiant court} », photo supprimée, adresse / coordonnées supprimées, contact d'urgence supprimé
- **Compte d'authentification désactivé** mais conservé dans Keycloak pour l'audit (pas de réutilisation possible de l'e-mail / téléphone par un autre compte pendant 5 ans)
- **Dossiers médicaux conservés 20 ans** depuis la dernière consultation, avec référence pseudonymisée (le médecin conserve l'accès à son dossier praticien comme avant FUENI)
- **Historique des RDV conservé** sous forme agrégée (statistiques anonymes), sans identifiant patient

**Décision à valider** (cf. §9 D8) : confirmer la durée de rétention dossiers médicaux à 20 ans (cohérent avec la décision médecin, voir SF-DOCTOR-REGISTRATION).

#### Droit à l'export RGPD

À tout moment avant suppression, le patient peut **exporter ses données personnelles** depuis ses paramètres :

- Export JSON structuré contenant : profil, RDV passés, documents personnels, historique de notifications
- Reçu par e-mail ou SMS (lien sécurisé valable 7 jours)

Cette fonctionnalité respecte l'article 20 du RGPD (droit à la portabilité des données).

---

## 5. Règles métier consolidées

| # | Règle |
|---|---|
| R1 | Le patient doit avoir 18 ans ou plus. La vérification d'âge est effectuée à la saisie du champ Date de naissance dans le Profil de base (§4.3) : validation **in-line** bloquant les boutons « Finaliser » et « Passer pour le moment » tant que l'âge < 18. En cas d'abandon, le compte reste en `PENDING_PROFILE_BASE` et est supprimé automatiquement à **J+7** (cf. §4.3.4). L'inscription des mineurs avec consentement parental est reportée en post-MVP. |
| R2 | Le patient renseigne **téléphone ET e-mail** à l'inscription, ainsi qu'un mot de passe. À l'inscription, **seul le téléphone est vérifié par SMS OTP** (bloquant, 6 chiffres, 5 min). L'e-mail est vérifié plus tard par un **OTP différé** (6 chiffres, 10 min), déclenché soit par le patient depuis son profil, soit automatiquement avant une action sensible (1ère prise de RDV, reset mot de passe). |
| R3 | Les CGU et la Politique de confidentialité doivent être acceptées explicitement (cases obligatoires). |
| R4 | L'inscription est entièrement libre-service et publique — aucune invitation préalable n'est requise. |
| R5 | Un téléphone ou un e-mail déjà utilisé par un compte FUENI (patient ou médecin) ne peut pas être réutilisé. |
| R6 | Une protection anti-robot (Cloudflare Turnstile, invisible) est appliquée sur toutes les soumissions du formulaire d'inscription. |
| R7 | Le parcours d'inscription est structuré en **4 étapes successives** : (1) Inscription minimale §4.1 → (2) Vérification OTP §4.2 → (3) Profil de base §4.3 → (4) Activation et tableau de bord §4.4. Le compte est techniquement créé après l'étape 2 mais devient `ACTIVE_COMPLETE` uniquement après l'étape 3 (sinon `ACTIVE_PROFILE_INCOMPLETE` avec accès limité). |
| R7bis | Pas d'état « En attente de validation Super Admin » pour le patient, contrairement au médecin — la vérification d'identité plateforme n'est pas appliquée au patient (cf. R8). |
| R8 | **Pas de vérification renforcée d'identité plateforme pour les patients en MVP.** La vérification d'identité reste la **responsabilité du praticien lors de la consultation**, conformément aux standards de l'industrie médicale numérique B2C. Seul l'OTP (téléphone ou e-mail) constitue la vérification plateforme. |
| R9 | Le patient peut enrichir son profil avec **nom de naissance, lieu de naissance, adresse** pour faciliter la vérification d'identité par le praticien en cabinet — ces champs sont optionnels mais encouragés. |
| R10 | Les 9 pays cibles MVP (Sénégal, Côte d'Ivoire, Mali, Bénin, Togo, Burkina Faso, Niger, Cameroun, RDC) sont affichés en tête des sélecteurs de pays. |
| R11 | Modification du profil : 3 niveaux selon la sensibilité du champ (libre / OTP / Super Admin). |
| R12 | Politique du mot de passe : 12 à 128 caractères, au minimum 1 majuscule, 1 minuscule, 1 chiffre, 1 caractère spécial (tout caractère non alphanumérique imprimable, sauf l'espace). Vérification automatique contre les bases de mots de passe compromis (HIBP). |
| R13 | OTP : 6 chiffres, validité 5 minutes, 3 tentatives maximum, renvoi possible après 60 secondes, max 5 renvois par heure par identifiant. |
| R14 | 2FA par SMS optionnelle, activable depuis les paramètres, recommandée à la première connexion. |
| R15 | Profils dépendants (enfants mineurs, parents sous tutelle) : reportés en post-MVP. En MVP, un compte = un adulte. |
| R16 | Suppression du compte : 2 étapes, 30 jours de rétractation, puis pseudonymisation (post-MVP). |
| R17 | Dossiers médicaux conservés 20 ans depuis la dernière consultation, même après suppression du compte patient (références pseudonymisées). |
| R18 | Le patient peut exporter ses données personnelles à tout moment au format JSON (droit à la portabilité RGPD, article 20). |
| R19 | Aucune donnée médicale n'est partagée avec un médecin sans consentement explicite du patient (mécanisme à détailler dans la spec dédiée Partage de dossier — post-MVP). |
| R20 | Sessions actives visualisables par le patient depuis ses paramètres, avec déconnexion forcée à distance possible. |

---

## 6. Champs collectés à chaque étape

### 6.1 Inscription (§4.1) — formulaire minimal

| # | Champ | Obligatoire | Notes |
|---|---|---|---|
| 1 | Prénom | ✅ | 1 à 80 caractères |
| 2 | Nom | ✅ | 1 à 80 caractères |
| 3 | Adresse e-mail | ✅ | Format RFC valide, Gmail/Yahoo acceptés |
| 4 | Confirmation e-mail | ✅ | Doit correspondre au champ 3 |
| 5 | Téléphone (indicatif + numéro) | ✅ | E.164, indicatif auto-détecté par géolocalisation IP |
| 6 | Mot de passe | ✅ | 12 à 128 caractères, politique stricte (R12) |
| 7 | Confirmation mot de passe | ✅ | Doit correspondre au champ 6 |
| — | Acceptation CGU | ✅ | Case obligatoire |
| — | Acceptation Politique de confidentialité + traitement données médicales (RGPD) | ✅ | Case obligatoire |

→ **6 champs principaux + 2 confirmations** + consentements. Le téléphone est vérifié par **SMS OTP bloquant** à l'inscription (cf. §4.2.1). L'e-mail est vérifié par un **OTP différé** déclenché ultérieurement par le patient ou avant une action sensible (cf. §4.2.2).

### 6.2 Profil de base (§4.3) — post-OTP, avant accès au tableau de bord

| # | Champ | Obligatoire | Notes |
|---|---|---|---|
| 1 | Date de naissance | ✅ | ≥ 18 ans, ≤ 120 ans — < 18 ans déclenche une suppression de compte automatique |
| 2 | Sexe | ✅ | Féminin / Masculin / Autre / Préfère ne pas répondre |
| 3 | Pays de résidence | ✅ | Liste des 9 pays MVP en tête (drapeau + nom) |
| 4 | Ville | ✅ | Liste déroulante en cascade depuis le pays |
| 5 | Langue de communication | ✅ | Français / English (défaut : langue détectée par le navigateur) |
| 6 | Adresse | ❌ Optionnel | Champ texte libre — quartier, rue, point de repère |

→ **5 champs obligatoires + 1 optionnel** — formulaire conçu pour être complété en ~ 1 minute.

### 6.3 Complétion ultérieure du profil (§4.5) — post-activation, tous optionnels

Voir §4.5 pour le détail des champs (coordonnées complémentaires, contact d'urgence, informations médicales, préférences, photo de profil).

### 6.4 Aide à la vérification praticien (§4.6) — facultatif

Champs supplémentaires que le patient peut renseigner depuis son profil pour faciliter la vérification d'identité par le médecin en cabinet :

| # | Champ | Obligatoire |
|---|---|---|
| 1 | Nom de naissance (si différent du nom d'usage) | ❌ Optionnel |
| 2 | Lieu de naissance | ❌ Optionnel |
| 3 | Adresse complète (si pas déjà saisie en §4.3 ou §4.5) | ❌ Optionnel |

---

## 7. Messages utilisateur clés

### 7.1 Messages d'erreur courants (inscription)

| Situation | Message |
|---|---|
| Téléphone déjà utilisé | « Un compte existe déjà avec ce numéro. Connectez-vous. » |
| E-mail déjà utilisé | « Un compte existe déjà avec cet e-mail. Connectez-vous. » |
| Confirmation e-mail ne correspond pas | « Les adresses e-mail ne correspondent pas. » |
| Format de téléphone invalide | « Numéro invalide. Format attendu : +221771234567. » |
| Format d'e-mail invalide | « Adresse e-mail invalide. » |
| Mot de passe trop faible | « Le mot de passe ne respecte pas les règles de sécurité. » |
| Confirmation mot de passe ne correspond pas | « Les mots de passe ne correspondent pas. » |
| Mot de passe compromis | « Ce mot de passe a été compromis dans une fuite de données. Choisissez-en un autre. » |
| Âge < 18 ans *(validation in-line au champ Date de naissance, §4.3)* | « Vous devez avoir 18 ans ou plus pour vous inscrire. Vérifiez la date saisie. » |
| Cleanup automatique d'un compte `PENDING_PROFILE_BASE` à J+7 | E-mail au patient : « Bonjour, votre inscription FUENI commencée le {date} n'a pas été finalisée. Conformément à notre politique de confidentialité, votre compte temporaire a été supprimé. Vous pouvez recommencer une inscription à tout moment. » |
| Avertissement avant cleanup long-terme à J+335 | E-mail : « Votre compte FUENI n'a pas été utilisé depuis 11 mois. Sans connexion d'ici 30 jours, il sera supprimé conformément à notre politique RGPD. » |
| Profil de base ignoré (« Passer pour le moment ») | Bannière persistante du dashboard : « ⚠️ Complétez votre profil de base pour prendre des rendez-vous. » |
| Code OTP incorrect | « Code incorrect. Il vous reste {n} tentatives. » |
| Code OTP expiré | « Votre code a expiré. » + bouton « Renvoyer le code » |
| 3e tentative incorrecte | « Trop d'essais incorrects. Veuillez recommencer l'inscription. » |
| Vérification anti-bot échouée | « Vérification de sécurité échouée. Veuillez réessayer. » |
| CGU ou RGPD non accepté | « Vous devez accepter les CGU et la Politique de confidentialité pour vous inscrire. » |

### 7.2 Notifications post-inscription

| Situation | Canal | Message |
|---|---|---|
| Inscription réussie | SMS | « Bienvenue sur FUENI, {Prénom} ! Votre compte est créé. » |
| Envoi SMS OTP à l'inscription | SMS | « FUENI : votre code de vérification est {code}. Valide 5 minutes. » |
| Bannière persistante e-mail non vérifié (in-app) | Bannière dashboard | « ⚠ Votre adresse e-mail n'est pas encore vérifiée. Vérifiez-la pour pouvoir prendre des rendez-vous et recevoir vos documents médicaux. [Vérifier maintenant] » |
| Envoi OTP différé pour vérification e-mail | E-mail | « Bonjour {Prénom}, votre code de vérification FUENI est : {code}. Il expire dans 10 minutes. Si vous n'avez pas demandé ce code, ignorez ce message. » |
| Soft-gate avant 1ère RDV si e-mail non vérifié | Pop-up in-app | « Pour finaliser ce rendez-vous, veuillez vérifier votre adresse e-mail afin de recevoir la confirmation et les documents associés. [Vérifier maintenant] » |
| Confirmation e-mail vérifié (in-app) | Bandeau in-app | « ✅ Votre adresse e-mail est confirmée. » |
| Encouragement à compléter le profil pour faciliter la vérification praticien | E-mail (J+3 si profil incomplet) | « Bonjour {Prénom}, complétez votre profil avec votre nom de naissance et votre lieu de naissance pour que votre médecin puisse vérifier rapidement votre identité lors de votre prochaine consultation. » |
| Demande de suppression enregistrée | E-mail + SMS | « Votre demande de suppression a été enregistrée. Votre compte sera définitivement supprimé dans 30 jours. Cliquez ici pour annuler. » |
| Suppression effective | E-mail | « Votre compte FUENI a été définitivement supprimé. Conformément à la réglementation médicale, vos dossiers médicaux restent conservés par vos médecins traitants pour une durée de 20 ans. » |

### 7.3 Messages de la modal de bienvenue

Voir §4.4 pour le contenu complet.

---

## 8. Hors périmètre

Les éléments suivants ne sont **pas inclus** dans le périmètre du MVP — ils seront livrés post-MVP :

| Élément | Notes |
|---|---|
| **Flux de connexion récurrent** (login d'un patient déjà inscrit qui revient se connecter) | Couvert dans une SF dédiée à venir : **SF-LOGIN** — transverse aux 3 rôles (patient, médecin, super admin) |
| Inscription des mineurs avec consentement parental | post-MVP |
| Profils dépendants (enfants, parents sous tutelle) | post-MVP |
| Vérification d'identité plateforme par service KYC tiers (Smile Identity, Ondato…) ou manuelle Super Admin | Hors périmètre MVP — la vérification d'identité reste de la responsabilité du praticien en consultation (cf. §4.6). Pourra être introduite en post-MVP uniquement pour des flux sensibles spécifiques (téléconsultation, prescription contrôlée, certificats médicaux). |
| Connexion sociale (Google, Apple, Facebook) | post-MVP |
| Connexion alternative via numéro de téléphone + OTP SMS (vs e-mail + mot de passe par défaut) | post-MVP — le MVP n'expose que la connexion par e-mail + mot de passe, le téléphone reste utilisé pour la 2FA et les notifications |
| Suppression de compte (§4.10) | post-MVP — politique RGPD à finaliser avec cabinet juridique |
| Partage de dossier patient avec un médecin | post-MVP — spécification dédiée à rédiger |
| Notifications WhatsApp Business | post-MVP |
| Triage IA (orientation médicale automatique) | post-MVP (régulation à analyser) |
| Trouver un médicament / une ambulance / un hôpital | post-MVP |
| Acompte mobile money à la prise de RDV (anti no-show) | post-MVP, activable selon mesure du taux de no-show |
| Question de sécurité pour récupération d'accès (canal Téléphone, perte du téléphone) | post-MVP |
| Gestion explicite des sessions actives (visualisation des appareils connectés, révocation à distance) | post-MVP — couvert en MVP par : invalidation auto à changement de MDP + expiration par inactivité (cf. §4.9.3) |
| Référencement entre patients (parrainage) | post-MVP |

---

## 9. Décisions à valider par Nazounki

| # | Sujet | Statut |
|---|---|---|
| **D1** | Textes finaux CGU et Politique de confidentialité (cabinet juridique « EN COURS ») | À fournir |
| **D2** | Contenu détaillé des e-mails / SMS de bienvenue, de validation KYC, de rejet, de suppression | À co-rédiger avec l'équipe marketing Nazounki |
| **D3** | ~~Canal unique à l'inscription~~ — Décision actée : **téléphone ET e-mail tous deux obligatoires à l'inscription**. Vérification du téléphone par SMS OTP bloquant à l'inscription (§4.2.1) ; vérification de l'e-mail par **OTP différé** non bloquant à l'inscription, déclenché par le patient ou avant une action sensible (§4.2.2). Aligné prototype + standards de l'industrie. | ✅ Acté 2026-05-22 (révisé 2026-05-25) |
| **D4** | Position du formulaire d'inscription : page dédiée plein écran (recommandation) ou modal sur la page d'accueil | À confirmer — recommandation : page dédiée pour clarté et confiance |
| **D5** | ~~Choix par défaut entre Téléphone et E-mail~~ — Sans objet depuis la décision D3 (les deux canaux obligatoires) | ✅ Acté 2026-05-22 |
| **D6** | ~~Vérification renforcée d'identité~~ — Décision actée : **pas de vérification d'identité plateforme pour les patients en MVP**. Aligné sur le standard de l'industrie médicale numérique B2C. La vérification reste la responsabilité du praticien lors de la consultation. À réintroduire éventuellement en post-MVP pour des flux sensibles spécifiques (cf. §4.6) | ✅ Acté 2026-05-22 |
| **D7** | Confirmer la liste des 9 pays cibles MVP (UEMOA + Cameroun + RDC) — Tchad, RCA, Gabon, Congo Brazzaville reportés en post-MVP | ✅ Acté 2026-05-22 |
| **D8** | Confirmer la durée de rétention des dossiers médicaux à 20 ans après suppression du compte patient | À confirmer (cohérent avec la décision médecin SF-DOCTOR-REGISTRATION) |
| **D9** | Confirmer que les profils dépendants sont reportés en post-MVP (un compte = un adulte en MVP) | À confirmer — recommandation : oui, simplifie significativement le MVP |
| **D10** | Stratégie d'acquisition B2C : SEO seul, ads payantes, partenariats avec mutuelles / pharmacies ? | À cadrer avec l'équipe marketing Nazounki |
| **D11** | Activation de la 2FA par défaut ou optionnelle | À confirmer — recommandation : optionnelle (friction trop forte pour B2C), avec encouragement actif à l'activation post-inscription |

---

## 10. Indicateurs de succès

À mesurer après la mise en production :

| Indicateur | Cible MVP |
|---|---|
| Taux d'inscription terminée (formulaire rempli → OTP validé) | ≥ 80 % |
| Durée moyenne d'inscription | ≤ 2 minutes |
| Taux d'abandon à l'écran SMS OTP (§4.2.1) | < 8 % |
| Délai moyen de saisie du SMS OTP | < 60 secondes |
| Taux de vérification e-mail différée (§4.2.2) sous 7 jours après inscription | ≥ 60 % |
| Taux de vérification e-mail différée avant la 1ère tentative de prise de RDV | ≥ 90 % |
| Taux de complétion du profil sous 7 jours | ≥ 40 % |
| Taux de renseignement des champs « aide à la vérification praticien » (nom de naissance, lieu de naissance) | ≥ 30 % à 30 jours |
| Taux d'activation 2FA | ≥ 25 % |
| Taux de réutilisation après 30 jours | ≥ 50 % |

---

## 11. Acceptation et signature

| Rôle | Nom | Statut | Date |
|---|---|---|---|
| Direction Nazounki | [À renseigner] | En attente | — |
| Direction de projet (Paris Partners) | Namchhoen VENG | Auteur | 2026-05-22 |

Validation par **signature électronique** (DocuSign ou Yousign) selon la procédure de collaboration QA-Dev-PM §11.

---

## 12. Notes complémentaires

- **Couverture complète assumée** — ce document décrit la fonctionnalité **du début à la fin du cycle de vie** d'un compte patient. Les éléments explicitement reportés à une phase ultérieure sont signalés par la mention « post-MVP » dans les sections concernées (§4.8, §4.10, §8 Hors périmètre).
- **Différences clés vs SF-DOCTOR-REGISTRATION** : pas de paiement à l'inscription, pas d'état « En attente de validation KYC » bloquant, vérification d'identité = responsabilité du praticien (pas de KYC plateforme), vérification du téléphone seul bloquante à l'inscription (e-mail différé), profils dépendants spécifiques au patient.
- **Référence implémentation technique** — les contrats d'API, codes d'erreur backend, schéma de base de données, détails d'architecture (BFF, Keycloak, Redis, Turnstile, Brevo, Africa's Talking) sont documentés dans la spécification technique destinée à l'équipe de développement : [`NAZOUNKI_FS_PATIENT_REGISTRATION.md`](./NAZOUNKI_FS_PATIENT_REGISTRATION.md).
- **Référence Espace Patient** — la spécification consolidée du module patient (MVP : Documents + Mes RDV + Prendre RDV) est dans [`NAZOUNKI_FUENI_PATIENT_SPACE_V1.md`](../NAZOUNKI_FUENI_PATIENT_SPACE_V1.md).
- **Maquettes** — les écrans seront déclinés dans des maquettes Figma avant développement. Le présent document décrit les comportements et règles, pas le design visuel.
- **Conformité** — la fonctionnalité respecte les standards de sécurité industriels (mot de passe haché Argon2id, données chiffrées en transit et au repos, vérification anti-robot, audit log des inscriptions, gestion du RGPD compatible avec l'obligation de rétention 20 ans des dossiers médicaux).
- **Périmètre 9 pays MVP** — les 4 pays CEMAC orphelins (Tchad, RCA, Gabon, Congo Brazzaville) sont reportés en post-MVP conformément à la décision client du 2026-05-22 (cf. e-mail `NAZOUNKI_EMAIL_PROVIDERS_COVERAGE`).
