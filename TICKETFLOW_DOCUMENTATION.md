# 📋 TicketFlow — Documentation Produit Complète

**Version:** 1.0  
**Date:** 2026-03-09  
**Auteur:** Consultant Senior Product Manager  
**Statut:** Approuvé

---

## 📑 Table des Matières

1. [Livrable 1 : Project Brief Complet](#livrable-1--project-brief-complet)
   - [1. Résumé Exécutif](#1-résumé-exécutif)
   - [2. Énoncé du Problème](#2-énoncé-du-problème)
   - [3. Solution Proposée](#3-solution-proposée--ticketflow)
   - [4. Indicateurs de Succès (KPIs)](#4-indicateurs-de-succès-kpis)
   - [5. Planning & Phases](#5-planning--phases)
   - [6. Risques & Mitigations](#6-risques--mitigations)
   - [7. Hypothèses & Contraintes](#7-hypothèses--contraintes)

2. [Livrable 2 : Fiches Personas Détaillées](#livrable-2--fiches-personas-détaillées)
   - [Persona 1 : Le Directeur](#persona-1--laurent-rousseau-directeur)
   - [Persona 2 : Le Référent IT](#persona-2--mathieu-dupont-référent-informatique)
   - [Persona 3 : Le Responsable Équipe](#persona-3--isabelle-martin-responsable-équipe-support-clients)
   - [Persona 4 : L'Employé](#persona-4--thomas-bernard-employé-support-client)

3. [Livrable 3 : Questions d'Atelier de Cadrage](#livrable-3--questions-datelier-de-cadrage)
   - [Bloc 1 : Vision & Objectifs](#bloc-1--vision--objectifs)
   - [Bloc 2 : Utilisateurs & Usages](#bloc-2--utilisateurs--usages)
   - [Bloc 3 : Périmètre Fonctionnel](#bloc-3--périmètre-fonctionnel)
   - [Bloc 4 : Contraintes Techniques & Organisationnelles](#bloc-4--contraintes-techniques--organisationnelles)
   - [Bloc 5 : Priorisation & Définition du MVP](#bloc-5--priorisation--définition-du-mvp)

---

# LIVRABLE 1 — PROJECT BRIEF COMPLET

## 1. Résumé Exécutif

### 🎯 Vision du Projet

TicketFlow est une application web CRUD de gestion centralisée des tickets et incidents conçue pour transformer le processus opérationnel d'une PME. En remplaçant les signalements par email et les conversations verbales par un système de suivi structuré, TicketFlow garantit la traçabilité complète de chaque incident, améliore la visibilité managériale et accélère les délais de résolution. Cette solution offre une source unique de vérité pour l'ensemble de l'organisation, éliminant les informations perdues et les malentendus.

### 🎯 Objectifs Stratégiques

| # | Objectif | Détail |
|---|----------|--------|
| **O1** | Centraliser la gestion des incidents | Remplacer 100% des canaux de signalement informels (email, verbal) par une plateforme unique et tracée |
| **O2** | Améliorer la visibilité opérationnelle | Fournir au management un tableau de bord temps réel de tous les incidents en cours avec statut et priorité |
| **O3** | Réduire le temps de résolution moyen | Diminuer le MTTR (Mean Time To Resolution) de 40% en organisant et priorisant automatiquement les incidents |
| **O4** | Augmenter la satisfaction utilisateur | Permettre aux employés de suivre l'état de leurs incidents en temps réel et recevoir des notifications de mise à jour |
| **O5** | Sécuriser l'information critique | Enregistrer un audit trail complet de chaque incident pour conformité, traçabilité et amélioration continue |

### 💰 Valeur Métier Attendue

- **Réduction de perte d'information** : Passage de ~25% d'incidents oubliés/perdus à 0%
- **Gain en productivité IT** : ~8 heures/semaine libérées (moins de emails, recherche d'infos simplifiée)
- **Amélioration du NPS employé** : +15 points via transparence et suivi actif
- **Conformité & Audit** : Historique complet pour traçabilité réglementaire
- **ROI estimé** : Retour sur investissement en < 6 mois

---

## 2. Énoncé du Problème

### 🔴 Points de Douleur Actuels

| Point de Douleur | Fréquence | Impact Quantifié |
|------------------|-----------|-----------------|
| **Incidents perdus/non traités** | ~25% des signalements | Perte de 20-30 incidents/mois non résolus |
| **Délai de réponse élevé** | Moyenne 48-72h | Employés ignorent si leur ticket a été lu |
| **Recherche d'information dispersée** | 3-4 canaux différents (email, Slack, verbal, notes papier) | 5-10 min/jour perdues par agent IT en recherche |
| **Aucune priorisation formelle** | Traitement au fil de l'eau | Incidents critiques traités après des problèmes mineurs |
| **Pas de rapports de suivi** | Directeur aveugle sur l'activité IT | Incapable de justifier budget ou identifier goulots |
| **Frustration des employés** | Suivi de demande impossible | Score NPS IT actuel : ~35/100 |

### 📊 Impact sur les Opérations Métier

- **Arrêts de production** : Les incidents critiques perdus entraînent des interruptions de service pouvant paralyser les opérations
- **Coûts cachés** : Perte de ~4-6 heures/semaine en emails non-structurés et réunions de coordination
- **Charge IT** : Référent IT surchargé, sans visibilité sur charge réelle (données empiriques manquantes)
- **Gouvernance inexistante** : Pas de SLA, pas de suivi de performance, pas d'amélioration continue mesurable

### 🔍 Analyse des Causes Racines

1. **Absence d'outil dédié** : Reliance sur email/chat généraliste, non optimisé pour gestion de tickets
2. **Processus informel** : Pas de workflow défini, chacun applique sa méthode
3. **Manque de visibilité** : Pas de tableau de bord, pas de reporting structuré
4. **Communication éclatée** : Plusieurs canaux parallèles non synchronisés
5. **Pas de responsabilisation** : Difficile de tracer qui fait quoi et quand

---

## 3. Solution Proposée — TicketFlow

### 💡 Concept Central

TicketFlow est une application web de gestion de tickets CRUD (Create, Read, Update, Delete) conçue selon une logique simple et scalable :

- **Pour les employés** : Interface simple pour signaler des incidents, suivre l'état, ajouter des commentaires
- **Pour le Responsable Équipe** : Visibilité sur les tickets de son équipe, soumission au nom de plusieurs personnes
- **Pour le Référent IT** : Tableau de bord complet, affectation, priorisation, résolution, suivi SLA
- **Pour le Directeur** : Rapports, KPIs, tendances, planification capacité

### ✨ Fonctionnalités Clés (Minimum 8)

| # | Fonctionnalité | MVP | Phase 2 | Description |
|---|----------------|-----|---------|-------------|
| **F1** | Signalement de ticket | ✅ | — | Les employés et responsables peuvent créer un ticket (titre, description, catégorie, priorité proposée) |
| **F2** | Gestion du statut | ✅ | — | Workflow : Ouvert → En cours → En attente → Résolu → Fermé (avec fermeture automatique après 7j inactivité) |
| **F3** | Affectation d'incidents | ✅ | — | Le Référent IT affecte les tickets à lui-même ou à des tiers, suivi de responsable |
| **F4** | Système de priorité | ✅ | — | 3 niveaux : Basse, Normale, Haute + possibilité d'escalade automatique si délai dépassé |
| **F5** | Suivi SLA en temps réel | ✅ | — | Affichage du temps restant avant dépassement SLA par priorité (P1: 2h, P2: 4h, P3: 24h) |
| **F6** | Commentaires & Historique | ✅ | — | Thread de commentaires, audit trail complet des modifications (qui, quand, quoi) |
| **F7** | Tableau de bord Référent IT | ✅ | — | Vue d'ensemble : tickets assignés, overdue, en attente réponse utilisateur, statistiques rapides |
| **F8** | Notifications en temps réel | ✅ | — | Email/in-app : création ticket, affectation, réponse, escalade, fermeture |
| **F9** | Rapports & Analytics | — | ✅ | Dashboards : MTTR par catégorie, volume par semaine, taux de résolution, taux de satisfaction |
| **F10** | Catégories d'incidents | ✅ | — | Taxonomy prédéfinie : Infrastructure, Accès, Logiciel, Matériel, Autre |
| **F11** | Gestion multi-utilisateur avec rôles | ✅ | — | 4 rôles : Employé, Responsable Équipe, Référent IT, Directeur (avec permissions graduées) |
| **F12** | Export & Archivage | — | ✅ | Export CSV/PDF des rapports, archivage des tickets fermés > 90j |

### ❌ Hors Périmètre (Phase 1 MVP)

- ❌ Intégration avec logiciels externes (ITSM, ticketing enterprise)
- ❌ Mobile app native (web responsive suffisant en MVP)
- ❌ Chat/vidéoconférence intégrés (rester sur email/Teams en parallèle)
- ❌ Automations complexes (bot IA, routage intelligent)
- ❌ Multi-sites/filiales (single location MVP)
- ❌ Gestion des absences/escalade alternative en cas d'absence Référent IT
- ❌ Système de files d'attente avec workload balancing avancé
- ❌ Facuration/Chargeback par département

---

## 4. Indicateurs de Succès (KPIs)

### 📈 KPIs de Suivi et Cibles

| KPI | Baseline Actuelle | Cible Y+1 | Cible Y+2 | Fréquence Mesure |
|-----|-------------------|-----------|-----------|------------------|
| **KPI1 : Taux de capture** | ~85% des incidents signalés (vs verbaux/non tracés) | 100% | 100% | Hebdomadaire |
| **KPI2 : MTTR (Mean Time To Resolution)** | 72h (moyen) | 36h | 24h | Hebdomadaire |
| **KPI3 : % d'incidents oubliés/perdus** | ~25% | < 2% | < 1% | Mensuel |
| **KPI4 : NPS utilisateurs finals (Employés)** | ~35/100 | ~55/100 | ~70/100 | Trimestrial |
| **KPI5 : Taux de résolution au premier contact** | ~55% | ~70% | ~80% | Mensuel |
| **KPI6 : % de SLA respectés** | N/A (pas de SLA actuellement) | ≥ 80% | ≥ 90% | Hebdomadaire |
| **KPI7 : Charge IT (heures/semaine gestion tickets)** | ~15h | ~8h | ~6h | Mensuel |
| **KPI8 : Adoption taux (% actifs mensuels)** | 0% | ≥ 75% | ≥ 90% | Mensuel |

---

## 5. Planning & Phases

### 🚀 Phases de Déploiement

#### **Phase 1 : MVP (Minimum Viable Product)**
**Durée estimée : 8-10 semaines**

**Objectif** : Déployer la version de base avec fonctionnalités essentielles (F1-F8)

- **Semaines 1-2** : Setup technique, base de données, architecture
- **Semaines 3-5** : Développement backend (API CRUD, authentification, rôles)
- **Semaines 5-7** : Frontend (interface employé, référent IT, directeur)
- **Semaine 8** : Tests, bug fixes, préparation déploiement
- **Semaines 9-10** : Déploiement progressif + formation utilisateurs

**Livrables MVP** :
- ✅ Application web fonctionnelle (CRUD complet)
- ✅ 4 rôles d'utilisateurs avec permissions
- ✅ Notifications emails
- ✅ Tableau de bord Référent IT
- ✅ Documentation utilisateur + vidéos tutoriels

---

#### **Phase 2 : Améliorations (Sprint 1-3)**
**Durée estimée : 6-8 semaines (2-3 mois après Phase 1)**

**Objectif** : Enrichir avec analytics, rapports et optimisations UX

- **Sprint 2.1** : Dashboards & reportings (F9, analytics avancées)
- **Sprint 2.2** : Optimisations UX/UI, recherche/filtrage avancé
- **Sprint 2.3** : Export, archivage, amélioration performance

**Livrables Phase 2** :
- ✅ Dashboards personnalisables par rôle
- ✅ Rapports PDF/CSV automatisés
- ✅ Système de search avec filtres
- ✅ Amélioration temps de chargement (< 2s)

---

#### **Phase 3 : Optimisation (Sprint 4-6)**
**Durée estimée : 8-10 semaines (4-5 mois après Phase 2)**

**Objectif** : Automatisations, scalabilité, intégrations externes

- **Sprint 3.1** : Automations simples (escalade, rappels, routage)
- **Sprint 3.2** : Intégrations (Microsoft Teams, Slack, email structuré)
- **Sprint 3.3** : Mobile app, offboarding/archivage intelligents

**Livrables Phase 3** :
- ✅ Automations de workflow
- ✅ Intégrations avec outils métier
- ✅ App mobile responsive
- ✅ Gestion avancée multi-sites

### 📅 Timeline Synthétique

```
Phase 1 MVP           Phase 2 Améliorations      Phase 3 Optimisation
|--------8-10w--------|-------6-8w--------|----------8-10w---------|
Déploiement   +0j     Déploiement  +10w   Déploiement +18w
```

**Go-Live MVP attendu** : Semaine 10 (estimé mi-avril 2026)

---

## 6. Risques & Mitigations

### ⚠️ Matrice Risques

| # | Risque | Probabilité | Impact | Gravité | Mitigation |
|---|--------|-------------|--------|---------|-----------|
| **R1** | Faible adoption utilisateurs (manque de formation / résistance au changement) | **Élevée** (65%) | **Élevé** | 🔴 Critique | - Plan de change complet (workshops, tutoriels vidéo) <br> - Champion user par équipe <br> - Incitations early adopters <br> - Support phase 1 intensive |
| **R2** | Performance/scalabilité : l'application ralentit avec montée en charge (100+ tickets/jour) | **Moyenne** (40%) | **Moyen** | 🟠 Majeur | - Architecture cloud scalable (auto-scaling) <br> - Tests charge en phase test <br> - Monitoring APM en prod <br> - Optimisations DB indexation |
| **R3** | Perte de données ou non-conformité RGPD (absence de backup, audit trail incomplet) | **Basse** (20%) | **Élevé** | 🟠 Majeur | - Backup journalier + tests restauration mensuels <br> - Audit trail intégral <br> - Chiffrement données sensibles <br> - Politique rétention + anonymisation |
| **R4** | Surcharge Référent IT : centralisation entraîne saturation sans processus de triage efficace | **Élevée** (70%) | **Moyen** | 🟠 Majeur | - Workflow de priorisation automatique (P1/P2/P3) <br> - Formation escalade/triage <br> - SLA réalistes calibrés avec charge <br> - Possibilité recruter support tier-1 futur |
| **R5** | Retard développement : underestimation complexité, turnover dev | **Moyenne** (45%) | **Élevé** | 🟠 Majeur | - Buffer 20% time planning <br> - Code review rigoureux <br> - Documentation technique <br> - Sprints agiles (2w) + démos bimensuelles |
| **R6** | Mauvaise acceptation des SLA proposés (trop stricts) | **Moyenne** (50%) | **Moyen** | 🟠 Majeur | - Co-construction SLA en atelier cadrage <br> - Phase bêta 2 semaines sans pénalité <br> - Révision SLA après 1 mois opérationnel |

### 🛡️ Plans Contingence

- **R1 (Adoption)** : Plan B = Extension déploiement MVP de 2 semaines pour formation addée
- **R2 (Performance)** : Plan B = Migration architecture serverless si cloud classique insuffisant
- **R3 (Sécurité)** : Plan B = Externaliser compliance à cabinet spécialisé avant go-live
- **R4 (Surcharge IT)** : Plan B = Embauche consultant externe temps partiel phase 1-2

---

## 7. Hypothèses & Contraintes

### 📌 Hypothèses Clés

| # | Hypothèse | Critère de Validation |
|---|-----------|----------------------|
| **H1** | Les utilisateurs auront accès à internet 24/7 et navigateur moderne | Vérifier parc informatique avant développement |
| **H2** | Directeur et Référent IT sont motivés pour succès du projet | Signatures accord sponsorship + présence ateliers |
| **H3** | La catégorisation incidents en 5 catégories suffira (pas besoin hiérarchie complexe) | Validation avec Référent IT en cadrage |
| **H4** | Les SLA P1: 2h, P2: 4h, P3: 24h sont réalistes et acceptés | Atelier consensus avec Référent IT + Directeur |
| **H5** | Aucune intégration système existant requise en Phase 1 MVP | Vérifier pas de dépendance email/ITSM actuels |

### 🔐 Contraintes

#### **Contraintes Techniques**
- 🖥️ **Infrastructure** : Cloud (AWS/Azure) recommandé pour scalabilité et sauvegarde automatique
- 🔌 **Compatibilité** : Support IE11+ (legacy Windows), Chrome, Firefox, Safari moderne
- 🔒 **Sécurité** : Chiffrement HTTPS obligatoire, authentification 2FA optionnelle future
- ⚡ **Performance** : Temps chargement page < 3s, SLA API 99.5% uptime
- 📊 **Data** : Capacité initiale 10 000 tickets, scalable à 100 000
- 🌐 **Intégrations** : Pas d'intégrations externes en MVP (email notifications only)

#### **Contraintes Organisationnelles**
- 👥 **Équipe projet** : 1 PM, 1 lead dev back, 1 lead dev front, 1 QA, 1 devops (équipe agile)
- 📆 **Timeline** : Go-live Phase 1 avant fin Q2 2026 (10 semaines max)
- 👨‍💼 **Sponsorship** : Directeur = sponsor principal (doit libérer 5h/mois pour ateliers)
- 📝 **Budget** : Non détaillé ici, estimation ~€80-120k pour Phase 1 (dev, infra, formation)
- 🎓 **Expertise** : Équipe dev avec expérience web modern (React/Vue recommandé) et APIs REST

#### **Contraintes Métier / Réglementaires**
- 📋 **RGPD** : Conformité européenne obligatoire (données personnelles employés)
- 🔐 **Confidentialité** : Employé voit seulement ses tickets (sauf manager > team)
- ⏰ **Heures d'ouverture** : Support IT de 8h-18h initialement (hors heures future scope)
- 🌍 **Single-language** : Français uniquement en MVP (anglais Phase 2)

---

---

# LIVRABLE 2 — FICHES PERSONAS DÉTAILLÉES

## Persona 1 — 👤 Laurent Rousseau, Directeur

### Profil de Base

| Attribut | Valeur |
|----------|--------|
| **Prénom / Rôle** | Laurent Rousseau, Directeur Général |
| **Âge** | 52 ans |
| **Ancienneté** | 8 ans dans le rôle, 20 ans dans l'entreprise |
| **Niveau technique** | ⭐ (1/5) — Très faible, utilise juste email et Excel |
| **Localisation** | Bureau principal |
| **Équipement** | Windows 10 + Outlook + Teams (utilisation basique) |

### 🎯 Objectifs Principaux

1. **Visibilité opérationnelle** — Avoir une vue d'ensemble temps réel de tous les incidents en cours et maîtriser les délais de résolution (ne plus se faire surprendre par une panne découverte par un client)
2. **Justifier les budgets IT** — Disposer de rapports concrets (volume incidents, coûts résolution, tendances) pour négocier budget et justifier éventuels recrutements auprès du board
3. **Sérénité de direction** — Dormir tranquille en sachant aucun incident n'est perdu, tout est tracé, et Référent IT travaille efficacement

### 😤 Frustrations & Points de Douleur

1. **Aucune visibilité** — Découvre parfois un problème IT quand c'est trop tard (client appelle directement). Référent IT dit "aucun problème" mais Laurent ne fait pas confiance aux chiffres verbaux
2. **Réunions improductives** — Doit demander chaque semaine à Référent IT le statut de 5-10 dossiers critiques, reçoit réponses vagues ("ça avance", "on regarde")
3. **Manque de données pour décisions** — Impossible de justifier auprès du board l'augmentation de charge IT, les heures supplémentaires, ou la nécessité d'un second technicien

### 💬 Citation Verbatim

> *"Je veux savoir en 30 secondes combien d'incidents critiques sont en attente, ça prend 3 réunions actuellement. Et quand j'appelle le Directeur Général du groupe, je veux avoir des chiffres, pas des impressions."*

### 📱 Habitudes Numériques

- ✅ Accès email quotidien 8h-18h + notifications mobiles (parfois le soir/weekend)
- ✅ Utilise Excel pour tracker projets personnels
- ✅ Rare sur navigateur, surtout intranet/email
- ❌ Ne lira jamais une doc de 10 pages
- ❌ Pas tech, délègue complètement infra à Référent IT

### ✅ Ce que TicketFlow doit faire pour Laurent

| Besoin | Fonctionnalité TicketFlow |
|--------|--------------------------|
| **Vue d'ensemble 1 clic** | Dashboard Directeur : nombre incidents ouvert/fermé, 5 plus critiques, statut général (🟢 OK / 🟠 Attention / 🔴 Critique) |
| **Rapports automatiques** | Rapport email hebdomadaire : 3-5 KPIs clés (MTTR, % SLA respectés, volume, tendances) |
| **Alertes escalade** | Notification immédiate si incident P1 non traité depuis > 2h |
| **Historique de tendances** | Graphique MTTR par mois, volume incidents par catégorie (pour justifier budgets) |
| **Aucune complexité UI** | Interface épurée, 3 clics max pour voir ce qu'il veut |

---

## Persona 2 — 👤 Mathieu Dupont, Référent Informatique

### Profil de Base

| Attribut | Valeur |
|----------|--------|
| **Prénom / Rôle** | Mathieu Dupont, Référent IT / Responsable Infrastructure |
| **Âge** | 38 ans |
| **Ancienneté** | 6 ans chez l'employeur, 12 ans en IT |
| **Niveau technique** | ⭐⭐⭐⭐⭐ (5/5) — Expert, utilise Linux, scripting, cloud, DevOps |
| **Localisation** | Bureau IT (backoffice) |
| **Équipement** | Windows 11 + Ubuntu VM + 3 écrans + Putty/VSCode/Postman |

### 🎯 Objectifs Principaux

1. **Organisé et priorisé** — Passer de traitement "au fil de l'eau" à une vraie priorisation méthodique (attaquer P1/P2 en priorité, puis P3, non l'inverse) pour optimiser son efficacité
2. **Déléguer confiance** — Disposer de données objectives pour défendre son temps (rapports d'activité, volume incidents) et justifier aide/consultant externe si nécessaire
3. **Moins de ping** — Réduire les emails/appels "ça avance ?" du Directeur et chercher "qui traite le ticket X". Utiliser ce temps pour vraiment résoudre

### 😤 Frustrations & Points de Douleur

1. **Chaos informel** — Incidents arrivent par 5 canaux (email, Slack, appels directs, verbal au café, post-it sur le bureau). Impossible de suivre tout, des tickets sont oubliés
2. **Priorisation inexistante** — Traite au fil de l'eau ce qui arrive en premier, pas ce qui est le plus critique. Résout des problems mineurs (prénom mal orthographié dans Outlook) avant une panne accès base de données
3. **Interrupteurs chroniques** — Arrêt constant du travail "productif" (maintenance, projets) pour répondre à des "j'ai besoin de vérifier que c'est bien un problème IT" → perte énorme de productivité
4. **Aucune traçabilité** — 6 mois après, ne peut pas retrouver qui a demandé quoi. Directeur demande "d'où vient ce coût ?", Mathieu ne peut pas justifier

### 💬 Citation Verbatim

> *"Je dois être détective le matin pour retrouver les demandes de la veille dans 4 boîtes mails différentes. Au lieu de vraiment dépanner, je passe 3h/jour à dire 'non je n'ai pas oublié, c'est dans mes notes quelque part'."*

### 📱 Habitudes Numériques

- ✅ Utilisateur avancé, aime les outils puissants avec raccourcis clavier
- ✅ Consultations fréquentes : 10+ fois/jour sur outils critiques
- ✅ Apprécie les APIs, webhooks, automations
- ⚠️ Peut être impatient face à UIs lentes ou peu logiques
- ❌ Ne fera jamais saisie manuelle si solution automatisée existe

### ✅ Ce que TicketFlow doit faire pour Mathieu

| Besoin | Fonctionnalité TicketFlow |
|--------|--------------------------|
| **Inbox consolidée** | Toutes les demandes (email, ticket) converties en tickets unifiés, pas de fragmentation |
| **Priorisation visuelle** | Vue liste avec tri priorité + SLA timer — voir en un coup d'oeil P1 overdue (rouge) |
| **Quick actions** | Affectation 1 clic, changement statut rapide, ajout notes sans saisie long |
| **Automations** | Escalade auto si P1 > 2h, rappel email si "en attente réponse" > 24h |
| **Statistiques objectives** | Temps moyen résolution par type, volume incidents, taux P1, pour justifier charge |
| **Affectation multi-user (futur)** | Quand embauche tier-1, pouvoir déléguer P3 facilement |

---

## Persona 3 — 👤 Isabelle Martin, Responsable Équipe Support Clients

### Profil de Base

| Attribut | Valeur |
|----------|--------|
| **Prénom / Rôle** | Isabelle Martin, Responsable Équipe Support Clients |
| **Âge** | 44 ans |
| **Ancienneté** | 4 ans dans le rôle, 10 ans chez l'employeur |
| **Niveau technique** | ⭐⭐ (2/5) — Utilisatrice "confortable" (Excel, Teams, CRM léger), non dev |
| **Localisation** | Bureau open-space équipe support (rez-de-chaussée) |
| **Équipement** | Windows 10 + Outlook + Teams + CRM interne |

### 🎯 Objectifs Principaux

1. **Coordonner facilement** — Quand un de ses 3 équipiers (EMP1, EMP2, EMP3) signale un problème, le transférer à IT sans perte d'info et suivre que c'est résolu (ne pas se faire reprocher "pourquoi mon équipe n'a pas ce qu'il faut !")
2. **Visibility équipe** — Savoir qui a des problèmes non résolus depuis longtemps impactant la productivité et aider IT à prioriser (ex: "EMP2 est bloqué depuis 3h sur un crash, c'est urgent")
3. **Justifier workload** — Pouvoir dire au Directeur "mon équipe perd 10h/semaine en problèmes IT" pour justifier recrutement ou investissements outils

### 😤 Frustrations & Points de Douleur

1. **Jeu du téléphone** — Quand EMP1 dit "mon PC est lent", Isabelle doit appeler Mathieu, réexpliquer le problème, puis envoyer email de confirmation. Perte de message et frustration pour tout le monde
2. **Aucun suivi** — Envoie un email à Mathieu "le PC de EMP2 crash au démarrage", puis rien. Après 3 jours : "c'est résolu ?", "ah oui je pense". Aucune certitude
3. **Équipiers impatients** — EMP1/2/3 demandent constantement "tu as des nouvelles ?". Isabelle n'a aucune réponse à donner, elle aussi est frustrée
4. **Manque d'importance** — Problèmes support clients pas pris au sérieux car IT ne voit pas l'impact métier ("c'est juste un accès", mais ça paralyse 3 personnes)

### 💬 Citation Verbatim

> *"Je dois jouer intermédiaire entre mes gens et Mathieu. Mais je n'ai aucun moyen de suivre vraiment ce qui se passe. Au bout de 2 jours, je dis 'ça avance ?' et Mathieu me dit 'oui' mais je sais pas si c'est vrai."*

### 📱 Habitudes Numériques

- ✅ Power user email, très active sur Teams
- ✅ Utilise Excel pour tracking projets équipe
- ✅ Accès régulier intranet, portails collaboratifs
- ✅ Confortable avec 2-3 onglets ouverts en parallèle
- ⚠️ Pas fan des interfaces trop "techniques", préfère simple et visuel
- ❌ Ne scripte pas, ne touche pas à config système

### ✅ Ce que TicketFlow doit faire pour Isabelle

| Besoin | Fonctionnalité TicketFlow |
|--------|--------------------------|
| **Créer au nom de** | Possibilité soumettre ticket pour EMP1/2/3 directement (formulaire simple : qui, quoi, priorité suggestion) |
| **Vue d'équipe** | Dashboard montrant les 3-5 tickets en cours de son équipe, statut, temps écoulé |
| **Notifications tracking** | Email/push quand ticket assigné à IT, quand statut change (en cours, résolu) |
| **Communication unifiée** | Thread de commentaires dans ticket pour dialogue Isabelle ↔ Mathieu (évite emails dispersés) |
| **Escalade simple** | Bouton "urgent, impact équipe" pour pousser P1 si vraiment critique |
| **Rapports équipe** | Rapport mensuel : combien incidents équipe, MTTR moyen, impact productivité |

---

## Persona 4 — 👤 Thomas Bernard, Employé Support Client

### Profil de Base

| Attribut | Valeur |
|----------|--------|
| **Prénom / Rôle** | Thomas Bernard, Employé Support Client (tier-1 téléphonique) |
| **Âge** | 28 ans |
| **Ancienneté** | 1.5 ans chez l'employeur, nouveau support |
| **Niveau technique** | ⭐ (1/5) — Très basique, savoir allumer/éteindre, utiliser applications bureautique |
| **Localisation** | Bureau open-space équipe support |
| **Équipement** | Windows 10 + Outlook + téléphone client + Firefox |

### 🎯 Objectifs Principaux

1. **Résoudre rapidement** — Juste que son problème soit réglé vite (< 1h idéalement) pour revenir au travail client, ne pas perdre sa productivité
2. **Savoir quoi il se passe** — Pas d'incertitude, vouloir suivi clair : "est-ce qu'on sait d'où ça vient ?", "ça prend combien de temps ?", "c'est quand c'est résolu ?"
3. **Ne pas déranger** — Veut signaler son problème 1x proprement et être tranquille. Pas 10 appels de suivi "c'est prêt ?" qui le dérangent au travail

### 😤 Frustrations & Points de Douleur

1. **Flou total** — Signale un problème PC à Isabelle, puis radio silence 4h. Ensuite "c'est en cours", puis rien. Après 2 jours, soudain "c'est bon". Thomas ne sait jamais où ça en est
2. **Perte productivité** — PC lent/crash, essaie de dépanner 30 min seul, puis appelle Isabelle. Appel non immédiat (elle occupe), puis email, puis attente. Perd 1h+ pour problème résolvable en 15 min
3. **Sentiment d'invisibilité** — Impression que son problème n'est pas important ("ce n'est qu'un PC"), vs priorité client, vs autres incidents IT. Se demande si c'est lui qui dérange
4. **Répétition d'explications** — Isabelle demande "décris le problème", puis Mathieu appelle "redécris ce qui se passe", puis "attends let me check"... fatigue

### 💬 Citation Verbatim

> *"Mon PC crashe, j'appelle Isabelle, elle dit 'je vais voir avec Mathieu', puis plus rien. 2h après on redémarre et magique c'est bon. Mais je saurais jamais ce qui s'est passé, ni combien de temps ça prenait. J'aurais pu travailler autrement."*

### 📱 Habitudes Numériques

- ✅ Utilisateur simple : email, navigateur, applications officielles
- ✅ Consulte l'intranet rarement mais peut le faire
- ✅ Très sur mobile/Teams hors travail (jeune, millénial)
- ⚠️ Pas fan de complications, veut intuitivité
- ❌ Très peu technique, ne troubleshootera pas seul

### ✅ Ce que TicketFlow doit faire pour Thomas

| Besoin | Fonctionnalité TicketFlow |
|--------|--------------------------|
| **Signalement simple** | Formulaire 1-2 min : "mon problème est..." avec catégories suggérées (PC lent, crash, accès, imprimante, etc.) |
| **Confirmation immédiate** | "Ticket #123 créé, Référent IT a été notifié" — voir numéro, date |
| **Suivi en temps réel** | Lien unique : statut actuel (Ouvert → En cours → Résolu), sans spam, pas besoin rafraîchir |
| **Notification changement** | Email/push simple quand statut change ou Mathieu ajoute commentaire |
| **Chat optionnel** | Possibilité ajouter info ("ça c'est pire", "j'ai rebooté c'est mieux") sans appel |
| **Satisfaction feedback** | À la fermeture : "était-ce utile ?" — petit vote, pas obligation |

---

---

# LIVRABLE 3 — QUESTIONS D'ATELIER DE CADRAGE

## 🎯 Guide d'Atelier de Cadrage TicketFlow

**Objectif de l'atelier** : Finaliser la vision et périmètre TicketFlow en consensus avec Directeur, Référent IT, et Responsable Équipe.  
**Durée recommandée** : 3 x 2h (6h total), répartis sur 2-3 semaines pour permettre préparation et réflexion.  
**Participants** : Directeur, Référent IT, Responsable Équipe, 1 Employé (représentant), 1 PM Facilitateur  
**Livrables attendus** : Document accord périmètre + SLA finalisés + liste risques validée

---

## Bloc 1 — Vision & Objectifs

### Q1.1 — Quelle est la définition du succès de TicketFlow pour vous, en 3-5 points clés ?

*[Facilitateur : Laisser chacun exprimer sa vision perso, puis aligner. Repérer s'il y a des objectifs conflictuels (ex: "zéro surcharge IT" vs "réponse immédiate 24/24h"]. Documenter verbatim.*

---

### Q1.2 — Quel dépassement de délai est acceptable pour un incident "normal" (priorité basse) ?

*[Facilitateur : Cible clarifier réalisme des SLA. Exemple : "48h c'est ok pour moi, mais 5 jours c'est trop" = consensus 2-3j. Référent IT doit valider c'est réaliste pour sa charge.*

---

### Q1.3 — Quels seraient les 3 indicateurs que vous regarderiez chaque semaine pour juger si TicketFlow fonctionne bien ?

*[Facilitateur : Valider KPIs doc (MTTR, % SLA, adoption). Ajouter points de vue chacun (Directeur = financier, Référent IT = efficacité, Responsable = satisfaction équipe)]*

---

### Q1.4 — Y a-t-il un risque majeur (technologique, organisationnel, budgétaire) qui pourrait bloquer le succès du projet ?

*[Facilitateur : Laisser exprimer inquiétudes non dites. Ex: "Référent IT démissionnaire risque", "Budget rejeté par board", "Pas de dev disponible". Documenter et inclure dans plan risques.]*

---

### Q1.5 — Quel est le délai de déploiement réaliste ? Tolérez-vous un délai "apprentissage" de 2-3 semaines post-go-live où tout n'est pas parfait ?

*[Facilitateur : S'aligner sur schedule. "Go-live 10 semaines c'est ok ?" "Vous acceptez pas 100% parfait jour 1 ?" Critère de succès "80% okej en semaine 2 c'est bon ?" vs "must-have 95% jour 1".*

---

## Bloc 2 — Utilisateurs & Usages

### Q2.1 — Qui sont les vrais utilisateurs ? Devons-nous supposer 100% des employés, ou seulement some departments/équipes ?

*[Facilitateur : Démarcation clair. "Seulement support clients ?", "Aussi back-office, finance ?", "Usine aussi ?". Scope définit implémentation (3 users vs 50). Valider représentativité.*

---

### Q2.2 — Quel est le volume incident ACTUEL par jour/semaine que vous gérez ?

*[Facilitateur : Demander chiffres (pas estimé), ou a minima fourchette : "20-30 par semaine", "5-10 max". Cela valide charge Référent IT et taille server.)*

---

### Q2.3 — Quelle est la distribution priorité incidents : grossièrement combien de P1, P2, P3 par semaine ?

*[Facilitateur : Ex : "5 P1 critiques, 15 P2 normales, 20 P3 mineures par semaine". Aide calibrer SLA et effort triage. Si tout "P1", alors SLA P1 unrealiste ou culture d'escalade trop agressive.*

---

### Q2.4 — Les employés doivent-ils accéder eux-mêmes à TicketFlow, ou seulement Responsable Équipe signale pour eux ?

*[Facilitateur : Choix d'architecture. "Tous les 30 font account ?" vs "Seulement 3 responsables/IT". Complexité support + adoption. Impact UX/onboarding.*

---

### Q2.5 — Y a-t-il des incidents qui arrivent de sources externes (clients, partenaires) ou seulement internes (employés) ?

*[Facilitateur : Scope futur multi-tenant. "Seulement employés internes" = more scope = phase future. Valider MVP simple = interne only.*

---

## Bloc 3 — Périmètre Fonctionnel

### Q3.1 — Les 5 catégories d'incidents proposées (Infrastructure, Accès, Logiciel, Matériel, Autre) vous conviennent-elles, ou en ajouteriez-vous d'autres ?

*[Facilitateur : Afficher la liste, demander si complète. Ajouter si besoin critique (ex: "Réseau" distinct de "Infra"). Enlever si redondant. Valider sera facile pour employé classifier.*

---

### Q3.2 — Doit-on supporter des "sous-tickets" (incident parent avec dépendances), ou 1 ticket = 1 incident atomique ?

*[Facilitateur : "Mon PC crash + imprimante crash, c'est 1 ou 2 tickets ?" Complexité vs clarté. MVP = ticket atomique, parent-child future.*

---

### Q3.3 — Quels champs optionnels faut-il demander employé au signalement ? (Titre, description, catégorie, contact, localisation PC, etc.)

*[Facilitateur : Aller-retour. Trop champs = abandon formulaire. Manque champs = infos perdues. Consensus "titre + description + catégorie" MVP, puis ajouts futures.*

---

### Q3.4 — Faut-il un système de statut personnalisé (Ouvert → En diagnostic → Patch en test → Patch en prod → Fermé) ou le simple 5-état suffira-t-il ? (Ouvert → En cours → En attente → Résolu → Fermé)

*[Facilitateur : Risk : over-engineering avec trop d'états. Valider "5-états simple c'est bon" vs "nous on a besoin de 8-états custom". Si custom, effort dev +15%, risque confusion utilisateurs.*

---

### Q3.5 — Dois-je supporter l'affectation à plusieurs personnes (ex: 2 techniciens co-owneront un ticket) ou 1 assigné par ticket ?

*[Facilitateur : "Mathieu et stagiaire travaillent ensemble ?" → co-assign futur nice-to-have. MVP = single assignee.*

---

### Q3.6 — Y a-t-il une intégration requise en Phase 1 (Teams, Slack, email entrant) ou email notifications out suffisent-elles ?

*[Facilitateur : Intégrations = coût dev. MVP non-intégré = email notifications seulement. Valider ok "aller consulter TicketFlow online" vs "tout en Teams".*

---

## Bloc 4 — Contraintes Techniques & Organisationnelles

### Q4.1 — Quel est votre budget estimé pour Phase 1 MVP ? Plage acceptable (€50k-150k, ex) ?

*[Facilitateur : Pas détails, juste ordres de grandeur. Aligner sur réalisme "bon, web app moderne = €80-120k", pas €10k. Identifier si contrainte budget bloque.*

---

### Q4.2 — Disposez-vous d'une équipe dev interne, ou faut-il recourir à prestataire externe ? Quelle est votre préférence et contrainte ?

*[Facilitateur : Interne = plus de contrôle, connaissance métier. Externe = coût clair, délai risqué si spéc mal faite. Budget cadrage accordingly.*

---

### Q4.3 — Quels sont les dépendances ou outils existants que TicketFlow DOIT intégrer (ITSM existant, email legacy, SSO, etc.) ?

*[Facilitateur : Inventorier integrations must-have. Si "zéro dépendance, greenfield", plus simple. Si "must connect à ITSM actuel", +40% complexité.*

---

### Q4.4 — Quels sont les critères de sécurité/conformité requis ? (RGPD, chiffrement, audit trail, authentification 2FA, etc.)

*[Facilitateur : Valider "RGPD standard" vs "HIPAA/ISO27001". Affecte architecture (+20% effort). Documenter must-have.*

---

### Q4.5 — Quel est le plan de support et maintenance post-déploiement ? (In-house vs prestataire, SLA uptime, backup, disaster recovery?)

*[Facilitateur : "Qui support en prod ?" Impact planning long-terme. Si prestataire continue, budget opérationnel inclure. Contractualiser tôt.*

---

## Bloc 5 — Priorisation & Définition du MVP

### Q5.1 — Parmi les 12 fonctionnalités proposées, lesquelles sont must-have MVP (impératif semaine 10) vs nice-to-have Phase 2 ?

*[Facilitateur : Montrer matrice. "F1-F8 MVP, F9-F12 Phase 2 ?" Laisser discuter. Exemple : "rapports = Phase 2 acceptable", "notifications = MVP must-have". Consensus par vote si blocage.]*

---

### Q5.2 — Quel est le nombre d'utilisateurs minimum pour considérer le MVP un succès au go-live ? (% adoption target : 50%, 75%, 90% ?)

*[Facilitateur : Réaliste. "75% actifs semaine 1 c'est ok ?" vs "must be 95%". Adoption phased = normal, 100% jour 1 = fantasme. Définir critère succès.*

---

### Q5.3 — En cas de débordement timeline (risque retard +3 semaines), êtes-vous prêts à réduire le périmètre MVP (ex: supprimer notifications, ou single-role vs multi-role) ou c'est go-live hard deadline ?

*[Facilitateur : "Lancer en Phase 1 sans rapports c'est ok, puis ajouter ?" vs "rapports inclus ou pas de go-live". Flexibility vs rigidity. Documenter arbitrage.*

---

### Q5.4 — Qui va être champion de change / super-user pour chaque rôle (1 directeur, 1 IT, 1 équipe, 1 employé) pour aider formation et adoption post-déploiement ?

*[Facilitateur : "Isabelle champion pour support ?" "Quelqu'un chez IT pour supp IT ?". Identifier noms/rôles. Inclure dans plan formation + communication.*

---

---

## 📋 Checklist Post-Atelier

À l'issue des 3 sessions, s'assurer d'avoir validé :

- ☑️ **Vision unifiée** : 3-5 objectifs clairs, sign-off Directeur + Référent IT
- ☑️ **Périmètre MVP** : Liste 8+ fonctionnalités à livrer semaine 10
- ☑️ **Utilisateurs** : Population identifiée (nombre, rôles, localisation)
- ☑️ **SLA finalisés** : P1/P2/P3 agreed, accepté par Référent IT comme réaliste
- ☑️ **KPIs** : Minimum 5 mesurables, baseline et targets définies
- ☑️ **Risques priorisés** : 5-6 principaux risks + mitigation plans documentés
- ☑️ **Budget & timeline** : Plage budget, date target go-live semaine 10
- ☑️ **Équipe & sponsor** : Directeur valide comme sponsor, Référent IT engage
- ☑️ **Champions de change** : 1 par rôle identifié pour post-go-live
- ☑️ **Hypothèses documentées** : Chiffres cités + validations prévues

---

---

## 📌 Note Finale du Consultant

Ce document de **cadrage complet TicketFlow** synthétise :

✅ **LIVRABLE 1** — Project Brief professionnelle (7 sections, risques, planning, KPIs)  
✅ **LIVRABLE 2** — 4 fiches personas détaillées (Directeur, IT, Responsable, Employé)  
✅ **LIVRABLE 3** — 25 questions d'atelier organisées en 5 blocs thématiques (vision, users, scope, constraints, prioritization)

**Prochaines étapes recommandées** :

1. **Semaine 1** : Présentation brief + personas aux stakeholders (1h)
2. **Semaines 2-4** : Ateliers cadrage (3 x 2h réparties)
3. **Semaine 5** : Consolidation accord, freeze périmètre MVP
4. **Semaines 6-15** : Développement + déploiement Phase 1
5. **Semaine 16+** : Post-go-live support + planification Phase 2

**Contact facilitateur** : Disponible pour itérations et questions clarification au cours ateliers.

---

*Document généré : 2026-03-09 | Consultant Senior Product Manager*
