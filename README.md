# ES Autos – Suivi des chantiers

Outil de suivi des chantiers du garage, utilisable sur téléphone et ordinateur, synchronisé en temps réel grâce à Firebase et hébergé gratuitement sur GitHub Pages.

Temps de mise en place : environ 30 minutes, une seule fois.

## Contenu du dossier

| Fichier | Rôle |
|---|---|
| `index.html` | L'application complète |
| `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png` | Icône et nom quand on l'ajoute à l'écran d'accueil |
| `firestore.rules` | Règles de sécurité à coller dans Firebase |
| `logo/` | Logo ES Autos (SVG pour l'impression et le web, PNG pour les réseaux) |

Sans configuration Firebase, l'application fonctionne déjà, mais chaque appareil garde ses propres données. Les étapes ci-dessous activent la synchronisation.

---

## Étape 1 : créer le projet Firebase

1. Aller sur https://console.firebase.google.com et se connecter avec un compte Google (idéalement celui du garage).
2. Cliquer sur **Créer un projet**, le nommer `es-autos`. Google Analytics n'est pas nécessaire, on peut le désactiver.
3. Le projet reste sur l'offre gratuite **Spark**. Aucune carte bancaire n'est demandée.

## Étape 2 : déclarer l'application web

1. Sur la page d'accueil du projet, cliquer sur l'icône **Web** (`</>`).
2. Nom de l'application : `ES Autos`. Ne pas cocher Firebase Hosting.
3. Firebase affiche un bloc `firebaseConfig`. Le garder ouvert, il sert à l'étape 6.

## Étape 3 : créer le compte de connexion du garage

1. Menu **Authentication** → **Commencer**.
2. Onglet **Méthode de connexion** → activer **Adresse e-mail/Mot de passe**.
3. Onglet **Utilisateurs** → **Ajouter un utilisateur** : saisir l'e-mail du garage et un mot de passe solide.

C'est ce compte qui servira à se connecter sur le téléphone et l'ordinateur. L'application ne permet pas de créer de compte elle-même, c'est volontaire.

## Étape 4 : créer la base de données

1. Menu **Firestore Database** → **Créer une base de données**.
2. Choisir un emplacement en Europe (par exemple `europe-west9`, Paris).
3. Démarrer en **mode production**.

## Étape 5 : protéger les données

1. Dans Firestore, onglet **Règles**.
2. Remplacer tout le contenu par celui du fichier `firestore.rules`.
3. Remplacer `contact@es-autos.fr` par l'e-mail créé à l'étape 3. Pour autoriser plusieurs comptes : `['garage@exemple.fr', 'associe@exemple.fr']`.
4. Cliquer sur **Publier**.

Sans cette étape, n'importe qui pourrait lire les chantiers. Ne pas la sauter.

## Étape 6 : brancher l'application

Ouvrir `index.html` avec un éditeur de texte (Bloc-notes suffit) et chercher `FIREBASE_CONFIG`. Recopier les valeurs du bloc de l'étape 2 :

```js
const FIREBASE_CONFIG = {
  apiKey: "AIza...",
  authDomain: "es-autos-xxxx.firebaseapp.com",
  projectId: "es-autos-xxxx",
  storageBucket: "es-autos-xxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

La clé `apiKey` n'est pas un secret : elle identifie le projet, c'est normal qu'elle soit visible dans la page. La sécurité repose sur le compte de connexion et les règles de l'étape 5.

## Étape 7 : mettre en ligne avec GitHub Pages

1. Créer un compte sur https://github.com.
2. **New repository**, nom : `es-autos`, visibilité **Public** (obligatoire pour GitHub Pages gratuit ; les chantiers, eux, restent dans Firebase et ne sont pas visibles).
3. **Add file** → **Upload files** : glisser tout le contenu du dossier (y compris le dossier `logo`), puis **Commit changes**.
4. **Settings** → **Pages** → Source : **Deploy from a branch**, branche `main`, dossier `/ (root)` → **Save**.
5. Après une à deux minutes, l'adresse s'affiche : `https://NOM-UTILISATEUR.github.io/es-autos/`.

## Étape 8 : autoriser l'adresse GitHub dans Firebase

**Authentication** → **Paramètres** → **Domaines autorisés** → **Ajouter un domaine** : `NOM-UTILISATEUR.github.io`.

Sans cela, la connexion est refusée.

## Étape 9 : installer sur le téléphone et l'ordinateur

- **iPhone** : ouvrir l'adresse dans Safari → bouton Partager → **Sur l'écran d'accueil**.
- **Android** : ouvrir l'adresse dans Chrome → menu ⋮ → **Ajouter à l'écran d'accueil** ou **Installer l'application**.
- **Ordinateur** : ouvrir l'adresse dans Chrome ou Edge et l'ajouter aux favoris (ou icône d'installation dans la barre d'adresse).

Se connecter une fois sur chaque appareil. La connexion est mémorisée.

---

## Utilisation au quotidien

- **Synchronisation** : une modification faite sur le téléphone apparaît sur l'ordinateur en quelques secondes. Sans réseau, l'outil continue de fonctionner et envoie les modifications au retour de la connexion (indicateur « Hors ligne » en haut).
- **Plaque connue** : en tapant une plaque déjà venue au garage, le modèle, le client et le téléphone sont repris automatiquement, avec l'historique des passages et le dernier kilométrage.
- **Sauvegarde** : menu ⋯ → **Télécharger une sauvegarde**, une fois par mois par précaution.
- **Récupérer des chantiers saisis ailleurs** : dans l'ancienne version, **Télécharger une sauvegarde**, puis dans la nouvelle, **Restaurer une sauvegarde**.
- **Effacer tout** : menu ⋯ → **Effacer tous les chantiers…**, avec confirmation. L'effacement s'applique à tous les appareils.

## Mettre à jour l'application

Remplacer `index.html` dans le dépôt GitHub (**Add file** → **Upload files**) en gardant la configuration Firebase de l'étape 6. Les chantiers ne sont pas touchés.

## Limites de l'offre gratuite

L'offre Spark de Firebase inclut un quota quotidien de lectures et d'écritures largement suffisant pour un garage (plusieurs dizaines de milliers d'opérations par jour). Les quotas exacts sont affichés dans la console Firebase, rubrique **Utilisation et facturation**.
