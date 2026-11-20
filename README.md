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
These two functions make the while loop simpler and will make further development easier. Instead of working inside run_game(), we can do most of our work in the middle game_functions. Now that our code is restructured to make it easier to add to, we can work on the dynamic aspects of the game!
## Piloting the Ship
Let's give the player the ability to move the ship right and left. To do this, we'll write code that responds when the player the right or left arrow key.
## Responds to a Keypress
Whenever the player presses a key, that keypress is registered in Pygame as an event. Each event is picked up by the pygame.get() method, so we need to specify in our check events () function what kind of events to check for. Each keypress is registered as KEYDOWN event. When a KEYDOWN event is detected, we need to check whether the key that was pressed is one that triggers a certain event. For example, if the right arrow is key is pressed, we increase the ship's rect.centerx value to move the ship to the right:
```python
import sys
import pygame
def check_events(ship):
    # Respond to keypresses and mouse events,
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                # Move the ship to the right
                ship.rect.centerx += 10

def update_screen(ai_settings, screen, ship):
    # Update images on the screen and flip to the new screen.
    # Redraw the screen during each pass through the loop.
    screen.fill(ai_settings.bg_color)
    ship.blitme()

    # Make the most recently dreawn screen visible
    pygame.display.flip()
```
we need to update the call to check_events() in alien_ivasion.py so it passes ship as an argument:
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
        gf.check_events(ship)
        gf.update_screen(ai_settings, screen, ship)

run_game()
```
## Allowing continuous movement
When the player holds down the right arrow key, ship to continue moving right until the player releases the key.
Here are the changes to Ship class:
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

        # Movement flag
        self.moving_right = False

    def update(self):
        # Update the ship's position based on the movement flag.
        if self.moving_right:
            self.rect.centerx += 1

    def blitme(self):
        # Draw the ship at its current location.
        self.screen.blit(self.image, self.rect)
```
Now modify check_events() so that moving_right is set to True when the right arrow key is pressed and False is pressed and False when the key is realeased:
```python
import sys
import pygame
def check_events(ship):
    # Respond to keypresses and mouse events,
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                # Move the ship to the right
                ship.moving_right = True
            elif event.type == pygame.KEYUP:
                if event.key == pygame.K_RIGHT:
                    ship.moving_right = False

def update_screen(ai_settings, screen, ship):
    # Update images on the screen and flip to the new screen.
    # Redraw the screen during each pass through the loop.
    screen.fill(ai_settings.bg_color)
    ship.blitme()

    # Make the most recently dreawn screen visible
    pygame.display.flip()
```
Finally, we modify the whil loop in alien_invasion.py so it calls the ship's update() method on each pass through the loop:
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
        gf.check_events(ship)
        ship.update()
        gf.update_screen(ai_settings, screen, ship)

run_game()
```
When you run alien_invasion.py and hold down the right key, the ship should move oontinuously to the right until you release the key.
## Moving both left and right
Now we can move continuously to the right, we add movement to the left.
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

        # Movement flags
        self.moving_right = False
        self.moving_left = False

    def update(self):
        # Update the ship's position based on the movement flags.
        if self.moving_right:
            self.rect.centerx += 1
        if self.moving_left:
            self.rect_centerx -= 1

    def blitme(self):
        # Draw the ship at its current location.
        self.screen.blit(self.image, self.rect)
```
We use two separate if blocks rather than elif in update() to allow the ship's rect.centerx value.<br>We have to make two adjustments to check_events():
```python
import sys
import pygame
def check_events(ship):
    # Respond to keypresses and mouse events,
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            if event.key == pygame.K_RIGHT:
                # Move the ship to the right
                ship.moving_right = True
            elif event.key == pygame.K_LEFT:
                ship.moving_left = True
        elif event.type == pygame.KEYUP:
            if event.key == pygame.K_RIGHT:
                ship.moving_right = False
            elif event.key == pygame.K_LEFT:
                ship.moving_left = False

def update_screen(ai_settings, screen, ship):
    # Update images on the screen and flip to the new screen.
    # Redraw the screen during each pass through the loop.
    screen.fill(ai_settings.bg_color)
    ship.blitme()

    # Make the most recently dreawn screen visible
    pygame.display.flip()
```
If you run alien_invasion.py now, you should be able to move the ship continuouly to the right and left. If you hold down both keys, the ship should stop moving.
## Adjusting the ship's speed
Currently, the ship moves one pixel per cycle through the while loop, but we can take finer control of the ship's speed by adding a ship_speed_factor attribute to the Settings class. Here's the new attribute in settings.py
```python
class Settings():
    # A class to store all settings for Alien Invasion.
    def __init__ (self):
        # Initialize the game's settings.
        # Screen settings
        self.screen_width = 1200
        self.screen_height = 500
        self.bg_color = (230,230,230)

        # Ship settings
        self.ship_speed_factor = 1.5
```
adjust it's position by 1.5 pixels rather than 1 pixel. Decimal values for the speed setting to give us finer control of the ship's speed when we increase the tempo of the game later on.<br> So we need to make some modification to Ship:
```python
import pygame
class Ship():
    def __init__ (self,ai_settings, screen):
        # Initialize the ship and set its starting position.
        self.screen = screen
        self.ai_settings = ai_settings

        # Load the ship image, resize it, and get its rect.
        self.image = pygame.image.load("images/ship.bmp")
        self.image = pygame.transform.scale(self.image, (100, 85))

        self.rect = self.image.get_rect()
        self.screen_rect = screen.get_rect()

        # Start each new ship at the bottom center of the screen.
        self.rect.centerx = self.screen_rect.centerx
        self.rect.bottom = self.screen_rect.bottom

        # Store a decimal value for the ship's center
        self.center = float(self.rect.centerx)

        # Movement flags
        self.moving_right = False
        self.moving_left = False

    def update(self):
        # Update the ship's position based on the movement flags.
        # Update the ship's center value, not the rect.
        if self.moving_right and self.rect.right < self.screen_rect.right:
            self.center += self.ai_settings.ship_speed_factor 
        if self.moving_left and self.rect.left > 0:
            self.center -= self.ai_settings.ship_speed_factor 

        # Update rect object from self.center
        self.rect.centerx = self.center

    def blitme(self):
        # Draw the ship at its current location.
        self.screen.blit(self.image, self.rect)
```
We need to pass ai_settings as an argument when we create an instance of ship in alien_invasion.py:
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
    ship = Ship(ai_settings, screen)

    # start the main loop for the game.
    while True:
        gf.check_events(ship)
        ship.update()
        gf.update_screen(ai_settings, screen, ship)

