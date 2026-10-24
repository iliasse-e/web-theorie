# ⚙️ Module 5 : Invariants des Frameworks & Paradigmes

---

## 5.1 State vs UI (L'État et la Vue)

Le principe fondamental de tout framework moderne se résume en une formule mathématique :


$$\text{UI} = f(\text{State})$$

* **La Vue (UI)** n'est qu'une projection visuelle de ton **État (State)** à un instant $T$.
* **La règle d'or du développement moderne :** On ne manipule plus jamais le DOM directement (plus de `document.getElementById().innerText = ...`). On modifie *uniquement* l'état, et c'est le framework qui se charge de répercuter le changement sur l'affichage. Rompre cette règle, c'est s'assurer des bugs de synchronisation intenables.

---

## 5.2 Virtual DOM vs Signals (La guerre des moteurs de rendu)

Comment le framework sait-il qu'il doit repeindre l'écran quand une donnée change ? Il y a deux grandes philosophies historiques :

### A. Le Virtual DOM (React / Vue)

* **Le principe :** À chaque modification d'état, le framework recrée en mémoire une copie légère de l'arbre DOM (le Virtual DOM). Il compare cet arbre avec le précédent via un algorithme de *diffing*, calcule exactement quelle case a changé, et applique *uniquement* cette modification sur le vrai DOM du navigateur.
* **Le défaut :** Même optimisé, ce mécanisme fait du travail de calcul permanent ("est-ce que ça a changé ?"), ce qui peut consommer de la CPU sur les grosses applications.

### B. Les Signals & la Réactivité Fine (Angular moderne / SolidJS)

* **Le principe :** Pas de Virtual DOM. Un **Signal** est un conteneur de valeur qui émet une notification dès que sa valeur change.
* **La magie :** Le framework (ou le compilateur) sait précisément quel nœud du DOM est branché sur quel Signal. Quand le Signal change, **le DOM est mis à jour directement au millilitre près**, sans recalculer tout un arbre de composants. C'est ultra-rapide et économe en mémoire.

---

## 5.3 L'Injection de Dépendances (DI)

L'**Injection de Dépendances** est un design pattern qui consiste à faire "descendre" ou "fournir" les dépendances (services, API clients) à une classe plutôt que de les créer en dur à l'intérieur.

* **Pourquoi c'est vital ?**
1. **Découplage (SoC) :** Ton composant Angular ne sait pas *comment* les données sont récupérées, il demande juste le service.
2. **Testabilité :** Lors d'un test unitaires, tu peux facilement remplacer ton service HTTP réel par un faux service (*mock*) grâce à la DI.


* **Le concept clé sous Angular : Le Tree-Shaking.**
En écrivant `@Injectable({ providedIn: 'root' })`, tu dis à Angular : *"Ce service est global. Si aucun composant ne l'utilise, le compilateur le supprime purement et simplement du bundle final (Tree-Shaking)"*.

---

## 5.4 La Séparation des Préoccupations (*Separation of Concerns* - SoC) & Le Pattern Smart/Dumb

Dans une application d'entreprise propre, on sépare strictement les composants en deux catégories :

1. **Les Smart Components (Containers / Conteneurs) :**
* Ils ont la charge de la **logique métier et des données**.
* Ce sont eux qui appellent les services HTTP, gèrent le state global ou les flux RxJS/Signals, et écoutent les actions.


2. **Les Dumb Components (Presentational / Présentationnels) :**
* Ils n'ont **aucune logique métier ni appel réseau**.
* Ils reçoivent des données via des attributs (`@Input()` sous Angular) et émettent des événements via des outputs (`@Output()`). Ils ne font que "rendre" proprement l'UI (ex: un tableau réutilisable, un bouton stylisé).



---

# 🎯 3 Questions Pièges d'Entretien (Module 5)

À ton tour de valider tes connaissances sur ces concepts d'architecture :

### Question 1

> *"Quelle est la différence fondamentale de performance entre le mécanisme de Virtual DOM (utilisé par React) et le mécanisme de Signals (adopté par les versions récentes d'Angular) ?"*

### Question 2

> *"Qu'appelle-t-on le pattern 'Smart / Dumb components' (ou Container / Presentational), et quel est l'intérêt concret de séparer ainsi tes composants front ?"*

### Question 3

> *"Quel est l'avantage majeur du décorateur `@Injectable({ providedIn: 'root' })` par rapport à l'enregistrement d'un service dans le tableau `providers: [...]` d'un composant ?"*

---

Balance tes réponses quand tu es chaud, et on décortiquera tout ça !
