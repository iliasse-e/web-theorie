# Cours : Les Invariants du Web et les Patterns Transversaux

Pour ne plus jamais être pris au dépourvu face à un nouveau framework (que ce soit Angular, React, Vue, Svelte ou le prochain à la mode), il faut comprendre une vérité simple : **les outils changent tous les 3 ans, mais les problèmes informatiques du web sont les mêmes depuis 20 ans.**

Voici les **5 invariants du web** et les grands patterns qui y répondent. Si tu maîtrises ça, tu comprends n'importe quelle archi front-end en surface en quelques heures.

---

## 1. L'Invariant 1 : La Réactivité (L'État vs l'UI)

* **Le Problème :** L'utilisateur clique, une donnée change (ex: le panier passe de 0 à 1 article). Comment on s'assure que l'écran (le DOM) se met à jour automatiquement sans que le dev ait à modifier le HTML à la main (`document.getElementById...`) ?
* **Les Solutions (Patterns) à travers les frameworks :**
* **Angular (Signals / RxJS) :** On déclare un état observable. Quand la valeur change, le système déclenche un mécanisme de mise à jour ciblée (Change Detection).
* **React (`useState`) :** Un composant est une fonction pure de son état. Quand l'état change, *tout* le composant est réexécuté (re-render) pour recréer un nouveau Virtual DOM.
* **Vue (`Refs` / Vue 3 Composition API) :** Utilisation de Proxies JavaScript pour intercepter les modifications de variables et mettre à jour le DOM de manière chirurgicale.


* **Ce qu'il faut retenir :** Tout framework moderne n'est qu'un "moteur de synchronisation" entre des données en mémoire (State) et des pixels sur l'écran (UI).

## 2. L'Invariant 2 : L'Arbre de Composants et le Flux de Données

* **Le Problème :** Une application web complexe ne peut pas être un seul gros fichier de code. On la découpe en briques réutilisables (Composants). Comment ces briques communiquent-elles entre elles ?
* **Les Solutions (Patterns) :**
* **Le flux descendant (Top-Down / Props & Events) :** Le parent donne des données aux enfants (props en React/Vue, `@Input` en Angular). L'enfant remonte les actions aux parents via des notifications (events, `@Output`, callbacks).
* **L'état global / Partagé (Store) :** Quand deux composants très éloignés dans l'arbre ont besoin de la même donnée (ex: l'utilisateur connecté), on sort la donnée de l'arbre pour la mettre dans un conteneur global (NgRx, Redux, Pinia, Signals globaux).



## 3. L'Invariant 3 : Le Cycle de Vie et la Gestion des Ressources (Memory Leaks)

* **Le Problème :** Un composant naît, vit (reçoit des clics, appelle des API), puis meurt (l'utilisateur change de page). Si on oublie de nettoyer derrière soi (ex: un écouteur d'événement `window.addEventListener`, un `setInterval`, ou un abonnement réseau), la mémoire s'accumule et l'app ralentit (fuite de mémoire / memory leak).
* **Les Solutions (Patterns) :**
* **Les hooks de cycle de vie :** Chaque framework propose des méthodes ou des fonctions pour exécuter du code à l'initialisation (ex: `ngOnInit`, `useEffect` avec tableau vide, `onMounted`) et au nettoyage (ex: `ngOnDestroy`, fonction de cleanup dans `useEffect`, `onUnmounted`).



## 4. L'Invariant 4 : L'I/O Asynchrone (Le Réseau et le Rendering)

* **Le Problème :** Le JavaScript s'exécute par défaut sur un seul thread (single-thread). Si on attend une réponse de serveur (API) ou qu'on fait un calcul lourd, l'interface se fige (le navigateur freeze, l'animation saccade).
* **Les Solutions (Patterns) :**
* **L'asynchronisme natif :** Utilisation des `Promises`, des `async/await` ou des flux réactifs (`RxJS`) pour déléguer l'attente au système et libérer le thread principal.
* **L'isolation :** Les Web Workers (qu'on a vus) pour pousser les gros calculs hors du thread principal.



## 5. L'Invariant 5 : Le Découplage et l'Architecture (Modularité)

* **Le Problème :** Comment éviter que le code devienne une soupe infâme de 50 000 lignes où tout est lié à tout ?
* **Les Solutions (Patterns) :**
* **L'Injection de Dépendances (DI) :** (Qu'on vient de bosser). Séparer la logique métier (services) de l'affichage (composants).
* **La séparation des responsabilités (SoC - Separation of Concerns) :** La vue gère l'UI, le service gère la data/API, les routes gèrent la navigation.



---

### Pourquoi cette grille de lecture te protège ?

Le jour où tu arrives dans une boîte qui fait du **React** ou du **Vue**, tu ne perds pas ton temps à apprendre la syntaxe en panique. Tu arrives avec ton cerveau d'ingénieur en te disant :

1. *Où est stocké l'état dans leur système ?*
2. *Comment ils gèrent le cycle de vie pour nettoyer la mémoire ?*
3. *Comment les composants se parlent ?*

Une fois que tu as coché ces 3 cases, tu es opérationnel.

Est-ce que cette vision macro des invariants te paraît claire pour aborder n'importe quel écosystème front-end ?
