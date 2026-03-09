
# 🏗️ TicketFlow — Documentation Technique Complète

**Version:** 1.0  
**Date:** 2026-03-09  
**Auteur:** Tech Lead Senior (Java/Spring Boot + React TypeScript)  
**Statut:** Approuvé pour Développement

---

## 📑 Table des Matières

1. [Livrable 1 : Exigences Fonctionnelles (F01-F06)](#livrable-1--exigences-fonctionnelles-f01-à-f06)
2. [Livrable 2 : Exigences Techniques (T01-T06)](#livrable-2--exigences-techniques-t01-à-t06)
3. [Livrable 3 : Matrice de Droits RBAC](#livrable-3--matrice-de-droits-rbac)
4. [Livrable 4 : Definition of Done (DoD)](#livrable-4--definition-of-done-dod)

---

# LIVRABLE 1 — EXIGENCES FONCTIONNELLES (F01 À F06)

## F01 — Authentification & Gestion de Session

| Champ | Contenu |
|---|---|
| **ID** | F01 |
| **Titre** | Authentification & Gestion de session utilisateur |
| **Description** | Système d'authentification sécurisé permettant aux utilisateurs de se connecter avec leurs identifiants (email + mot de passe), de maintenir une session active via JWT, et de se déconnecter. Le système gère la génération de tokens, leur validation, l'expiration de session, et la redirection automatique selon le rôle utilisateur. |
| **Acteurs concernés** | DIRECTEUR, REFERENT_IT, RESPONSABLE, EMPLOYE |
| **Critères d'acceptation** | 1. L'utilisateur peut se connecter avec un email et mot de passe valides<br>2. Un token JWT est généré avec durée de vie de 24h (access token) et 7j (refresh token)<br>3. Le token est automatiquement injecté dans les headers Authorization de chaque requête API<br>4. Les tokens expirés sont rejetés (401 Unauthorized)<br>5. L'utilisateur est redirigé vers la page appropriée selon son rôle (dashboard directeur, liste tickets IT, etc.)<br>6. La déconnexion invalide le token et efface les données du navigateur (localStorage/cookies)<br>7. Les tentatives de connexion échouées après 5 essais verrouillent le compte pour 15 minutes<br>8. Les mots de passe sont hashés avec BCrypt (strength 12) en base de données |
| **Priorité** | MUST |
| **Complexité** | HIGH |

---

## F02 — Création et soumission d'un ticket

| Champ | Contenu |
|---|---|
| **ID** | F02 |
| **Titre** | Création et soumission d'un ticket incident |
| **Description** | Les utilisateurs (EMPLOYE, RESPONSABLE) peuvent créer un nouveau ticket en remplissant un formulaire. Le ticket reçoit un numéro unique auto-généré, est stocké en base de données, et une notification email est envoyée au Référent IT. L'historique de création est enregistré avec timestamp et données de l'auteur. |
| **Acteurs concernés** | EMPLOYE (crée ses propres tickets), RESPONSABLE (crée pour son équipe), REFERENT_IT (consulte) |
| **Critères d'acceptation** | 1. Le formulaire inclut obligatoirement : titre, description, catégorie (enum : INFRASTRUCTURE, ACCES, LOGICIEL, MATERIEL, AUTRE), priorité suggérée (BASSE/NORMALE/HAUTE)<br>2. Les pièces jointes sont optionnelles (max 5 MB, formats : PDF, JPG, PNG, DOCX)<br>3. Un numéro de ticket unique est auto-généré au format TF-YYYY-XXXX (ex: TF-2026-0001)<br>4. Le ticket est créé avec le statut initial : OUVERT<br>5. L'auteur du ticket reçoit une confirmation email avec le numéro et un lien vers le suivi<br>6. Le Référent IT reçoit une notification email avec titre, description et priorité<br>7. Le timestamp de création et l'ID de l'auteur sont enregistrés automatiquement<br>8. Le formulaire validé côté client ET côté serveur (côté serveur = source de vérité)<br>9. Un utilisateur RESPONSABLE peut créer un ticket au nom d'un de ses équipiers (champ "pour qui ?")<br>10. Les validations des champs sont claires (messages d'erreur explicites) |
| **Priorité** | MUST |
| **Complexité** | MEDIUM |

---

## F03 — Consultation et suivi des tickets

| Champ | Contenu |
|---|---|
| **ID** | F03 |
| **Titre** | Consultation et suivi des tickets (lecture et filtrage) |
| **Description** | Chaque rôle peut consulter les tickets qui lui sont accessibles via une vue liste avec filtres avancés et une vue détail complète. Les permissions sont enforced au backend (RBAC). Chaque utilisateur voit son ticket historique complet avec tous les commentaires, modifications de statut, et actions effectuées. Possibilité d'exporter un ticket en PDF pour archivage. |
| **Acteurs concernés** | DIRECTEUR (tous les tickets), REFERENT_IT (tous les tickets), RESPONSABLE (tickets de son équipe + ses propres), EMPLOYE (ses propres tickets uniquement) |
| **Critères d'acceptation** | 1. Vue liste affiche tous les tickets accessibles à l'utilisateur avec colonnage : N° ticket, titre, statut (couleur codée), priorité, auteur, assigné à, date création<br>2. Filtres disponibles : par statut, par priorité, par catégorie, par date (range), par auteur, par assigné<br>3. Tri possible sur chaque colonne (ascendant/descendant)<br>4. Pagination : 20 items par page, navigation fluide<br>5. Vue détail affiche : numéro, titre, description, catégories, priorité, statut, dates (création, dernière modif), auteur, assigné, pièces jointes, historique complet des changements<br>6. Historique chronologique : chaque changement (statut, assignation, ajout commentaire) est tracé avec timestamp, auteur, avant/après<br>7. Export PDF unique du ticket : mise en page professionnelle avec toutes les informations visibles à l'écran<br>8. Les permissions RBAC sont validées à chaque requête (backend enforce, frontend cache si possible)<br>9. Les tickets "résolvés" depuis > 90j sont archivés automatiquement (marqués ARCHIVÉ, masqués de la liste par défaut)<br>10. Un utilisateur ne peut jamais voir les tickets d'un autre utilisateur sauf s'il en a la permission (RESPONSABLE voit équipe, REFERENT_IT/DIRECTEUR voient tout) |
| **Priorité** | MUST |
| **Complexité** | MEDIUM |

---

## F04 — Mise à jour du statut et assignation

| Champ | Contenu |
|---|---|
| **ID** | F04 |
| **Titre** | Mise à jour du statut et assignation d'un ticket |
| **Description** | Le Référent IT (et seulement lui en MVP) peut modifier le statut d'un ticket selon le workflow défini, assigner un ticket à lui-même ou à un collègue (futur), ajouter des commentaires publics (visibles par l'auteur) ou internes (visibles seulement IT), et demander des informations complémentaires à l'auteur via le système de commentaires. Chaque action est tracée dans l'historique. |
| **Acteurs concernés** | REFERENT_IT (change statut, assigne, commente), RESPONSABLE/EMPLOYE (lisent les commentaires, peuvent répondre), DIRECTEUR (voit en lecture seule) |
| **Critères d'acceptation** | 1. Workflow de statuts implémenté : OUVERT → EN_COURS → EN_ATTENTE → RESOLU → FERME (avec possibilité rouvrir d'EN_ATTENTE à EN_COURS)<br>2. Seul le REFERENT_IT peut changer le statut (pas RESPONSABLE/EMPLOYE)<br>3. Au passage en EN_ATTENTE : notification email à l'auteur demandant des infos complémentaires<br>4. Au passage en RESOLU : notification email à l'auteur que le problème est résolu<br>5. Au passage en FERME : confirmation que le ticket est définitivement fermé (pas de réouverture possible)<br>6. Un ticket en FERME depuis 7j sans action peut être fermé automatiquement (cron job ou event listener)<br>7. Assignation : le REFERENT_IT peut assigner à lui-même ou à un autre technicien (futur multi-assign)<br>8. À chaque assignation, notification email au technicien assigné<br>9. Commentaires "publics" (visibles par l'auteur) sont distincts des commentaires "internes" (IT only)<br>10. Chaque commentaire inclut : texte, auteur, timestamp, type (public/interne)<br>11. L'auteur du ticket peut répondre aux commentaires publics dans un thread<br>12. L'historique enregistre : ancien statut, nouveau statut, auteur du changement, timestamp, raison (optionnelle)<br>13. Un timer de SLA s'affiche en rouge sur chaque ticket P1 si > 2h, P2 si > 4h, P3 si > 24h sans changement<br>14. Aucune action possible sur un ticket FERME (sauf consultation) |
| **Priorité** | MUST |
| **Complexité** | HIGH |

---

## F05 — Tableau de bord et reporting

| Champ | Contenu |
|---|---|
| **ID** | F05 |
| **Titre** | Tableau de bord (dashboard) et reportings |
| **Description** | Chaque rôle accède à un tableau de bord personnalisé affichant les KPIs temps réel pertinents pour son rôle. Les dashboards incluent des graphiques (bar chart, pie chart, line chart), des filtres temporels, et la possibilité d'exporter les rapports en PDF ou CSV. Les données sont actualisées en temps réel (auto-refresh toutes les 5 min ou réquête manuelle). |
| **Acteurs concernés** | DIRECTEUR (vue globale), REFERENT_IT (vue opérationnelle), RESPONSABLE (vue équipe), EMPLOYE (vue personnelle) |
| **Critères d'acceptation** | **DIRECTEUR dashboard :**<br>1. Nombre total de tickets : ouvert, en cours, résolu, fermé<br>2. MTTR (Mean Time To Resolution) global en heures<br>3. % de tickets ayant respecté leur SLA<br>4. Top 5 incidents ouverts depuis le plus longtemps<br>5. Volume incidents par catégorie (pie chart)<br>6. Évolution volume incidents par semaine (line chart)<br><br>**REFERENT_IT dashboard :**<br>7. Liste de mes tickets assignés (filtres : status, priorité)<br>8. Tickets OVERDUE (SLA dépassé)<br>9. Tickets EN_ATTENTE (en attente réponse auteur)<br>10. MTTR pour mes tickets résolus<br>11. Volume incidents par catégorie et par statut<br><br>**RESPONSABLE dashboard :**<br>12. Incidents de mon équipe : nombre, statut, MTTR<br>13. Employés bloqués (ticket ouvert > 4h)<br>14. Productivity loss : estimation heures perdues / semaine<br><br>**EMPLOYE dashboard :**<br>15. Mes tickets avec statut et date de création<br>16. Dernier commentaire du IT sur chaque ticket<br><br>**Commun à tous :**<br>17. Filtres par période : cette semaine, ce mois, ce trimestre, custom (date picker)<br>18. Possibilité exporter rapport en PDF (mise en page professionnelle) et CSV (données brutes)<br>19. Auto-refresh données toutes les 5 minutes<br>20. Aucun utilisateur ne voit des KPIs pour lesquels il n'a pas accès (EMPLOYE ne voit que ses stats) |
| **Priorité** | SHOULD (MVP sans graphiques complexes, ajout Phase 2) |
| **Complexité** | HIGH |

---

## F06 — Administration et gestion des utilisateurs

| Champ | Contenu |
|---|---|
| **ID** | F06 |
| **Titre** | Administration et gestion des utilisateurs (CRUD users + rôles) |
| **Description** | L'administrateur (DIRECTEUR, REFERENT_IT) peut créer, lire, modifier et supprimer les comptes utilisateurs, assigner ou modifier les rôles RBAC, activer/désactiver les accès, et consulter un journal d'audit de toutes les actions administratives. Cette section garantit la gouvernance des accès et la traçabilité. |
| **Acteurs concernés** | DIRECTEUR (admin complet), REFERENT_IT (admin limité : création/mod/désactivation, pas suppression), autres rôles (lecture seule) |
| **Critères d'acceptation** | 1. Interface d'administration accessible seulement à DIRECTEUR et REFERENT_IT (avec permissions granulaires)<br>2. Vue liste de tous les utilisateurs : nom, email, rôle, statut (actif/inactif), date création, dernière connexion<br>3. Création d'utilisateur : formulaire avec email, nom, prénom, rôle, statut initial (actif par défaut)<br>4. Email de bienvenue envoyé avec lien de réinitialisation de mot de passe (valide 24h)<br>5. Modification utilisateur : changement de rôle, activation/désactivation, modification nom/prénom<br>6. Suppression logique (pas de suppression physique) : l'utilisateur est marqué SUPPRIME, ses tickets demeurent avec "auteur inconnu"<br>7. Réactivation d'un compte désactivé possible (remet en état ACTIF)<br>8. Journal d'audit exhaustif : qui a créé/modifié/supprimé quel utilisateur, quand, quoi a changé (ancien rôle → nouveau rôle, etc.)<br>9. Audit accessible uniquement à DIRECTEUR en lecture seule<br>10. Pagination et filtres : par rôle, par statut (actif/inactif), par date<br>11. Recherche par email ou nom<br>12. Export de la liste utilisateurs en CSV (données de base, pas les mots de passe)<br>13. Impossibilité de supprimer le dernier administrateur système<br>14. Aucun utilisateur ne peut modifier son propre rôle (même s'il est admin) |
| **Priorité** | MUST |
| **Complexité** | MEDIUM |

---

---

# LIVRABLE 2 — EXIGENCES TECHNIQUES (T01 À T06)

## T01 — Architecture REST & Sécurité Spring Boot

| Champ | Contenu |
|---|---|
| **ID** | T01 |
| **Titre** | Architecture REST & Sécurité Spring Boot 3 |
| **Objectif** | Garantir une architecture REST cohérente, maintenable, sécurisée et documentée pour l'ensemble de l'API TicketFlow. |
| **Implémentation technique** | **Structure des packages :**<br><pre>com.ticketflow<br>├── controller/      (REST endpoints, @RestController)<br>├── service/         (logique métier, @Service)<br>├── repository/      (accès données, @Repository)<br>├── entity/          (JPA entities, @Entity)<br>├── dto/             (Data Transfer Objects, records)<br>├── mapper/          (mappers entity ↔ DTO, interfaces)<br>├── exception/       (exceptions custom, handlers)<br>├── config/          (Spring configuration, @Configuration)<br>├── security/        (Spring Security config, filters)<br├── utils/           (utilitaires, helpers)<br>└── TicketflowApplication.java</pre><br><br>**Annotations et conventions :**<br>- @RestController sur tous les controllers<br>- @RequestMapping("/api/v1/{ressource}") pour versioning<br>- Tous les endpoints retournent des DTOs (jamais directement l'Entity)<br>- @Valid sur tous les @RequestBody<br>- @ExceptionHandler global avec @ControllerAdvice<br>- Logging avec SLF4J (LoggerFactory.getLogger)<br>- Pas de System.out.println |
| **Contraintes spécifiques** | 1. Version API : /api/v1/ uniquement en MVP (pas de /api/v2)<br>2. Codes HTTP standardisés : 200 (OK), 201 (CREATED), 204 (NO CONTENT), 400 (BAD REQUEST), 401 (UNAUTHORIZED), 403 (FORBIDDEN), 404 (NOT FOUND), 409 (CONFLICT), 500 (INTERNAL ERROR)<br>3. Tous les endpoints doivent retourner une réponse structurée (pas de strings nues)<br>4. Messages d'erreur multilingues : clés i18n (fr, en future)<br>5. Validation déclarative (annotations) prioritaire sur validation programmatique<br>6. Aucune logique métier dans les controllers (controller = orchestration seulement)<br>7. Transactions gérées au niveau service (@Transactional)<br>8. Aucun endpoint sans authentification (sauf /api/v1/auth/login et /api/v1/auth/register) |
| **Critères de validation** | 1. Tous les endpoints retournent un objet ResponseEntity ou ApiResponse structuré<br>2. Exécution de tests d'intégration couvrant tous les codes HTTP<br>3. Swagger/OpenAPI généré automatiquement et accessible via /swagger-ui.html<br>4. Audit du code : pas d'imports inutilisés, pas de variables non-utilisées<br>5. SonarQube : couverture de code ≥ 80% backend, 0 issues "blocker" ou "critical"<br>6. Performance : GET /api/v1/tickets < 300ms pour 100 tickets, POST /api/v1/tickets < 500ms |

---

## T02 — Authentification JWT avec Spring Security

| Champ | Contenu |
|---|---|
| **ID** | T02 |
| **Titre** | Authentification JWT avec Spring Security 6 |
| **Objectif** | Implémenter un système d'authentification sécurisé, stateless, basé sur JWT avec refresh tokens et contrôle d'accès granulaire par rôle. |
| **Implémentation technique** | **Configuration Spring Security :**<br><pre>@Configuration<br>@EnableWebSecurity<br>public class SecurityConfig {<br>  @Bean<br>  public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {<br>    http.csrf().disable()<br>       .cors(cors -> cors.configurationSource(...))<br>       .authorizeRequests()<br>         .antMatchers("/api/v1/auth/**").permitAll()<br>         .anyRequest().authenticated()<br>       .and()<br>       .addFilterBefore(jwtAuthenticationFilter(), UsernamePasswordAuthenticationFilter.class)<br>       .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);<br>    return http.build();<br>  }<br>}</pre><br><br>**JwtTokenProvider :**<br><pre>@Component<br>public class JwtTokenProvider {<br>  private final String SECRET_KEY = env.getProperty("jwt.secret");<br>  private final long EXPIRATION_TIME = 86400000; // 24h<br>  <br>  public String generateToken(User user) {<br>    // GenerateToken avec userId, email, roles<br>    return Jwts.builder()<br>      .setSubject(user.getId())<br>      .claim("email", user.getEmail())<br>      .claim("roles", user.getRoles())<br>      .setIssuedAt(new Date())<br>      .setExpiration(new Date(System.currentTimeMillis() + EXPIRATION_TIME))<br>      .signWith(SignatureAlgorithm.HS512, SECRET_KEY)<br>      .compact();<br>  }<br>  <br>  public boolean validateToken(String token) {<br>    // Valider signature + expiration<br>  }<br>  <br>  public String extractUserId(String token) {<br>    return Jwts.parser().setSigningKey(SECRET_KEY)<br>      .parseClaimsJws(token).getBody().getSubject();<br>  }<br>}</pre><br><br>**JwtAuthenticationFilter :**<br><pre>@Component<br>public class JwtAuthenticationFilter extends OncePerRequestFilter {<br>  @Override<br>  protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)<br>    throws ServletException, IOException {<br>    String token = extractToken(request);<br>    if (token != null && jwtTokenProvider.validateToken(token)) {<br>      String userId = jwtTokenProvider.extractUserId(token);<br>      // Set authentication in SecurityContext<br>      SecurityContextHolder.getContext().setAuthentication(auth);<br>    }<br>    filterChain.doFilter(request, response);<br>  }<br>}</pre><br><br>**Endpoints d'authentification :**<br>- POST /api/v1/auth/login → { email, password } → { accessToken, refreshToken }<br>- POST /api/v1/auth/refresh → { refreshToken } → { accessToken }<br>- POST /api/v1/auth/logout → invalide le refresh token |
| **Contraintes spécifiques** | 1. Access token : 24 heures de durée de vie<br>2. Refresh token : 7 jours de durée de vie<br>3. Token stockage : Authorization header "Bearer {token}" (client décide cookie vs header)<br>4. Refresh token révoqué après utilisation (one-time use)<br>5. Secret key : minimum 256 bits, stocké en variable d'environnement (jamais en code)<br>6. PasswordEncoder : BCryptPasswordEncoder(12) pour hashing<br>7. Tentatives échouées : max 5 / 15 minutes (brute-force protection)<br>8. Headers de sécurité obligatoires : X-Content-Type-Options, X-Frame-Options, HSTS<br>9. CORS : domaines whitelist explicites (jamais "*")<br>10. Tokens jamais exposés en logs |
| **Critères de validation** | 1. Un token expiré est rejeté (401)<br>2. Un token invalide (mauvaise signature) est rejeté (401)<br>3. Un endpoint sans token retourne 401<br>4. Un endpoint avec token de rôle insuffisant retourne 403<br>5. @PreAuthorize sur chaque endpoint doit matcher les besoins métier<br>6. Tests : LoginControllerTest couvre au moins 10 scénarios (login ok, bad password, account disabled, token expiration, refresh token ok/ko)<br>7. Aucun token loggé en base ou logs<br>8. Performance : /api/v1/auth/login < 300ms (hash check + DB query) |

---

## T03 — Persistance des données avec Spring Data JPA

| Champ | Contenu |
|---|---|
| **ID** | T03 |
| **Titre** | Persistance des données avec Spring Data JPA & PostgreSQL 15 |
| **Objectif** | Implémenter la couche de persistance avec JPA, assurer la cohérence des données, faciliter les migrations schéma, et optimiser les performances d'accès. |
| **Implémentation technique** | **Entités principales :**<br><pre>@Entity<br>@Table(name = "users")<br>@Data<br>public class User {<br>  @Id<br>  @GeneratedValue(strategy = GenerationType.UUID)<br>  private String id;<br>  <br>  @Column(nullable = false, unique = true)<br>  private String email;<br>  <br>  @Column(nullable = false)<br>  private String passwordHash;<br>  <br>  @ElementCollection(targetClass = Role.class)<br>  @Enumerated(EnumType.STRING)<br>  @CollectionTable(name = "user_roles")<br>  @Column(name = "role")<br>  private Set<Role> roles;<br>  <br>  @OneToMany(mappedBy = "author", cascade = CascadeType.ALL)<br>  private List<Ticket> ticketsCreated;<br>  <br>  @CreatedDate<br>  @Column(nullable = false, updatable = false)<br>  private LocalDateTime createdAt;<br>  <br>  @LastModifiedDate<br>  private LocalDateTime updatedAt;<br>}</pre><br><br><pre>@Entity<br>@Table(name = "tickets")<br>@Data<br>public class Ticket {<br>  @Id<br>  @GeneratedValue(strategy = GenerationType.SEQUENCE)<br>  private Long id;<br>  <br>  @Column(nullable = false, unique = true)<br>  private String ticketNumber; // TF-2026-XXXX<br>  <br>  @ManyToOne(fetch = FetchType.LAZY)<br>  @JoinColumn(name = "author_id", nullable = false)<br>  private User author;<br>  <br>  @ManyToOne(fetch = FetchType.LAZY)<br>  @JoinColumn(name = "assigned_to_id")<br>  private User assignedTo;<br>  <br>  @Enumerated(EnumType.STRING)<br>  @Column(nullable = false)<br>  private TicketStatus status;<br>  <br>  @Enumerated(EnumType.STRING)<br>  @Column(nullable = false)<br>  private Priority priority;<br>  <br>  @OneToMany(mappedBy = "ticket", cascade = CascadeType.ALL)<br>  private List<Comment> comments;<br>  <br>  @CreatedDate<br>  @Column(updatable = false)<br>  private LocalDateTime createdAt;<br>  <br>  @LastModifiedDate<br>  private LocalDateTime updatedAt;<br>}</pre><br><br>**Repositories :**<br><pre>@Repository<br>public interface TicketRepository extends JpaRepository<Ticket, Long> {<br>  @Query("SELECT t FROM Ticket t WHERE t.author.id = :userId " +<br>         "ORDER BY t.createdAt DESC")<br>  Page<Ticket> findByAuthorId(@Param("userId") String userId, Pageable pageable);<br>  <br>  @Query("SELECT t FROM Ticket t WHERE t.status = :status")<br>  List<Ticket> findByStatus(@Param("status") TicketStatus status);<br>  <br>  Optional<Ticket> findByTicketNumber(String ticketNumber);<br>}</pre><br><br>**Migration Flyway :**<br><pre>-- V1__init.sql<br>CREATE TABLE users (<br>  id UUID PRIMARY KEY,<br>  email VARCHAR(255) NOT NULL UNIQUE,<br>  password_hash VARCHAR(255) NOT NULL,<br>  created_at TIMESTAMP NOT NULL,<br>  updated_at TIMESTAMP<br>);<br><br>CREATE TABLE tickets (<br>  id BIGSERIAL PRIMARY KEY,<br>  ticket_number VARCHAR(20) NOT NULL UNIQUE,<br>  author_id UUID NOT NULL,<br>  assigned_to_id UUID,<br>  status VARCHAR(50) NOT NULL,<br>  priority VARCHAR(50) NOT NULL,<br>  created_at TIMESTAMP NOT NULL,<br>  updated_at TIMESTAMP,<br>  FOREIGN KEY (author_id) REFERENCES users(id),<br>  FOREIGN KEY (assigned_to_id) REFERENCES users(id)<br>);</pre> |
| **Contraintes spécifiques** | 1. Pas de SQL natif sauf cas exceptionnel avec @Query(nativeQuery=true) justifié en commentaire<br>2. Toutes les relations critiques doivent avoir fetch = FetchType.LAZY (evite N+1 problem)<br>3. Indexes sur les colonnes fréquemment filtrées/triées : status, priority, author_id, assigned_to_id<br>4. Audit automatique : @CreatedDate, @LastModifiedDate via @EnableJpaAuditing<br>5. Constraints de BD enforced : NOT NULL, UNIQUE, FOREIGN KEY<br>6. Migrations : fichiers nommés V{version}__{description}.sql (Flyway)<br>7. Aucune donnée sensible en texte clair : passwords hashés, données perso chiffrées si nécessaire<br>8. Backup automatique nuit (hors scope MVP mais architecture compatible)<br>9. Pagination obligatoire sur tous les GET list (pas de retour de 10 000 records)<br>10. Transactions gérées au niveau service, pas controller |
| **Critères de validation** | 1. Migration Flyway exécutée automatiquement au démarrage (zéro action manuel)<br>2. Tests d'intégration JPA : insertion, lecture, modification, suppression logique<br>3. Tests d'unicité : creation d'emails ou ticket numbers dupliqués échouent<br>4. Performance : pagination 20 items < 100ms<br>5. Audit trail : tous les changements tracés avec timestamp + utilisateur<br>6. Aucune perte de données : suppression logique (soft delete) par défaut<br>7. Migrations réversibles (DOWN scripts V{version}__undo.sql pour rollback) |

---

## T04 — Frontend React 18 TypeScript

| Champ | Contenu |
|---|---|
| **ID** | T04 |
| **Titre** | Frontend React 18 + TypeScript + Tailwind CSS |
| **Objectif** | Construire une Single Page Application (SPA) moderne, type-safe, performante, accessible, avec routing basé sur les rôles et gestion d'état centralisée. |
| **Implémentation technique** | **Structure des dossiers :**<br><pre>src/<br>├── components/        (réutilisables : Button, Modal, Card)<br>├── pages/            (pages complètes : Dashboard, TicketList, LoginPage)<br>├── hooks/            (custom hooks : useAuth, useTickets, useFetch)<br>├── services/         (API calls : ticketService, userService)<br>├── store/            (Zustand stores : authStore, ticketStore)<br>├── types/            (TypeScript interfaces & types)<br>├── utils/            (helpers : formatDate, validateEmail)<br>├── styles/           (global CSS, Tailwind config)<br>├── middleware/       (API interceptors, errorHandling)<br>└── App.tsx</pre><br><br>**Authentification :**<br><pre>// services/authService.ts<br>export const login = async (email: string, password: string) => {<br>  const response = await axios.post('/api/v1/auth/login', { email, password });<br>  const { accessToken, refreshToken } = response.data;<br>  localStorage.setItem('accessToken', accessToken);<br>  localStorage.setItem('refreshToken', refreshToken);<br>  return response.data;<br>};<br><br>// middleware/axiosInterceptor.ts<br>axiosInstance.interceptors.request.use((config) => {<br>  const token = localStorage.getItem('accessToken');<br>  if (token) {<br>    config.headers.Authorization = `Bearer ${token}`;<br>  }<br>  return config;<br>});<br><br>axiosInstance.interceptors.response.use(<br>  (response) => response,<br>  async (error) => {<br>    if (error.response.status === 401) {<br>      const refreshToken = localStorage.getItem('refreshToken');<br>      const newToken = await refreshAccessToken(refreshToken);<br>      localStorage.setItem('accessToken', newToken);<br>      // Retry original request<br>    }<br>    return Promise.reject(error);<br>  }<br>);</pre><br><br>**State Management (Zustand) :**<br><pre>// store/authStore.ts<br>interface AuthStore {<br>  user: User | null;<br>  isAuthenticated: boolean;<br>  login: (email: string, password: string) => Promise<void>;<br>  logout: () => void;<br>}</pre><br><br>**Routing avec RBAC :**<br><pre>// ProtectedRoute.tsx<br>const ProtectedRoute: React.FC<ProtectedRouteProps> = ({ element, requiredRoles }) => {<br>  const { user } = useAuthStore();<br>  <br>  if (!user) return <Navigate to="/login" />;<br>  if (!requiredRoles.includes(user.role)) return <Navigate to="/unauthorized" />;<br>  <br>  return element;<br>};</pre><br><br>**Validation de formulaires (React Hook Form + Zod) :**<br><pre>const TicketFormSchema = z.object({<br>  title: z.string().min(5),<br>  description: z.string().min(10),<br>  category: z.enum(['INFRASTRUCTURE', 'ACCES', 'LOGICIEL', 'MATERIEL', 'AUTRE']),<br>  priority: z.enum(['BASSE', 'NORMALE', 'HAUTE']),<br>});<br><br>const { register, handleSubmit, formState: { errors } } = useForm<<br>  z.infer<typeof TicketFormSchema><br>>({ resolver: zodResolver(TicketFormSchema) });</pre> |
| **Contraintes spécifiques** | 1. TypeScript strict mode : no "any", interfaces explicites pour tous les modèles<br>2. Components fonctionnels avec hooks (jamais class components)<br>3. Props typées avec TypeScript (interface Props { ... })<br>4. Aucune donnée sensible en localStorage sauf tokens (et tokens à TTL court)<br>5. CORS : requests autorisés seulement vers /api/v1 et /auth<br>6. Dark mode support : Tailwind dark: prefix<br>7. Accessibilité WCAG 2.1 AA minimum : alt text, ARIA labels, keyboard navigation<br>8. Mobile responsive : breakpoints Tailwind (sm, md, lg, xl)<br>9. Aucune requête réseau dans le render : utiliser useEffect avec dépendances<br>10. Gestion d'erreurs globale : ErrorBoundary + Sentry (future) |
| **Critères de validation** | 1. TypeScript compiler en strict mode, zéro erreurs<br>2. Tests composants : Vitest + React Testing Library, 70% couverture<br>3. Lighthouse score : ≥ 90 performance, ≥ 95 accessibility<br>4. Bundle size : < 250 KB (gzipped) pour main app.js<br>5. FCP (First Contentful Paint) : < 1.5s<br>6. Navigation fluide : transitions <300ms<br>7. Tous les formules validées côté client avant envoi<br>8. Tests d'authentification : login/logout/refresh token<br>9. Tests RBAC : utilisateur EMPLOYE ne voit pas les fonctions DIRECTEUR |

---

## T05 — Performance & Qualité de Code

| Champ | Contenu |
|---|---|
| **ID** | T05 |
| **Titre** | Performance & Qualité de Code |
| **Objectif** | Assurer une application performante, maintenable, testée, et conforme aux standards de qualité industrielle. |
| **Implémentation technique** | **Testing Backend :**<br><pre>// TicketControllerTest.java<br>@SpringBootTest<br>@WebMvcTest(TicketController.class)<br>class TicketControllerTest {<br>  @MockBean<br>  private TicketService ticketService;<br>  <br>  @Test<br>  void testCreateTicket_Success() {<br>    // Arrange, Act, Assert<br>  }<br>  <br>  @Test<br>  void testCreateTicket_UnauthorizedUser_Returns403() {<br>    // Test RBAC enforcement<br>  }<br>}</pre><br><br>**Testing Frontend :**<br><pre>// LoginPage.test.tsx<br>import { render, screen, userEvent } from '@testing-library/react';<br>import { describe, it, expect } from 'vitest';<br><br>describe('LoginPage', () => {<br>  it('should display login form', () => {<br>    render(<LoginPage />);<br>    expect(screen.getByText('Sign In')).toBeInTheDocument();<br>  });<br>  <br>  it('should call login API on form submit', async () => {<br>    render(<LoginPage />);<br>    await userEvent.type(screen.getByLabelText('Email'), 'test@example.com');<br>    await userEvent.click(screen.getByText('Login'));<br>    // Assertions<br>  });<br>});</pre><br><br>**Caching :**<br><pre>@Cacheable(value = "tickets", key = "#userId")<br>public List<Ticket> getUserTickets(String userId) {<br>  return ticketRepository.findByAuthorId(userId);<br>}<br><br>@CacheEvict(value = "tickets", key = "#ticket.author.id")<br>public Ticket updateTicket(Ticket ticket) {<br>  return ticketRepository.save(ticket);<br>}</pre><br><br>**Logging :**<br><pre>private static final Logger log = LoggerFactory.getLogger(TicketService.class);<br><br>public Ticket createTicket(TicketCreateDTO dto) {<br>  log.info("Creating ticket: {}", dto.getTitle()); // INFO<br>  log.debug("Author ID: {}", dto.getAuthorId());   // DEBUG<br>  try {<br>    // ...<br>  } catch (Exception e) {<br>    log.error("Failed to create ticket", e); // ERROR<br>  }<br>}</pre> |
| **Contraintes spécifiques** | 1. Couverture minimale tests : 80% backend (JUnit 5 + Mockito), 70% frontend (Vitest)<br>2. Temps réponse API : 95e percentile < 300ms pour GET, < 500ms pour POST/PUT<br>3. SonarQube : 0 issues "blocker", 0 "critical", max 5 "major"<br>4. Code complexity : cyclomatic complexity max 10 par méthode<br>5. Duplication de code : max 3% du codebase<br>6. Maintainability index : ≥ 80<br>7. Caching : @Cacheable sur requêtes fréquentes (getUserTickets, getCategories)<br>8. Logging : SLF4J + Logback, niveau INFO en prod (DEBUG en dev)<br>9. Aucune donnée sensible en logs (passwords, tokens, numéros carte)<br>10. Métriques : tracer MTTR, volume requêtes, erreur rates via Micrometer (future) |
| **Critères de validation** | 1. Tous les tests passent (mvn test, npm test)<br>2. SonarQube quality gate réussi<br>3. Lighthouse : Performance ≥ 90, Accessibility ≥ 95<br>4. Load testing : 100 requêtes concurrent sur GET /api/v1/tickets → tous reçoivent réponse < 500ms<br>5. Database queries : < 3 requêtes par endpoint (éviter N+1)<br>6. Bundle size analysé avec webpack-bundle-analyzer<br>7. Coverage report généré (JaCoCo pour Java, Vitest pour React)<br>8. Zero memory leaks en Frontend (via Chrome DevTools memory profiler)<br>9. Performance budget respecté : main chunk < 250 KB, vendors < 150 KB |

---

## T06 — Sécurité & Conformité

| Champ | Contenu |
|---|---|
| **ID** | T06 |
| **Titre** | Sécurité & Conformité RGPD |
| **Objectif** | Garantir que l'application respecte les standards de sécurité OWASP, protège les données personnelles en conformité RGPD, et prévient les attaques communes. |
| **Implémentation technique** | **Headers de sécurité HTTP :**<br><pre>// SecurityConfig.java<br>@Configuration<br>public class SecurityHeadersConfig {<br>  @Bean<br>  public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {<br>    http.headers()<br>      .xssProtection().and()<br>      .contentSecurityPolicy("default-src 'self'; script-src 'self'; style-src 'self' 'unsafe-inline'").and()<br>      .frameOptions().sameOrigin().and()<br>      .httpStrictTransportSecurity()<br>        .includeSubDomains(true)<br>        .maxAgeInSeconds(31536000); // 1 year<br>    return http.build();<br>  }<br>}</pre><br><br>**Rate Limiting :**<br><pre>// RateLimitInterceptor.java<br>@Component<br>public class RateLimitInterceptor extends HandlerInterceptor {<br>  @Override<br>  public boolean preHandle(HttpServletRequest request, ...) {<br>    String clientIp = getClientIp(request);<br>    if (isEndpoint(request, "/api/v1/auth/login")) {<br>      if (rateLimiter.isLimited(clientIp, "5/15m")) {<br>        response.sendError(429, "Too Many Requests");<br>        return false;<br>      }<br>    }<br>    return true;<br>  }<br>}</pre><br><br>**Input Validation :**<br><pre>// TicketDTO.java<br>@Data<br>public class TicketCreateDTO {<br>  @NotBlank<br>  @Size(min = 5, max = 200)<br>  private String title;<br>  <br>  @NotBlank<br>  @Size(min = 10, max = 5000)<br>  private String description;<br>  <br>  @NotNull<br>  @Pattern(regexp = "^[A-Z]+$")<br>  private String category;<br>}</pre><br><br>**Données sensibles :**<br><pre>// User.java<br>@JsonIgnore // Jamais exposé en API<br>private String passwordHash;<br><br>// AuditLog.java<br>// Pas de logs de passwords, tokens, données sensitives<br>log.info("User login attempt: {}", maskEmail(email));</pre><br><br>**Variables d'environnement :**<br><pre># application-prod.yml<br>jwt:<br>  secret: ${JWT_SECRET} # Depuis secrets Kubernetes<br>  expiration: 86400000<br>\nspring:<br>  datasource:<br>    url: ${DB_URL}<br>    username: ${DB_USER}<br>    password: ${DB_PASSWORD}</pre> |
| **Contraintes spécifiques** | 1. HTTPS obligatoire en prod (HTTP en dev accepté, mais HTTPS recommandé)<br>2. Headers de sécurité : X-Content-Type-Options: nosniff, X-Frame-Options: DENY, HSTS<br>3. CSRF : disabled pour REST stateless, enabled pour formulaires HTML (future)<br>4. CORS : whitelist explicite, jamais "*"<br>5. Rate limiting : max 5 tentatives login / 15 min par IP<br>6. Password policy : minimum 8 caractères, 1 majuscule, 1 chiffre, 1 spécial<br>7. Session timeout : 24h access token, 7j refresh token<br>8. Aucun secret en code source (.env en gitignore)<br>9. Données perso : suppression après 1 an d'inactivité (RGPD right to be forgotten)<br>10. Audit trail : toutes les actions sensibles tracées (création user, modification rôle, suppression données) |
| **Critères de validation** | 1. OWASP Top 10 checklist complétée (A01 injection, A02 authentication, etc.)<br>2. Pentest : au minimum scan automatisé OWASP ZAP vert<br>3. RGPD : data processing agreement signé, privacy policy à jour<br>4. SSL/TLS : certificat valide, TLS 1.2 minimum<br>5. Headers de sécurité : vérifiés via securityheaders.com<br>6. Pas de secrets en logs, pas de passwords exportés<br>7. GDPR data export fonctionnel (endpoint /api/v1/users/{id}/data-export)<br>8. Suppression logique : données anciennes non supprimées physiquement immédiatement<br>9. Encryption : données sensibles chiffrées en transit (HTTPS) et at rest (future)<br>10. Conformité : audit trail accessible aux auditeurs (logs centralisés) |

---

---

# LIVRABLE 3 — MATRICE DE DROITS RBAC

## Tableau 1 — Matrice Fonctionnelle par Rôle

| Fonctionnalité | DIRECTEUR | REFERENT_IT | RESPONSABLE | EMPLOYE |
|---|---|---|---|---|
| **Créer un ticket** | ⚠️ Via Responsable | ✅ | ✅ | ✅ |
| **Voir ses propres tickets** | ❌ | ⚠️ Assignés | ✅ | ✅ |
| **Voir tous les tickets** | ✅ | ✅ | ⚠️ Équipe seulement | ❌ |
| **Modifier le statut d'un ticket** | ❌ | ✅ | ��� | ❌ |
| **Assigner un ticket** | ❌ | ✅ (à soi/futur tiers) | ❌ | ❌ |
| **Ajouter commentaire public** | ✅ | ✅ | ✅ | ✅ |
| **Ajouter commentaire interne** | ❌ | ✅ | ❌ | ❌ |
| **Exporter un ticket en PDF** | ✅ | ✅ | ✅ | ✅ |
| **Accéder au dashboard global** | ✅ (tous KPIs) | ✅ (IT opérationnel) | ✅ (équipe) | ✅ (personnel) |
| **Accéder au dashboard personnel** | ❌ | ✅ | ✅ | ✅ |
| **Gérer les utilisateurs (CRUD)** | ✅ | ⚠️ Lecture + désactivation | ❌ | ❌ |
| **Modifier les rôles utilisateurs** | ✅ | ❌ | ❌ | ❌ |
| **Consulter le journal d'audit** | ✅ | ⚠️ Lecture seule | ❌ | ❌ |
| **Exporter rapports CSV/PDF** | ✅ | ✅ | ⚠️ Équipe seulement | ⚠️ Personnel seulement |
| **Supprimer un utilisateur** | ✅ | ❌ | ❌ | ❌ |
| **Réassigner un ticket (futur)** | ❌ | ✅ | ❌ | ❌ |
| **Escalader un ticket P1** | ⚠️ Via Responsable | ✅ Auto | ✅ (flag) | ✅ (flag) |
| **Voir historique complet ticket** | ✅ | ✅ | ✅ Équipe | ✅ Ses tickets |

### Légendes
- ✅ **Autorisé** : Accès complet, aucune restriction
- ❌ **Interdit** : Accès refusé, 403 Forbidden retourné
- ⚠️ **Partiel** : Accès restreint, voir détail en parenthèses

---

## Tableau 2 — Matrice des Annotations Spring Security par Endpoint

| Endpoint REST | Méthode | @PreAuthorize | Rôles autorisés | Description |
|---|---|---|---|---|
| `/api/v1/auth/login` | POST | `permitAll()` | PUBLIC | Authentification (pas de token requis) |
| `/api/v1/auth/refresh` | POST | `permitAll()` | PUBLIC | Refresh token (pas de token actuel requis) |
| `/api/v1/auth/logout` | POST | `isAuthenticated()` | Tous (connectés) | Déconnexion |
| `/api/v1/tickets` | POST | `hasAnyRole('EMPLOYE','RESPONSABLE','REFERENT_IT')` | Employé, Responsable, Référent IT | Créer un ticket |
| `/api/v1/tickets/{id}` | GET | `hasAnyRole('DIRECTEUR','REFERENT_IT','RESPONSABLE','EMPLOYE') and (@ticketRepository.findById(#id)?.author.id == authentication.principal.id or hasAnyRole('DIRECTEUR','REFERENT_IT') or hasTicketAccess(#id))` | Tous (avec RBAC) | Consulter un ticket (contrôle d'accès granulaire) |
| `/api/v1/tickets` | GET | `hasAnyRole('DIRECTEUR','REFERENT_IT','RESPONSABLE','EMPLOYE')` | Tous | Lister tickets (filtrage backend) |
| `/api/v1/tickets/{id}` | PUT | `hasRole('REFERENT_IT')` | Référent IT | Modifier statut / assigner |
| `/api/v1/tickets/{id}/comments` | POST | `hasAnyRole('DIRECTEUR','REFERENT_IT','RESPONSABLE','EMPLOYE')` | Tous | Ajouter commentaire |
| `/api/v1/tickets/{id}/comments/internal` | POST | `hasRole('REFERENT_IT')` | Référent IT | Ajouter commentaire interne |
| `/api/v1/tickets/{id}/export-pdf` | GET | `hasAnyRole('DIRECTEUR','REFERENT_IT','RESPONSABLE','EMPLOYE')` | Tous (avec accès au ticket) | Exporter en PDF |
| `/api/v1/dashboard` | GET | `hasAnyRole('DIRECTEUR','REFERENT_IT','RESPONSABLE','EMPLOYE')` | Tous | Dashboard personnalisé |
| `/api/v1/dashboard/global` | GET | `hasAnyRole('DIRECTEUR','REFERENT_IT')` | Directeur, Référent IT | Dashboard global |
| `/api/v1/reports/csv` | GET | `hasAnyRole('DIRECTEUR','REFERENT_IT')` | Directeur, Référent IT | Export rapports CSV |
| `/api/v1/users` | GET | `hasRole('DIRECTEUR')` | Directeur | Lister utilisateurs |
| `/api/v1/users` | POST | `hasRole('DIRECTEUR')` | Directeur | Créer utilisateur |
| `/api/v1/users/{id}` | PUT | `hasRole('DIRECTEUR')` | Directeur | Modifier utilisateur |
| `/api/v1/users/{id}/role` | PUT | `hasRole('DIRECTEUR')` | Directeur | Modifier rôle utilisateur |
| `/api/v1/users/{id}` | DELETE | `hasRole('DIRECTEUR')` | Directeur | Supprimer logique utilisateur |
| `/api/v1/audit` | GET | `hasRole('DIRECTEUR')` | Directeur | Consulter journal d'audit |
| `/api/v1/users/{id}/disable` | PUT | `hasAnyRole('DIRECTEUR','REFERENT_IT')` | Directeur, Référent IT (limité) | Désactiver utilisateur |

### Format des annotations Spring Security

```java
// Exemple simple : un rôle
@PreAuthorize("hasRole('DIRECTEUR')")
public ResponseEntity<DashboardDTO> getGlobalDashboard() { }

// Exemple : plusieurs rôles (OR)
@PreAuthorize("hasAnyRole('DIRECTEUR', 'REFERENT_IT')")
public ResponseEntity<List<UserDTO>> getAllUsers() { }

// Exemple : logique complexe (ET)
@PreAuthorize("hasRole('REFERENT_IT') and @ticketRepository.existsById(#id)")
public ResponseEntity<TicketDTO> updateTicketStatus(@PathVariable Long id) { }

// Exemple : accès conditionnels (propriétaire OU admin)
@PreAuthorize("hasRole('DIRECTEUR') or (hasRole('EMPLOYE') and #userId == authentication.principal.id)")
public ResponseEntity<UserDTO> getUser(@PathVariable String userId) { }
```

---

---

# LIVRABLE 4 — DEFINITION OF DONE (DoD)

## Definition of Done Complète pour TicketFlow

Une User Story / Feature n'est considérée **DONE** que si **les 6 niveaux** ci-dessous sont entièrement validés.

---

## 📋 Niveau 1 — Code & Développement

- [ ] Code écrit selon les conventions du projet (nommage camelCase Java, PascalCase React, snake_case SQL)
- [ ] Pas de code commenté (si logique complexe, expliquer via commentaire clair, pas du code désactivé)
- [ ] Pas de TODOs non trackés (si TODO, créer issue JIRA correspondante avec lien)
- [ ] Structure respectée : packages Java organisés, dossiers React hiérarchisés, imports triés
- [ ] Typage strict TypeScript : zéro `any`, toutes les props typées, interfaces explicites
- [ ] Imports inutilisés supprimés (audit via IDE ou ESLint)
- [ ] Variables non-utilisées supprimées (compiler warning = 0)
- [ ] Pas de duplication de code : réutiliser componentes/services existants
- [ ] Formatage uniforme (Prettier + Eslint backend, Prettier frontend)
- [ ] Logs appropriés : SLF4J en Java, console en React (dev), aucun System.out

---

## 🧪 Niveau 2 — Tests

- [ ] Tests unitaires écrits pour au moins 80% du code backend (JUnit 5 + Mockito)
- [ ] Tests unitaires écrits pour au moins 70% du code frontend (Vitest + React Testing Library)
- [ ] Tests d'intégration : au minimum 1 test d'intégration par endpoint critique
- [ ] Tous les tests passent localement (`mvn test` et `npm test` retournent 0 erreurs)
- [ ] Cas limites (edge cases) testés : null input, empty list, max length strings, etc.
- [ ] Tests de sécurité : endpoints non-authentifiés retournent 401, accès refusé retourne 403
- [ ] Tests de validation : formulaires invalides rejetés côté server avec message clair
- [ ] Couverture de code calculée (JaCoCo backend, Vitest frontend) et rapportée
- [ ] Aucun test "flaky" (qui passe/échoue aléatoirement)
- [ ] Tests documentés avec comment explicatif du scénario testé

---

## 🔒 Niveau 3 — Sécurité

- [ ] Authentification requise sur tous les endpoints sensibles (sauf `/auth/login`, `/auth/register`)
- [ ] @PreAuthorize présent sur chaque endpoint avec rôles corrects
- [ ] Validation de tous les DTOs entrants avec @Valid + Jakarta Validation
- [ ] Pas de données sensibles exposées en réponse API (passwords, tokens, SSN)
- [ ] Headers de sécurité HTTP vérifiés : X-Frame-Options, X-Content-Type-Options, HSTS présents
- [ ] Aucune donnée sensible loggée (passwords, tokens, emails d'utilisateurs)
- [ ] Mots de passe hashés avec BCryptPasswordEncoder (strength 12)
- [ ] Rate limiting sur endpoints d'authentification (max 5 essais/15 min)
- [ ] SQL injection : aucune concaténation SQL, utiliser @Query ou JPA
- [ ] XSS prévenu : tous les inputs sanitisés, CSP header configuré en frontend

---

## 📚 Niveau 4 — Documentation

- [ ] Swagger/OpenAPI à jour pour tous les endpoints (descriptions, parameters, responses)
- [ ] README mis à jour : setup local, commandes de démarrage, dépendances
- [ ] Javadoc rédigé pour toutes les méthodes publiques (classes, services)
- [ ] Commentaires sur la logique métier complexe (pas sur du code simple)
- [ ] Migration Flyway documentée (fichier V{version}__description.sql avec commentaires)
- [ ] Architecture diagram mis à jour (si changement structure packages)
- [ ] CHANGELOG.md mis à jour avec description du feature / fix
- [ ] API documentation : exemples cURL ou Postman pour chaque endpoint
- [ ] Erreurs documentées : codes HTTP possibles et signification
- [ ] Type definitions mises à jour (interfaces TypeScript pour nouveaux DTOs)

---

## 👀 Niveau 5 — Revue & Intégration

- [ ] Pull Request créée sur branche feature (format : `feature/JIRA-XXX-description`)
- [ ] PR approuvée par au minimum 1 autre dev (code review complète)
- [ ] Pipeline CI/CD ✅ vert : tests passent, SonarQube OK, build réussi
- [ ] Conflits de merge résolus (si rebase nécessaire, bien effectué)
- [ ] Branche feature supprimée après merge dans main
- [ ] Commits bien structurés : message clair, 1 commit par changement logique
- [ ] Références JIRA incluses en messages commit (`TICKETFLOW-123 : Add login functionality`)
- [ ] Changelog fusionné dans CHANGELOG.md
- [ ] Notifications Slack/email envoyées aux stakeholders (si feature importante)
- [ ] Tags de version créés si release (format : `v1.0.0`)

---

## ✅ Niveau 6 — Validation Métier

- [ ] Tous les critères d'acceptation de la US validés fonctionnellement
- [ ] Démo effectuée au Product Owner (si feature importante) avec acceptance
- [ ] Tests menés sur environnement staging (zéro régression détectée)
- [ ] Aucune régression sur features existantes (smoke tests passés)
- [ ] Performance acceptée : < 300ms pour GET, < 500ms pour POST (95e percentile)
- [ ] UX testée : navigation fluide, messages d'erreur clairs, pas de dead ends
- [ ] Responsive design validé : desktop + mobile (tablet si applicable)
- [ ] Accessibility validée : WCAG 2.1 AA respecté (alt text, ARIA labels, keyboard nav)

---

## 🎯 Checklist Finale (À l'Issue de la US)

```
Avant déploiement en production, cocher les 6 sections ci-dessus :

Niveau 1 — Code & Développement        : [ ] ✅ COMPLET
Niveau 2 — Tests                       : [ ] ✅ COMPLET
Niveau 3 — Sécurité                    : [ ] ✅ COMPLET
Niveau 4 — Documentation               : [ ] ✅ COMPLET
Niveau 5 — Revue & Intégration         : [ ] ✅ COMPLET
Niveau 6 — Validation Métier           : [ ] ✅ COMPLET

Status Global : ☐ EN COURS  ☐ DONE  ☐ BLOCKED (motif : ______)
```

---

## ⚠️ IMPORTANT

> **Une User Story n'est considérée DONE que si tous les 6 niveaux sont complètement validés.**
>
> **Toute case non cochée bloque le passage en production.**
>
> En cas de blocage, ouvrir une issue JIRA sub-task pour corriger l'écart avant merge.
>
> Le Definition of Done s'applique à **chaque feature, chaque bugfix, chaque refactor**.

---

---
