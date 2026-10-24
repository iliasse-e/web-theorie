# 🛡️ Module 4 : Sécurité Web Front-End

---

## 4.1 XSS (*Cross-Site Scripting*)

L'attaque **XSS** consiste pour un attaquant à injecter du code JavaScript malveillant dans une page web vue par d'autres utilisateurs. Si le code s'exécute, il peut voler les cookies, le token d'authentification (`localStorage`), ou usurper l'identité de l'utilisateur.

### Les 3 types d'XSS :

1. **XSS Stockée (Persistante) :** Le code malveillant est sauvegardé en base de données (ex: un utilisateur poste un commentaire contenant `<script>stealData()</script>`). Chaque utilisateur qui charge le commentaire exécute le script. C'est la plus dangereuse.
2. **XSS Réfléchie (Reflected) :** Le script est injecté via l'URL (ex: un paramètre de recherche non filtré `?q=<script>...</script>`). Le serveur renvoie le script dans la page de réponse.
3. **XSS basée sur le DOM :** Le code JS de la page manipule le DOM de manière non sécurisée en récupérant des données directement depuis l'URL (`location.search`) et en les injectant via `innerHTML`.

### Comment s'en protèger ?

* **L'échappement par défaut :** Des frameworks comme Angular ou React échappent automatiquement les variables affichées dans les templates (le texte brut est transformé pour que `<` devienne `&lt;`).
* **Éviter le danger :** Ne jamais utiliser de méthodes comme `innerHTML` ou `bypassSecurityTrustHtml` (sous Angular) sans avoir préalablement assaini (*sanitized*) les données.

---

## 4.2 CSRF (*Cross-Site Request Forgery*)

L'attaque **CSRF** force un utilisateur connecté à exécuter des actions non désirées sur une application web tierce où il est authentifié.

* **Le scénario classique :** Tu es connecté à ta banque en ligne. Tu visites un site malveillant. Ce site contient un formulaire caché qui envoie une requête `POST` vers `[ma-banque.com/virement](https://ma-banque.com/virement)` avec ton compte. Comme ton navigateur envoie automatiquement tes cookies de session avec la requête, la banque croit que c'est toi qui fais le virement.
* **Les défenses modernes :**
1. **L'attribut `SameSite` sur les cookies :** En le positionnant à `SameSite=Lax` ou `Strict`, le navigateur refuse d'envoyer le cookie de session lors d'une requête provenant d'un site tiers. C'est la protection moderne numéro un.
2. **Les jetons CSRF (Anti-CSRF Tokens) :** Un jeton unique généré par le serveur et attendu dans les en-têtes de chaque requête modifiant l'état (`POST`, `PUT`, `DELETE`). Un site tiers ne peut pas lire ce jeton à cause de la politique de sécurité du navigateur.



---

## 4.3 CORS (*Cross-Origin Resource Sharing*) & Same-Origin Policy

La **Same-Origin Policy** est une règle de sécurité fondamentale des navigateurs : par défaut, un script s'exécutant sur un site (`[http://site-A.com](http://site-A.com)`) ne peut pas faire de requêtes vers une autre origine (`[http://site-B.com](http://site-B.com)`).

* **CORS**, c'est la "passerelle officielle" qui permet de contourner cette règle proprement.
* **Comment ça marche ?** Le serveur distant répond via des en-têtes HTTP spécifiques (`Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`) pour dire au navigateur : *"Oui, j'autorise ce domaine à venir chercher mes données"*.
* **La requête Pré-vol (*Preflight Request*) :** Pour les requêtes complexes (ex: `PUT`, `DELETE`, ou `POST` avec des headers custom comme un JWT), le navigateur envoie d'abord une requête de type `OPTIONS` en amont pour demander au serveur s'il autorise la vraie requête à venir. Si le serveur répond oui, la vraie requête part.

---

## 4.4 L'Éternel Débat : Où stocker le Token JWT ? (`localStorage` vs Cookies `HttpOnly`)

C'est une question piège classique en entretien : *Où stockes-tu ton token d'authentification ?*

| Mode de stockage | `localStorage` / `sessionStorage` | Cookies `HttpOnly` + `Secure` |
| --- | --- | --- |
| **Accès en JavaScript** | **Accessible** (via `localStorage.getItem`) | **Inaccessible** (protégé contre le JS) |
| **Vulnérabilité principale** | **XSS** (si un script malveillant tourne, il vole le token) | **CSRF** (nécessite une protection `SameSite`) |
| **Verdict de l'industrie** | Plus simple à manipuler côté Front, mais **risqué** en cas de faille XSS. | **Le standard recommandé** pour la sécurité maximale des applications d'entreprise. |

---

# 🎯 3 Questions Pièges d'Entretien (Module 4)

À toi de jouer pour valider ce module :

### Question 1

> *"Comment Angular se protège-t-il naturellement contre les failles XSS, et dans quel cas précis cette protection automatique saute-t-elle ?"*

### Question 2

> *"Pourquoi dit-on qu'un stockage de jeton JWT dans le `localStorage` expose l'application à des risques de vol de données en cas de faille XSS ?"*

### Question 3

> *"Qu'est-ce qu'une requête CORS de type 'Preflight' (pré-vol), et avec quelle méthode HTTP le navigateur l'envoie-t-il ?"*

---
