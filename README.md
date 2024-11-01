
# Ping Pong Game

import pygame
import sys

# Initialize Pygame
pygame.init()

# Constants
WIDTH, HEIGHT = 800, 600
WHITE = (255, 255, 255)
BLACK = (0, 0, 0)
BALL_SPEED_X, BALL_SPEED_Y = 7, 7
PADDLE_SPEED = 10

# Set up the display
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pygame.display.set_caption("Ping Pong Game")

# Create the ball
ball = pygame.Rect(WIDTH // 2 - 15, HEIGHT // 2 - 15, 30, 30)

# Create paddles
left_paddle = pygame.Rect(30, HEIGHT // 2 - 70, 10, 140)
right_paddle = pygame.Rect(WIDTH - 40, HEIGHT // 2 - 70, 10, 140)

# Ball movement
ball_speed_x = BALL_SPEED_X
ball_speed_y = BALL_SPEED_Y

# Game loop
while True:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    keys = pygame.key.get_pressed()
    if keys[pygame.K_w] and left_paddle.top > 0:
        left_paddle.y -= PADDLE_SPEED
    if keys[pygame.K_s] and left_paddle.bottom < HEIGHT:
        left_paddle.y += PADDLE_SPEED
    if keys[pygame.K_UP] and right_paddle.top > 0:
        right_paddle.y -= PADDLE_SPEED
    if keys[pygame.K_DOWN] and right_paddle.bottom < HEIGHT:
        right_paddle.y += PADDLE_SPEED

    # Move the ball
    ball.x += ball_speed_x
    ball.y += ball_speed_y

    # Ball collision with top and bottom
    if ball.top <= 0 or ball.bottom >= HEIGHT:
        ball_speed_y = -ball_speed_y

    # Ball collision with paddles
    if ball.colliderect(left_paddle) or ball.colliderect(right_paddle):
        ball_speed_x = -ball_speed_x

    # Reset ball if it goes out of bounds
    if ball.left <= 0 or ball.right >= WIDTH:
        ball.x = WIDTH // 2 - 15
        ball.y = HEIGHT // 2 - 15
        ball_speed_x = BALL_SPEED_X * (-1 if ball.left <= 0 else 1)
        ball_speed_y = BALL_SPEED_Y

    # Fill the screen with black
    screen.fill(BLACK)

    # Draw paddles and ball
    pygame.draw.rect(screen, WHITE, left_paddle)
    pygame.draw.rect(screen, WHITE, right_paddle)
    pygame.draw.ellipse(screen, WHITE, ball)

    # Update the display
    pygame.display.flip()
    pygame.time.Clock().tick(60)
