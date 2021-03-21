from battleship_classes import *  # imports the classes from the other file (battleship_classes.py)
import pygame
from pygame import mixer  # for music
from random import randint  # for random generators
import os

pygame.init()  # initializes pygame
pygame.mixer.init()  # initializes the mixer
pygame.font.init()

# Screen Dimensions
HEIGHT = 600
WIDTH = 1000
GRIDSIZE = HEIGHT // 15
screen = pygame.display.set_mode((WIDTH, HEIGHT))

# Misc
inPlay = True  # boolean to keep game running
ship_placement = True  # boolean for step 1 - choosing ship placement
player_select = False  # boolean for step 2 - choosing which space to attack
computer_select = False  # boolean for step 3 - computer chooses where to attack
player_win = False  # boolean for step 4v1 - game is finished after player wins the game
enemy_win = False  # boolean for step 4v2 - game is finished after the enemy wins the game
yellowX = 50  # x coordinate for the highlighted square when selecting which space to target
yellowY = 20  # y coordinate for the highlighted square when selecting which space to target
guess = False  # so that the box is draw for the player to guess
guesses = 1  # to stop the computer from guessing an infinite amount of times
finished = False  # so that the code runs until it guesses a space not guessed yet

# Computer guessing modes
hunt = True  # hunting mode: guessing random areas
target = False  # target mode: guessing targeted areas based on the previous guess
north = True  # for guessing the space to the north of the previous guess
east = False  # for guessing the space to the east of the previous guess
south = False  # for guessing the space to the south of the previous guess
west = False  # for guessing the space to the west of the previous guess
savedX = 0  # saved x coordinate
savedY = 0  # saved y coordinate

# Music variables
music = True  # so that music can play when you come back to main menu
miss = mixer.Sound('Minecraft Sound Effects Walking and Running on Wood Sound Effect-[AudioTrimmer.com](3).mp3')
hit = mixer.Sound('Creeper Explosion - Sound Effect-[AudioTrimmer.com].mp3')

# How many of each enemy ship are left
player_lives = 17
ship6_lives = 2
ship7_lives = 3
ship8_lives = 3
ship9_lives = 4
ship10_lives = 5
ships_sunk = 0

# Buttons
pos = pygame.mouse.get_pos()  # position of the mouse
opacity1 = 0  # colour of button
opacity2 = 0
opacity3 = 0
one_white = False  # for text
one_black = True
two_white = False
two_black = True
three_white = False
three_black = True

# Declaring Images
image1 = pygame.image.load('minecraft_bg.png')
image2 = pygame.image.load("overworld_bg.png")
image3 = pygame.image.load("lava_bg.png")
image4 = pygame.image.load("text.png")
image5 = pygame.image.load("battleship_menu.png")
image6 = pygame.image.load("battleship_help_menu.png")

# Rendering Images
image1 = pygame.transform.scale(image1, (WIDTH, HEIGHT))
image2 = pygame.transform.scale(image2, (400, 400))
image3 = pygame.transform.scale(image3, (400, 400))
image4 = pygame.transform.scale(image4, (900, 100))
image5 = pygame.transform.scale(image5, (WIDTH, HEIGHT))
image6 = pygame.transform.scale(image6, (WIDTH, HEIGHT))

# Texts
word = pygame.font.Font(os.getcwd() + "/MachineStd.otf", 40)
text1 = word.render("Place your ships on the game board", True, BLACK)
text2 = word.render("Select a space on the enemy board to attack", True, BLACK)
text3 = word.render("Congratulations! You sunk every enemy battleship!", True, BLACK)
text4 = word.render("Creeper aw man, all of your battleships were sunk!", True, BLACK)
text5 = word.render("Play Game", True, BLACK)
text6 = word.render("Play Game", True, WHITE)
text7 = word.render("Help Menu", True, BLACK)
text8 = word.render("Help Menu", True, WHITE)
text9 = word.render("Back", True, BLACK)
text10 = word.render("Back", True, WHITE)


