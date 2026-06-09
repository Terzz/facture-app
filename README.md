# TERZ PROD — Générateur de factures

Outil de facturation **statique** (un seul fichier HTML, sans backend ni base de données) pour
auto-entrepreneur (EI). Formulaire à gauche, aperçu de la facture en direct à droite, impression /
PDF, et gestion **multi-factures** avec historique enregistré dans le navigateur.

➡️ **En ligne :** https://terzz.github.io/facture-app/

## Ce que fait l'outil

- Formulaire à gauche, **aperçu live** de la facture à droite.
- **Multi-factures** : enregistre plusieurs factures et les garde en mémoire du navigateur.
- **Historique** : liste des factures enregistrées (numéro, client, date, total) ; clic pour rouvrir
  et modifier ; suppression possible.
- **Numérotation automatique** : `INV-1`, `INV-2`, `INV-3`… le prochain numéro se calcule tout seul
  (le champ reste modifiable manuellement).
- Calcul des totaux au format français, mention **« TVA non applicable, art. 293 B du CGI »**.
- **Impression / PDF** via le navigateur (`window.print()`).
- **Export / Import JSON** (par facture, et sauvegarde complète de l'historique).

## Comment l'utiliser

- **En ligne** : ouvrir https://terzz.github.io/facture-app/
- **En local** : double-cliquer sur `index.html` (aucune installation, aucun build).
- Remplir les champs ; ajouter / dupliquer / réordonner / supprimer des lignes.
- **« Nouvelle facture »** : crée une facture vierge (émetteur pré-rempli, numéro auto, date du jour).
- **« Enregistrer »** : sauvegarde la facture en cours dans l'historique. Un point orange ● à côté du
  bouton signale des **modifications non enregistrées**.
- **Rouvrir** une facture depuis l'historique pour la modifier, puis « Enregistrer » à nouveau.
- **Imprimer / PDF** : bouton « Imprimer / PDF » → « Enregistrer au format PDF » dans la boîte
  d'impression du navigateur.

## ⚠️ Où sont stockées les données

Les factures sont enregistrées **dans CE navigateur, sur CET appareil** (via `localStorage`).

- Elles **ne sont pas synchronisées** : un autre appareil, un autre navigateur ou le mode navigation
  privée **ne les verront pas**.
- **Vider le cache / les données du site = perte des factures.**
- 👉 La **vraie sauvegarde**, c'est l'**export JSON**.

## Sauvegarde & transfert (export JSON)

- **« Exporter JSON »** : télécharge la facture **en cours** (fichier nommé d'après son numéro).
- **« Exporter tout »** : télécharge **tout l'historique** (`factures-sauvegarde.json`) — c'est votre
  sauvegarde réelle et votre moyen de transfert entre appareils.
- **« Importer JSON »** : recharge un fichier exporté. L'import **détecte automatiquement** le format
  (facture seule ou sauvegarde complète).
- ⚠️ Ne **committez pas** ces `.json` dans le repo public (données client). Le `.gitignore` les exclut
  déjà.

## Tes informations émetteur (vie privée)

Pour préserver ta vie privée, **aucune donnée personnelle n'est dans le code public**. Tes informations
émetteur (raison sociale, SIRET, adresse, téléphone, IBAN, BIC) sont **saisies une fois et conservées
dans ton navigateur** (`localStorage`), puis pré-remplies automatiquement sur chaque nouvelle facture.
Elles voyagent aussi dans tes exports JSON.

- Sur un **nouvel appareil / navigateur** : ressaisis-les une fois (elles seront mémorisées), ou importe
  une sauvegarde JSON (qui les contient).
- Le site publié ne sert que des champs vides — rien de personnel n'est exposé.

## Déploiement (GitHub Pages)

Site statique pur, hébergé gratuitement sur GitHub Pages depuis la branche `main`, dossier racine.
**Toute modification poussée sur `main` est republiée automatiquement** (~1 min).

Mise en place initiale (déjà faite, conservée pour mémoire) :

```bash
git init
git add index.html README.md .gitignore
git commit -m "Initial commit"
gh repo create Terzz/facture-app --public --source=. --remote=origin --push
gh api -X POST repos/Terzz/facture-app/pages -f "source[branch]=main" -f "source[path]=/"
```

Mises à jour ensuite :

```bash
git add -A
git commit -m "Décrire le changement"
git push
```

## Développement

Fichier unique `index.html` (HTML + CSS + JS *inline*). Aucune dépendance, aucun build. Modifier,
ouvrir dans le navigateur pour tester, committer, pousser.
