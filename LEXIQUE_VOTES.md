# SYSTÈME DE VOTES AURYNDEL - GUIDE ONBOARDING

## Vue d'ensemble

Le système de votes d'Auryndel permet aux membres de prendre des décisions collectives sur tous les aspects de la plateforme : création de lois, modifications de la Charte, projets majeurs, sanctions d'utilisateurs, etc.

**Principes fondamentaux :**
- **Transparence totale** : Tous les votes sont publics (résultats, participation, critères)
- **Validation préalable** : Tous les votes doivent être validés par un validateur (rôles Vigie ou Virtus) avant ouverture
- **Méthode ranked choice** : Les électeurs classent les options par préférence (1er choix, 2e choix, etc.)
- **Quorum obligatoire** : Pourcentage minimum de participation requis pour valider un vote
- **Départage structuré** : En cas d'égalité, la Vigie/Virtus départage via vote interne du Conseil

---

## 1. Types de votes

Chaque type de vote a ses propres paramètres par défaut (durée, quorum, nombre de validateurs).

### 1.1 Votes globaux

**Création de loi** (`law_creation`)
- **Objectif** : Créer une nouvelle règle s'appliquant à toute la communauté
- **Quorum par défaut** : 50% des utilisateurs actifs avec shard
- **Durée par défaut** : 7 jours
- **Validateurs minimum** : 1 membre Vigie/Virtus selon impact et domaine
- **Exemple** : "Interdire les contributions anonymes dans les projets publics"

**Modification de la Charte** (`charter_change`)
- **Objectif** : Modifier les principes fondamentaux d'Auryndel (neutralité, valeurs, gouvernance)
- **Quorum par défaut** : 50% des utilisateurs actifs avec shard
- **Durée par défaut** : 7 jours
- **Validateurs minimum** : 3 membres Virtus (renforcement sécurité)
- **Exemple** : "Ajouter un 6e principe à la Charte de neutralité"

### 1.2 Votes de projets

**Projet majeur** (`project_major`)
- **Objectif** : Valider un projet collectif important nécessitant engagement communautaire
- **Quorum par défaut** : 30% des membres de la guilde concernée
- **Durée par défaut** : 5 jours
- **Validateurs minimum** : 1 membre Vigie
- **Exemple** : "Créer un wiki collaboratif pour documenter l'histoire d'Auryndel"

### 1.3 Votes de sanctions

**Bannissement utilisateur** (`user_ban`)
- **Objectif** : Décider du bannissement définitif d'un utilisateur
- **Quorum par défaut** : 60% (seuil élevé pour décision grave)
- **Durée par défaut** : 10 jours (plus long pour permettre débat)
- **Validateurs minimum** : 5 membres Virtus (collégialité renforcée)
- **Exemple** : "Bannir l'utilisateur X suite à violations répétées de la Charte"

---

## 2. Workflow complet d'un vote

### 2.1 Proposition (étape 1)

**Qui peut proposer ?**
- Tout utilisateur avec au moins 1 shard

**Limite de proposition :**
- **1 proposition par jour et par utilisateur** (tous types confondus)
- Évite le spam et encourage propositions réfléchies

**Formulaire de proposition :**
```
Titre : [150 caractères max]
Description : [Texte libre, détails du vote]
Résumé votable : [280 caractères max - affiché sur le bulletin]
Type de vote : [law_creation | charter_change | project_major | user_ban]
Branche(s) responsable(s) : [Branche concernée par le sujet]
Impacts :
  - Bénéfice : [1 bénéfice attendu]
  - Risque : [1 risque identifié]
Dossier optionnel : [Fichier TXT/MD ≤50 KB, privé avant validation]
```

**Options du vote :**
- Minimum **2 options**, maximum libre
- Chaque option = texte court décrivant le choix
- Exemple pour "Durée mandat Vigie/Virtus" : Option 1 "6 mois", Option 2 "12 mois", Option 3 "18 mois"

**État initial :** `proposed`

**Note importante :** Le dossier reste **privé** tant que la Vigie/Virtus n'a pas validé le vote.

---

### 2.2 Validation Vigie/Virtus (étape 2)

**Rôle de la Vigie (Ancre-Conseil) :**
- Vérifier la **conformité procédurale** du vote (pas le contenu/fond)
- S'assurer du respect de la Charte de neutralité
- Valider que les critères d'éligibilité sont appropriés

**Critères de validation :**