def draw_enemy_grid(lineY=GRIDSIZE * 0.5, lineX=GRIDSIZE * 1.25):  # drawing the enemy's grid
    for i in range(11):
        pygame.draw.line(screen, BLACK, (GRIDSIZE * 1.25, lineY), (WIDTH - (GRIDSIZE * 13.75), lineY), 3)
        lineY += GRIDSIZE
    for j in range(11):
        pygame.draw.line(screen, BLACK, (lineX, GRIDSIZE * 0.5), (lineX, HEIGHT - (GRIDSIZE * 4.5)), 3)
        lineX += GRIDSIZE


def draw_player_grid(lineY=GRIDSIZE * 0.5, lineX=GRIDSIZE * 13.75):  # drawing the player's grid
    for i in range(11):
        pygame.draw.line(screen, BLACK, (GRIDSIZE * 13.75, lineY), (WIDTH - (GRIDSIZE * 1.25), lineY), 3)
        lineY += GRIDSIZE
    for j in range(11):
        pygame.draw.line(screen, BLACK, (lineX, GRIDSIZE * 0.5), (lineX, HEIGHT - (GRIDSIZE * 4.5)), 3)
        lineX += GRIDSIZE


def check_for_ships():

    global ship6_lives, ship7_lives, ship8_lives, ship9_lives, ship10_lives, ships_sunk

    locationX = box.return_x()  # returns the x coord
    locationY = box.return_y()  # returns the y coord

    x = int(locationX - 1.25)  # subtracts by 1.25 to get an index
    y = int(locationY - 0.5)  # subtracts by 0.5 to get an index

    if enemy_ships[y][x] == 0:  # there is no ship in this square
        miss.play()  # miss sound effect
        black_square = Shape(locationX, locationY, 7)
        black_square.draw(screen, 40)  # drawing the square in the location
        board.append(black_square)  # placing the box on the grid
    elif enemy_ships[y][x] == 1:  # there is a 2-piece ship in this square
        hit.play()  # hit sound effect
        green_square = Shape(locationX, locationY, 8)
        green_square.draw(screen, 40)
        board.append(green_square)
        enemy_ships[y].pop(x)  # get rid of the 1
        enemy_ships[y].insert(x, -1)   # inserts a -1 to show that it has been guessed now
        if ship6_lives != 0:  # so that it stops once the ship is sunk
            ship6_lives -= 1
        if ship6_lives == 0:
            enemy1.draw(screen, 40)  # reveal the sunken ship on the board
            board.append(enemy1)
            ships_sunk += 1
    elif enemy_ships[y][x] == 2:  # there is a 3-piece ship in this square
        hit.play()  # hit sound effect
        green_square = Shape(locationX, locationY, 8)
        green_square.draw(screen, 40)
        board.append(green_square)
        enemy_ships[y].pop(x)  # get rid of the 2
        enemy_ships[y].insert(x, -1)   # inserts a -1 to show that it has been guessed now
        if ship7_lives != 0:  # so that it stops once the ship is sunk
            ship7_lives -= 1
        if ship7_lives == 0:
            enemy2.draw(screen, 40)  # reveal the sunken ship on the board
            board.append(enemy2)
            ships_sunk += 1
    elif enemy_ships[y][x] == 3:  # there is a 3-piece ship in this square
        hit.play()  # hit sound effect
        green_square = Shape(locationX, locationY, 8)
        green_square.draw(screen, 40)
        board.append(green_square)
        enemy_ships[y].pop(x)  # get rid of the 3
        enemy_ships[y].insert(x, -1)   # inserts a -1 to show that it has been guessed now
        if ship8_lives != 0:  # so that it stops once the ship is sunk
            ship8_lives -= 1
        if ship8_lives == 0:
            enemy3.draw(screen, 40)  # reveal the sunken ship on the board
            board.append(enemy3)
            ships_sunk += 1
    elif enemy_ships[y][x] == 4:  # there is a 4-piece ship in this square
        hit.play()  # hit sound effect
        green_square = Shape(locationX, locationY, 8)
        green_square.draw(screen, 40)
        board.append(green_square)
        enemy_ships[y].pop(x)  # get rid of the 4
        enemy_ships[y].insert(x, -1)   # inserts a -1 to show that it has been guessed now
        if ship9_lives != 0:  # so that it stops once the ship is sunk
            ship9_lives -= 1
        if ship9_lives == 0:
            enemy4.draw(screen, 40)  # reveal the sunken ship on the board
            board.append(enemy4)
            ships_sunk += 1
    elif enemy_ships[y][x] == 5:  # there is a 5-piece ship in this square
        hit.play()  # hit sound effect
        green_square = Shape(locationX, locationY, 8)
        green_square.draw(screen, 40)
        board.append(green_square)
        enemy_ships[y].pop(x)  # get rid of the 5
        enemy_ships[y].insert(x, -1)   # inserts a -1 to show that it has been guessed now
        if ship10_lives != 0:  # so that it stops once the ship is sunk
            ship10_lives -= 1
        if ship10_lives == 0:
            enemy5.draw(screen, 40)  # reveal the sunken ship on the board
            board.append(enemy5)
            ships_sunk += 1
    elif enemy_ships[y][x] == -1:  # there is a 5-piece ship in this square
        pass  # they waste a turn for guessing a space already correctly guesses


