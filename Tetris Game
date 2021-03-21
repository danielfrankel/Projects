from tetris_classes import *  # imports the classes from the other file (tetris_classes.py)
from random import randint  # to randomize which shape comes next
import pygame  # for displaying the game
from pygame import mixer  # for the music
import time  # for the timer

pygame.init()  # initializes pygame
pygame.mixer.init()  # initializes the mixer

# Screen Dimensions
HEIGHT = 600
WIDTH = 824
GRIDSIZE = HEIGHT // 24
screen = pygame.display.set_mode((WIDTH, HEIGHT))

# Boarder Dimensions
COLUMNS = 14
ROWS = 21
LEFT = 2
TOP = 1
MIDDLE = LEFT + COLUMNS // 2
RIGHT = LEFT + COLUMNS
FLOOR = TOP + ROWS

# Misc
timer = 0  # time between drops in game
stopwatch = 0  # for the time during the game
lines = 0  # number of lines cleared
level = 1  # current level
score = 0  # current score
tetris = False  # whether or not the player cleared 4 rows at a time
paused = False  # pause button
gameStart = False
time_passed = None

# Music
kill = mixer.Sound('Among Us (Kill) - Sound Effect (HD).mp3')
ding = mixer.Sound('Making Simon Says (Among Us Task OpenComplete) - Sound Effect for editing(1)-[AudioTrimmer.com].mp3')
music = True  # so that music can play when you come back to main menu

# Defining Images
image8 = pygame.image.load('skeld.png')
image9 = pygame.image.load('mira.png')
image10 = pygame.image.load('polus.png')
image11 = pygame.image.load('background.png')
image12 = pygame.image.load('paused.png')  # same background as image11, but with the pause button glowing
image13 = pygame.image.load('mainMenu.png')
image14 = pygame.image.load('help_menu.png')

# Rendering Images
image8 = pygame.transform.scale(image8, (GRIDSIZE * 13, GRIDSIZE * 22))
image9 = pygame.transform.scale(image9, (GRIDSIZE * 13, GRIDSIZE * 22))
image10 = pygame.transform.scale(image10, (GRIDSIZE * 13, GRIDSIZE * 22))
image11 = pygame.transform.scale(image11, (WIDTH, HEIGHT))
image12 = pygame.transform.scale(image12, (WIDTH, HEIGHT))
image13 = pygame.transform.scale(image13, (WIDTH, HEIGHT))
image14 = pygame.transform.scale(image14, (WIDTH, HEIGHT))

# Texts
word = pygame.font.SysFont('In your face, Joffrey!', 50)
word2 = pygame.font.SysFont('In your face, Joffrey!', 30)
word3 = pygame.font.SysFont('VCR OSD Mono', 30)
text5 = word.render("Play Game", True, WHITE)
text6 = word.render("Play Game", True, BLACK)
text7 = word.render("Help Menu", True, WHITE)
text8 = word.render("Help Menu", True, BLACK)
text9 = word2.render("Back", True, WHITE)
text10 = word2.render("Back", True, BLACK)

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


def redraw_screen():

    global time_passed

    screen.fill(WHITE)

    # different backgrounds based on the level (Skeld, Mira HQ, Polus)
    if level == 1:
        level_background = image8
    if level == 2:
        level_background = image9
    if level == 3:
        level_background = image10

    # Images
    if not paused:
        screen.blit(image11, (0, 0))
    if paused:
        screen.blit(image12, (0, 0))

    screen.blit(level_background, (GRIDSIZE * 2, GRIDSIZE))
    shape.draw(screen, GRIDSIZE)  # draw player's shape
    shadow.draw_shadow(screen, GRIDSIZE)  # draw player's shadow
    nextshape.draw(screen, GRIDSIZE)  # draw the next shape
    floor.draw_walls(screen, GRIDSIZE)  # draw the floor
    roof.draw_walls(screen, GRIDSIZE)  # draw the roof
    leftWall.draw_walls(screen, GRIDSIZE)  # draw the left wall
    rightWall.draw_walls(screen, GRIDSIZE)  # draw the right wall
    obstacles.draw_obstacles(screen, GRIDSIZE)  # draw the obstacles

    b1.create_button()  # creates the button

    text = word.render(str(lines), True, WHITE)  # number of lines
    text2 = word.render(str(level), True, WHITE)  # level the player is at
    text3 = word.render(str(score), True, WHITE)  # current score

    text4 = word.render(str(time_passed), True, WHITE)  # time elapsed is shown
    screen.blit(text4, (675, 515))

    screen.blit(text, (675, 185))
    screen.blit(text2, (675, 245))
    screen.blit(text3, (675, 310))

    pygame.display.update()


