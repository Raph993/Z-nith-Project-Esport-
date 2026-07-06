# Guide de déploiement — Zenith Esport (100% gratuit, Firebase Spark)

Ce guide t'explique, étape par étape, comment mettre le site en ligne gratuitement,
depuis la création du projet Firebase jusqu'à l'adresse finale en `.web.app`.

---

## 1. Prérequis

- [Node.js](https://nodejs.org/) installé (version 18 ou plus).
- Un compte Google (pour la Console Firebase).
- [Visual Studio Code](https://code.visualstudio.com/) (ou un autre éditeur).

Vérifie que Node est bien installé :
```bash
node -v
npm -v
```

---

## 2. Créer le projet Firebase

1. Va sur https://console.firebase.google.com
2. Clique sur **"Ajouter un projet"**.
3. Donne-lui un nom, par exemple `zenith-esport`.
4. Désactive Google Analytics si tu ne veux pas t'en servir (optionnel).
5. Clique sur **Créer le projet**.

⚠️ Le projet est automatiquement sur le plan **Spark (gratuit)**. Tu n'as
**rien à payer** et rien à activer de payant pour ce site.

---

## 3. Activer Firebase Authentication

1. Dans le menu de gauche : **Build > Authentication**.
2. Clique sur **Get started**.
3. Onglet **Sign-in method** :
   - Active **Google**.
   - Active **E-mail/Mot de passe**.
4. Enregistre.

---

## 4. Activer Firestore Database

1. Menu de gauche : **Build > Firestore Database**.
2. Clique sur **Créer une base de données**.
3. Choisis le mode **Production** (les règles de sécurité du projet gèrent déjà les accès).
4. Choisis une région proche de tes utilisateurs (ex: `eur3 (europe-west)`).

---

## 5. Activer Firebase Storage

1. Menu de gauche : **Build > Storage**.
2. Clique sur **Get started**.
3. Garde le mode production (les règles du projet sont déjà prévues).

---

## 6. Récupérer la configuration de ton application web

1. Dans la Console Firebase, clique sur l'icône ⚙️ (Paramètres du projet).
2. Descends jusqu'à **"Vos applications"**.
3. Clique sur l'icône **</>** (Web) pour ajouter une application web.
4. Donne-lui un nom (ex : `zenith-esport-web`).
5. **Ne coche PAS** "Configurer Firebase Hosting" ici (on le fera en ligne de commande).
6. Firebase t'affiche un objet `firebaseConfig` : copie-le.

Ouvre le fichier **`js/firebase-config.js`** dans le projet et remplace les valeurs :

```js
const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  storageBucket: "...",
  messagingSenderId: "...",
  appId: "...",
};
```

---

## 7. Créer ton compte administrateur et récupérer ton UID

1. Lance le site en local (étape 9) OU déploie-le une première fois sans te
   soucier des droits admin.
2. Connecte-toi une fois sur le site avec le compte que tu veux utiliser
   comme administrateur (Google ou e-mail/mot de passe).
3. Dans la Console Firebase : **Authentication > Users**, repère la colonne
   **User UID** de ton compte, et copie cette valeur.
4. Colle cet UID à **trois endroits** :
   - `js/firebase-config.js` → `export const ADMIN_UID = "TON_UID";`
   - `firebase/firestore.rules` → dans la fonction `isAdmin()`
   - `firebase/storage.rules` → dans la fonction `isAdmin()`
5. Redéploie ensuite les règles et le site (voir étapes 10 et 11).

---

## 8. Installer les outils Firebase (CLI)

Dans un terminal, à la racine du projet :

```bash
npm install -g firebase-tools
firebase login
```

Une fenêtre de navigateur s'ouvre : connecte-toi avec le même compte Google
que celui utilisé pour créer le projet Firebase.

Relie ensuite ce dossier à ton projet Firebase :

```bash
firebase use --add
```

Sélectionne ton projet (`zenith-esport`) et choisis un alias, par exemple `default`.
Cela met à jour automatiquement le fichier `.firebaserc`.

---

## 9. Tester le site en local (facultatif mais recommandé)

```bash
firebase emulators:start --only hosting
```

Ouvre l'URL affichée dans le terminal (en général `http://localhost:5000`).

---

## 10. Déployer les règles de sécurité et les index

```bash
firebase deploy --only firestore:rules,firestore:indexes,storage
```

---

## 11. Déployer le site (Hosting)

```bash
firebase deploy --only hosting
```

À la fin de la commande, Firebase affiche ton **Hosting URL**, du type :

```
https://zenith-esport.web.app
```

C'est l'adresse officielle et gratuite de ton site !

Pour tout redéployer en une seule commande (règles + site) après une modification :

```bash
firebase deploy
```

---

## 12. Ajouter tes premières données

Va dans **Firestore Database** (Console Firebase) et crée tes premiers
documents, ou connecte-toi sur `/pages/admin.html` avec ton compte
administrateur et utilise le tableau de bord pour tout ajouter
(joueurs, matchs, récompenses, actualités, partenaires) sans toucher au code.

Collections utilisées par le site (créées automatiquement au premier ajout) :
- `users` — profils utilisateurs (créés automatiquement à l'inscription)
- `joueurs` — roster de l'équipe
- `matchs` — matchs à venir et résultats
- `pronostics` — votes des utilisateurs
- `recompenses` — boutique de récompenses
- `demandes_recompenses` — suivi des échanges
- `actualites` — articles publiés
- `partenaires` — sponsors affichés en page d'accueil

---

## 13. Connecter un nom de domaine personnalisé (plus tard)

1. Console Firebase > **Hosting** > **Ajouter un domaine personnalisé**.
2. Saisis ton domaine (ex : `zenithesport.com`), acheté chez n'importe quel
   registrar (OVH, Namecheap, Google Domains, etc. — l'achat du nom de
   domaine reste payant chez le registrar, mais le **branchement à Firebase
   Hosting est gratuit**).
3. Suis les instructions pour ajouter les enregistrements DNS (A / TXT)
   chez ton registrar.
4. Aucune modification de code n'est nécessaire.

---

## Résumé des commandes utiles

```bash
firebase login                # Connexion au compte Google
firebase use --add            # Lier le dossier à ton projet Firebase
firebase emulators:start      # Tester en local
firebase deploy               # Déployer le site + règles + index
firebase deploy --only hosting            # Déployer uniquement le site
firebase deploy --only firestore:rules    # Déployer uniquement les règles Firestore
```

---

## Dépannage rapide

- **"Missing or insufficient permissions"** : vérifie que ton `ADMIN_UID`
  est identique dans `firebase-config.js`, `firestore.rules` et
  `storage.rules`, et que tu as bien redéployé les règles.
- **Les images ne s'affichent pas** : remplace les URLs d'images par des
  liens valides (Firebase Storage, ou n'importe quelle URL d'image publique)
  depuis le tableau de bord admin.
- **"The query requires an index"** : Firestore affiche un lien direct dans
  la console du navigateur pour créer l'index en un clic — ou redéploie
  `firebase/firestore.indexes.json` avec `firebase deploy --only firestore:indexes`.