run_game()
```
Now any value of ship_speed_factor greater than one will make the ship move faster. This will be helpful in making the ship respond quickly enough to shoot down aliens, and it will let us change the tempo of the game as the player progresses in gameplay:
## Limiting the Ship's Range
At this point the ship will disappear off either edge of the screen if you hold down an arrow key long enough. The ship stops moving when it reaches the edge of the screen. We do this by modifying the update() method in Ship:
```python
def update(self):
    # Update the ship's position based on movement flags.
    # Update the ship's center value, not the react.
    if self.moving_right and self.react.right < self.screen_rect.right:
        self.center += self.ai_settings.ship_speed_factor
    if self.moving_left and self.react.left > 0:
        self.center -= self.ai_settings.ship_speed_factor

    # Update rect object from self.center.
    self.rect.centerx = self.center
```
if you run alien_invasion.py now, the ship should stop moving at either edge of the screen.
## Refactoring check_events()
The check_events() function will increase in length as we continue to develop the game, so let's break check_events() into two more functions: one that handles KEYDOWN events and another that handles KEYUP events:
```python
def check_keydown_events(event, ship):
    # Respond to keypresses.
    if event.key == pygame.K_RIGHT:
        ship.moving_right = True
    elif event.key == pygame.K_LEFT:
        ship.moving_left = True

def check_keyup_events(event, ship):
    # Respond to key releases.
    if event.key == pygame.K_RIGHT:
        ship.moving_right = False
    elif event.key == pygame.K_LEFT:
        ship.moving_left = False

def check_events(ship):
    # Respond to keypresses and mouse events.
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            sys.exit()
        elif event.type == pygame.KEYDOWN:
            check_keydow_events(event, ship)
        elif event.type == pygame.KEYUP:
            check_keyup_events(event, ship)
```
We make two new functions: check_keydown_events() and check_key_up_events(). Each needs an event parameter and a ship parameter. The bodies of these two functions are copied from check_events(), and we've replaced the old code with calls to the new functions. The check_events() function is simpler now with this cleaner code structure, which will make it easier to develop further responses to player input.
# A Quick Recap
## alien_invasion.py
The main file, alien_invasion.py, creates a number of important objects used throughout the game: the settings are stored in ai_settings, the main display surface is stored in screen and ship instance is created in this file as well. alien_invasion.py is the only file you need to run when you want play alien invasion. The other files-settings.py, game_functions.py, ship.py - contain code that is imported, directly or indirectly, into this file.
## settings.py
The settings.py file contains the Settings class. This class only has an __init__ () method, which initializes attributes controlling the game's apperance and the ship's speed.
## game_functions.py
The game_functions.py file contains a number of functions that carry out the bulk of the work in the game. The check_events() functions that carry out the bulk of the work in the game. The check_events() function detects relevant events, such as keypresses and releases, and process each of these types of events through the helper functions check_keydown_events() and check_keyup_events(). For now, these functions manage the movement of the ship. The game_functions module also contains update_screen(), which redraws the screen on each pass through the main loop.
## ship.py
The ship.py file contains the Ship class. Ship has an __init__() method, an update() method to manage the ship's position and a blitme() method to draw the ship to the screen. The actual imagae of the ship is stored in ship.bmp, which is in the images folder.