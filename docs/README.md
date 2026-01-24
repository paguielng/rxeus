# Slime Soccer

Un jeu de football 2D basé sur la physique mettant en scène deux slimes qui s'affrontent sur un terrain bleu. Ce projet est une réinterprétation fidèle du classique jeu de navigateur original créé par Quin Pendragon, développée avec React et Canvas.

## Vue d'ensemble

**Slime Soccer** est un jeu arcade rétro qui combine des mécaniques de physique réalistes avec une jouabilité simple mais profonde. Deux joueurs contrôlent des créatures gélatineuses (slimes) dans le but de marquer des buts en propulsant une balle dans les filets adverses. Le jeu propose plusieurs modes de jeu chronométrés et une interface intuitive.

## Caractéristiques principales

### Mécanique de jeu

- **Physique réaliste** : Implémentation complète de la gravité, des rebonds et des collisions pour une expérience de jeu authentique
- **Contrôle des slimes** : Les joueurs peuvent se déplacer horizontalement, sauter et interagir avec la balle de manière fluide
- **Système de scoring** : Détection automatique des buts avec comptage en temps réel
- **Balle dynamique** : La balle répond aux forces physiques, aux collisions avec les slimes et aux limites du terrain

### Modes de jeu

Le jeu propose cinq durées de match différentes pour adapter le temps de jeu à vos préférences :

| Mode | Durée | Utilisation |
|------|-------|------------|
| 1 Minute | 60 secondes | Matchs rapides et intenses |
| 2 Minutes | 120 secondes | Jeu équilibré et accessible |
| 4 Minutes | 240 secondes | Matchs standard |
| 8 Minutes | 480 secondes | Parties longues et stratégiques |
| World Cup | 300 secondes | Mode spécial (5 minutes) |

### Interface utilisateur

- **Menu principal** : Sélection facile des modes de jeu avec boutons clairs
- **Affichage du score** : Visualisation en temps réel des scores des deux équipes
- **Chronomètre** : Compte à rebours visible au centre de l'écran
- **Écran de fin** : Affichage du gagnant avec option pour rejouer

## Contrôles

### Joueur de gauche (Slime cyan)

| Action | Touche |
|--------|--------|
| Déplacement gauche | **A** |
| Déplacement droit | **D** |
| Saut | **W** |

### Joueur de droite (Slime rouge)

| Action | Touche |
|--------|--------|
| Déplacement gauche | **J** |
| Déplacement droit | **L** |
| Saut | **I** |

## Architecture technique

### Structure du code

Le projet est construit autour d'un composant React monolithique (`SlimeSoccer`) qui gère l'ensemble de la logique du jeu. Voici les principaux éléments :

#### État du jeu

```typescript
gameStateRef.current = {
  leftSlime: { x, y, vx, vy, color },
  rightSlime: { x, y, vx, vy, color },
  ball: { x, y, vx, vy }
}
```

#### Constantes de physique

| Constante | Valeur | Description |
|-----------|--------|-------------|
| `GRAVITY` | 0.6 | Accélération gravitationnelle appliquée aux objets |
| `SLIME_SPEED` | 5 | Vitesse de déplacement horizontal des slimes |
| `SLIME_JUMP_POWER` | -12 | Force initiale du saut (valeur négative = vers le haut) |
| `BALL_DAMPING` | 0.99 | Amortissement de l'air (ralentissement progressif) |
| `BALL_BOUNCE_DAMPING` | 0.8 | Perte d'énergie lors des rebonds |

### Boucle de jeu

La boucle de jeu utilise `requestAnimationFrame` pour assurer une animation fluide à 60 FPS. Elle exécute deux étapes principales à chaque frame :

1. **Mise à jour de la physique** (`updatePhysics`) : Traite les entrées clavier, met à jour les positions et vérifie les collisions
2. **Rendu** (`draw`) : Dessine tous les éléments du jeu sur le canvas

### Système de collision

Le jeu implémente plusieurs types de collisions :

- **Collision avec les limites** : Les slimes et la balle ne peuvent pas sortir du terrain
- **Collision avec le sol** : Les slimes et la balle rebondissent sur le sol gris
- **Collision slime-balle** : Détection de collision circulaire avec transfert de vélocité
- **Détection de but** : La balle qui touche le sol dans les zones de but marque un point

### Rendu graphique

Le jeu utilise l'API Canvas 2D pour le rendu. Les éléments visuels incluent :