def computer_guess():

    global finished, player_lives, enemy_win, target, hunt, north, east, south, west, savedX, savedY
    finished = False

    if hunt:  # Hunting Mode: randomly guessing spaces

        while not finished:  # to make sure it generates a location that was not yet picked

            x = randint(0, 9)  # randomize x coordinate
            y = randint(0, 9)  # randomize y coordinate

            if player_ships[y][x] == 0:  # there is no ship in this square
                miss.play()  # miss sound effect
                black_block = Shape(x + 13.75, y + 0.5, 7)
                black_block.draw(screen, 40)
                board.append(black_block)  # place the box on the game field
                player_ships[y].pop(x)  # get rid of the 0
                player_ships[y].insert(x, -1)   # inserts a -1 to show that it has been guessed now
                finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
            elif player_ships[y][x] == 1:  # there is a ship in this square
                hit.play()  # hit sound effect
                green_block = Shape(x + 13.75, y + 0.5, 8)
                green_block.draw(screen, 40)
                board.append(green_block)  # place the box on the game field
                player_ships[y].pop(x)  # gets rid of the 1
                player_ships[y].insert(x, -1)  # inserts a -1 to show that it has been guessed now
                finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
                target = True  # computer will now make targeted guesses
                hunt = False  # the computer is no longer in hunting mode
                savedX = x  # save the current x coordinate for the target mode
                savedY = y  # save the current y coordinate for the target mode
                player_lives -= 1  # player loses a life
            elif player_ships[y][x] == -1:
                pass  # pass so it can generate a new guess that has not been guessed already

    elif target:  # Target Mode: guessing spaces above/below and to the left/right of the targeted space

        if not finished:  # to make sure it does not choose a space already picked

            if north and not finished:  # if it is instructed to guess to the north and no space has been chosen yet
                savedY -= 1  # pick the space above
                if savedY >= 0:  # makes sure that the computer does not pick the space out of index (off the board)
                    if player_ships[savedY][savedX] == 0:  # there is no ship in this square
                        miss.play()  # miss sound effect
                        black_block = Shape(savedX + 13.75, savedY + 0.5, 7)
                        black_block.draw(screen, 40)
                        board.append(black_block)  # place the box on the game field
                        player_ships[savedY].pop(savedX)  # get rid of the 0
                        player_ships[savedY].insert(savedX, -1)   # inserts a -1 to show that it has been guessed now
                        north = False  # north mode is off
                        east = True  # now will guess to the east
                        finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
                    elif player_ships[savedY][savedX] == 1:  # there is a ship in this square
                        hit.play()  # hit sound effect
                        green_block = Shape(savedX + 13.75, savedY + 0.5, 8)
                        green_block.draw(screen, 40)
                        board.append(green_block)  # place the box on the game field
                        player_ships[savedY].pop(savedX)  # gets rid of the 1
                        player_ships[savedY].insert(savedX, -1)  # inserts a -1 to show that it has been guessed now
                        player_lives -= 1  # player loses a life
                        finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
                    elif player_ships[savedY][savedX] == -1:
                        north = False  # north mode is off
                        east = True  # now will guess to the east
                else:
                    north = False  # north mode is off
                    east = True  # now will guess to the east
            if east and not finished:  # if it is instructed to guess to the east and no space has been chosen yet
                savedY += 1  # resets y coordinate to the targeted spot
                savedX += 1  # pick the space to the right
                if savedX <= 9:  # makes sure that the computer does not pick the space out of index (off the board)
                    if player_ships[savedY][savedX] == 0:  # there is no ship in this square
                        miss.play()  # miss sound effect
                        black_block = Shape(savedX + 13.75, savedY + 0.5, 7)
                        black_block.draw(screen, 40)
                        board.append(black_block)  # place the box on the game field
                        player_ships[savedY].pop(savedX)  # get rid of the 0
                        player_ships[savedY].insert(savedX, -1)   # inserts a -1 to show that it has been guessed now
                        east = False  # east mode is off
                        south = True  # now will guess to the south
                        finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
                    elif player_ships[savedY][savedX] == 1:  # there is a ship in this square
                        hit.play()  # hit sound effect
                        green_block = Shape(savedX + 13.75, savedY + 0.5, 8)
                        green_block.draw(screen, 40)
                        board.append(green_block)  # place the box on the game field
                        player_ships[savedY].pop(savedX)  # gets rid of the 1
                        player_ships[savedY].insert(savedX, -1)  # inserts a -1 to show that it has been guessed now
                        player_lives -= 1  # player loses a life
                        east = False  # east mode is off
                        north = True  # since it found a new space, it will target that new space and will restart
                        finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
                    elif player_ships[savedY][savedX] == -1:
                        east = False  # east mode is off
                        south = True  # now will guess to the south
                else:
                    east = False  # east mode is off
                    south = True  # now will guess to the south
            if south and not finished:  # if it is instructed to guess to the south and no space has been chosen yet
                savedY += 1  # pick the space below
                savedX -= 1  # resets the x coordinate to the targeted space
                if savedY <= 9:  # makes sure that the computer does not pick the space out of index (off the board)
                    if player_ships[savedY][savedX] == 0:  # there is no ship in this square
                        miss.play()  # miss sound effect
                        black_block = Shape(savedX + 13.75, savedY + 0.5, 7)
                        black_block.draw(screen, 40)
                        board.append(black_block)  # place the box on the game field
                        player_ships[savedY].pop(savedX)  # get rid of the 0
                        player_ships[savedY].insert(savedX, -1)   # inserts a -1 to show that it has been guessed now
                        south = False  # south mode is off
                        west = True  # now will guess to the west
                        finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
                    elif player_ships[savedY][savedX] == 1:  # there is a ship in this square
                        hit.play()  # hit sound effect
                        green_block = Shape(savedX + 13.75, savedY + 0.5, 8)
                        green_block.draw(screen, 40)
                        board.append(green_block)  # place the box on the game field
                        player_ships[savedY].pop(savedX)  # gets rid of the 1
                        player_ships[savedY].insert(savedX, -1)  # inserts a -1 to show that it has been guessed now
                        player_lives -= 1  # player loses a life
                        south = False  # south mode is off
                        north = True  # since it found a new space, it will target that new space and will restart
                        finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
                    elif player_ships[savedY][savedX] == -1:
                        south = False  # south mode is off
                        west = True  # now will guess to the west
                else:
                    south = False  # south mode is off
                    west = True  # now will guess to the west
            if west and not finished:  # if it is instructed to guess to the west and no space has been chosen yet
                savedY -= 1  # resets the y coordinate to the targeted space
                savedX -= 1  # pick the space to the left
                if savedX >= 0:  # makes sure that the computer does not pick the space out of index (off the board)
                    if player_ships[savedY][savedX] == 0:  # there is no ship in this square
                        miss.play()  # miss sound effect
                        black_block = Shape(savedX + 13.75, savedY + 0.5, 7)
                        black_block.draw(screen, 40)
                        board.append(black_block)  # place the box on the game field
                        player_ships[savedY].pop(savedX)  # get rid of the 0
                        player_ships[savedY].insert(savedX, -1)   # inserts a -1 to show that it has been guessed now
                        west = False  # west mode is off
                        hunt = True  # computer can go back to hunting
                        north = True  # now will guess to the north
                        finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
                    elif player_ships[savedY][savedX] == 1:  # there is a ship in this square
                        hit.play()  # hit sound effect
                        green_block = Shape(savedX + 13.75, savedY + 0.5, 8)
                        green_block.draw(screen, 40)
                        board.append(green_block)  # place the box on the game field
                        player_ships[savedY].pop(savedX)  # gets rid of the 1
                        player_ships[savedY].insert(savedX, -1)  # inserts a -1 to show that it has been guessed now
                        player_lives -= 1  # player loses a life
                        west = False  # west mode is off
                        north = True  # since it found a new space, it will target that new space and will restart
                        finished = True  # the computer has selected a spot that was not yet guessed, stop guessing
                    elif player_ships[savedY][savedX] == -1:
                        west = False  # west mode is off
                        hunt = True  # computer can go back to hunting
                        north = True  # north is true for the next time that the computer is in target mode
                else:
                    west = False  # west mode is off
                    hunt = True  # computer can go back to hunting
                    north = True  # north is true for the next time that the computer is in target mode


