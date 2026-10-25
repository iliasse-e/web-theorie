# 🚀 Module 6 : Architectures de rendu, Core Web Vitals & Offline

---

## 6.1 Les Stratégies de Rendu (Où s'exécute le code ?)

Selon le type d'application que tu développes (une application métier ultra-sécurisée ou un site e-commerce grand public), le choix de l'architecture de rendu change radicalement.

1. **CSR (*Client-Side Rendering* - Ex: SPA Angular/React classique) :**
* Le serveur renvoie un fichier HTML presque vide (`<div id="root"></div>`) accompagné d'un gros fichier JavaScript.
* Le navigateur télécharge le JS, l'exécute, construit le DOM et affiche la page.
* *Avantage :* Expérience ultra-fluide une fois chargé (comme une application desktop).
* *Inconvénient :* Le **TTFB** (Time to First Byte) est bon, mais le **LCP** (temps avant de voir du contenu) peut être long sur mobile, et c'est une horreur nativement pour le référencement (SEO) si les crawlers ne savent pas exécuter le JS.


2. **SSR (*Server-Side Rendering* - Ex: Angular SSR / Next.js) :**
* À chaque requête d'un utilisateur, le serveur génère dynamiquement le HTML complet de la page, y injecte les données, et renvoie une page prête à l'emploi.
* *Avantage :* Excellent SEO, premier affichage ultra-rapide.
* *Inconvénient :* Charge CPU importante sur le serveur, et la page n'est interactive qu'une fois que le JavaScript client s'est hydraté (*hydration*).


3. **SSG (*Static Site Generation* - Ex: Astro, Gatsby) :**
* Les pages HTML sont générées **une seule fois au moment de la compilation** (build).
* *Avantage :* Vitesse absolue (les fichiers sont stockés sur un CDN mondial et servis instantanément), sécurité maximale, zéro charge serveur. Idéal pour les blogs, documentations, sites vitrines.



---

## 6.2 Les Core Web Vitals (Les métriques de performance Google)

Quand tu passes un entretien technique chez des clients soucieux de la performance de leur produit, ils te parleront forcément des **Core Web Vitals**. Ce sont les trois indicateurs clés mesurés par Google pour juger de l'expérience utilisateur (UX) :

1. **LCP (*Largest Contentful Paint*) - Le Chargement :**
* Mesure le temps que met le plus grand élément visible de la page (ex: une grosse image, un titre principal) à s'afficher.
* *Objectif :* Moins de **2.5 secondes**.


2. **INP (*Interaction to Next Paint*) - L'Interactivité :** *(Il a remplacé l'ancien FID)*
* Mesure la réactivité globale de la page lorsque l'utilisateur clique, tape ou interagit avec l'interface. Il vérifie que le thread principal n'est pas bloqué par de longs scripts JavaScript.
* *Objectif :* Moins de **200 millisecondes**.


3. **CLS (*Cumulative Layout Shift*) - La Stabilité Visuelle :**
* Mesure les mouvements impromptus des éléments sur la page pendant le chargement (ex: une publicité qui charge et décale tout le texte au moment où tu vas cliquer sur un bouton).
* *Objectif :* Moins de **0.1**.



---

## 6.3 Le Mode Hors-Ligne & Les Service Workers

Pour transformer une web app en **PWA (*Progressive Web App*)**, on s'appuie sur un outil clé : le **Service Worker**.

* **Qu'est-ce que c'est ?** Un script JavaScript qui tourne en arrière-plan dans le navigateur, totalement indépendant de la page web.
* **Son rôle :** Il agit comme un **proxy réseau intelligent** positionné entre ton application et le réseau.
* **Ce qu'il permet :**
* Intercepter les requêtes HTTP.
* Mettre en cache les fichiers statiques (HTML, CSS, JS, images) pour que l'application s'installe et fonctionne même **sans connexion Internet**.
* Gérer les notifications push en arrière-plan.



---

# 🎯 3 Questions Pièges d'Entretien (Module 6)

À toi de jouer pour valider ce dernier module et boucler le parcours théorique :

### Question 1

> *"Pourquoi dit-on qu'une application en rendu purement Client-Side (CSR) est généralement moins performante pour le SEO (référencement naturel) et le premier affichage (LCP) par rapport à une approche Server-Side (SSR) ?"*

### Question 2

> *"Qu'est-ce que mesure l'indicateur **CLS** (Cumulative Layout Shift) dans les Core Web Vitals, et comment peut-on l'éviter concrètement dans le code CSS ?"*

### Question 3

> *"Quel est le rôle d'un Service Worker dans une architecture web moderne ?"*

