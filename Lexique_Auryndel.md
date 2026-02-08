# Lexique Auryndel - Guide d'onboarding

## 📘 Général

### Concepts fondamentaux

**Auryndel**
Plateforme web interactive simulant une société utopique collaborative organisée autour de 5 guildes. Permet aux utilisateurs d'explorer, participer et contribuer à une communauté structurée par des valeurs et des rôles.
Le système est pensé à l'échelle d'une ville. Une nouvelle instance pourrait être créée si une masse jugée critique est atteinte.

**Guilde**
Organisation thématique regroupant 3 branches. 5 guildes existent : L'Étoile (éducation), L'Ancre (justice), La Tresse (solidarité), L'Atelier (contribution), Le Pont (économie). L'appartenance à une guilde définit la position temporelle philosophique d'un membre.

**Branche**
Subdivision d'une guilde incarnant des valeurs spécifiques. Chaque guilde possède exactement 3 branches. Exemple : L'Étoile contient La Lueur (curiosité), Le Codex (conservation), Le Souffle (innovation).

**Shard**
Badge virtuel obtenu après réussite d'un test de guilde. Débloque l'accès aux rôles contributifs de cette guilde et aux capacités de mentorat. Invisible par défaut sur le profil (activation manuelle).

**Sparks**
Monnaie symbolique représentant l'engagement, le temps et l'énergie investis. Générés par missions, rituels, mentorat et initiatives. Plafonnés individuellement (seuil Icare). Granularité x10 pour faciliter taxes/redistribution.

**Icare**
Statut de prestige symbolique atteint quand un utilisateur atteint le plafond maximum de Sparks (1000). Les excédents sont redistribués vers le fonds commun ou projets collectifs.

**Capability**
Permission granulaire définissant ce qu'un utilisateur peut faire (ex: `access_forum_read`, `mentor_guild_1`). Structure : `{ action, scope }`. Attribuées dynamiquement selon progression, shards obtenus ou décisions admin.

---

## 👥 Rôles & Progression

### Système de rôles

**Rôle contributif / Métier**
Fonction choisie par l'utilisateur pour contribuer à la plateforme, indépendante de sa vie réelle. 18 rôles disponibles (Éveilleur, Chronarque, Pondâme, Sentinelle, Vigie, Vitalis, Nexus, Régulateur, Forma, Argilus, InspirÃ©, Aurige, Visionnaire, Catalyste, Chroniqueur, Explorateur, Facilitateur, Virtus). Maximum 1 rôle actif simultanément.

**Rôle transversal**
Rôle accessible depuis plusieurs branches/guildes. Exemple : Éveilleur (Étoile-Lueur + Tresse-Métier), Forma (Atelier-Main + Pont-Ruche).

**Mandat / Fonction de service**
Rôle à durée limitée (6 mois indicatif MVP) avec responsabilités temporaires. Concerné : Visionnaire, Aurige, Catalyste (MVP). Volontariat prioritaire, jamais obligatoire. Cumulable avec 1 autre rôle actif.

**Rank / Rang**
Niveau de progression au sein d'un rôle. 3 niveaux universels : Apprenti (découverte), Compagnon (autonomie), Sage (maîtrise). Promotion par validation humaine (Sage existant) ou seuils automatiques selon le rôle.

### Rôles spécifiques

**Vigie**
Rôle de la branche Ancre-Conseil. Valide la conformité procédurale des propositions de votes (format, charte neutralité, branches responsables). Peut départager les égalités lors des votes. Ne statue jamais sur le fond, uniquement sur la forme.

**Virtus**
Mandat temporaire bi-guilde (Atelier-Cœur + Ancre-Balance). Interprète la Charte d'Auryndel, maintient cohérence philosophique, alerte sur glissements éthiques. Rôle de conseil sans pouvoir décisionnel.

**Veilleur**
Mentor inter-guildes (distinct des mentors test et des mentors rôles). Accompagne relations correcteur/corrigé entre branches. Facilitateur cohésion et médiation.