def main_menu():

    global music, e, opacity1, opacity2, opacity3, one_black, one_white, two_black, two_white

    if music:
        mixer.music.load('Minecraft Music - Menu 1.mp3')  # Among Us main menu theme
        pygame.mixer.music.set_volume(5)
        mixer.music.play(-1)  # -1 is put so it can play on loop
        music = False  # so that it does not start over and over again

    while True:

        screen.fill(WHITE)
        screen.blit(image5, (0, 0))  # background image

        for e in pygame.event.get():
            if e.type == pygame.QUIT:  # the quit button on the pop-out screen
                pygame.quit()  # pygame ends
            if e.type == pygame.MOUSEMOTION:
                if b1.activate(pygame.mouse.get_pos()):  # if mouse is over button 2
                    opacity1 = 250
                    one_black = False
                    one_white = True
                else:
                    opacity1 = 0
                    one_white = False
                    one_black = True
                if b2.activate(pygame.mouse.get_pos()):  # if mouse is over button 3
                    opacity2 = 250
                    two_black = False
                    two_white = True
                else:
                    opacity2 = 0
                    two_white = False
                    two_black = True
            if e.type == pygame.MOUSEBUTTONDOWN:
                if b1.activate(pygame.mouse.get_pos()):  # if mouse is clicked while hovering over button 2
                    mixer.music.pause()
                    main_game()  # go to game
                if b2.activate(pygame.mouse.get_pos()):  # if mouse is clicked while hovering over button 3
                    help_menu()  # go to help menu

        b1.create_button()  # creates the button
        b2.create_button()
        b1.fade(opacity1)  # changes colour of button
        b2.fade(opacity2)

        if one_black:  # text changes colour based on whether mouse is over the button or not
            screen.blit(text5, (110, 375))
        if one_white:
            screen.blit(text6, (110, 375))
        if two_black:
            screen.blit(text7, (110, 495))
        if two_white:
            screen.blit(text8, (110, 495))

        pygame.display.update()


