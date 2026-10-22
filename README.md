Voici la structure complète et détaillée des **6 modules de la formation Invariants du Web & Architecture Front-End**. Ce sommaire servira de table des matières exhaustive pour ton document de référence (README).

---

# 📚 Plan de Formation : Invariants du Web & Architecture Front-End

---

## 🏛️ Module 1 : Le Moteur du Navigateur & le Critical Rendering Path

*Comprendre la mécanique de transformation du code brut en pixels et l'impact sur la fluidité d'affichage.*

* **1.1 La chaîne de rendu critique (Critical Rendering Path) :**
* **HTML $\rightarrow$ DOM** (*Document Object Model*) : Parsing, jetons (tokens) et construction de l'arbre.
* **CSS $\rightarrow$ CSSOM** (*CSS Object Model*) : Cascade, spécificité et coût du calcul des règles.
* **Render Tree** : Fusion du DOM et du CSSOM (filtrage des éléments non affichés comme `display: none`).


* **1.2 La géométrie et la peinture à l'écran :**
* **Layout / Reflow** : Calcul des coordonnées géométriques ($x, y, \text{largeur}, \text{hauteur}$) et propagation dans l'arbre.
* **Paint / Repaint** : Remplissage des pixels (couleurs, ombres, bordures).
* **Composite** : Assemblage des calques (*layers*) sur le GPU via les propriétés optimisées (`transform`, `opacity`).


* **1.3 Stratégies de chargement et d'exécution des ressources :**
* Scripting : Différences fondamentales entre `<script>`, `<script async>` et `<script defer>`.
* Optimisation du réseau : Utilisation des directives d'en-tête `<link rel="preload">`, `prefetch`, `preconnect` et `dns-prefetch`.



---

## ⚙️ Module 2 : Moteur JS, Event Loop & Gestion Mémoire

*Maîtriser l'exécution mono-thread du JavaScript, l'asynchronisme et la prévention des fuites mémoire.*

* **2.1 L'architecture du moteur JS (ex: V8) :**
* **Call Stack** : Empilement et exécution synchrone des contextes d'exécution.
* **Memory Heap** : Allocation dynamique de la mémoire (objets, tableaux, closures).


* **2.2 L'Event Loop (Boucle d'événements) :**
* **Macro-tasks (Task Queue)** : `setTimeout`, `setInterval`, `requestAnimationFrame`, I/O, événements DOM.
* **Micro-tasks (Microtask Queue)** : `Promise.then/catch/finally`, `queueMicrotask`, `MutationObserver`.
* **Ordonnancement et fréquence de rafraîchissement** : Priorité des micro-tasks, synchronisation avec les 60 FPS ($16.6\text{ms}$ par frame).


* **2.3 Gestion de la mémoire et Garbage Collection :**
* **Garbage Collector** : Algorithme *Mark-and-Sweep* (accessibilité depuis la racine / *GC Root*).
* **Fuites mémoire (Memory Leaks) en SPA** : Abonnements non résiliés (RxJS/Events), références retained par des closures, éléments DOM détachés conservés en mémoire.



---

## 🌐 Module 3 : Communication, Protocoles & Asynchronisme

*Comprendre comment les données transitent entre le client et le serveur, et choisir le bon canal de communication.*

* **3.1 L'évolution du protocole HTTP :**
* **HTTP/1.1** : Connexions persistantes (*Keep-Alive*), mais blocage en tête de ligne (*Head-of-Line Blocking* sur TCP).
* **HTTP/2** : Multiplexage de requêtes sur une seule connexion TCP, compression d'en-têtes HPACK, Server Push.
* **HTTP/3 (QUIC)** : Transit sur UDP, élimination du Head-of-Line blocking au niveau réseau, Poignée de main (*Handshake*) accélérée.


* **3.2 Architectures et modes de communication dynamique :**
* **Polling vs Long Polling** : Reconstitution d'un flux par requêtes périodiques ou maintenues.
* **Server-Sent Events (SSE)** : Flux unidirectionnel persistant du serveur vers le client basé sur le protocole HTTP (streaming text/event-stream).
* **WebSockets** : Protocole duplex bidirectionnel temps réel sur TCP (Handshake HTTP initial puis upgrade de socket).


