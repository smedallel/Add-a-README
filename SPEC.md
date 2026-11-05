# Spec : Liste de tâches (Todo)

## Hypothèses
1. Application web simple, utilisable sur téléphone et ordinateur (pas d'app native).
2. Pas de compte ni de serveur : les tâches sont enregistrées dans le navigateur (localStorage).
3. HTML, CSS et JavaScript sans framework, pour rester simple à comprendre et à héberger.
4. Interface en français.
5. Hébergement gratuit possible plus tard avec GitHub Pages.

## Objectif
Permettre à une personne de noter ses tâches du jour, les cocher quand elles sont faites, et les retrouver en rouvrant la page.

Récits utilisateur :
- En tant qu'utilisateur, je tape une tâche et j'appuie sur « Ajouter » (ou Entrée) pour l'ajouter à la liste.
- Je coche une tâche pour la marquer comme faite ; elle apparaît barrée.
- Je supprime une tâche avec un bouton dédié.
- Je filtre l'affichage : Toutes / À faire / Faites.
- Je vois le nombre de tâches restantes.
- Je vide d'un coup toutes les tâches faites.
- Je retrouve mes tâches après avoir fermé et rouvert la page.

## Stack technique
- HTML5, CSS3, JavaScript (modules ES), aucune dépendance en production.
- Tests : exécuteur de tests intégré à Node.js (`node --test`, Node 20+).

## Commandes
- Lancer en local : `npx serve .` puis ouvrir http://localhost:3000
- Tests : `node --test tests/`

## Structure du projet
```
index.html        → page unique
src/style.css     → styles (mobile d'abord)
src/todos.js      → logique pure (ajouter, cocher, supprimer, filtrer, compter)
src/storage.js    → lecture/écriture localStorage
src/app.js        → liaison entre la page et la logique
tests/todos.test.js → tests de la logique
```

## Style de code
- `const`/`let`, jamais `var` ; fonctions courtes ; noms en anglais dans le code, textes affichés en français.
- La logique ne modifie jamais l'état en place : elle renvoie une nouvelle liste.

```js
export function addTodo(todos, text) {
  const title = text.trim();
  if (!title) return todos;
  return [...todos, { id: crypto.randomUUID(), title, done: false }];
}
```

## Stratégie de test
- Tests unitaires sur toute la logique de `src/todos.js` (cas normaux + texte vide, espaces, id inconnu).
- Vérification manuelle dans le navigateur : ajout, cochage, suppression, filtres, rechargement de page.

## Limites
- Toujours : tester la logique avant chaque commit, échapper le texte saisi (pas d'`innerHTML` avec du contenu utilisateur).
- Demander d'abord : ajouter une dépendance, un serveur ou une base de données.
- Jamais : envoyer les tâches à un service extérieur, committer des secrets.

## Critères de réussite
- Une tâche vide ou faite uniquement d'espaces n'est pas ajoutée.
- Les tâches et leur état coché survivent à un rechargement de la page.
- Les filtres et le compteur « X tâches restantes » sont toujours justes.
- La page est utilisable sur un écran de 360 px de large.
- `node --test tests/` passe sans erreur.

## Questions ouvertes
1. Faut-il pouvoir modifier le texte d'une tâche après l'avoir ajoutée ?
2. Faut-il des dates d'échéance ou des priorités ? (proposé : non pour la première version)
3. Veux-tu la mettre en ligne sur GitHub Pages à la fin ?