1. **Conformité formelle**
   - Titre clair et non ambiguë
   - Au moins 2 options proposées
   - Résumé votable ≤280 caractères
   - Impacts (bénéfice + risque) renseignés

2. **Conformité Charte neutralité**
   - Aucun keyword proscrit (religion, politique, idéologie réelle)
   - Valeurs transposées dans le langage Auryndel
   - Pas de domination morale implicite
   - Symbolique fictionnelle respectée

3. **Cohérence type/paramètres**
   - Durée adaptée au type (5-10j)
   - Quorum approprié (30-60%)
   - Branches responsables pertinentes

**Processus de validation :**

**Vote interne Conseil (collégial) :**
- Chaque membre Vigie/Virtus vote Oui/Non sur la conformité
- Seuil : `min_validators` (configurable par type de vote)
- Exemple : Pour `charter_change`, 3 validateurs minimum requis

**Possibilité override :**
- Un membre Vigie/Virtus peut approuver immédiatement (bypass seuil)
- Utile pour votes urgents ou évidemment conformes

**Si validé :**
- État passe à `open`
- Dossier devient **public**
- Critères d'éligibilité définis par la Vigie/Virtus
- Notification **CRITICAL** envoyée à tous les utilisateurs éligibles

**Si rejeté :**
- État passe à `rejected`
- Notification au proposant avec :
  - Motif du refus
  - Suggestions de reformulation
- Proposant peut reproposer sous 24h (limite 1/jour respectée)

**Si timeout (délai dépassé) :**
- Configurable admin : `vote_validator_validation_deadline_days`
- Par défaut : `null` (pas de timeout)
- Si timeout défini et dépassé → escalade admin

---

### 2.3 Critères d'éligibilité (définis par Vigie/Virtus)

**Configurables lors de la validation :**

**Guildes éligibles** (JSONB array)
- Liste des guildes dont les membres peuvent voter
- Exemple : `["L'Étoile", "L'Ancre"]` pour vote concernant ces guildes
- Si vide : toutes les guildes éligibles

**Branches éligibles** (JSONB array)
- Sous-ensemble plus fin que les guildes
- Exemple : `["ancre_balance", "ancre_conseil"]` pour vote justice
- Si vide : toutes les branches éligibles

**Rang minimum requis** (rank_id)
- Exemple : Apprenti (tous), Compagnon (expérimentés), Sage (experts)
- Permet de limiter votes complexes aux utilisateurs avec expérience

**Shards minimum requis** (number)
- Exemple : 2 shards minimum pour voter sur modifications Charte
- Garantit engagement multi-guildes pour décisions structurantes

**Pondération par rang** (JSONB)
- Exemple : `{"Apprenti": 1, "Compagnon": 1.5, "Sage": 2}`
- Vote d'un Sage compte 2× plus qu'un Apprenti
- Par défaut : pondération égale (tous = 1)

**Affichage pour l'utilisateur :**
- Critères visibles sur la page du vote
- Si utilisateur non éligible : message explicatif
- Si utilisateur éligible : bouton "Voter" accessible

---

### 2.4 Vote ouvert (étape 3)

**Durée du vote :**
- Définie par type de vote (5-10 jours par défaut)
- Configurable par la Vigie/Virtus lors de validation
- Compte à rebours affiché en temps réel

**Méthode : Ranked Choice Voting**

**Comment voter :**
1. Utilisateur voit toutes les options
2. Classe les options par ordre de préférence (drag & drop)
   - 1er choix = option préférée
   - 2e choix = alternative si 1er éliminé
   - 3e choix, etc.
3. Soumet son bulletin

**Exemple concret :**
```
Vote : "Durée mandat Vigie/Virtus"
Options :
- A : 6 mois
- B : 12 mois
- C : 18 mois

Bulletin de Alice :
1er choix : B (12 mois)
2e choix : C (18 mois)
3e choix : A (6 mois)
```

**Règles de vote :**
- **1 vote par utilisateur** (unicité garantie par table `vote_ballots`)
- Vote modifiable jusqu'à clôture (écrase le précédent)
- Vote anonyme dans les résultats publics (agrégation)
- Pondération appliquée selon rang si configurée

**Quorum en temps réel :**
- Pourcentage de participation affiché continuellement
- Rappel J-2 si quorum non atteint
- Extension possible +7j si <30% participation (fallback configurable)

**Notification de participation :**
- Tag **INFO** au vote
- Tag **WARNING** si J-2 et utilisateur n'a pas voté
- Tag **CRITICAL** si nouveau vote ouvert

