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
Try to find a file with a transparent background that can replace with any background color using an image editor. Your games will llok best if the image's background color matches your game's background color. Alternatively, you can match your game's background to the image's background. Make a folder called images inside your main project folder. Save the file ship.bmp in the images folder.
## Creating the Ship Class
After choosing an image for the ship, we need to display it onscreen. To use our ship, we'll write a module called ship, which contains the class Ship. This class will manage most of the behaviour of the player's ship.
```python
import pygame
class Ship():
    def __init__ (self, screen):
        # Initialize the ship and set its starting position.
        self.screen = screen

        # Load the ship image, resize it, and get its rect.
        self.image = pygame.image.load("images/ship.bmp")
        self.image = pygame.transform.scale(self.image, (100, 85))

        self.rect = self.image.get_rect()
        self.screen_rect = screen.get_rect()

        # Start each new ship at the bottom center of the screen.
        self.rect.centerx = self.screen_rect.centerx
        self.rect.bottom = self.screen_rect.bottom

    def blitme(self):
        # Draw the ship at its current location.
        self.screen.blit(self.image, self.rect)
``` 
## Drawing the Ship to the Screen
```python
import sys
import pygame
from settings import Settings
from ship import Ship


def run_game():
    # initialize game and create a screen object.
    pygame.init()
    ai_settings = Settings()
    screen = pygame.display.set_mode((ai_settings.screen_width, ai_settings.screen_height))
    pygame.display.set_caption("Alien Invasion")

    # Make a ship
    ship = Ship(screen)

    # start the main loop for the game.
    while True:
        # Watch for keyboard and mouse events.
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                sys.exit()
        # Redraw the screen during each pass through the loop.
        screen.fill(ai_settings.bg_color)
        ship.blitme()
        
        # Make the most recently drawn screen visible.
        pygame.display.flip()

run_game()
```
## Refactoring: the game_functions Module
In larger projects, you'll often refactor code you've written before adding more code. Refactoring simplifies the structure of the code you've already written, making it easier to build on. The game_functions module will prevent alien_invasion.py easier to follow.
## The chek_events() Function
we'll start by moving the code that manages events to a separate function called check_events(). This will simplify run_game() and isolate the event management loop. Isolating the event loop allows you to manage events separately from other aspects of the game, like updating the screen.
```python
import sys
import pygame
def check_events():
    # Respond to keypresses and mouse events,
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
```
This module import sys and pygame, which are used in the event checking loop. Now let's modify alien_invasion.py so it imports the game_function module, and we'll replace the event loop with a call_events();
```python
import pygame
from settings import Settings
from ship import Ship
import game_functions as gf

def run_game():
    # initialize game and create a screen object.
    pygame.init()
    ai_settings = Settings()
    screen = pygame.display.set_mode((ai_settings.screen_width, ai_settings.screen_height))
    pygame.display.set_caption("Alien Invasion")

    # Make a ship
    ship = Ship(screen)

    # start the main loop for the game.
    while True:
        gf.check_events()

        # Redraw the screen during each pass through the loop.
        screen.fill(ai_settings.bg_color)
        ship.blitme()

        # Make the most recently drawn screen visible.
        pygame.display.flip()

run_game()
```
We no longer need to import sys directly into the main program file, because it's only being used in the game_functions module now.
## The update_screen() Function
Let's move the code for updating the screen to a separate function called update_screen() in game_functions.py to further simplify run_game():
```python
import sys
import pygame
def check_events():
    # Respond to keypresses and mouse events,
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()

def update_screen(ai_settings, screen, ship):
    # Update images on the screen and flip to the new screen.
    # Redraw the screen during each pass through the loop.
    screen.fill(ai_settings.bg_color)
    ship.blitme()

    # Make the most recently dreawn screen visible
    pygame.display.flip()
```
The new update_screen() function takes three parameters: ai_settings, screen and ship. Now we need to update the while loop from alien_invasion.py with call to update_screen():
```python
import pygame
from settings import Settings
from ship import Ship
import game_functions as gf

def run_game():
    # initialize game and create a screen object.
    pygame.init()
    ai_settings = Settings()
    screen = pygame.display.set_mode((ai_settings.screen_width, ai_settings.screen_height))
    pygame.display.set_caption("Alien Invasion")

    # Make a ship
    ship = Ship(screen)

    # start the main loop for the game.
    while True:
        gf.check_events()
        gf.update_screen(ai_settings, screen, ship)

run_game()
```
These two functions make the while loop simpler and will make further development easier. Instead of working inside run_game(), we can do most of our work in the middle game_functions.