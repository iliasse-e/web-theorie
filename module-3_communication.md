
# 🌐 Module 3 : Communication, Protocoles & Asynchronisme

---

## 3.1 L'Évolution du Protocole HTTP

Pour comprendre les performances d'une application web, il faut regarder sous le capot du protocole de transport le plus utilisé au monde : HTTP.

### A. HTTP/1.1 (Le modèle historique)

* **Le fonctionnement :** Chaque requête réseau vers le serveur nécessite l'ouverture d'une connexion TCP (ou sa réutilisation via l'en-tête `Keep-Alive`).
* **Le gros point noir : Le Head-of-Line Blocking (HolB) au niveau TCP.**
En HTTP/1.1, sur une même connexion, les requêtes doivent se suivre sagement. Si la requête numéro 1 met du temps à répondre, toutes les requêtes suivantes sont bloquées derrière dans la file d'attente. Pour contourner ça, les navigateurs ouvrent traditionnellement 6 connexions TCP en parallèle par domaine, mais cela reste lourd.

### B. HTTP/2 (Le multiplexage)

* **La révolution :** HTTP/2 introduit le **multiplexage**.
Il n'y a plus qu' **une seule connexion TCP** ouverte entre le client et le serveur. Sur cette unique connexion, toutes les requêtes et réponses sont découpées en petits morceaux binaires appelés *frames* (trames), étiquetées avec un identifiant de flux.
* **Résultat :** Plus de blocage en tête de ligne. Le client peut envoyer 50 requêtes en même temps sur la même connexion, et le serveur les renvoie dans le désordre selon leur disponibilité. Le navigateur se charge de les réassembler.
* **Compression :** HTTP/2 compresse aussi les en-têtes HTTP (headers) via l'algorithme HPACK pour réduire la taille des données envoyées.

### C. HTTP/3 (Le passage à UDP)

* **Le changement majeur :** HTTP/2 a éliminé le blocage au niveau HTTP, mais il subissait encore le Head-of-Line Blocking au niveau de **TCP** (si un paquet TCP se perd en route sur le réseau, tout s'arrête le temps qu'il soit renvoyé).
* **HTTP/3** abandonne complètement TCP et repose sur **QUIC (construit sur UDP)**.
* **Résultat :** Chaque flux de données est indépendant. Si un paquet se perd sur un flux, les autres flux continuent de tourner sans être impactés. De plus, la connexion s'établit quasi instantanément (*0-RTT Handshake*), ce qui est idéal pour les réseaux mobiles instables.

---

## 3.2 Les Modes de Communication Dynamique (Temps Réel)

Quand une application a besoin de recevoir des données du serveur en temps réel (ex: un chat, un tableau de bord boursier, une notification médico-sociale), les requêtes HTTP classiques (`GET`/`POST`) ne suffisent plus. On a 3 grands patterns :

### 1. Le Polling vs Long Polling

* **Polling classique :** Le client envoie une requête HTTP toutes les X secondes (*"T'as du nouveau ?"*). C'est simple, mais inefficace et ça surcharge le serveur pour rien si les données n'ont pas changé.
* **Long Polling :** Le client envoie une requête. Le serveur garde la connexion ouverte tant qu'il n'a pas de nouvelle donnée. Dès qu'il a une info, il répond, et le client relance immédiatement une nouvelle requête.

### 2. Server-Sent Events (SSE)

* **Le principe :** Un flux unidirectionnel permanent **du serveur vers le client**, basé sur le protocole HTTP standard (Content-Type : `text/event-stream`).
* **Comment ça marche :** Le client ouvre une connexion `EventSource`. Le serveur pousse du texte brut dès qu'il y a un événement.
* **Avantage / Inconvénient :** Très simple à mettre en place, s'appuie sur HTTP (gère les reconnexions auto, passe les firewalls facilement), mais c'est **unidirectionnel** (le client ne peut pas répondre via ce même canal, il doit utiliser un `POST` classique).

### 3. Les WebSockets

* **Le principe :** Un canal de communication **bidirectionnel (full-duplex) en temps réel**, qui s'affranchit du modèle HTTP classique après une poignée de main initiale (*Handshake HTTP* qui upgrade la connexion sur le protocole TCP brut).
* **Avantage / Inconvénient :** Le client et le serveur peuvent s'envoyer des messages à tout moment de manière instantanée avec un overhead réseau minimal. En contrepartie, ça demande une gestion d'infrastructure spécifique (les load balancers doivent savoir maintenir des connexions persistantes, gestion des reconnexions à la main si le réseau coupe).

---

## 3.3 Paradigmes d'API (REST vs GraphQL vs gRPC)

* **REST :** Le standard basé sur les verbes HTTP (`GET`, `POST`, `PUT`, `DELETE`), les ressources et les codes de statut. Idéal pour la simplicité et le cache HTTP.
* **GraphQL :** Le client demande exactement les champs dont il a besoin (évite le *over-fetching* et *under-fetching* de données), via un point de terminaison unique (`POST /graphql`).
* **gRPC-Web :** Utilisé pour la communication haute performance (souvent entre microservices ou pour du front ultra-exigeant), basé sur HTTP/2 et des protocoles binaires compacts (*Protocol Buffers*).

---

# 🎯 3 Questions Pièges d'Entretien (Module 3)

À toi de jouer ! Réponds à ces 3 cas pratiques :

### Question 1

> *"Ton application web doit afficher des notifications en temps réel pour l'utilisateur (flux descendant uniquement), et tu veux que ça passe sans encombre à travers les firewalls d'entreprise stricts qui bloquent parfois les protocoles exotiques. Entre WebSockets et Server-Sent Events (SSE), que choisis-tu et pourquoi ?"*

### Question 2

> *"Qu'est-ce que le Head-of-Line Blocking, et comment HTTP/2 a-t-il résolu ce problème par rapport à HTTP/1.1 ?"*

### Question 3

> *"Pourquoi dit-on qu'une architecture REST est 'stateless' (sans état) ?"*

---

Prends ton temps, propose tes réponses, et on débriefe ça comme d'habitude !