---

### 2.5 Clôture et résultats (étape 4)

**Déclenchement clôture :**
- Automatique à échéance (fin durée vote)
- Manuelle par admin (cas exceptionnel)

**Calcul ranked choice :**

**Algorithme :**
1. Compter tous les 1ers choix
2. Si une option a majorité absolue (>50%) → elle gagne
3. Sinon : éliminer l'option avec le moins de 1ers choix
4. Redistribuer les votes de l'option éliminée vers leur 2e choix
5. Recommencer jusqu'à majorité absolue ou dernière option

**Exemple calcul :**
```
100 votants, 3 options (A, B, C)

Tour 1 (1ers choix) :
A : 35 votes
B : 40 votes
C : 25 votes
→ Aucune majorité absolue (≥51 requis)

Tour 2 :
C éliminé (plus faible)
Redistribution des 25 votes C :
  - 15 avaient B en 2e choix → +15 pour B
  - 10 avaient A en 2e choix → +10 pour A

Nouveau total :
A : 35 + 10 = 45 votes
B : 40 + 15 = 55 votes (majorité absolue)

→ B gagne
```

**Application pondération :**
- Si pondération par rang active
- Chaque vote multiplié par poids avant comptage
- Exemple : Vote Sage = 2 votes Apprenti

**Quorum vérifié :**
- Si participation < quorum requis → vote échoue (`failed_quorum`)
- Même si une option a majorité, vote invalide sans quorum

---

### 2.6 Départage en cas d'égalité

**Cas d'égalité :**
- 2 options ou plus avec exactement le même score final
- Arrive rarement avec ranked choice mais possible

**Procédure départage :**

**Étape 1 : Vote interne Vigie/Virtus**
- Notification **ACTION_REQUIRED** à tous les membres Vigie/Virtus
- Chaque Vigie/Virtus vote pour 1 des options égales
- Délai : 3 jours
- Méthode de base suceptible d'être modifiée par la suite : Majorité simple (>50% des Vigie/Virtus votants)

**Étape 2 : Résultat départage**
- Si majorité simple atteinte → option gagnante choisie
- État final : `closed` (succès avec départage)
- Résultats publics avec mention "Départagé par Vigie"

**Étape 3 : Échec départage**
- Si Vigie/Virtus aussi en égalité (cas très rare)
- État final : `failed_tie`
- Notification au proposant :
  - "Vote échoué : égalité non départagée"
  - "Reformulez et reproposez pour clarifier les options"

**Historique conservé :**
- Tous les votes (réussis + échoués) archivés
- Résultats publics avec raison échec
- Permet apprentissage communautaire

---

## 3. États du vote

**Cycle de vie complet :**

```
proposed
  ↓
pending_validator (en attente validation)
  ↓
├─→ rejected (non-conforme, peut reproposer)
└─→ open (validé, vote ouvert)
      ↓
    closed (succès)
      OU
    failed_tie (égalité non départagée)
      OU
    failed_quorum (participation insuffisante)
```

**Détails des états :**

| État | Description | Visible par | Actions possibles |
|------|-------------|-------------|-------------------|
| `proposed` | Vote soumis, attend validation Vigie/Virtus | Proposant, Vigie/Virtus | Vigie/Virtus valide/rejette |
| `pending_validator` | En cours de validation collégiale | Proposant, Vigie/Virtus | Vigie/Virtus votent conformité |
| `rejected` | Rejeté par Vigie/Virtus (non-conforme) | Proposant, Vigie/Virtus | Proposant reformule |
| `open` | Vote ouvert, en cours | Tous éligibles | Voter, voir résultats partiels |
| `closed` | Vote terminé avec succès | Tous | Consulter résultats |
| `failed_tie` | Échec égalité non départagée | Tous | Proposant reformule |
| `failed_quorum` | Échec quorum non atteint | Tous | Proposant peut reproposer |

---

## 4. Notifications et communications

### 4.1 Notifications automatiques

**Au proposant :**
- Vote soumis → `INFO` "Vote proposé, en attente validation Vigie/Virtus"
- Validation réussie → `CRITICAL` "Vote ouvert, durée X jours"
- Rejet → `WARNING` "Vote rejeté : [motif]"
- Clôture réussie → `INFO` "Vote terminé : option [X] gagnante"
- Échec → `WARNING` "Vote échoué : [raison]"

