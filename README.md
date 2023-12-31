import pygame
import sys

# יצירת משחק
pygame.init()
screen = pygame.display.set_mode((800, 600))

class Character(pygame.sprite.Sprite):
    def __init__(self, x, y):
        super().__init__()
        self.image = pygame.Surface((50, 50))
        self.image.fill((255, 0, 0))
        self.rect = self.image.get_rect(center=(x, y))

    def update(self):
        self.rect.x += 5
        if self.rect.x > 800:
            self.rect.x = -50

class Player(Character):
    def __init__(self, x, y):
        super().__init__(x, y)
        self.image.fill((0, 255, 0))

    def shoot(self, group):
        for character in group:
            if pygame.sprite.collide_rect(self, character):
                character.kill()

characters = pygame.sprite.Group()
player = Player(400, 300)
characters.add(player)

for i in range(5):
    character = Character(-50*i, 100)
    characters.add(character)

# לולאת המשחק
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    player.shoot(characters)
    characters.update()

    screen.fill((255, 255, 255))
    characters.draw(screen)
    pygame.display.flip()
    pygame.time.delay(100)