- **Terrain** : Fond bleu (ciel) avec zone grise (sol)
- **Slimes** : Demi-cercles avec yeux pour l'expression
- **Balle** : Cercle jaune doré
- **Buts** : Grille de carrés blancs représentant les filets

## Installation et utilisation

### Prérequis

- Node.js 18+ ou version supérieure
- npm ou pnpm comme gestionnaire de paquets

### Installation

```bash
# Cloner le dépôt
git clone <repository-url>
cd slime-soccer-game

# Installer les dépendances
npm install
# ou
pnpm install
```

### Lancer le jeu

```bash
# Mode développement
npm run dev
# ou
pnpm dev
```

Le jeu sera accessible à `http://localhost:3000` dans votre navigateur.

### Construire pour la production

```bash
# Créer une version optimisée
npm run build
# ou
pnpm build

# Prévisualiser la version de production
npm run preview
# ou
pnpm preview
```

## Mécanique de physique détaillée

### Mouvement des slimes

Les slimes sont contrôlés par le joueur via les touches directionnelles. Leur mouvement est limité à l'axe horizontal, tandis que la gravité affecte leur position verticale. Chaque slime peut sauter une seule fois tant qu'elle est en contact avec le sol.

### Comportement de la balle

La balle suit une physique réaliste avec les caractéristiques suivantes :

- **Gravité** : La balle accélère vers le bas à chaque frame
- **Amortissement de l'air** : La vélocité horizontale diminue progressivement
- **Rebonds** : Lorsque la balle touche une surface, sa vélocité est inversée et réduite par le coefficient d'amortissement
- **Transfert de momentum** : Quand un slime frappe la balle, sa vélocité est transférée à la balle

### Détection des collisions

La détection de collision slime-balle utilise une approche basée sur la distance. Si la distance entre le centre de la balle et le centre du slime est inférieure à la somme de leurs rayons, une collision est détectée. La réponse à la collision repositionne la balle et recalcule sa vélocité en fonction de l'angle de collision.

## Améliorations futures

Le jeu peut être enrichi avec les fonctionnalités suivantes :

- **Mode IA** : Ajouter un adversaire contrôlé par l'ordinateur pour le jeu en solo
- **Système de saisie de balle** : Permettre aux slimes de saisir et de tenir la balle
- **Anti-camping** : Pénaliser les joueurs qui restent trop longtemps dans leur zone de but
- **Effets sonores** : Ajouter des sons pour les rebonds, les buts et les événements de jeu
- **Animations améliorées** : Intégrer des animations fluides pour les slimes et la balle
- **Statistiques** : Tracker les performances des joueurs (buts marqués, temps de possession, etc.)
- **Thèmes visuels** : Proposer différents thèmes et palettes de couleurs
- **Contrôles personnalisables** : Permettre aux joueurs de redéfinir leurs touches

## Dépannage

### Le jeu ne démarre pas

Vérifiez que vous avez installé toutes les dépendances avec `npm install` ou `pnpm install`. Assurez-vous également que vous utilisez une version compatible de Node.js (18+).

### Les contrôles ne répondent pas

Assurez-vous que la fenêtre du navigateur a le focus. Cliquez sur le canvas du jeu avant de commencer à jouer pour vous assurer que les événements clavier sont correctement capturés.

### Le jeu est saccadé ou lent

Vérifiez que votre navigateur n'exécute pas d'autres applications gourmandes en ressources. Fermez les onglets inutiles et essayez de relancer le jeu. Si le problème persiste, mettez à jour votre navigateur vers la dernière version.

## Crédits

- **Jeu original** : Créé par Quin Pendragon (début des années 2000)
- **Remake** : Hector Bennett
- **Réimplémentation React** : Développé avec React 19 et Canvas 2D

## Licence

Ce projet est fourni à titre éducatif et récréatif. Veuillez consulter le fichier LICENSE pour plus de détails sur les conditions d'utilisation.

## Contribution

Les contributions sont bienvenues ! Si vous souhaitez améliorer le jeu, veuillez :

1. Fork le dépôt
2. Créer une branche pour votre fonctionnalité (`git checkout -b feature/ma-fonctionnalite`)
3. Commit vos modifications (`git commit -m 'Ajouter ma fonctionnalité'`)
4. Push vers la branche (`git push origin feature/ma-fonctionnalite`)
5. Ouvrir une Pull Request

## Support

Pour toute question, bug ou suggestion, veuillez ouvrir une issue sur le dépôt GitHub du projet.

---

**Dernière mise à jour** : Janvier 2026

**Version** : 1.0.0
