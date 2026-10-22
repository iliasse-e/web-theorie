
# ⚙️ Module 2 : Moteur JS, Event Loop & Gestion Mémoire

---

## 2.1 L'architecture du moteur JS (ex: V8)

Le moteur JavaScript (comme V8 de Google Chrome / Node.js) est composé principalement de deux grandes zones en mémoire :

1. **La *Memory Heap* (Le tas) :**
* C'est la zone de stockage non structurée, un grand "tas" de mémoire où sont alloués dynamiquement les objets, les tableaux, les fonctions et les *closures*.


2. **La *Call Stack* (La pile d'exécution) :**
* C'est une structure de type **LIFO** (*Last In, First Out* - Dernier entré, premier sorti).
* JavaScript est **mono-thread** (*single-threaded*) : il n'a qu'une seule Call Stack. Il ne peut exécuter **qu'une seule chose à la fois**. Quand une fonction est appelée, elle s'empile sur la pile. Quand elle est terminée, elle se dépile.



---

## 2.2 L'Event Loop (La boucle d'événements)

Puisque JS est mono-thread, comment fait-il pour gérer des opérations asynchrones (requêtes HTTP, timers, clics utilisateur) sans bloquer l'application ? C'est le rôle de l'**Event Loop**, aidée par l'environnement (le navigateur).

```
[ Call Stack ] ──(Synchrone)──> Exécution immédiate
      │
      ▼ (Asynchrone: setTimeout, fetch)
[ Web APIs (Navigateur) ]
      │
      ├──> (Fin de l'attente) ──> [ Microtask Queue ] (Promises, queueMicrotask)
      │                                   │ (Priorité ABSOLUE)
      └──> (Fin de l'attente) ──> [ Macro-task Queue ] (setTimeout, DOM events)

```

### La distinction critique : Micro-tasks vs Macro-tasks

Quand des opérations asynchrones se terminent, leurs rappels (*callbacks*) vont dans des files d'attente (*queues*) distinctes :

* **Les Micro-tasks (File prioritaire) :**
* *Ce qu'on y trouve :* Les promesses (`Promise.then`/`catch`/`finally`), `queueMicrotask`, `MutationObserver`.
* *Règle d'or :* La file des micro-tasks est **vidée entièrement** par l'Event Loop juste après chaque instruction de la Call Stack, avant même de passer à la tâche suivante.


* **Les Macro-tasks (File standard) :**
* *Ce qu'on y trouve :* `setTimeout`, `setInterval`, `requestAnimationFrame`, les événements de clic ou de scroll du DOM.
* *Règle :* L'Event Loop traite **une seule** macro-tâche par tour de boucle.



> **Piège classique d'ordre d'exécution :**
> Entre un `setTimeout(..., 0)` et une `Promise.resolve().then(...)`, c'est **toujours la Promise** qui s'exécute en premier, car elle atterrit dans les micro-tasks.

---

## 2.3 Gestion de la mémoire & Fuites (*Memory Leaks*)

Puisque le moteur JS alloue de la mémoire dans le *Heap*, il doit aussi la nettoyer pour éviter de saturer la RAM du navigateur.

### Comment fonctionne le Garbage Collector (GC) ?

Le moteur utilise principalement l'algorithme **Mark-and-Sweep** (*Marquer et Balayer*) :

1. Le GC part d'un point de départ global (le *Root*, comme l'objet `window`).
2. Il parcourt toutes les références accessibles en cascade ("Est-ce que cet objet est lié à la racine ?").
3. **Marquage :** Tout ce qui est accessible est marqué "vivant".
4. **Balayage (Sweep) :** Tout ce qui n'a pas été marqué (les objets isolés, inaccessibles) est détruit et la mémoire est libérée.

### Les fuites mémoire (*Memory Leaks*) typiques en SPA (Angular/React)

Une fuite mémoire se produit lorsqu'un objet est "inutile" pour ton application, mais que **le Garbage Collector croit qu'il est toujours vivant** parce qu'il reste une référence active qui pointe vers lui.

1. **Abonnements RxJS non résiliés (Très courant en Angular) :**
* Tu t'abonnes à un `Observable` dans un composant (`this.myService.getData().subscribe(...)`). Si tu quittes le composant sans détruire l'abonnement (`unsubscribe`, `takeUntil`, ou l'opérateur `take`), l'observable garde une référence en mémoire sur ton composant, qui ne peut pas être nettoyé par le GC.


2. **Écouteurs d'événements globaux oubliés :**
* Ajouter un `window.addEventListener('resize', ...)` dans un composant et oublier de le retirer (`removeEventListener`) lors de sa destruction.


3. **Éléments DOM détachés :**
* Conserver une référence en JavaScript vers un élément HTML que tu as supprimé du DOM. L'élément reste stocké en mémoire.



---

# 🎯 3 Questions Pièges d'Entretien (Module 2)

À toi de jouer. Essaie de répondre à ces 3 grands classiques :

### Question 1

> *"Quel sera l'ordre d'affichage dans la console pour ce code, et pourquoi ?"*
> ```javascript
> console.log('1');
> setTimeout(() => console.log('2'), 0);
> Promise.resolve().then(() => console.log('3'));
> console.log('4');
> 
> ```
> 
> 

### Question 2

> *"Qu'est-ce que l'Event Loop, et que se passe-t-il si une macro-tâche (ou un calcul lourd) prend 2 secondes à s'exécuter dans la Call Stack ?"*

### Question 3

> *"Dans une application Angular, tu observes que la mémoire RAM augmente continuellement à chaque fois que l'utilisateur navigue entre deux pages précises. Quelle est la cause la plus probable et comment la corriges-tu ?"*

---
## Loupe sur l'Event loop

Comment interagit l'Event loop avec la call stack ?

1. **Le code synchrone d'abord :** Le moteur exécute tout le script ligne par ligne, de haut en bas, en empilant et dépilant sur la Call Stack.
2. **Le stockage en attendant :** Pendant que la Call Stack est occupée, si des promesses se résolvent ou si des `setTimeout` se déclenchent, ils ne s'exécutent pas : ils patientent sagement dans leur file d'attente respective (Microtask Queue ou Macrotask Queue).
3. **Le réveil de l'Event Loop :** Dès que la Call Stack est **totalement vide**, l'Event Loop se déclenche, vide d'abord *toutes* les micro-tâches d'un coup, puis prend *une* macro-tâche, et ainsi de suite.

---

### La toute petite subtilité de pro pour finir (Le piège des Micro-tâches)

Puisque tu as parfaitement compris le mécanisme, retiens juste ce dernier détail :
Si, pendant que l'Event Loop vide la file des **Micro-tâches**, une micro-tâche crée *elle-même* une nouvelle promesse (une nouvelle micro-tâche), **l'Event Loop la traite immédiatement** avant de passer aux macro-tâches.

* *Le risque :* Si tu fais une boucle infinie de promesses (récursivité de micro-tâches), tu bloques l'Event Loop et tu affames les macro-tâches (on appelle ça la *starvation*). Mais en pratique, on évite de faire ça !

---