**Aux membres Vigie/Virtus :**
- Nouvelle proposition → `ACTION_REQUIRED` "Nouveau vote à valider"
- Égalité vote → `ACTION_REQUIRED` "Départage requis"

**Aux électeurs éligibles :**
- Vote ouvert → `CRITICAL` "Nouveau vote : [titre]"
- Rappel J-2 → `WARNING` "Vote [titre] clôture dans 2 jours, votez !"
- Résultats → `INFO` "Résultats vote [titre] : [option] gagne"

### 4.2 Transparence publique

**Informations publiques :**
- Titre, description, résumé votable
- Options disponibles
- Critères éligibilité (guildes, ranks, shards)
- Durée et quorum requis
- Pourcentage participation en temps réel
- Résultats détaillés post-clôture

**Informations privées :**
- Qui a voté pour quelle option (anonymat)
- Dossier avant validation Vigie/Virtus
- Délibérations internes Vigie/Virtus

---

## 5. Sécurité et anti-abus

### 5.1 Limites et cooldowns

**Limite propositions :**
- 1 proposition/jour/utilisateur (tous types)
- Évite spam et surcharge Vigie/Virtus
- Reset à minuit serveur

**Aucune limite votes :**
- Utilisateur peut voter sur tous les votes ouverts où il est éligible
- Pas de cooldown entre votes

### 5.2 Validation Charte

**Keywords proscrits :**
- Table `charte_filters` (MVP : modération manuelle)
- Post-MVP : filtrage automatique mots religion/politique/idéologie

**Vérification manuelle Virtus :**
- Virtus lit proposition complète
- Vérifie neutralité réelle vs apparente
- Rejette si violation 5 principes Charte

**Sanctions violations répétées :**
- 1ère violation : Avertissement + reformulation obligée
- 2e violation (<30j) : Suspension 7j propositions votes
- 3e violation (<90j) : Suspension 30j + review admin
- 4e violation (<1an) : Ban définitif

### 5.3 Audit et traçabilité

**Logs automatiques :**
- Toutes propositions enregistrées (même rejetées)
- Tous votes Vigie/Virtus (validation/rejet) tracés avec userId
- Tous bulletins enregistrés (anonymisés dans résultats)
- Tous départages Virtus tracés

**Historique public :**
- Tous votes (réussis + échoués) consultables
- Résultats avec participation, options, score final
- Raison échec si applicable

**Audit privilégié :**
- Admin peut voir détail validations Vigie/Virtus
- Admin peut voir qui a voté (pas le choix, juste la participation)

---

## 6. Interface utilisateur (Frontend)

### 6.1 Pages principales

**`/app/votes` - Liste des votes**
- Filtres : En cours | Mes votes passés | Tous les votes
- Tri : Date, Participation, Type
- Cards affichant :
  - Titre + résumé
  - Type de vote
  - Progression (barre quorum)
  - État (badge coloré)
  - Date limite si ouvert

**`/app/votes/:id` - Détail d'un vote**
- En-tête : Titre, description complète, type
- Section : Critères éligibilité (guildes, ranks, shards)
- Section : Options disponibles
- Section : Bulletin de vote (si éligible et ouvert)
  - Drag & drop options pour ranked choice
  - Bouton "Soumettre mon vote"
- Section : Résultats partiels (si ouvert)
  - Participation actuelle vs quorum
  - Compte à rebours clôture
- Section : Résultats finaux (si closed)
  - Score par option
  - Option gagnante mise en évidence
  - Détail calcul ranked choice (tours successifs)

**`/app/votes/validation` - Page Vigie/Virtus (capability `votes_validator`)**
- Liste votes `pending_validator`
- Pour chaque vote :
  - Affichage complet proposition
  - Critères validation (checklist)
  - Bouton Valider / Rejeter + commentaire
- Votes en départage (`failed_tie`)
  - Options égales
  - Vote interne Vigie/Virtus

### 6.2 Composants UI

**VoteCard** (liste)
- Compact, affichage résumé
- Badge état (couleur selon état)
- Barre progression quorum
- Indicateur "Vous avez voté" si déjà voté

**VoteBallot** (bulletin ranked choice)
- Drag & drop fluide
- Aperçu classement en temps réel
- Validation avant submit (au moins 1er choix)

**VoteResults** (résultats)
- Graphique barres par option
- Détail tours ranked choice (expandable)
- Mention départage si applicable

---

## 7. Exemples concrets

### 7.1 Vote création de loi