def help_menu():

    while True:

        global opacity3, three_white, three_black, e

        screen.fill(WHITE)
        screen.blit(image6, (0, 0))  # help menu background

        for e in pygame.event.get():
            if e.type == pygame.QUIT:  # the quit button on the pop-out screen
                pygame.quit()  # pygame ends
            if e.type == pygame.MOUSEMOTION:
                if b3.activate(pygame.mouse.get_pos()):  # if mouse is over button 4
                    opacity3 = 250
                    three_black = False
                    three_white = True
                else:
                    opacity3 = 0
                    three_white = False
                    three_black = True
            if e.type == pygame.MOUSEBUTTONDOWN:
                if b3.activate(pygame.mouse.get_pos()):  # if mouse is clicked while hovering over button 4
                    main_menu()

        b3.create_button()  # creates the button
        b3.fade(opacity3)

        if three_black:  # text changes colour based on whether mouse is over the button or not
            screen.blit(text9, (70, 525))
        if three_white:
            screen.blit(text10, (70, 525))

        pygame.display.update()


b1 = Button(BLACK, 0, 3, 60, 340, 250, 100)  # play button
b2 = Button(BLACK, 0, 3, 60, 460, 250, 100)  # help menu button
b3 = Button(BLACK, 0, 3, 35, 500, 150, 75)  # back button

