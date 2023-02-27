# moden
Juego de carreras
import pygame
import random

# Inicializar Pygame
pygame.init()

# Definir el tamaño de la pantalla y el título del juego
screen_width = 800
screen_height = 600
screen = pygame.display.set_mode((screen_width, screen_height))
pygame.display.set_caption("Juego de Carreras")

# Definir los colores
white = (255, 255, 255)
black = (0, 0, 0)
red = (255, 0, 0)
green = (0, 255, 0)

# Definir la velocidad de los jugadores y la posición inicial
player_speed = 5
player_start_x = screen_width // 2
player_start_y = screen_height - 100

# Crear el jugador
player_width = 50
player_height = 100
player = pygame.Rect(player_start_x, player_start_y, player_width, player_height)

# Definir la velocidad y la posición inicial de los obstáculos
obstacle_speed = 5
obstacle_start_x = random.randint(0, screen_width - player_width)
obstacle_start_y = -100

# Crear la lista de obstáculos
obstacles = []

# Definir la fuente para el texto
font = pygame.font.SysFont(None, 50)

# Definir la función para mostrar el texto
def draw_text(text, color, x, y):
    text_surface = font.render(text, True, color)
    text_rect = text_surface.get_rect()
    text_rect.center = (x, y)
    screen.blit(text_surface, text_rect)

# Definir la función principal del juego
def game():
    # Establecer el estado inicial del juego
    game_over = False
    score = 0
    
    # Bucle principal del juego
    while not game_over:
        # Gestionar los eventos del juego
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
        
        # Mover el jugador
        keys = pygame.key.get_pressed()
        if keys[pygame.K_LEFT] and player.x > 0:
            player.x -= player_speed
        if keys[pygame.K_RIGHT] and player.x < screen_width - player_width:
            player.x += player_speed
        
        # Mover los obstáculos y eliminar los obstáculos que salen de la pantalla
        for obstacle in obstacles:
            obstacle.y += obstacle_speed
            if obstacle.y > screen_height:
                obstacles.remove(obstacle)
                score += 1
        
        # Crear nuevos obstáculos
        if len(obstacles) < 5:
            obstacle = pygame.Rect(obstacle_start_x, obstacle_start_y, player_width, player_height)
            obstacles.append(obstacle)
            obstacle_start_x = random.randint(0, screen_width - player_width)
            obstacle_start_y = -100
        
        # Comprobar si el jugador colisiona con un obstáculo
        for obstacle in obstacles:
            if player.colliderect(obstacle):
                game_over = True
        
        # Limpiar la pantalla y dibujar los elementos del juego
        screen.fill(white)
        pygame.draw.rect(screen, green, player)
        for obstacle in obstacles:
            pygame.draw.rect(screen, red, obstacle)
        draw_text("Puntuación: " + str(score), black, screen_width // 2, 50)
        
        # Actualizar la pantalla
        pygame.display.update()
    
    # Mostrar la puntuación