**Proposition :**
```
Titre : Limite contributions actives par utilisateur
Type : law_creation
Résumé : Limiter à 5 contributions actives simultanées par utilisateur
Description : Actuellement, aucun plafond. Risque de dispersion et 
surcharge. Proposition : max 5 contributions actives (tous types).
Impacts :
  - Bénéfice : Meilleur focus, qualité accrue
  - Risque : Frustration utilisateurs très actifs
Options :
  1. Limite à 3 contributions
  2. Limite à 5 contributions
  3. Limite à 10 contributions
  4. Aucune limite (statu quo)
```

**Validation Vigie/Virtus :**
- Conforme : oui (neutre, clair, options variées)
- Critères éligibilité : Tous avec ≥1 shard, pondération Sage ×1.5
- Ouverture : 7 jours, quorum 50%

**Résultat (exemple) :**
```
Participation : 65% (quorum atteint)
Tour 1 : Option 4 (28%), Option 3 (22%), Option 2 (35%), Option 1 (15%)
Tour 2 : Élimination Option 1, redistribution → Option 2 (45%), Option 3 (25%), Option 4 (30%)
Tour 3 : Élimination Option 3, redistribution → Option 2 (55%), Option 4 (45%)
→ Option 2 gagne : "Limite à 5 contributions"
```

### 7.2 Vote modification Charte

**Proposition :**
```
Titre : Ajout 6e principe Charte - Droit à l'erreur
Type : charter_change
Résumé : Ajouter "Le droit à l'erreur est fondamental" comme 6e principe
Description : Actuellement 5 principes. Proposition d'ajouter reconnaissance 
explicite du droit à l'erreur pour encourager expérimentation sans crainte.
Impacts :
  - Bénéfice : Culture bienveillante, innovation encouragée
  - Risque : Possible laxisme face aux erreurs répétées
Options :
  1. Ajouter le principe tel quel
  2. Ajouter avec nuance "sous réserve de bonne foi"
  3. Ne pas ajouter (statu quo)
```

**Validation Vigie/Virtus :**
- Conforme : oui (après discussion collégiale)
- 3 Vigie/Virtus valident
- Critères : Tous avec ≥2 shards (engagement multi-guildes)
- Ouverture : 7 jours, quorum 50%

**Résultat (exemple) :**
```
Participation : 48% (quorum non atteint)
→ Vote failed_quorum
Notification proposant : "Quorum non atteint, reproposez avec plus de mobilisation"
```

---

## 8. Bonnes pratiques

### 8.1 Pour les proposants

**Rédiger une bonne proposition :**
- ✅ Titre court, clair, sans ambiguïté
- ✅ Résumé votable concis (≤280 chars)
- ✅ Description détaillée avec contexte et justification
- ✅ Impact bénéfice + risque honnêtes
- ✅ Options variées et équilibrées
- ✅ Dossier annexe si besoin (données, exemples)

**Éviter :**
- ❌ Options biaisées (1 bonne + 3 absurdes)
- ❌ Langage émotionnel ou culpabilisant
- ❌ Références directes au monde réel (politique, religion)
- ❌ Urgence artificielle ("il faut voter OUI sinon...")

### 8.2 Pour les Vigie/Virtus (validateurs)

**Rôle de la Vigie/Virtus :**
- ✅ Vérifier conformité procédurale uniquement
- ✅ S'assurer respect Charte neutralité
- ✅ Signaler ambiguïtés formulation
- ❌ Ne PAS juger le fond/contenu du vote
- ❌ Ne PAS orienter vers une option

**Critères validation stricts :**
- Format complet (titre, résumé, description, options ≥2)
- Impacts renseignés (bénéfice + risque)
- Langage neutre (pas de keywords proscrits)
- Options claires et distinctes

### 8.3 Pour les électeurs

**Voter de façon éclairée :**
- ✅ Lire proposition complète + dossier si fourni
- ✅ Considérer impacts long terme
- ✅ Classer options selon préférence réelle (pas stratégique)
- ✅ Participer même si "mon option va perdre" (ranked choice = voix compte)

**Comprendre ranked choice :**
- 1er choix = option idéale
- 2e choix = acceptable si 1er éliminé
- Ne pas hésiter à classer toutes les options

---

## 9. Paramètres configurables (Admin)

### 9.1 Site config