def main_menu():

    global music

    if music:
        mixer.music.load('AMONG US - OST - MAIN THEME SONG [HQ].mp3')  # Among Us main menu theme
        pygame.mixer.music.set_volume(0.7)
        mixer.music.play(-1)  # -1 is put so it can play on loop
        music = False  # so that it does not start over and over again

    while True:

        global opacity1, opacity2, one_black, one_white, two_black, two_white, e

        screen.fill(WHITE)
        screen.blit(image13, (0, 0))  # background image

        for e in pygame.event.get():
            if e.type == pygame.QUIT:  # the quit button on the pop-out screen
                pygame.quit()  # pygame ends
        if e.type == pygame.MOUSEMOTION:
            if b2.activate(pygame.mouse.get_pos()):  # if mouse is over button 2
                opacity1 = 250
                one_black = False
                one_white = True
            else:
                opacity1 = 0
                one_white = False
                one_black = True
            if b3.activate(pygame.mouse.get_pos()):  # if mouse is over button 3
                opacity2 = 250
                two_black = False
                two_white = True
            else:
                opacity2 = 0
                two_white = False
                two_black = True
        if e.type == pygame.MOUSEBUTTONDOWN:
            if b2.activate(pygame.mouse.get_pos()):  # if mouse is clicked while hovering over button 2
                mixer.music.pause()
                impostor_reveal()  # go to game
            if b3.activate(pygame.mouse.get_pos()):  # if mouse is clicked while hovering over button 3
                help_menu()  # go to help menu

        b2.create_button()  # creates the button
        b3.create_button()
        b2.fade(opacity1)  # changes colour of button
        b3.fade(opacity2)

        if one_black:  # text changes colour based on whether mouse is over the button or not
            screen.blit(text5, (165, 430))
        if one_white:
            screen.blit(text6, (165, 430))
        if two_black:
            screen.blit(text7, (535, 430))
        if two_white:
            screen.blit(text8, (535, 430))

        pygame.display.update()


def help_menu():

    while True:

        global opacity3, three_white, three_black

        screen.fill(WHITE)
        screen.blit(image14, (0, 0))  # help menu background

        for e in pygame.event.get():
            if e.type == pygame.QUIT:  # the quit button on the pop-out screen
                pygame.quit()  # pygame ends
            if e.type == pygame.MOUSEMOTION:
                if b4.activate(pygame.mouse.get_pos()):  # if mouse is over button 4
                    opacity3 = 250
                    three_black = False
                    three_white = True
                else:
                    opacity3 = 0
                    three_white = False
                    three_black = True
            if e.type == pygame.MOUSEBUTTONDOWN:
                if b4.activate(pygame.mouse.get_pos()):  # if mouse is clicked while hovering over button 4
                    main_menu()

        b4.create_button()  # creates the button
        b4.fade(opacity3)

        if three_black:  # text changes colour based on whether mouse is over the button or not
            screen.blit(text9, (65, 537))
        if three_white:
            screen.blit(text10, (65, 537))

        pygame.display.update()


def impostor_reveal():

    index = 1  # so that the code can run through each photo

    mixer.music.load('Among Us (Role Reveal) - Sound Effect (HD).mp3')
    pygame.mixer.music.set_volume(0.7)  # changing volume so sound effects can be properly heard
    mixer.music.play(-1)

    while True:

        image15 = pygame.image.load('reveal_frame'+str(index)+'.png')  # every photo has a different number
        image15 = pygame.transform.scale(image15, (WIDTH, HEIGHT))
        screen.blit(image15, (0, 0))
        index += 1  # increase index so a new photo is on the screen
        time.sleep(0.075)  # slight delay so it is not too quick

        if index == 43:  # when finished going through every photo, the game starts
            main_game()

        pygame.display.update()