---

## 🎯 Tests & Mentorat

### Système de tests (acquisition des valeurs d'une guilde)

**Test de guilde**
Épreuve obligatoire pour obtenir un Shard et débloquer l'accès aux rôles d'une guilde. Composé de steps (étapes) séquentiels validés un par un. 1 seul test actif par utilisateur à la fois.

**Step / Étape**
Unité d'un test. Types : QCM, texte court, rapport, essai. États : inactive → active → validated. Validation automatique (regex, exact match) ou humaine (mentor/validateur).

**Cooldown**
Délai d'attente obligatoire après échec d'un step avant nouvelle tentative. Progressif selon nombre d'échecs : palier 1 (configurable), palier 2 (24h forcé dès 3 échecs), palier 3 (blocage définitif dès 5 échecs avec notification mentors).

**Session de test**
Instance active d'un test pour un utilisateur. Stocke progression (step actuel, statut, tentatives). États : in_progress → finished / abandoned.

### Mentorat

**Mentor test**
Utilisateur ayant obtenu le Shard d'une guilde, accompagnant un élève durant son test. Maximum 2 mentors simultanés par élève. Limite 1 nouvel élève/jour par mentor. Messages anonymisés dans thread test spécial.

**Mentor rôle**
Utilisateur Sage ou Compagnon accompagnant un Apprenti ou Compagnon du même rôle (indépendant de la guilde). Maximum 3 mentors par mentee, 5 mentees par mentor.

**Thread test**
Fil de messages privé créé automatiquement au début d'un test. User + mentors (max 2) participants. Messages mentors anonymisés affichés "Mentor". Lecture seule après test terminé, jamais supprimé.

---

## 🗳️ Votes & Gouvernance

### Système de votes

**Vote**
Mécanisme de décision collective. Types : global (lois, charte), projet (initiatives locales), événement (temporaire). Méthode ranked choice (classement options). Requiert validation Vigie/Virtus avant ouverture.

**Proposition**
Soumission initiale d'un vote par un utilisateur. Champs : titre, description, résumé ≤280 chars, type, branches responsables, impacts (bénéfice + risque), dossier optionnel (TXT/MD ≤50KB privé avant validation). Limite 1 proposition/jour/user.

**Quorum**
Pourcentage minimum de participants requis pour valider un vote. Configurable par type (30-60%). Base : utilisateurs actifs 30j avec shard. Fallback si non atteint : extension +7j puis baisse à 30%.

**Égalité / Tie**
Situation où plusieurs options obtiennent scores égaux. Résolution : Vigie/Virtus départage via vote interne Conseil (majorité simple). Si Vigie/Virtus en égalité → vote échoue (failed_tie), notification proposant pour reformulation.

**Dossier vote**
Document optionnel joint à une proposition (TXT/MD ≤50KB). Privé pendant validation Vigie/Virtus, public après approbation. Permet d'étayer arguments de la proposition.

### Processus

**Validation Vigie/Virtus**
Étape obligatoire après proposition avant ouverture vote. Vérifie : format correct, charte neutralité respectée, branches responsables pertinentes, durée/quorum appropriés. Seuil configurable par type (`min_validators`). Rejet → motif + suggestions reformulation.

**Éligibilité vote**
Critères définissant qui peut voter. Configurables par la Vigie/Virtus : guildes éligibles, branches éligibles, rang minimum, shards requis, pondération selon rangs. Affichés clairement sur page vote.

**Ballot**
Acte de voter. Méthode ranked choice : classement des options par préférence. Pondération possible selon rang utilisateur (si configurée). 1 vote par utilisateur, modifiable avant clôture.

---

## 📝 Contributions

### Types de contributions

**Contribution**
Terme générique englobant projets, rituels et missions. Table unifiée avec type discriminant et paramètres JSONB (`type_params`). États : draft → recruiting → active → closing → completed / failed / archived.

