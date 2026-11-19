# **ALIEN INVASION**
# A SHIP THAT FIRES BULLETS
# Planning your project
# Description of the overall gameplay
In Aliens Invasion, the player controls a ship that appears at the bottom center of the screen. The player can move the ship right and left using the arrow keys and shoot bullets using the spacebar. When the game begins, a fleet of aliens fills the sky and moves across and down the screen. The player shoots and destroys the aliens. If the player shoots all the aliens, a new fleet appears that moves faster than the previous fleet.If any alien hits the player's ship or reaches the bottom of the screen, the player loses a ship. If the player loses three ships, the game ends.
For the first phase of development, we'll make a ship that can move right and left. The ship should be able to fire bullets when the player presses the spacebar. After setting up this behaviour, we can turn our attention to the aliens and refine the gameplay.

## Creating a Pygame window and responding to User Input
First, we'll create an empty Pygame window.

```python
import sys
import pygame

def run_game():# function starts
    # initialize game and create a screen object.
    pygame.init() # initializes background settings that pygame needs to work properly.
    screen = pygame.display.set_mode((1200, 800)) """This argument is a tuple that defines the dimensions of the game window, by passing these dimensions to pygame.display.set_mode(),
    we create a game window 1200 pixels wide by 800 pixels high."""
    pygame.display.set_caption("Alien Invasion")

    # start the main loop for the game.
    while True:
        # Watch for keyboard and mouse events.
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
        # Make the most recently drawn screen visible.
        pygame.display.flip()

run_game()
```
## Setting the Background color
Pygame creates a black screen by default, but that's boring. Let's set a different background color:
```python
import sys
import pygame

def run_game():
    # initialize game and create a screen object.
    pygame.init()
    screen = pygame.display.set_mode((1200, 800))
    pygame.display.set_caption("Alien Invasion")

    # Set the background color.
    bg_color = (230, 230, 230)

    # start the main loop for the game.
    while True:
        # Watch for keyboard and mouse events.
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
        # Redraw the screen during each pass through the loop.
        screen.fill(bg_color)
        # Make the most recently drawn screen visible.
        pygame.display.flip()

run_game()
```
## Creating a Setting Class
Each time we introduce new functionality into our game, we'll typically introduce some new settings as well. Instead of adding settings throughout the code, let's write a module called settings that contains a class called Settings to store all the settings in one place.<br>This approach allows us to pass around one settings object instead of many individual settings.<br>In addtion, it makes our function calls simpler and makes it easier to modify the game's appearance as our game's appearance as our project grows.To modify the game, we'll simply change some values in settings.py instead of searching for different settings throughout our files.
```python
import sys
import pygame
from settings import Settings

def run_game():
    # initialize game and create a screen object.
    pygame.init()
    ai_settings = Settings()
    screen = pygame.display.set_mode((ai_settings.screen_width, ai_settings.screen_height))
    pygame.display.set_caption("Alien Invasion")

    # start the main loop for the game.
    while True:
        # Watch for keyboard and mouse events.
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
        # Redraw the screen during each pass through the loop.
        screen.fill(ai_settings.bg_color)
        # Make the most recently drawn screen visible.
        pygame.display.flip()

run_game()
```
## Adding the ship image
Try to find a file with a transparent