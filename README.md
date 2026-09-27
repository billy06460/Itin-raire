# Itinéraire & Péages

Application de calcul d'itinéraire avec détection des gares de péage sur le
trajet, packagée en application native Android/iOS via
[Capacitor](https://capacitorjs.com/).

Tout le code de l'app (formulaire, carte, logique, jeu de données des gares
de péage) vit dans un seul fichier : `www/index.html`. Capacitor se charge
juste de l'emballer dans une vraie app native.

## Pourquoi Capacitor plutôt qu'un simple wrapper WebView

Contrairement à un outil comme WebIntoApp qui charge le HTML tel quel
(`file://...`), Capacitor sert le contenu local via une véritable origine
`https://localhost` (voir `"androidScheme": "https"` dans
`capacitor.config.json`). Les requêtes réseau (géocodage, calcul
d'itinéraire, tuiles de carte) envoient donc un `Origin`/`Referer` correct,
ce qui règle les blocages rencontrés précédemment.

## Prérequis

- [Node.js](https://nodejs.org/) (18+) et npm
- [Android Studio](https://developer.android.com/studio) (pour builder/tester en local)
- Un compte GitHub, si tu veux utiliser la génération automatique d'APK par CI

## Mettre ce projet sur GitHub (aucune installation locale requise)

Le build de l'APK (npm install, ajout de la plateforme Android, compilation)
est entièrement automatisé par `.github/workflows/build-android.yml` et
s'exécute sur les serveurs de GitHub. Tu n'as donc besoin ni de Node.js, ni
d'Android Studio en local pour obtenir un APK :

```bash
cd capacitor-app
git init
git add .
git commit -m "Init app Itinéraire & Péages (Capacitor)"
git branch -M main
git remote add origin https://github.com/<ton-compte>/<nom-du-repo>.git
git push -u origin main
```

Puis va dans l'onglet **Actions** du dépôt sur GitHub : le workflow démarre
automatiquement, construit l'APK, et le propose en téléchargement dans la
section "Artifacts" du run (compte 2 à 5 minutes).

## Builder/tester en local à la place (optionnel)

Utile seulement si tu veux tester sur un émulateur ou modifier le projet
natif directement. Nécessite Node.js et Android Studio.

```bash
# 1. Installer les dépendances
npm install

# 2. Ajouter la plateforme Android (génère le dossier android/, une seule fois)
npx cap add android

# 3. Copier www/ dans le projet natif et resynchroniser après chaque modif de l'app
npx cap sync android

# 4. Ouvrir le projet dans Android Studio pour lancer/tester sur un émulateur ou un téléphone
npx cap open android
```

Depuis Android Studio : Run ▶ pour tester sur un appareil connecté, ou
Build > Build Bundle(s)/APK(s) > Build APK(s) pour générer un `.apk`
installable directement.

À chaque fois que tu modifies `www/index.html`, relance `npx cap sync
android` puis reconstruis dans Android Studio.

## Générer l'APK automatiquement via GitHub Actions

Ce dépôt inclut `.github/workflows/build-android.yml` : à chaque push sur
`main`, GitHub construit automatiquement un APK de debug et le met à
disposition en téléchargement dans l'onglet **Actions** du dépôt (sous
"Artifacts"), sans que tu aies besoin d'Android Studio.

## Mettre ce projet sur GitHub

```bash
cd capacitor-app
git init
git add .
git commit -m "Init app Itinéraire & Péages (Capacitor)"
git branch -M main
git remote add origin https://github.com/<ton-compte>/<nom-du-repo>.git
git push -u origin main
```

Va ensuite dans l'onglet **Actions** du dépôt sur GitHub : le build de
l'APK démarre automatiquement.

## Icône et splash screen (optionnel)

Pour personnaliser l'icône et l'écran de démarrage :

```bash
npm install @capacitor/assets --save-dev
npx capacitor-assets generate
```

(nécessite de placer une image source dans `resources/icon.png` et
`resources/splash.png`, voir la doc de
[`@capacitor/assets`](https://github.com/ionic-team/capacitor-assets)).

## Publier sur le Play Store

Pour une publication officielle (pas seulement un test), il faudra signer
l'APK/AAB avec une clé de release — voir la doc Capacitor :
https://capacitorjs.com/docs/android/deploying-to-google-play