shipNo = 1
ship = Shape(16.75, 4.5, shipNo)  # creating the ship

# creating enemy ships
if pattern == 1:
    enemy1 = Enemy(3.25, 2.5, 1)  # zombie piglin
    enemy2 = Enemy(8.25, 2.5, 2)  # wither skeleton
    enemy3 = Enemy(1.25, 1.5, 3)  # magma cube
    enemy4 = Enemy(6.25, 6.5, 4)  # blaze
    enemy5 = Enemy(3.25, 8.5, 5)  # ghast
if pattern == 2:
    enemy1 = Enemy(5.25, 6.5, 1)  # zombie piglin
    enemy2 = Enemy(1.25, 7.5, 2)  # wither skeleton
    enemy3 = Enemy(8.25, 8.5, 3)  # magma cube
    enemy4 = Enemy(3.25, 4.5, 4)  # blaze
    enemy5 = Enemy(5.25, 1.5, 5)  # ghast
if pattern == 3:
    enemy1 = Enemy(7.25, 8.5, 1)  # zombie piglin
    enemy2 = Enemy(7.25, 3.5, 2)  # wither skeleton
    enemy3 = Enemy(2.25, 6.5, 3)  # magma cube
    enemy4 = Enemy(6.25, 6.5, 4)  # blaze
    enemy5 = Enemy(2.25, 1.5, 5)  # ghast
if pattern == 4:
    enemy1 = Enemy(9.25, 8.5, 1)  # zombie piglin
    enemy2 = Enemy(5.25, 1.5, 2)  # wither skeleton
    enemy3 = Enemy(2.25, 2.5, 3)  # magma cube
    enemy4 = Enemy(2.25, 6.5, 4)  # blaze
    enemy5 = Enemy(6.25, 4.5, 5)  # ghast
if pattern == 5:
    enemy1 = Enemy(6.25, 8.5, 1)  # zombie piglin
    enemy2 = Enemy(3.25, 3.5, 2)  # wither skeleton
    enemy3 = Enemy(8.25, 3.5, 3)  # magma cube
    enemy4 = Enemy(5.25, 1.5, 4)  # blaze
    enemy5 = Enemy(2.25, 9.5, 5)  # ghast

floor = Floor(13.75, 10.5, 10)  # floor in game
roof = Floor(13.75, -0.5, 10)  # roof in game
leftWall = Wall(12.75, 0.5, 10)  # left wall in game
rightWall = Wall(23.75, 0.5, 10)  # right wall in game

board = Board(13.75, 0.5)  # So that the ships will stick to the board
box = Shape(1.25, 0.5, 6)  # the selection box


