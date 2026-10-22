# 🏛️ Module 1 : Le Moteur du Navigateur & le Critical Rendering Path

---

## 1.1 La Chaîne de Rendu Critique (*Critical Rendering Path*)

Lorsque le navigateur télécharge le code d'une page web, il doit convertir ces fichiers texte en structures de données en mémoire. Cette transformation se fait en **3 grandes étapes de parsing et de construction**.

```
[ Code HTML ] ───> DOM Tree ┐
                            ├───> Render Tree ───> Layout ───> Paint ───> Composite
[ Code CSS  ] ───> CSSOM   ┘

```

### A. HTML $\rightarrow$ DOM (*Document Object Model*)

Le navigateur lit le flux de données HTML sous forme d'octets (*bytes*) et effectue trois opérations :

1. **Conversion en caractères** (selon l'encodage, ex: UTF-8).
2. **Tokenisation** : Le navigateur identifie les balises (`<html>`, `<body>`, `<div>`, `<h1>`).
3. **Arbre du DOM** : Ces jetons (*tokens*) sont transformés en nœuds (*Nodes*) reliés entre eux dans une structure arborescente.

> **Propriété clé :** La construction du DOM est **incrémentale**. Le navigateur n'attend pas de recevoir 100 % du fichier HTML pour commencer à construire le DOM. Dès qu'il reçoit les premiers paquets, il commence à les parser.

### B. CSS $\rightarrow$ CSSOM (*CSS Object Model*)

Pendant qu'il construit le DOM, s'il rencontre une balise `<link rel="stylesheet">` ou `<style>`, le navigateur interrompt le traitement des composants dépendants pour télécharger et parser le CSS.

* Contrairement au DOM, **le CSSOM n'est PAS incrémental**. Le CSS est **bloquant pour le rendu** (*render-blocking*).
* **Pourquoi ?** Parce qu'une règle située à la fin du fichier CSS (`body { display: none; }`) peut annuler ou surcharger toutes les règles précédentes. Le navigateur doit avoir analysé l'intégralité du CSS pour connaître le style final de chaque élément.

### C. Le *Render Tree* (Arbre de Rendu)

Une fois le DOM et le CSSOM construits, le navigateur les combine pour créer le **Render Tree**.

* **Ce qu'il contient :** Seuls les éléments visibles à l'écran avec leurs styles appliqués.
* **Le piège classique d'entretien :**
* Un élément en `visibility: hidden` ou `opacity: 0` **FAIT PARTIE** du *Render Tree* (il prend de la place et est calculé, même s'il est invisible).
* Un élément en `display: none` **NE FAIT PAS PARTIE** du *Render Tree* (il est totalement exclu de la chaîne de rendu, tout comme la balise `<head>`).



---

## 1.2 La Géométrie et la Peinture : Reflow, Repaint et Composite

Une fois le *Render Tree* constitué, le navigateur passe aux 3 phases de calcul et d'affichage visuel :

### 1. Layout / Reflow (Calcul de géométrie)

Le navigateur parcourt le *Render Tree* pour calculer la position exacte ($x, y$) et les dimensions (largeur, hauteur) de chaque élément par rapport au *viewport* (l'écran).

* **Ce qui le déclenche :** Modifier la taille de la fenêtre, ajouter/supprimer un élément du DOM, lire ou modifier des propriétés géométriques en JS (`offsetWidth`, `getBoundingClientRect()`, `width`, `margin`).
* **Coût CPU :** **Extrêmement coûteux.** Un *Reflow* sur un parent peut recalculer la géométrie de tous ses enfants dans l'arbre.

### 2. Paint / Repaint (Remplissage des pixels)

Une fois les positions géométriques connues, le navigateur dessine chaque nœud à l'écran (texte, couleurs, bordures, ombres, images).

* **Ce qui le déclenche :** Modifier une propriété purement visuelle qui ne change pas la géométrie (ex: `background-color`, `color`, `box-shadow`, `visibility`).
* **Coût CPU :** Moins coûteux qu'un *Reflow*, mais reste lourd si de grandes zones de l'écran doivent être repeintes.

### 3. Composite (Assemblage par le GPU)

Pour optimiser les performances, le navigateur découpe la page en plusieurs calques (*Layers*). Durant la phase de *Composite*, le GPU (carte graphique) superpose ces calques pour composer l'image finale.

* **Propriétés optimisées GPU :** `transform` (ex: `translate3d`, `scale`) et `opacity`.
* **Pourquoi c'est rapide ?** Modifier `transform` ou `opacity` **ne déclenche ni Reflow ni Repaint**. Le calcul est délégué directement au GPU, garantissant une animation fluide à 60 FPS (images par seconde).

---

## 1.3 Stratégies de Chargement des Scripts

Par défaut, lorsqu'un navigateur rencontre une balise `<script src="app.js">` dans le HTML, il **stoppe immédiatement le parsing du DOM** (comportement *parser-blocking*), télécharge le fichier JS, puis l'exécute avant de reprendre la lecture du HTML.

Pour éviter de bloquer l'affichage de la page, on utilise les attributs `async` et `defer` :

| Attribut | Chargement (Réseau) | Exécution du JS | Respect de l'ordre d'écriture ? | Bloque le DOM ? |
| --- | --- | --- | --- | --- |
| **Par défaut** | Synchrone | **Immédiate** (stoppe le DOM) | Oui | **Oui** |
| **`async`** | Asynchrone (en arrière-plan) | **Dès la fin du téléchargement** (interrompt le DOM) | Non (le 1er chargé s'exécute) | **Oui** (au moment de l'exécution) |
| **`defer`** | Asynchrone (en arrière-plan) | **Après le parsing complet du DOM** (juste avant `DOMContentLoaded`) | **Oui** | **Non** |

> **Règle d'or :** Pour une Single Page Application (Angular/React), on utilise **`defer`** pour charger les bundles principaux sans bloquer le rendu du HTML initial. `async` est réservé aux scripts indépendants (ex: Google Analytics, scripts de tracking).

---

# 🎯 3 Questions Pièges d'Entretien (Module 1)

Voici 3 questions réelles posées en entretien pour vérifier si tu maîtrises ce sujet. Entraîne-toi à synthétiser tes réponses (ou enregistre-toi) :

### Question 1

> *"Quelle est la différence fondamentale entre `display: none` et `visibility: hidden` du point de vue des performances du navigateur et de la chaîne de rendu ?"*

---

### Question 2

> *"Pourquoi dit-on que le CSS est 'render-blocking' alors que le JS est 'parser-blocking' ?"*

---

### Question 3

> *"Qu'est-ce que le Layout Thrashing (ou Forced Synchronous Layout) en JavaScript, et comment l'évite-t-on ?"*

---

As-tu des questions sur le **Module 1**, ou souhaite-tu qu'on passe au **Module 2 : Moteur JS, Event Loop & Gestion Mémoire** ?