**Projet**
Contribution ponctuelle (one-time) avec client (externe/interne), multi-organisateurs possibles (max 10), deadline modifiable, budget Sparks négocié. Validation par client. Inactivité 15j → warning → rapport 7j → vote passation.

**Rituel**
Contribution surprise ludique créée par admin MVP. Récurrence potentielle. Sparks pool automatique. Participation ouverte auto-validée. Pas de client externe. Notification CRITICAL globale lors annonce.

**Mission**
Contribution assignée par Sage/Admin avec participation par invitation. Sparks pool fixe. Validation accomplissement par Sage guilde. Échéance expirée → failed automatiquement si configuré.

### Acteurs

**Organisateur**
Utilisateur créant ou co-gérant une contribution. Maximum 10 organisateurs par contribution avec droits égaux. Peut abandonner (libère place). Si tous abandonnent → vote passation automatique (durée configurable).

**Participant**
Utilisateur contribuant à un projet/mission/rituel. Rôle à l'entrée figé (`role_at_entry`). Sparks négociés individuellement (projets) ou fixes (missions/rituels). Statut : pending → accepted → active → completed.

**Client**
Commanditaire d'un projet (user interne ou externe). Définit budget Sparks, valide livraisons finales. Non-réactivité 30j → clôture auto sans Sparks. Reprise possible 7j après abandon.

### Workflow

**Livraison / Delivery**
Soumission de travail accompli dans une contribution. Types : mini_test (onboarding), final_delivery (projet), report (mission). Statuts : pending → validated / rejected. Validée par client (projet) ou Sage (mission).

**Timeline contribution**
Journal chronologique des événements d'une contribution : création, recrutement ouvert/fermé, participant ajouté/quitté, livraison soumise/validée, statut changé. Immuable append-only.

**Inactivité contribution**
Absence d'événement timeline pendant durée configurable (15j par défaut). Déclenche : warning organisateurs → 7j délai rapport → 7j vote passation si pas d'action.

---

## ⚖️ Abus & Modération

### Gestion des abus

**Abuse report / Signalement**
Déclaration d'un comportement problématique par un utilisateur. Types : mentor_test_abuse, spam, harassment, gaming_sparks, other. Statuts : pending → investigating → resolved / dismissed.

**Investigation**
Examen d'un signalement par membre désigné. Collecte : vérification conformité, preuves, audition parties. Conclusion transmise à branche responsable ou admin.

**Sanction**
Mesure graduée appliquée suite à décision sur abuse report. Échelle : avertissement (1ère violation) → suspension 7j (2e) → suspension 30j + review (3e) → ban définitif (4e <1an).

**Appel**
Contestation d'une décision de sanction par l'utilisateur concerné. Délai : 7j après décision. Examen collégial par Conseil (vote interne). Décision finale sous 14j.

### Charte & Neutralité

**Charte Auryndel**
Document fondateur établissant 5 principes : neutralité monde réel, valeurs transposées (langage Auryndel), symbolique vivante (non prescriptive), pas de domination morale, cadre évolutif. Référence pour validation conformité votes/projets.

**Keyword proscrit**
Mot ou expression bloqués pour violation charte neutralité (table `charte_filters` post-MVP). Catégories : religion, politique, idéologie. Sévérité : warning (alerte) ou block (rejet auto). Modération manuelle MVP.

---

## 💰 Économie & Ressources

### Assets & Transactions

**Asset**
Ressource quantifiable détenue par utilisateur. Types : sparks (monnaie), message_limit (quota messages). Structure : value, max, reset_date. Gérés comme objets extensibles.

**Transaction**
Mouvement d'asset historisé. Champs : user_id, asset_type, delta, reason, metadata, timestamp. Permet audit complet flux Sparks/ressources.

**Sparks par rôle**
Compteur traçant Sparks gagnés spécifiquement via contributions d'un rôle. Table `user_sparks_by_role`. Permet historique détaillé progression. Contributions hors-rôle comptées séparément (total user uniquement).

### Fonds & Redistribution

