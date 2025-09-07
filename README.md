# Snake Bogota

## 1. Comment jouer ?

### Principe de gameplay
**Snake Bogota** est une version revisitée du classique Snake. Vous contrôlez un serpent qui doit manger des pommes pour grandir et marquer des points, tout en évitant de se mordre la queue ou de heurter les murs. Le terrain de jeu est décoré et centré sur une zone boueuse, avec des pommes classiques et des pommes en or qui rapportent plus de points.

### Commandes
- **Déplacement** :  
  - Flèches directionnelles (↑, ↓, ←, →)  
  - ZQSD ou WASD (Z/W = haut, Q/A = gauche, S = bas, D = droite)
- **Pause / Reprendre** : Espace
- **Valider dans les menus** : Entrée ou Espace
- **Naviguer dans les menus** : Flèches ou ZQSD/WASD
- **Quitter** : Échap (dans les menus)

### Comment gagner ?
Le but est de faire le meilleur score possible en mangeant un maximum de pommes sans mourir. Il n’y a pas de fin, le jeu continue tant que vous ne perdez pas.

### Comment perdre ?
Vous perdez si :
- Le serpent touche un mur.
- Le serpent se mord la queue.

---

## 2. Originalité

### Spécificités de Gameplay
- **Terrain centré et décoré** : La grille de jeu est centrée sur une zone boueuse, avec un décor visible autour.
- **Deux types de pommes** :  
  - Pomme classique (rouge)  
  - Pomme en or (goldapple.png) qui rapporte plus de points et a une probabilité d’apparition configurable.
- **Effets visuels** :  
  - La tête du serpent a deux yeux noirs, la queue est colorée en rose.
  - Les pommes sont affichées avec des images personnalisées.
- **Effets sonores** : Un son est joué à chaque fois que le serpent mange une pomme.
- **Menus stylisés** : Utilisation d’une police personnalisée et navigation clavier uniquement.

### Algorithmes spécifiques
- **Placement aléatoire des pommes** sans collision avec le serpent (`GameScene.cs`, méthode `SpawnApple`).
- **Gestion de la direction et de la queue** du serpent avec une structure de type LIFO (`Snake.cs`, classe `Snake`, méthodes `Move`, `Grow`).
- **Service Locator** pour la gestion centralisée des services (`ServiceLocator.cs`).

#### Références code :
- Placement des pommes : `GameScene.cs` > `SpawnApple()`
- Gestion du serpent : `Snake.cs` > `Move()`, `Grow()`, `Draw()`
- Grille de jeu : `Grid.cs` > `GridToWorld()`, `Draw()`
- Service Locator : `ServiceLocator.cs` > `Register<T>()`, `Get<T>()`

**Expérience** :  
L’intégration de la grille centrée et des pommes spéciales a nécessité de repenser la gestion des coordonnées et l’algorithme de placement des objets pour éviter toute collision et garantir une expérience fluide et originale.

---

## 3. Votre code source

### Structure

- **Program.cs** : Point d’entrée, initialisation Raylib, boucle principale.
- **Scene.cs** : Classe abstraite pour la gestion des différentes scènes (menu, jeu, game over).
- **ScenesManager.cs** : Gestionnaire de scènes.
- **MenuScene.cs** : Menu principal, navigation, affichage.
- **GameScene.cs** : Logique principale du jeu, gestion du serpent, des pommes, du score.
- **GameOverScene.cs** : Affichage de l’écran de fin, navigation.
- **Snake.cs** : Logique du serpent (corps, déplacement, dessin).
- **Grid.cs** : Gestion de la grille de jeu, conversion coordonnées grille/monde.
- **Apple.cs** : Représentation d’une pomme (position, type).
- **ServiceLocator.cs** : Gestion centralisée des services (paramètres, score, assets, etc).

#### Relations principales :
- `ScenesManager` gère les transitions entre les différentes `Scene`.
- `GameScene` utilise `Grid`, `Snake`, `Apple`, et les services via `ServiceLocator`.
- `Snake` manipule la grille et son propre corps (queue).
- Les ressources (sons, images, polices) sont chargées/déchargées dans chaque scène.

### Algorithme de queue LIFO (Snake)
Le serpent est représenté par une `Queue<Coordinates>`.  
- **Avancer** : On ajoute une nouvelle tête à la fin de la queue (`Enqueue`) et on retire la queue (`Dequeue`).
- **Grandir** : On ajoute une nouvelle tête sans retirer la queue.
- **Collision** : On vérifie si la nouvelle tête existe déjà dans la queue.

**Code** :  
`Snake.cs` > `Move()`, `Grow()`, `Contains()`

### Algorithmes de grilles
- **Conversion grille → monde** :  
  `Grid.cs` > `GridToWorld(Coordinates)`  
  Permet de centrer la grille sur le terrain boueux en appliquant un offset à l’origine.
- **Affichage** :  
  `Grid.cs` > `Draw()`  
  Parcourt chaque cellule pour dessiner la grille.

### Service Locator
**Services mis en œuvre** :
- `GameSettings` : Paramètres du jeu (taille grille, vitesse, etc.)
- `ScoreManager` : Gestion du score et du highscore.
- `RandomProvider` : Générateur de nombres aléatoires.
- `AssetManager` : Gestion des ressources (images, sons, polices).
- `input` : Gestion des entrées clavier.

**Rôle** :
- Centralise l’accès aux services partagés.
- Facilite la maintenance et l’évolution du code (pas de dépendances directes entre classes).

**Exemple d’utilisation** :
var settings = ServiceLocator.Get<GameSettings>(); var rng = ServiceLocator.Get<RandomProvider>().Rng;
Cela permet d’accéder aux paramètres et au générateur aléatoire depuis n’importe quelle classe, sans couplage fort.

---

## 4. Ressources

- Placez toutes les images (`ingame.png`, `background.png`, `pomme.png`, `goldapple.png`), sons (`apple.mp3`), et polices (`Tytoon Mist.ttf`) dans le dossier d’exécution (`bin/Release/net8.0/`).
- Configurez chaque ressource dans Visual Studio pour qu’elle soit copiée automatiquement dans le dossier de sortie.

---

## 5. Build & Lancement

- **Build** : `Ctrl+Maj+B` dans Visual Studio ou `dotnet build -c Release`
- **Lancement** : Double-cliquez sur le `.exe` dans `bin/Release/net8.0/`
- **Dépendances** : .NET 8, Raylib-cs

---

Bon jeu !