def game_over():

    global e

    index = 1  # for the ejected gif
    index2 = 1  # for the defeat screen gif
    ejected = True  # so it doesn't run through it after being finished
    defeated = True  # so it doesn't run through it after being finished
    already_played = False  # so music doesn't glitch and is called once

    mixer.music.load('Among Us Eject Sound Effect.mp3')
    pygame.mixer.music.set_volume(0.7)  # changing volume so sound effects can be properly heard
    mixer.music.play(1)

    while True:

        for e in pygame.event.get():
            if e.type == pygame.QUIT:  # the quit button on the pop-out screen
                pygame.quit()  # pygame ends
            if e.type == pygame.MOUSEBUTTONDOWN:
                if b5.activate(pygame.mouse.get_pos()):
                    mixer.music.pause()
                    game_reset()
                    main_menu()
                if b6.activate(pygame.mouse.get_pos()):
                    mixer.music.pause()
                    game_reset()
                    impostor_reveal()
        if ejected:
            image16 = pygame.image.load('ejected'+str(index)+'.png')  # every photo has a different number
            image16 = pygame.transform.scale(image16, (WIDTH, HEIGHT))
            screen.blit(image16, (0, 0))
            index += 1  # increase index so a new photo is on the screen
            time.sleep(0.075)  # slight delay so it is not too quick

        if index == 65 and defeated:  # when finished going through every photo, move to the next set of images
            ejected = False
            if not already_played:
                mixer.music.load('Among Us Defeat (Impostor WinVictory) Sound Effect.mp3')
                pygame.mixer.music.set_volume(0.7)  # changing volume so sound effects can be properly heard
                mixer.music.play(1)
                already_played = True
            image17 = pygame.image.load('defeat'+str(index2)+'.png')  # every photo has a different number
            image17 = pygame.transform.scale(image17, (WIDTH, HEIGHT))
            screen.blit(image17, (0, 0))
            index2 += 1  # increase index so a new photo is on the screen
            time.sleep(0.075)  # slight delay so it is not too quick

        if index2 == 18:  # when finished going through every photo, keep the final photo as the end game screen
            defeated = False
            image17 = pygame.image.load('defeat18.png')
            image17 = pygame.transform.scale(image17, (WIDTH, HEIGHT))
            screen.blit(image17, (0, 0))
            text11 = word3.render("Final Score: "+str(score), True, WHITE)  # final score is shown at the end
            screen.blit(text11, (275, 525))

        b5.create_button()  # creates the 'exit' button
        b6.create_button()  # creates the 'play again' button

        pygame.display.update()


def game_reset():

    global timer, stopwatch, lines, level, score, music

    # resetting all the variables so that there are no issues
    timer = 0
    stopwatch = 0
    lines = 0
    level = 1
    score = 0
    music = True

    obstacles.clear_board()  # clears every row
    mixer.music.rewind()  # rewinds the music


shapeNo = randint(1, 7)  # random shape is chosen
shape = Shape(8, 3, shapeNo)
nextShapeNo = randint(1, 7)  # random shape is chosen to be next
nextshape = Shape(LEFT + 21, TOP + 4, nextShapeNo)
inPlay = True  # whether the game is running or not

floor = Floor(LEFT, FLOOR, COLUMNS)  # floor in game
roof = Floor(LEFT, TOP, COLUMNS)  # roof in game
leftWall = Wall(LEFT, TOP, ROWS)  # left wall in game
rightWall = Wall(RIGHT - 1, TOP, ROWS)  # right wall in game
obstacles = Obstacles(LEFT, FLOOR)  # the obstacles in game
shadow = Shape(MIDDLE-1, FLOOR - 1, shapeNo)  # the shadows of the shapes

b1 = Button(NAVY, 0, 1, 450, 515, 50, 50)  # pause button
b2 = Button(WHITE, 0, 3, 110, 400, 250, 100)  # play button
b3 = Button(WHITE, 0, 3, 475, 400, 250, 100)  # help menu button
b4 = Button(WHITE, 0, 3, 35, 525, 100, 50)  # back button
b5 = Button(BLACK, 0, 1, 5, 440, 120, 150)  # exit button
b6 = Button(BLACK, 0, 1, 700, 425, 120, 175)  # play again button