**Fonds branche**
Réserve collective de Sparks par branche. Alimentée par redistributions Icare, dons, excédents projets. Déblocage via vote spécial (quantité votée). Table `branch_funds` avec transactions historisées.

**Allocation Icare**
Choix de destination des Sparks excédentaires quand plafond atteint. Options : fonds commun, guilde, rôle, projet. 1 allocation active max, changement cooldown 1j (configurable).

---

## 🔔 Communication

### Messagerie

**Message**
Communication texte entre utilisateurs. Types : auto_notif (système), admin_notif (broadcast admin), admin_message (admin → user), test_thread (mentor anonymisé), default (1-to-1), draft (brouillon).

**Thread test**
Fil messages spécial lié à une session test. Créé auto au démarrage test. Participants : user testé + mentors (max 2). Messages mentors anonymisés "Mentor". Lecture seule post-test, jamais supprimé.

**Anonymisation**
Masquage identité expéditeur message. Disponible si capability `send_anonymous` (obtenue après test réussi). Destinataire voit pseudo généré, admin voit trace complète (`realSender`).

### Notifications

**Notification**
Alerte système envoyée à utilisateur. Tags : INFO (information), WARNING (attention), CRITICAL (urgent), ACTION_REQUIRED (action nécessaire). Types : nouveau_shard, test_step_validated, contribution_delivery_validated, vote_opened, etc.

**Polling**
Mécanisme rafraîchissement périodique données. Intervals configurables admin : messages (10s si thread actif), notifications (30s), assets (1h). Alternative future : WebSockets temps réel.

---

## 🔧 Administration

### Configuration

**Site config**
Paramètres globaux plateforme stockés table `site_config`. Clés/valeurs JSONB. Exemples : contribution_max_active_per_user, vote_proposal_daily_limit, shard_default_visibility. Modifiables via admin panel.

**Admin panel**
Interface réservée utilisateurs capability `view_admin`. 4 sections : Config (édition JSONB), Capabilities (grant/revoke), Guilds (création/assignation), Abuse (traitement signalements).

### Système technique

**Contexte / Context**
État React partagé application. 8 contexts : Auth (user/token/capabilities/modales), Assets, Guilds, Tests, Messaging, Notifications, Votes, Theme. Imbrication définie `AppProviders.jsx`.

**Protected Route**
Route React nécessitant authentification. Vérifie présence user AuthContext. Redirige vers landing si non authentifié.

**Capability check**
Vérification possession capability utilisateur. Frontend : `checkCapability(action)` dans AuthContext. Backend : service `capabilitiesService.getUserCapabilities(userId)`. Double validation sécurité.

---

## 📊 Événements & Historique

### Système événements

**Event / Événement**
Enregistrement immuable action significative. Table `events` append-only. Champs : profil_id, event_type, context_type/id, metadata (JSONB), visibility (public/restricted/private), timestamp.

**Event type**
Catégorie événement. Exemples : shard_acquired, role_activated, contribution_joined, vote_cast, cooldown_started, rank_promoted. Permet filtrage timeline.

**Visibility événement**
Niveau accès événement. Public : tous participants concernés. Restricted : initiateur/rôle porteur/métier affecté. Private : système uniquement (logs techniques).

**Timeline**
Vue chronologique événements profil/contexte. Filtrages : since (date), limit (pagination), visibility, event_type. Tri DESC (plus récents en premier). Materialized views performance (`events_last_month`, `events_last_year`).

### Triggers & Immuabilité

**Trigger immuabilité**
Fonction PL/pgSQL empêchant UPDATE/DELETE table `events`. Garantit intégrité historique. Tests vérifient impossibilité modification post-création.

**Eligible participants**
Fonction calculant qui peut voir un événement selon visibility + context. Logique : public → tous participants, restricted → initiateur + rôle concerné, private → système seul.

---

## 🎨 Interface & UX

### Thème & Design

