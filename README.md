# Pacman

Tu es un expert en développement de jeux en JavaScript vanilla.
Crée un jeu Pac-Man complet, jouable et fidèle à l'original de 1980,
en un seul fichier HTML autonome (HTML + CSS + JS inline).

═══════════════════════════════════════════
🗺️ LABYRINTHE
═══════════════════════════════════════════
- Grille de 28 colonnes × 31 lignes (tuiles de 16px)
- Dessiné sur un <canvas> HTML5 (448×496px)
- Murs bleus (#1a1aff) avec coins arrondis style arcade
- Fond noir (#000)
- Deux tunnels de téléportation gauche/droite au milieu
- Disposition exacte du niveau 1 original Pac-Man (encode la map
  sous forme de tableau 2D : 0=vide, 1=mur, 2=pac-gomme, 3=super-gomme,
  4=tunnel, 5=maison fantômes)

═══════════════════════════════════════════
🟡 PAC-MAN (joueur)
═══════════════════════════════════════════
- Rayon 7px, couleur jaune (#FFD700)
- Animation bouche : ouvre/ferme en 8 frames (angle 0° → 45° → 0°)
- Orientée selon la direction de déplacement (rotation canvas)
- Contrôles clavier : flèches directionnelles
- Mémorisation du prochain mouvement demandé (buffer 1 frame)
- Vitesse : 75% de la vitesse de base sur les pac-gommes, normale ailleurs
- Collision murs : pixel-perfect sur les centres de tuile
- Téléportation instantanée dans les tunnels

═══════════════════════════════════════════
👻 LES 4 FANTÔMES
═══════════════════════════════════════════
Blinky (rouge #FF0000) :
  - IA "Chase" : cible toujours la tuile exacte de Pac-Man
  - Sort immédiatement de la maison au démarrage

Pinky (rose #FFB8FF) :
  - IA "Ambush" : cible 4 tuiles devant Pac-Man
  - Sort après 0 pac-gommes mangées

Inky (cyan #00FFFF) :
  - IA "Flanking" : calcul vectoriel entre Blinky et 2 tuiles
    devant Pac-Man (× 2)
  - Sort après 30 pac-gommes mangées

Clyde (orange #FFB852) :
  - IA "Random/Shy" : chase si > 8 tuiles de Pac-Man,
    sinon retourne à son coin (bas-gauche)
  - Sort après 1/3 des pac-gommes mangées

Comportements communs :
  - 3 modes cycliques par niveau : Scatter (coin assigné) →
    Chase → Scatter → Chase... (minuteries originales)
  - Mode Frightened (bleu #0000FF → clignotant blanc après 2s)
    déclenché par super-gomme, durée 6s niveau 1
  - Mode Eyes (retour maison) : yeux animés, vitesse × 2,
    traversent les murs sauf tunnels
  - Interdiction de faire demi-tour (sauf changement de mode)
  - Ralentissement dans les tunnels (40% vitesse)
  - Pathfinding : toujours choisir la direction minimisant
    la distance euclidienne vers la cible (aux intersections)

═══════════════════════════════════════════
🎯 SCORE & SYSTÈME DE POINTS
═══════════════════════════════════════════
- Pac-gomme : 10 pts
- Super-gomme : 50 pts
- Fantôme mangé : 200→400→800→1600 (combo cumulé par super-gomme)
  Afficher brièvement le score à la position du fantôme
- Cerise (niveau 1) : 100 pts — apparaît après 70 et 170 pac-gommes
  mangées, pendant 9 secondes
- 1 vie bonus à 10 000 pts
- Score affiché en haut (police monospace pixel-art, blanc)
- HIGH SCORE centré en haut

═══════════════════════════════════════════
🏠 MAISON DES FANTÔMES
═══════════════════════════════════════════
- Zone centrale protégée avec porte rose (2 tuiles)
- Les fantômes sortent un par un selon leur compteur
- Mouvement de "rebond" vertical dans la maison en attente
- Après avoir mangé tous les fantômes ou fin de frightened :
  retour automatique à la maison, renaissance

═══════════════════════════════════════════
🎬 ÉTATS DU JEU
═══════════════════════════════════════════
1. ATTRACT : écran titre clignotant "INSERT COIN" + animation démo
2. START : "READY!" jaune 2 secondes, tous figés
3. PLAYING : jeu actif
4. DYING : animation mort Pac-Man (rotation 0°→360° en 1.5s)
           puis respawn ou GAME OVER
5. LEVEL_CLEAR : tous les murs clignotent blanc/bleu × 3, pause 2s
6. GAME_OVER : texte rouge clignotant, retour attract après 5s
- Compteur de vies : 3 vies, icônes Pac-Man en bas à gauche

═══════════════════════════════════════════
🔊 SONS (Web Audio API — synthèse, pas de fichiers externes)
═══════════════════════════════════════════
- Waka-waka : oscillateur carré alterné 220Hz/440Hz rythmé
- Super-gomme : son grave long
- Fantôme mangé : bruit blanc filtré descendant
- Mort : mélodie chromatique descendante
- Sirène de fond : ton sinusoïdal qui accélère avec les fantômes restants
- Intermède début niveau : jingle 3 notes fidèle

═══════════════════════════════════════════
📐 TECHNIQUE & QUALITÉ CODE
═══════════════════════════════════════════
- Moteur basé sur requestAnimationFrame avec delta time
- Game loop fixe à 60fps avec interpolation
- Tout dans une classe `PacManGame` avec méthodes claires
- Police "Press Start 2P" (Google Fonts) pour le texte
- Responsive : le canvas se centre dans la page
- Fond de page : #000
- Commentaires JSDoc sur chaque méthode principale
- Aucune dépendance externe sauf la police Google

Lance le jeu automatiquement au chargement de la page.
