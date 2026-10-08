# Dossier Deliveroo — mode d'emploi

## Les fichiers

| Fichier | Contenu |
|---|---|
| `index.html` | La question en trois lignes et le premier calcul |
| `entreprise.html` | Les comptes de Deliveroo France SAS |
| `marche.html` | La page « Le marché » demandée au TD 1 |
| `mesure.html` | Le protocole du test A/B |
| `questionnaire.html` | Le questionnaire à envoyer aux répondants |
| `proposition.html` | La proposition finale (structure à remplir) |
| `methode.html` | Sources et usage de l'IA |

Les encadrés jaunes signalent ce que le groupe doit encore faire ou vérifier.

## Relier le questionnaire à Google Sheets

Tant que `ENDPOINT` est vide dans `questionnaire.html`, le questionnaire est en mode test : rien n'est enregistré en ligne.

1. Créer une feuille Google Sheets vide.
2. Menu Extensions, puis Apps Script. Remplacer le code par celui-ci :

```javascript
const COLONNES = ["id", "horodatage", "version", "filtre",
  "vw_trop_bon_marche", "vw_bon_marche", "vw_cher", "vw_trop_cher", "vw_coherent",
  "gg_2_49", "gg_3_49", "gg_4_49", "gg_5_99", "gg_6_99", "gg_coherent",
  "controle", "age", "statut", "commandes_30j", "abonnements"];

function doPost(e) {
  const feuille = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
  if (feuille.getLastRow() === 0) feuille.appendRow(COLONNES);
  const r = JSON.parse(e.postData.contents);
  feuille.appendRow(COLONNES.map(c => r[c] === undefined ? "" : r[c]));
  return ContentService.createTextOutput("ok");
}
```

3. Déployer, Nouveau déploiement, type « Application Web », exécuter en tant que « Moi », accès « Tout le monde ».
4. Copier l'adresse qui finit par `/exec` et la coller entre les guillemets de `const ENDPOINT = "";` dans `questionnaire.html`.
5. Répondre une fois au questionnaire et vérifier qu'une ligne apparaît dans la feuille.

## Publier

Sur GitHub Pages :

1. Se connecter sur github.com, puis « New repository ». Nom : par exemple `dossier-deliveroo`. Visibilité : Public. Créer.
2. Sur la page du dépôt vide, cliquer « uploading an existing file ».
3. Glisser tous les fichiers de ce dossier (pas le dossier lui-même) : les sept `.html`, `style.css` et ce `LISEZMOI.md`. Cliquer « Commit changes ».
4. Onglet Settings, menu Pages. Source : « Deploy from a branch ». Branche : `main`, dossier `/ (root)`. Save.
5. Attendre une à deux minutes : l'adresse du site s'affiche en haut de cette page, sous la forme `https://VOTRE-NOM.github.io/dossier-deliveroo/`.

Pour modifier une page ensuite : ouvrir le fichier sur GitHub, icône crayon, modifier, « Commit changes ». Le site se met à jour seul.

Le site doit rester en ligne jusqu'au jury du semestre 1. Le lien à envoyer aux répondants est l'adresse du site suivie de `questionnaire.html`.

## Avant de lancer la collecte (12 octobre)

- Faire tester le questionnaire par quelques personnes extérieures au groupe.
- Vérifier dans la feuille que les versions A et B arrivent toutes les deux.
- Supprimer les lignes de test avant de commencer.