**Thème**
Palette couleurs application. 4 thèmes : purple (défaut), blue, green, slate. Modes : light, dark. CSS variables dynamiques (`--bg`, `--text`, `--accent`, etc.). Switch via `ThemeContext`.

**Container class**
Style réutilisable composant. Types : card, section, table, panel, badge. Variantes : active, completed, pending, error, warning, success. Fonction `getContainerClass()` génère classes Tailwind.

**UI Intent**
Grammaire sémantique UI stable. Intents : neutral, muted, pending, info, success, warning, danger, special. Indépendants métier. Structure : `{ container, badge, text }`. Fichier `uiIntents.js`.

### Navigation

**Sidebar**
Barre navigation latérale fixe. Header Auryndel (logo cliquable → town-center), User Profile Card (avatar + username → profile), sections filtrées capabilities (Mission, Guildes, Votes, Forum). Collapsible avec chevron.

**Topbar**
Barre horizontale haut page. Menus dropdown (Guildes, Aide), bouton Test, bouton Déconnexion. Responsive hamburger mobile. Notifications (icône + badge + dernière notif).

**Active link**
Lien navigation souligné/mis en évidence quand route active. `NavLink` React Router avec styling conditionnel `isActive`.

---

## 🔍 Terminologie technique

### Base de données

**JSONB**
Type PostgreSQL stockant JSON binaire optimisé. Utilisé pour : type_params (contributions), metadata (events), contributions (rôles), validation_rules (steps). Indexable, requêtable GIN.

**Index partiel**
Index PostgreSQL avec clause WHERE. Exemple : `idx_one_active_branch_role` sur `user_branch_roles(user_id) WHERE is_active=TRUE`. Optimise contraintes unicité contextuelles.

**Materialized View**
Vue PostgreSQL pré-calculée stockée physiquement. Refresh périodique nécessaire. Exemples : `events_last_month`, `user_guild_time`. Performance requêtes fréquentes.

**ON CONFLICT**
Clause SQL PostgreSQL gérant doublons INSERT. Actions : DO NOTHING (ignore), DO UPDATE (met à jour). Permet idempotence seeds/migrations.

### Backend

**Middleware**
Fonction Express intercalant requête/réponse. Exemples : `verifyJWT` (auth), `checkCapability` (permissions), `errorHandler` (erreurs). Chaînés via `app.use()`.

**Service layer**
Couche logique métier backend. Fichiers `*.service.js`. Exemples : `authService`, `capabilitiesService`, `rolesService`. Appelés par controllers, interagissent DB.

**Controller**
Couche gestion requêtes HTTP. Fichiers `*.controller.js`. Parse params/body, appelle services, renvoie réponses standardisées `{ success, data/error }`.

**AppError**
Classe erreur personnalisée backend. Propriétés : code (identifiant), message (description), statusCode (HTTP). Exemple : `new AppError('SHARD_NOT_FOUND', 'Shard not found', 404)`.

---

## 📚 Acronymes & Abréviations

**MVP** : Minimum Viable Product - Version initiale fonctionnelle plateforme

**TTL** : Time To Live - Durée validité cache

**JSONB** : JSON Binary - Type données PostgreSQL

**QCM** : Questionnaire à Choix Multiples - Type step test

**SPOF** : Single Point Of Failure - Point unique défaillance

**API** : Application Programming Interface - Interface programmation applicative

**JWT** : JSON Web Token - Jeton authentification

**CRUD** : Create Read Update Delete - Opérations base données

**UI** : User Interface - Interface utilisateur

**UX** : User Experience - Expérience utilisateur

**CSS** : Cascading Style Sheets - Feuilles style

**SQL** : Structured Query Language - Langage requêtes base données

**DB** : Database - Base de données

**PG** : PostgreSQL - Système gestion base données

**PL/pgSQL** : Procedural Language/PostgreSQL - Langage procédural PostgreSQL

---

**VERSION** : 1.0  
**DERNIÈRE MAJ** : Janvier 2026  
**MAINTENANCE** : Adapter selon évolution projet + feedback utilisateurs