import pygame
import random

# Initialisation de Pygame
pygame.init()

# Définition des constantes
SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Création de la fenêtre du jeu
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Course de Moto avec Freestyle")

# Classe pour la moto
class Moto(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((50, 30))
        self.image.fill(RED)
        self.rect = self.image.get_rect()
        self.rect.center = (SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2)
        self.speed = 0

    def update(self):
        self.rect.x += self.speed

# Classe pour les obstacles (figures de freestyle)
class Obstacle(pygame.sprite.Sprite):
    def __init__(self):
        super().__init__()
        self.image = pygame.Surface((20, 20))
        self.image.fill(BLACK)
        self.rect = self.image.get_rect()
        self.rect.x = random.randrange(SCREEN_WIDTH - self.rect.width)
        self.rect.y = random.randrange(-100, -40)
        self.speedy = random.randrange(3, 8)

    def update(self):
        self.rect.y += self.speedy
        if self.rect.top > SCREEN_HEIGHT + 10:
            self.rect.x = random.randrange(SCREEN_WIDTH - self.rect.width)
            self.rect.y = random.randrange(-100, -40)
            self.speedy = random.randrange(3, 8)

# Création des groupes de sprites
all_sprites = pygame.sprite.Group()
obstacles = pygame.sprite.Group()

# Création de la moto
moto = Moto()
all_sprites.add(moto)

# Boucle de jeu
running = True
while running:
    # Gestion des événements
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    # Mise à jour de la moto
    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        moto.speed = -5
    elif keys[pygame.K_RIGHT]:
        moto.speed = 5
    else:
        moto.speed = 0

    # Génération d'obstacles
    if random.randrange(100) < 3:
        obstacle = Obstacle()
        all_sprites.add(obstacle)
        obstacles.add(obstacle)

    # Mise à jour de tous les sprites
    all_sprites.update()

    # Vérification des collisions entre la moto et les obstacles
    collisions = pygame.sprite.spritecollide(moto, obstacles, True)
    if collisions:
        # Ici, vous pouvez ajouter des actions à effectuer en cas de collision,
        # comme réinitialiser la position de la moto ou réduire le score, etc.

    # Effacement de l'écran
    screen.fill(WHITE)

    # Dessin de tous les sprites
    all_sprites.draw(screen)

    # Rafraîchissement de l'écran
    pygame.display.flip()

    # Limite de FPS
    pygame.time.Clock().tick(60)

# Quitter Pygame
pygame.quit()
![414353527_332862489600884_2949598922720242094_n](https://github.com/boussim06/boussim06/assets/160792650/53949391-3ee0-4e2d-b8f4-7efbc9a07c88)
