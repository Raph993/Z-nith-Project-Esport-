# Zenith Esport — Site officiel (Brawl Stars)

Site officiel d'équipe e-sport : HTML5 / CSS3 / JavaScript (frontend) +
Firebase Authentication / Firestore / Storage / Hosting (backend), 100%
gratuit sur le plan Spark.

## Démarrage rapide

1. Ouvre ce dossier dans Visual Studio Code.
2. Suis **GUIDE_DEPLOIEMENT.md** pour créer ton projet Firebase et remplir
   `js/firebase-config.js`.
3. Déploie avec `firebase deploy`.

## Structure du projet

```
/css                 → feuille de style globale (design system)
/js                  → toute la logique (auth, pages, admin, config Firebase)
/images /icons       → assets visuels
/pages               → toutes les pages secondaires (joueurs, matchs, admin...)
/assets              → réservé aux futurs fichiers additionnels
/firebase            → règles de sécurité Firestore/Storage + index
firebase.json        → configuration de déploiement Firebase Hosting
.firebaserc          → alias du projet Firebase
GUIDE_DEPLOIEMENT.md → guide pas-à-pas complet
```

## Pages

| Page | Fichier | Description |
|---|---|---|
| Accueil | `index.html` | Bannière, présentation, résultats, prochain match, roster, partenaires |
| Joueurs | `pages/joueurs.html` | Roster complet |
| Matchs | `pages/matchs.html` | Matchs à venir + résultats (BO1/BO3/BO5) |
| Pronostics | `pages/pronostics.html` | Vote avant chaque match, historique, statistiques |
| Classement | `pages/classement.html` | Top 100 automatique |
| Récompenses | `pages/recompenses.html` | Boutique à points |
| Actualités | `pages/actualites.html` | Articles (annonces, recrutement, news) |
| Profil | `pages/profil.html` | Infos utilisateur, avatar, récompenses débloquées |
| Administration | `pages/admin.html` | Tableau de bord complet (accès restreint) |

## Points d'attention avant mise en production

- Remplace **ADMIN_UID** dans `js/firebase-config.js`, `firebase/firestore.rules`
  et `firebase/storage.rules` par ton UID Firebase Auth réel.
- Remplace les images de `/images` (générées comme simples placeholders) par
  ton vrai logo et tes visuels d'équipe.
- Ajoute tes vrais liens Discord / X / YouTube / TikTok dans
  `js/components.js` (section footer) et `index.html` (section hero).
- Le crédit des points de pronostics (+15) et l'échange de récompenses sont
  volontairement gérés côté client + règles Firestore, car le plan Spark ne
  permet pas les Cloud Functions déclenchées automatiquement en base. C'est
  une limite du 100% gratuit — largement suffisante pour un site
  communautaire, mais à garder en tête si le site grandit beaucoup.