* **3.3 Paradigmes d'API :**
* **REST** : Verbes HTTP, statut, stateless, niveau de maturité Richardson.
* **GraphQL / gRPC-Web** : Réponses ciblées côté client vs requêtes binaires sur Protobuf.



---

## 🛡️ Module 4 : Sécurité Web Front-End

*Identifier les vecteurs d'attaque sur le navigateur et appliquer les mécanismes de défense en profondeur.*

* **4.1 Les attaques côté client :**
* **XSS (Cross-Site Scripting)** : Injections de scripts malveillants (*Stored*, *Reflected*, *DOM-based*). Moteur de sanitization des frameworks.
* **CSRF (Cross-Site Request Forgery)** : Exécution de requêtes non sollicitées via la session de l'utilisateur.


* **4.2 Protections et en-têtes HTTP de sécurité :**
* **SameSite Cookies** : Directives `Strict`, `Lax`, `None` pour contrer le CSRF.
* **CORS (Cross-Origin Resource Sharing)** : Mécanisme de sécurité du navigateur, requêtes *Preflight* (`OPTIONS`), en-têtes `Access-Control-Allow-*`.
* **CSP (Content Security Policy)** : Restriction des sources d'exécution via les directives `script-src`, `style-src`, `connect-src`.


* **4.3 Stockage sécurisé des données :**
* Comparatif critique : `localStorage` vs `sessionStorage` vs **Cookies `HttpOnly` + `Secure**`.



---

## 🧩 Module 5 : Invariants des Frameworks & Paradigmes d'Architecture

*Décoder la mécanique interne des frameworks modernes (Angular, React, Vue, Svelte).*

* **5.1 Synchronisation de l'état et de l'interface (State vs UI) :**
* **Virtual DOM vs Dirty Checking vs Proxies / Directives de compilation**.
* **Réactivité à base de Signaux (Signals)** : Graphe de dépendances à ciblage fin (*fine-grained reactivity*) vs Réévaluation globale de composants.


* **5.2 Principes de conception et découplage :**
* **Injection de Dépendances (DI)** : Inversion de contrôle (IoC), portées (*Hierarchical Injectors*), modularité et testabilité.
* **Séparation des Responsabilités (SoC)** : Conteneurs logique métier (Services/Stores) vs Composants d'affichage (Presentational Components).
* **State Management** : Gestion d'état local vs état global immuable (Redux/NgRx/Signals store).


* **5.3 Cycles de vie et découplage :**
* Hooks d'initialisation, de détection de changement et de nettoyage (*cleanup*).



---

## 🚀 Module 6 : Architectures de Rendu, Métriques (Core Web Vitals) & Offline

*Optimiser l'expérience utilisateur, le référencement et la résilience réseau.*

* **6.1 Stratégies de rendu applicatif :**
* **CSR (Client-Side Rendering)** : Fonctionnement des SPAs, compromis sur le premier affichage et le SEO.
* **SSR (Server-Side Rendering)** : Rendu serveur, problématique de l'hydratation (Island Architecture / Progressive Hydration).
* **SSG (Static Site Generation) & ISR (Incremental Static Regeneration)** : Pré-rendu à la compilation.


* **6.2 Performance et mesure (Core Web Vitals) :**
* **LCP** (*Largest Contentful Paint*) : Mesure de la vitesse de chargement perçue.
* **INP** (*Interaction to Next Paint*) : Mesure de la réactivité aux interactions.
* **CLS** (*Cumulative Layout Shift*) : Mesure de la stabilité visuelle.
* **Techniques d'optimisation** : *Tree-shaking*, *Bundle Splitting*, *Dynamic Imports*, *Debounce/Throttle*, offloading via *Web Workers*.


* **6.3 Mode hors-ligne et stockage lourd (PWA) :**
* **Service Workers & Cache API** : Interception réseau, stratégies de mise en cache (*Cache-First*, *Network-First*, *Stale-While-Revalidate*).
* **IndexedDB** : Base de données NoSQL asynchrone côté client pour la gestion de volumes importants.