```json
{
  "vote_proposal_daily_limit": 1,
  "vote_validator_validation_deadline_days": null,
  "vote_quorum_fallback_extension_days": 7,
  "vote_quorum_fallback_threshold": 30,
  "vote_tie_break_deadline_days": 3
}
```

### 9.2 Vote types (table)

Modifiable via admin panel :
- `quorum_default` : Pourcentage requis (30-60%)
- `duration_default_days` : Durée vote (5-10j)
- `min_validators_default` : Vigie/Virtus requis (1-5)

---

## 10. Cas limites et edge cases

### 10.1 Vote avec 0 éligibles

**Scénario :** Critères trop restrictifs (ex: "seulement Sages guilde X" mais aucun Sage)

**Comportement :**
- Vote validé par Vigie/Virtus (critères semblaient OK)
- Ouverture normale
- Notification 0 utilisateur
- À échéance : `failed_quorum` (participation 0%)

**Solution :** Vigie/Virtus devrait vérifier nombre d'éligibles avant validation

### 10.2 Tous les Vigie absents

**Scénario :** Aucun Vigie/Virtus ne valide une proposition sous 30j (si deadline configurée)

**Comportement :**
- Timeout atteint
- Escalade admin automatique
- Admin peut valider manuellement ou rejeter

**Solution :** Élire suffisamment de Vigie/Virtus, rotation active

### 10.3 Vigie/Virtus propose un vote

**Scénario :** Membre Vigie propose lui-même un vote

**Comportement :**
- Traité comme toute proposition normale
- Ce Vigie/Virtus **doit s'abstenir** de valider son propre vote (conflit intérêt)
- Autres Vigie/Virtus valident
- Si seul Vigie/Virtus actif : escalade admin

---

## 11. Évolutions futures (post-MVP)

### 11.1 Votes délégués

**Concept :** Déléguer son vote à un autre utilisateur de confiance

**Use case :** User absent mais veut participer via représentant

**Implémentation :**
- Table `vote_delegations` (delegator, delegate, vote_id)
- Delegate vote avec poids ×2 (son vote + délégué)

### 11.2 Votes avec budget Sparks

**Concept :** Vote débloque budget Sparks pour projet validé

**Use case :** "Allouer 500 Sparks au projet Wiki communautaire"

**Implémentation :**
- Vote spécifique `budget_allocation`
- Si vote réussit → création automatique fonds projet
- Traçabilité complète utilisation budget

### 11.3 Votes quadratiques

**Concept :** Coût exponentiel pour votes multiples

**Use case :** Éviter domination minorité très active

**Implémentation :**
- Voter sur X votes = coût X² en "crédits vote"
- Chaque user a capital crédits limité par mois

---

## 12. Ressources et aide

### 12.1 Documentation connexe

- **LEXIQUE_AURYNDEL.md** : Terminologie complète
- **CHARTE_NEUTRALITE.md** : 5 principes détaillés
- **GUIDE_VALIDATEUR.md** : Manuel validateur
- **rules.json** : Règles techniques votes

### 12.2 Support

**Questions procédurales :**
- `/app/help/votes` - FAQ votes
- Contacter Vigie/Virtus via `/app/messages`

**Signalement abus :**
- `/app/help/abuse` - Formulaire signalement
- Violations Charte : traité par Virtus

**Propositions amélioration :**
- Créer vote `law_creation` pour modifier système votes
- Passer par validation Vigie/Virtus comme tout vote

---

## 13. Glossaire votes

| Terme | Définition |
|-------|------------|
| **Ballot** | Bulletin de vote, classement ranked choice d'un utilisateur |
| **Départage** | Procédure résolution égalité par vote interne Vigie/Virtus |
| **Dossier** | Fichier annexe (TXT/MD ≤50KB) avec détails vote, privé avant validation |
| **Éligible** | Utilisateur remplissant critères pour voter (guildes, ranks, shards) |
| **Fallback** | Extension automatique +7j si quorum <30% avant échéance |
| **Quorum** | Pourcentage minimum participation requis (30-60% selon type) |
| **Ranked choice** | Méthode vote par classement préférences (1er, 2e, 3e choix) |
| **Résumé votable** | Texte court (≤280 chars) affiché sur bulletin vote |
| **Vigie** | Rôle Ancre-Conseil, valide conformité votes avant ouverture |
| **Virtus** | Valide conformité des remontées d'abuse, des mofications structurelles en conformité avec la charte avant ouverture vote |
---

**Version** : 1.0 (Février 2026)  
**Maintenance** : Document évolutif selon feedback communauté  
