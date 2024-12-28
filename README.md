import pygame
import math

# Инициализация Pygame
pygame.init()

# Параметры окна
WIDTH, HEIGHT = 800, 600
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Дрифт Машина")

# Цвета
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
RED = (255, 0, 0)

# Класс для машины
class Car:
    def __init__(self, x, y, angle=0):
        self.x = x
        self.y = y
        self.angle = angle
        self.speed = 0
        self.max_speed = 5
        self.turn_speed = 5
        self.image = pygame.Surface((50, 30))
        self.image.fill(RED)
        self.rect = self.image.get_rect(center=(self.x, self.y))

    def update(self, keys):
        # Управление машиной
        if keys[pygame.K_UP]:
            self.speed = min(self.speed + 0.1, self.max_speed)  # Увеличение скорости
        if keys[pygame.K_DOWN]:
            self.speed = max(self.speed - 0.1, -self.max_speed)  # Уменьшение скорости
        if keys[pygame.K_LEFT]:
            self.angle -= self.turn_speed  # Поворот влево
        if keys[pygame.K_RIGHT]:
            self.angle += self.turn_speed  # Поворот вправо

        # Перемещение машины
        self.x += self.speed * math.cos(math.radians(self.angle))
        self.y -= self.speed * math.sin(math.radians(self.angle))

        # Ограничиваем движение машины внутри окна
        self.x = max(0, min(WIDTH, self.x))
        self.y = max(0, min(HEIGHT, self.y))

        # Обновляем положение машины
        self.rect.center = (self.x, self.y)

    def draw(self, surface):
        rotated_image = pygame.transform.rotate(self.image, self.angle)
        rotated_rect = rotated_image.get_rect(center=self.rect.center)
        surface.blit(rotated_image, rotated_rect.topleft)

# Главный игровой цикл
def main():
    clock = pygame.time.Clock()
    car = Car(WIDTH // 2, HEIGHT // 2)  # Начальная позиция машины
    running = True

    while running:
        screen.fill(WHITE)
        
        # Обработка событий
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False

        # Получение состояния клавиш
        keys = pygame.key.get_pressed()

        # Обновление состояния машины
        car.update(keys)

        # Отображение машины
        car.draw(screen)

        # Обновление экрана
        pygame.display.flip()

        # Ограничение FPS
        clock.tick(60)

    pygame.quit()

# Запуск игры
if __name__ == "__main__":
    main()