def main_game():

    mixer.music.load('RevengeA Minecraft Parody Creeper Aw Man (Music Video).mp3')
    pygame.mixer.music.set_volume(0.3)  # changing volume so sound effects can be properly heard
    mixer.music.play(-1)

    global inPlay, ship_placement, ship, shipNo, player_select, computer_select, guess, player_win, enemy_win, guesses

    while inPlay:

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                inPlay = False  # stops running if quit button is pressed
            if ship_placement:
                if event.type == pygame.KEYDOWN:  # checks if key is pressed
                    if event.key == pygame.K_LEFT:  # checks left key
                        ship.move_left()
                        if ship.collides(leftWall):
                            ship.move_right()
                    if event.key == pygame.K_RIGHT:  # checks right key
                        ship.move_right()
                        if ship.collides(rightWall):
                            ship.move_left()
                    if event.key == pygame.K_UP:  # checks up key
                        ship.move_up()
                        if ship.collides(roof):
                            ship.move_down()
                    if event.key == pygame.K_DOWN:  # checks up key
                        ship.move_down()
                        if ship.collides(floor):
                            ship.move_up()
                    if event.key == pygame.K_LSHIFT:  # checks space key
                        ship.rot = (ship.rot + 1) % 4  # Four possible values for rotation - 0, 1, 2, and 3
                        ship.rotate()
                        if ship.collides(leftWall):  # checks collision with left wall
                            ship.move_right()
                            ship.rotate()
                        if ship.collides(rightWall):  # checks collision with right wall
                            ship.move_left()
                            if ship.num >= 4:  # so it moves an extra time to stay in the area
                                ship.move_left()
                            if ship.num == 5:  # so it moves 2 extra times to stay in the area
                                ship.move_left()
                            ship.rotate()
                        if ship.collides(floor):  # checks collision with the floor
                            ship.move_up()
                            if ship.num >= 4:  # so it moves an extra time to stay in the area
                                ship.move_up()
                            ship.rotate()
                        if ship.collides(roof):  # checks collision with the floor
                            ship.move_down()
                            if ship.num == 5:  # so it moves an extra time to stay in the area
                                ship.move_down()
                            ship.rotate()
                    if event.key == pygame.K_SPACE and not ship.collides(board):
                        board.append(ship)  # place the ship on the grid
                        ship.record_location()  # recording where the ship is and adding it to the double list
                        shipNo += 1
                        if shipNo != 6:
                            ship = Shape(16.75, 4.5, shipNo)
                        else:  # once all ships have been placed
                            ship_placement = False  # ships will no longer be placed
                            player_select = True  # player can now select a space to attack
            if player_select:  # checking if it's time for the player to select a space
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_LEFT and box.col > 1.3:  # making sure box is on the grid
                        box.move_left()
                    if event.key == pygame.K_RIGHT and box.col < 9.75:
                        box.move_right()
                    if event.key == pygame.K_UP and box.row > 0.5:
                        box.move_up()
                    if event.key == pygame.K_DOWN and box.row < 9.5:
                        box.move_down()
                    if event.key == pygame.K_RETURN:
                        guess = True  # allows the player to guess
                        guesses = 1  # to reset guesses to allow computer to guess

        screen.fill(WHITE)  # base background

        enemy1.draw(screen, 40)  # drawing the enemy ships behind the lava
        enemy2.draw(screen, 40)
        enemy3.draw(screen, 40)
        enemy4.draw(screen, 40)
        enemy5.draw(screen, 40)

        screen.blit(image1, (0, 0))  # background image
        screen.blit(image2, (550, 20))
        screen.blit(image3, (50, 20))
        screen.blit(image4, (50, 465))

        if ship_placement:  # if you are at the stage when you are placing your ships
            screen.blit(text1, (200, 500))  # instructional text for placing your ship

        draw_enemy_grid()  # drawing the enemy's grid
        draw_player_grid()  # drawing the player's grid

        if shipNo != 6:  # as long as the ship is the first 5
            ship.draw(screen, 40)  # the player's ship is drawn

        if ships_sunk == 5:  # if all the enemy's ships are sunk
            player_win = True  # the player now wins
            guess = False  # shutting off all other steps
            player_select = False  # shutting off all other steps
            computer_select = False  # shutting off all other steps

        if player_lives == 0:  # if the player runs out of lives
            enemy_win = True  # the enemy now wins
            guess = False  # shutting off all other steps
            player_select = False  # shutting off all other steps
            computer_select = False  # shutting off all other steps

        if guess:  # if player is permitted to guess

            check_for_ships()  # code for player guessing a space
            player_select = False  # turning it off so the yellow highlight box stops being draw
            computer_select = True  # computer's turn starts
            guess = False  # written to stop this code from running more than once

        if computer_select and guesses == 1:  # when it's the computer's turn
            player_select = True  # so that the player can select after the computer's turn
            computer_guess()  # code for the computer guessing
            guesses -= 1  # method to reduce it only to one guess

        if player_win:  # if the player has won
            screen.blit(text3, (110, 500))

        if enemy_win:  # if the enemy has won
            screen.blit(text4, (105, 500))

        board.draw(screen, 40)  # drawing the object to append the ships and boxes to

        if not ship_placement and player_select:  # once you're done placing your ships and if you can select a space
            screen.blit(text2, (150, 500))
            box.draw(screen, 40)  # drawing the highlight box

        pygame.display.update()  # constantly updating the screen


main_menu()  # calls main menu to start the game

pygame.quit()  # quit function when game ends