def main_game():

    global inPlay, start_time, elapsed_time, time_passed, gameStart

    start_time = time.time()  # setting start time to the time.time() function
    gameStart = True  # makes sure that the timer doesn't count until the game has started

    mixer.music.load('AmongUs.mp3')
    pygame.mixer.music.set_volume(0.7)  # changing volume so sound effects can be properly heard
    mixer.music.play(-1)

    while inPlay:

        global shadow, paused, shape, nextshape, nextShapeNo, score, lines, tetris, level, timer, shapeNo

        shadow.shadow_create()  # Creates the shadow

        if not paused and gameStart:  # so the timer won't continue while the game is paused
            elapsed_time = time.time() - start_time  # calculating how much time has gone by
            time_passed = stopwatch + int(elapsed_time)  # time left is determined by subtracting elapsed time from timer

        if not paused:
            for event in pygame.event.get():
                if event.type == pygame.MOUSEBUTTONDOWN:
                    if b1.activate(pygame.mouse.get_pos()):  # if the mouse is hovering over the button and is pressed
                        paused = True  # game is paused, actions are blocked
                if event.type == pygame.QUIT:
                    inPlay = False  # stops running if quit button is pressed
                if event.type == pygame.KEYDOWN:  # checks if key is pressed
                    if event.key == pygame.K_LEFT:  # checks left key
                        shape.move_left()
                        shadow.move_left()
                        if shape.collides(leftWall):  # checks collision with left wall
                            shape.move_right()
                            shadow.move_right()
                        if shape.collides(obstacles):  # checks collision with obstacles
                            shape.move_right()
                            shadow.move_right()
                    if event.key == pygame.K_RIGHT:  # checks right key
                        shape.move_right()
                        shadow.move_right()
                        if shape.collides(rightWall):  # checks collision with right wall
                            shape.move_left()
                            shadow.move_left()
                        if shape.collides(obstacles):  # checks collision with obstacles
                            shape.move_left()
                            shadow.move_left()
                    if event.key == pygame.K_UP:  # checks up key
                        shape.rot = (shape.rot + 1) % 4  # Four possible values for rotation - 0,1,2,3
                        shape.rotate()
                        shadow.rotate_shadow(shape)
                        if shape.collides(leftWall):  # checks collision with left wall
                            shape.move_right()
                            shape.rotate()
                            shadow.move_right()
                            shadow.rotate_shadow(shape)
                        if shape.collides(rightWall):  # checks collision with right wall
                            shape.move_left()
                            shape.rotate()
                            shadow.move_left()
                            shadow.rotate_shadow(shape)
                        if shape.collides(floor):  # checks collision with the floor
                            shape.move_up()
                            shape.rotate()
                            shadow.move_up()
                            shadow.rotate_shadow(shape)

                    fullRows = obstacles.find_full_rows(TOP, FLOOR,
                                                        COLUMNS)  # finds the full rows and removes their blocks from the obstacles
                    print("full rows: ",
                          fullRows)  # printing the full rows is done to visualize the process remove it afterwards
                    obstacles.remove_full_rows(fullRows)
                    if event.key == pygame.K_SPACE:  # checks space key
                        while shape.collides(floor) is not True and shape.collides(obstacles) is not True:
                            shape.move_down()  # move down as long as it hasn't touched the floor yet
                        shape.move_up()  # shape moves up to prevent being inside the floor/obstacles
                        obstacles.append(shape)
                        ding.play()  # sound effect is played
                        shape = Shape(8, 3, nextShapeNo)  # new shape is placed at top
                        shadow = Shape(MIDDLE-1, FLOOR - 1, nextShapeNo)  # new shadow is created
                        nextShapeNo = randint(1, 7)
                        nextshape = Shape(LEFT + 21, TOP + 4, nextShapeNo)  # next shape is placed in "next up" section
                        timer = 0  # timer for dropping block is reset so the new block has a full second before it drops

                        obstacles.show()  # printing the blocks is done to visualize the process remove it afterwards
                        fullRows = obstacles.find_full_rows(TOP, FLOOR,
                                                            COLUMNS - 2)  # finds the full rows and removes their blocks from the obstacles
                        print("full rows: ",
                              fullRows)  # printing the full rows is done to visualize the process remove it afterwards
                        obstacles.remove_full_rows(fullRows)

                        if len(fullRows) == 4 and tetris is True:  # keeps track of score
                            score += 1200
                            lines += 1
                            tetris = False
                            kill.play()  # sound effect is played

                        elif len(fullRows) == 4 and tetris is not True:  # if the played had previously not gotten tetris
                            score += 800
                            lines += 1
                            tetris = True
                            kill.play()  # sound effect is played

                        elif len(fullRows) == 3:
                            score += 300
                            lines += 1
                            tetris = False
                            kill.play()  # sound effect is played

                        elif len(fullRows) == 2:
                            score += 200
                            lines += 1
                            tetris = False
                            kill.play()  # sound effect is played

                        elif len(fullRows) == 1:
                            score += 100
                            lines += 1
                            tetris = False
                            kill.play()  # sound effect is played

                        if score >= 500:  # level 2 starts after the player reaches 500 points
                            level = 2

                        if score >= 1000:  # level 3 starts after the player reaches 1000 points
                            level = 3

            while not (shadow.collides(floor) or shadow.collides(obstacles)):  # Shadow moves down when not colliding
                shadow.move_down()

            while shadow.collides(floor) or shadow.collides(obstacles):  # Shadow moves up once when collides with obstacle
                shadow.move_up()

            if level == 1:  # slow speed at level 1
                timer += 4
            elif level == 2:  # medium speed at level 2
                timer += 5
            elif level == 3:  # fast speed at level 3
                timer += 6

            if timer == 60:  # once the timer reaches 60
                shape.move_down()  # the shape moves down once
                timer = 0  # then it resets
            if shape.collides(floor) or shape.collides(obstacles):
                shape.move_up()
                obstacles.append(shape)
                ding.play()
                obstacles.show()
                shape = Shape(8, 3, nextShapeNo)
                shadow = Shape(MIDDLE-1, FLOOR - 1, nextShapeNo)
                shapeNo = nextShapeNo
                nextShapeNo = randint(1, 7)
                nextshape = Shape(LEFT + 21, TOP + 4, nextShapeNo)

                if obstacles.collides(roof):  # Checks if the obstacles collide with the roof
                    game_over()

                obstacles.show()  # printing the blocks is done to visualize the process remove it afterwards
                fullRows = obstacles.find_full_rows(TOP, FLOOR,
                                                    COLUMNS - 2)  # finds the full rows and removes their blocks from the obstacles
                print("full rows: ",
                      fullRows)  # printing the full rows is done to visualize the process remove it afterwards
                obstacles.remove_full_rows(fullRows)

                if len(fullRows) == 4 and tetris is True:  # keeps track of score
                    score += 1200
                    lines += 1
                    tetris = False
                    kill.play()  # sound effect is played

                elif len(fullRows) == 4 and tetris is not True:
                    score += 800
                    lines += 1
                    tetris = True
                    kill.play()  # sound effect is played

                elif len(fullRows) == 3:
                    score += 300
                    lines += 1
                    tetris = False
                    kill.play()  # sound effect is played

                elif len(fullRows) == 2:
                    score += 200
                    lines += 1
                    tetris = False
                    kill.play()  # sound effect is played

                elif len(fullRows) == 1:
                    score += 100
                    lines += 1
                    tetris = False
                    kill.play()  # sound effect is played

                if score >= 500:  # level 2 starts after the player reaches 500 points
                    level = 2

                if score >= 1000:  # level 3 starts after the player reaches 1000 points
                    level = 3

        for event in pygame.event.get():
            if event.type == pygame.MOUSEBUTTONDOWN:
                if b1.activate(pygame.mouse.get_pos()):
                    paused = False

        redraw_screen()  # update the screen


main_menu()  # calls main menu to start game

pygame.quit()  # quit function when game ends
