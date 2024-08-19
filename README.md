import pygame
import sys

pygame.init()

GREEN = (34, 177, 76)
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)

SCREEN_WIDTH = 800
SCREEN_HEIGHT = 600
screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
pygame.display.set_caption("Juego de Fútbol")

player_width = 50
player_height = 50
player_x = SCREEN_WIDTH // 2 - player_width // 2
player_y = SCREEN_HEIGHT // 2 - player_height // 2
player_speed = 5

ball_radius = 15
ball_x = SCREEN_WIDTH // 2
ball_y = SCREEN_HEIGHT // 2
ball_speed_x = 3
ball_speed_y = 3

goal_width = 100
goal_height = 20
goal1_x = (SCREEN_WIDTH - goal_width) // 2
goal1_y = 0
goal2_x = (SCREEN_WIDTH - goal_width) // 2
goal2_y = SCREEN_HEIGHT - goal_height

clock = pygame.time.Clock()

while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()
    if keys[pygame.K_LEFT]:
        player_x -= player_speed
    if keys[pygame.K_RIGHT]:
        player_x += player_speed
    if keys[pygame.K_UP]:
        player_y -= player_speed
    if keys[pygame.K_DOWN]:
        player_y += player_speed

    player_x = max(0, min(SCREEN_WIDTH - player_width, player_x))
    player_y = max(0, min(SCREEN_HEIGHT - player_height, player_y))

    ball_x += ball_speed_x
    ball_y += ball_speed_y

    if ball_x - ball_radius < 0 or ball_x + ball_radius > SCREEN_WIDTH:
        ball_speed_x = -ball_speed_x
    if ball_y - ball_radius < 0 or ball_y + ball_radius > SCREEN_HEIGHT:
        ball_speed_y = -ball_speed_y

    if (player_x < ball_x < player_x + player_width and
        player_y < ball_y < player_y + player_height):
        ball_speed_x = -ball_speed_x
        ball_speed_y = -ball_speed_y

    screen.fill(GREEN)
    pygame.draw.rect(screen, WHITE, (goal1_x, goal1_y, goal_width, goal_height))
    pygame.draw.rect(screen, WHITE, (goal2_x, goal2_y, goal_width, goal_height))
    pygame.draw.line(screen, WHITE, (SCREEN_WIDTH // 2, 0), (SCREEN_WIDTH // 2, SCREEN_HEIGHT), 5)
    pygame.draw.circle(screen, WHITE, (SCREEN_WIDTH // 2, SCREEN_HEIGHT // 2), 70, 5)

    pygame.draw.rect(screen, BLACK, (player_x, player_y, player_width, player_height))
    pygame.draw.circle(screen, BLACK, (ball_x, ball_y), ball_radius)

    pygame.display.flip()
    clock.tick(60)
