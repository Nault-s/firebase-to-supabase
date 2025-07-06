
# NaultAgent : Agent Conversationnel Full-Stack (Android & Web)

Bienvenue dans le projet *NaultAgent*, une implémentation d'un agent conversationnel intelligent capable d'interagir avec l'utilisateur via une interface mobile (Android) et une interface web. Cet agent est conçu pour comprendre des intentions, planifier des actions et les exécuter, tout en gérant les ambiguïtés.

Le projet est divisé en deux parties principales :

1.  *`android/`*: Une application mobile native développée en Kotlin avec Jetpack Compose. L'agent logique est embarqué directement dans l'application.

2.  *`web/`*: Une application web comprenant un frontend en React.js et un backend en Node.js/Express. L'agent logique réside côté serveur pour la partie web.

## Table des Matières

1.  [Fonctionnalités](#fonctionnalités)

2.  [Prérequis](#prérequis)

3.  [Installation et Configuration](#installation-et-configuration)
    *   [Clés API](#clés-api)
    *   [Partie Android](#partie-android)
    *   [Partie Web](#partie-web)

4.  [Structure du Projet](#structure-du-projet)

5.  [Architecture Modulaire](#architecture-modulaire)
    *   [Modules Conceptuels de l'Agent](#modules-conceptuels-de-lagent)

6.  [Déploiement](#déploiement)

7.  [Utilisation](#utilisation)

8.  [Extensions Futures](#extensions-futures)

9.  [Licence](#licence)

## Fonctionnalités

•   *Compréhension du Langage Naturel (NLU)*: Analyse des requêtes utilisateur pour extraire les intentions et les entités.

•   *Planification d'Actions*: Détermination des étapes nécessaires pour accomplir une tâche.

•   *Exécution d'Actions*: Interfaçage avec les capacités du système (appels, SMS, navigation web, calendrier, etc.).

•   *Gestion des Alternatives*: Proposer des choix ou demander des précisions en cas d'ambiguïté.

•   *Interfaces Utilisateur*:
    *   *Android*: Interface de chat intuitive en Jetpack Compose.
    *   *Web*: Interface de chat réactive en React.js.

•   *Internationalisation (i18n)*: Support multilingue pour les deux plateformes.

•   *Architecture Modulaire*: Facilite l'ajout de nouvelles actions ou de nouveaux modèles d'IA.

## Prérequis

Assurez-vous d'avoir les éléments suivants installés sur votre machine :

### Pour le développement Android :

•   *Android Studio Flamingo | 2022.2.1 ou plus récent*

•   *JDK 17 ou plus récent*

•   *SDK Android 34* (API Level 34)

•   Un émulateur Android ou un appareil physique avec les services Google Play.

### Pour le développement Web :

•   *Node.js (LTS version recommandée, ex: 18.x ou 20.x)*

•   *npm* (généralement inclus avec Node.js) ou *Yarn*

## Installation et Configuration

### Clés API

L'agent peut utiliser des modèles de langage externes (LLM) comme OpenAI ou Google Gemini pour une meilleure compréhension du langage naturel. Vous aurez besoin de clés API pour cela.

•   *OpenAI API Key*: Obtenez-la sur [platform.openai.com](https://platform.openai.com/).

•   *Google Gemini API Key*: Obtenez-la sur [ai.google.dev](https://ai.google.dev/).

*Il est crucial de ne jamais exposer vos clés API dans le code client (Android ou React frontend).* Elles doivent être gérées côté serveur ou via des fichiers de configuration non versionnés.

### Partie Android

1.  *Cloner le dépôt :*
    bash
    git clone https://github.com/votre_utilisateur/NaultAgent.git
    cd NaultAgent/android
    
2.  *Ouvrir dans Android Studio :*
    Ouvrez le dossier `NaultAgent/android` avec Android Studio. Laissez Android Studio synchroniser le projet et télécharger les dépendances Gradle.

3.  *Configuration des Clés API (Android) :*
    Créez un fichier `local.properties` à la racine du dossier `android/` (au même niveau que `build.gradle.kts` du projet, pas du module `app/`).
    Ajoutez vos clés API comme suit :

properties
OPENAIAPIKEY="votrecleapiopenaiici"
GEMINIAPIKEY="votrecleapigeminiici"

    Ces clés seront injectées dans `BuildConfig` et utilisées par les services d'API. Si vous n'utilisez pas de LLM externe, vous pouvez ignorer cette étape.

4.  *Configuration du `AppModule.kt` :*
    Dans `android/app/src/main/java/com/naultcorp/naultagent/di/AppModule.kt`, décommentez la ligne correspondant à l'implémentation de l'agent que vous souhaitez utiliser (`OpenAIIntentRecognizer`, `GeminiIntentRecognizer` ou `LocalIntentRecognizer` par défaut).

kotlin
// Dans AppModule.kt, la méthode provideIntentRecognizer:
@Provides
@Singleton
fun provideIntentRecognizer(
    @ApplicationContext appContext: Context,
    openAIApiService: OpenAIApiService,
    geminiApiService: GeminiApiService
): IntentRecognizer {
    val openAIApiKey = BuildConfig.OPENAIAPIKEY
    val geminiApiKey = BuildConfig.GEMINIAPIKEY

    // Choisissez l'implémentation désirée:
    return LocalIntentRecognizer(appContext) // Agent local par défaut
    // return OpenAIIntentRecognizer(openAIApiService, openAIApiKey) // Décommentez pour OpenAI
    // return GeminiIntentRecognizer(geminiApiService, geminiApiKey) // Décommentez pour Gemini
}

5.  *Exécuter l'application :*
    Connectez un appareil Android ou lancez un émulateur. Cliquez sur le bouton `Run` (triangle vert) dans Android Studio.

    Lors du premier lancement, l'application demandera les permissions nécessaires. Acceptez-les pour un fonctionnement optimal.

### Partie Web

La partie web se compose d'un backend Node.js et d'un frontend React.js.

1.  *Navigation vers le dossier Web :*

bash
cd NaultAgent/web

2.  *Configuration des Clés API (Web Backend) :*
    Créez un fichier `.env` à la racine du dossier `web/`.


OPENAIAPIKEY=votrecleapiopenaiici
GEMINIAPIKEY=votrecleapigeminiici
PORT=3001 # Port pour le backend

    Ces clés seront utilisées par le backend Node.js.

3.  *Installation et Lancement du Backend (Node.js) :*

bash
cd server
npm install # ou yarn install
npm start   # ou yarn start

    Le backend démarrera sur `http://localhost:3001` (ou le port spécifié dans votre `.env`).

4.  *Installation et Lancement du Frontend (React.js) :*
    Ouvrez un *nouveau terminal*.

bash
cd ../client # Retournez au dossier web, puis allez dans client
npm install  # ou yarn install
npm start    # ou yarn start

    Le frontend démarrera sur `http://localhost:3000` (par défaut pour React). Il se connectera au backend Node.js.

## Structure du Projet


NaultAgent/
├── android/
│   ├── app/
│   │   ├── src/main/java/com/naultcorp/naultagent/
│   │   │   ├── NaultAgentApp.kt          # Application Hilt
│   │   │   ├── ui/                       # Composants UI (Jetpack Compose)
│   │   │   │   ├── MainActivity.kt
│   │   │   │   ├── components/
│   │   │   │   ├── screens/
│   │   │   │   └── theme/
│   │   │   ├── data/                     # Source de données (Room, Retrofit)
│   │   │   ├── di/                       # Modules d'injection de dépendances (Hilt)
│   │   │   ├── domain/                   # Interfaces et modèles métier
│   │   │   ├── agent/                    # Logique cœur de l'agent
│   │   │   │   ├── dialogue/             # Gestion du dialogue
│   │   │   │   ├── planning/             # Planification d'actions
│   │   │   │   ├── execution/            # Exécution d'actions
│   │   │   │   └── alternatives/         # Gestion des alternatives
│   │   │   └── util/                     # Fonctions utilitaires
│   │   ├── src/main/res/                 # Ressources Android (layouts, strings, etc.)
│   │   │   ├── values/strings.xml        # Strings par défaut (anglais)
│   │   │   └── values-fr/strings.xml     # Strings en français
│   │   └── AndroidManifest.xml           # Manifeste de l'application
│   ├── build.gradle.kts                  # Fichier Gradle du module
│   └── ... autres fichiers Gradle de projet
├── web/
│   ├── client/
│   │   ├── public/                       # Fichiers statiques du frontend
│   │   ├── src/
│   │   │   ├── App.js                    # Composant principal React
│   │   │   ├── components/               # Composants React réutilisables (ChatInput, Message)
│   │   │   ├── services/                 # Services pour les appels API au backend
│   │   │   ├── i18n.js                   # Configuration i18n (internationalisation)
│   │   │   └── index.js
│   │   ├── package.json                  # Dépendances du frontend
│   │   └── .env                          # Variables d'environnement pour le frontend (non sensibles)
│   ├── server/
│   │   ├── src/
│   │   │   ├── index.js                  # Point d'entrée de l'application Express
│   │   │   ├── routes/                   # Définition des routes API
│   │   │   ├── services/                 # Logique métier du backend (LLM, agent)
│   │   │   │   ├── llmService.js         # Wrapper pour les API LLM
│   │   │   │   ├── agentService.js       # Orchestrateur de l'agent
│   │   │   │   ├── dialogue.js           # Logique de dialogue (backend)
│   │   │   │   ├── planning.js           # Logique de planification (backend)
│   │   │   │   ├── execution.js          # Logique d'exécution (backend)

│   │   │   │   └── alternatives\.js       \# Logique de gestion des alternatives \(backend\)
│   │   │   └── utils/
│   │   ├── package\.json                  \# Dépendances du backend
│   │   └── \.env                          \# Variables d'environnement pour le backend \(sensibles\)
├── README\.md                             \# Ce fichier
└── \.gitignore                            \# Fichiers à ignorer par Git
\`\`\`

▎*Architecture Modulaire*

L'architecture est conçue pour être modulaire, facilitant la séparation des préoccupations et l'évolutivité\.

▎*Modules Conceptuels de l'Agent*

Les deux plateformes \(Android et Web\) implémentent les modules logiques suivants :

1\.  *Module de Dialogue \(`dialogue`\)*:
    \*   *Responsabilité*: Gérer la conversation avec l'utilisateur, comprendre l'intention \(NLU\), extraire les entités, et générer la réponse textuelle\.
    \*   *Implémentation*:
        \*   *Android*: `IntentRecognizer` \(Local, OpenAI, Gemini\) qui mappe les requêtes utilisateur à des `Intent` prédéfinies\.
        \*   *Web*: `dialogue\.js` dans le backend, utilisant les services LLM pour interpréter les messages et formuler des réponses\.

2\.  *Module de Planification \(`planning`\)*:
    \*   *Responsabilité*: À partir d'une intention reconnue, déterminer la ou les actions spécifiques à exécuter\. Cela peut impliquer des étapes multiples\.
    \*   *Implémentation*:
        \*   *Android*: `ActionPlanner` qui prend un `Intent` et retourne une `Action` concrète ou une séquence d'actions\.
        \*   *Web*: `planning\.js` dans le backend, qui convertit les intentions interprétées par le module de dialogue en des instructions exécutables\.

3\.  *Module d'Exécution \(`execution`\)*:
    \*   *Responsabilité*: Exécuter les actions planifiées\. Cela implique d'interfacer avec les APIs du système d'exploitation \(Android\) ou des services externes \(Web\)\.
    \*   *Implémentation*:
        \*   *Android*: `ActionExecutor` qui contient des fonctions pour `makeCall`, `sendSMS`, `openApp`, `setAlarm`, `addToCalendar`, etc\.
        \*   *Web*: `execution\.js` dans le backend\. Pour le web, les "actions" sont plus limitées au niveau du système de l'utilisateur \(on ne peut pas "appeler" depuis le serveur\)\. Elles concerneront plutôt des interactions avec des APIs web externes \(météo, actualités, base de données, etc\.\) ou la génération de données à afficher au client\. Pour cette démo, les actions web seront principalement des réponses informatives ou la simulation d'interactions\.

4\.  *Module d'Alternatives \(`alternatives`\)*:
    \*   *Responsabilité*: Gérer les cas où l'intention est ambiguë, incomplète ou lorsque plusieurs actions sont possibles\. Proposer des options à l'utilisateur ou demander des précisions\.
    \*   *Implémentation*:
        \*   *Android*: Intégré dans le `ViewModel` et le `IntentRecognizer` qui peut retourner un `Intent` de type `CLARIFICATION` ou `AMBIGUOUS`\.
        \*   *Web*: `alternatives\.js` dans le backend, qui gère les scénarios de clarification ou de choix multiples, souvent en générant des invites pour l'utilisateur\.

▎*Déploiement*

▎*Android :*

•   *Debug Build*: Lancez directement depuis Android Studio sur un émulateur ou un appareil connecté\.

•   *Release Build*: Générez un APK ou un App Bundle signé via `Build \> Generate Signed Bundle / APK\.\.\.` dans Android Studio\.

▎*Web :*

•   *Déploiement Local*: Suivez les étapes d'installation et de lancement ci-dessus\. Le frontend sera accessible via `http://localhost:3000` et le backend via `http://localhost:3001`\.

•   *Déploiement en Production*:
    \*   *Frontend \(React\)*: Utilisez `npm run build` \(ou `yarn build`\) pour créer une version optimisée de votre application React\. Le dossier `build/` peut ensuite être servi par n'importe quel serveur web statique \(Nginx, Apache, Netlify, Vercel, etc\.\)\.

\*   *Backend \(Node\.js\)*: Le serveur Node\.js peut être déployé sur des plateformes comme Heroku, AWS EC2, Google Cloud Run, Vercel \(pour les fonctions sans serveur\), Render, etc\. Assurez-vous de configurer correctement les variables d'environnement \(`\.env`\) sur votre plateforme d'hébergement\.

▎*Utilisation*

Une fois l'application lancée \(mobile ou web\) :

•   *Saisissez des requêtes* dans le champ de texte\.

•   *Exemples de requêtes \(avec `LocalIntentRecognizer` par défaut\) :*
    \*   `Call 1234567890` \(Android\)
    \*   `Send SMS to 0987654321 saying Hello there` \(Android\)
    \*   `Open Chrome` \(Android\)
    \*   `Browse google\.com` \(Android\)
    \*   `Set an alarm for 8:30 AM with message Wake up` \(Android\)
    \*   `Add an event to calendar titled Meeting at 10 AM tomorrow` \(Android\)
    \*   `Share this text: This is a test message` \(Android\)
    \*   `What can you do?` \(Android & Web\)
    \*   `I need help` \(Android & Web\)
    \*   `Tell me about NaultAgent` \(Web\)
    \*   `What is the weather like?` \(Web - si intégré via une API externe\)
    \*   `Clarify: Do you mean call John or Jane?` \(Android & Web - exemple de dialogue\)

▎*Extensions Futures*

Ce projet est une base solide\. Voici quelques pistes pour l'améliorer :

•   *Reconnaissance Vocale \(ASR\)*: Intégrer `SpeechRecognizer` sur Android et des APIs de reconnaissance vocale sur le web\.

•   *Gestion du Contexte*: Améliorer la capacité de l'agent à retenir des informations des conversations précédentes et à les utiliser pour des requêtes ultérieures\.

•   *Planification Avancée*: Utiliser des algorithmes de planification plus sophistiqués ou des modèles de langage plus grands pour générer des plans multi-étapes complexes \(e\.g\., "Réserve-moi un vol pour Paris demain et envoie un SMS à ma mère"\)\.

•   *Intégration d'API Tierces*: Permettre à l'agent d'interagir avec d'autres services web \(météo, actualités, réservations, etc\.\) pour étendre ses capacités\.

•   *Personnalisation des Actions*: Définir des actions personnalisées pour l'utilisateur\.

•   *Amélioration de l'UI/UX*: Ajouter des animations, des feedbacks visuels plus riches, des indicateurs de progression\.

•   *Sécurité Renforcée*: Implémenter une authentification et une autorisation robustes pour le backend web\.

•   *Base de Connaissances*: Intégrer une base de connaissances pour répondre à des questions factuelles\.

▎*Licence*

Ce projet est sous licence MIT\. Voir le fichier `LICENSE` pour plus de détails\.