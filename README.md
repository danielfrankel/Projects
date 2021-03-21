import pygame
from random import randint

# Screen Dimensions
HEIGHT = 600
WIDTH = 824
screen = pygame.display.set_mode((WIDTH, HEIGHT))
pattern = randint(1, 5)
guessed = False  # so that the computer won't guess a space already guessed

# Colours
BLACK = (0, 0, 0)
RED = (255, 0, 0)
GREEN = (0, 255, 0)
BLUE = (0, 0, 255)
ORANGE = (255, 127, 0)
CYAN = (0, 183, 235)
MAGENTA = (255, 0, 255)
YELLOW = (255, 255, 0)
WHITE = (255, 255, 255)
NAVY = (10, 27, 68)

# Defining Images
ship1 = pygame.image.load('zombie.png')
ship2 = pygame.image.load('skeleton.png')
ship3 = pygame.image.load('spider.png')
ship4 = pygame.image.load('creeper.png')
ship5 = pygame.image.load('enderman.png')
ship6 = pygame.image.load('z_piglin.png')
ship7 = pygame.image.load('w_skeleton.png')
ship8 = pygame.image.load('m_cube.png')
ship9 = pygame.image.load('blaze.png')
ship10 = pygame.image.load('ghast.png')
yellow = pygame.image.load("yellow_square.png")
black = pygame.image.load("black_square.png")
green = pygame.image.load("green_square.png")

# Rendering images
ship1 = pygame.transform.scale(ship1, (40, 40))  # zombie
ship2 = pygame.transform.scale(ship2, (40, 40))  # skeleton
ship3 = pygame.transform.scale(ship3, (40, 40))  # spider
ship4 = pygame.transform.scale(ship4, (40, 40))  # creeper
ship5 = pygame.transform.scale(ship5, (40, 40))  # enderman
ship6 = pygame.transform.scale(ship6, (40, 40))  # zombie piglin
ship7 = pygame.transform.scale(ship7, (40, 40))  # wither skeleton
ship8 = pygame.transform.scale(ship8, (40, 40))  # magma cube
ship9 = pygame.transform.scale(ship9, (40, 40))  # blaze
ship10 = pygame.transform.scale(ship10, (40, 40))  # ghast
yellow = pygame.transform.scale(yellow, (40, 40))  # yellow box
black = pygame.transform.scale(black, (40, 40))  # black (miss) box
green = pygame.transform.scale(green, (40, 40))  # green (hit) box

# Layout template for the player's ships, will be altered when a new ship is placed
player_ships = [
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
]
# creating enemy ships, 3 different patterns for 3 different layouts
if pattern == 1:
    enemy_ships = [
        [3, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [3, 0, 0, 0, 0, 0, 0, 2, 0, 0],
        [3, 0, 1, 1, 0, 0, 0, 2, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 2, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 4, 4, 4, 4, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 5, 5, 5, 5, 5, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    ]
if pattern == 2:
    enemy_ships = [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 5, 5, 5, 5, 5, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 4, 4, 4, 4, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [2, 0, 0, 0, 1, 1, 0, 0, 0, 0],
        [2, 0, 0, 0, 0, 0, 0, 3, 0, 0],
        [2, 0, 0, 0, 0, 0, 0, 3, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 3, 0, 0]
    ]
if pattern == 3:
    enemy_ships = [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [5, 5, 5, 5, 5, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 2, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 2, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 2, 0, 0, 0],
        [0, 3, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 3, 0, 0, 4, 4, 4, 4, 0, 0],
        [0, 3, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 1, 1, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    ]
if pattern == 4:
    enemy_ships = [
        [0, 0, 0, 0, 2, 0, 0, 0, 0, 0],
        [0, 3, 0, 0, 2, 0, 0, 0, 0, 0],
        [0, 3, 0, 0, 2, 0, 0, 0, 0, 0],
        [0, 3, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 5, 5, 5, 5, 5, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [4, 4, 4, 4, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 1, 1],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
    ]
if pattern == 5:
    enemy_ships = [
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 4, 4, 4, 4, 0, 0, 0],
        [0, 0, 2, 0, 0, 0, 0, 3, 0, 0],
        [0, 0, 2, 0, 0, 0, 0, 3, 0, 0],
        [0, 0, 2, 0, 0, 0, 0, 3, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
        [0, 0, 0, 0, 0, 1, 1, 0, 0, 0],
        [5, 5, 5, 5, 5, 0, 0, 0, 0, 0]
    ]


class Spaces(object):

    def __init__(self, col=1, row=1, num=1, chosen_image=ship1):  # initializes all the variables
        self.player = True
        self.borders = False
        self.col = col
        self.row = row
        self.num = num
        self.chosen_image = chosen_image

    def __eq__(self, other):
        if self.col == other.col and self.row == other.row:
            return True
        else:
            return False

    def draw(self, surface, gridsize=50):  # drawing the image on the board
        x = self.col * gridsize
        y = self.row * gridsize
        surface.blit(self.chosen_image, (x, y))

    def draw_boarder(self, surface, gridsize=20):  # drawing the boarder around the game
        x = self.col * gridsize  # makes it so it's a square
        y = self.row * gridsize
        CLR = BLACK
        pygame.draw.rect(surface, CLR, (x, y, gridsize, gridsize), 0)

    def move_down(self):  # moves ship down
        self.row = self.row + 1

    def move_up(self):  # moves ship up
        self.col = self.col + 1


class Ship(object):

    """ Collection of block
        data:
            col - column where the anchor(reference) block is located
            row - row where the anchor(reference) block is located
            blocksNo - number of blocks
        auxiliary data:
            blocksXoffset - list of horizontal offsets for each block, in reference to the anchor block
            blocksYoffset - list of vertical offsets for each block, in reference to the anchor block
    """

    def __init__(self, col=1, row=1, blocksNo=1):  # completely new init method
        self.col = col  # the column number of the anchor(reference) block
        self.row = row  # the row number of the anchor(reference) block
        self.num = 0  # this is an abstract class, so the colour is not important (invisible)
        self.blocks = [Spaces()] * blocksNo
        self.blocksXoffset = [0] * blocksNo
        self.blocksYoffset = [0] * blocksNo

    def draw_walls(self, surface, gridsize):  # separate draw function for walls only
        for block in self.blocks:
            block.draw_boarder(surface, gridsize)

    def update(self):
        for i in range(len(self.blocks)):
            blockCOL = self.col + self.blocksXoffset[i]
            blockROW = self.row + self.blocksYoffset[i]
            blockNUM = self.num
            blockImage = ship1
            self.blocks[i] = Spaces(blockCOL, blockROW, blockNUM, blockImage)

    def append(self, other):  # sticking the obstacles together

        """ Append all blocks from another ship to an object
        """
        for blocks in other.blocks:
            self.blocks.append(blocks)

    def collides(self, other):  # checking collisions

        """ Compare each block from a ship to all blocks from another object
            Return True only if there is a location conflict.
        """

        for block in self.blocks:
            for obstacle in other.blocks:
                if block == obstacle:  # if the block collides with the obstacle block
                    return True
        return False


class Shape(Ship):

    """ There are 5 ships and they all are different lengths (2 blocks, 3 blocks, 4 blocks, 5 blocks)
        data:               behaviour:
            col - column        move left/right/up/down
            row - row           draw
            num - number        rotate
                * figure/shape is defined by the colour
            rot - rotation
        auxiliary data:
            colOffsets - list of horizontal offsets for each block, in reference to the anchor block
            rowOffsets - list of vertical offsets for each block, in reference to the anchor block
    """

    def __init__(self, col=1, row=1, num=1):  # initializing the variables from the Ship class
        Ship.__init__(self, col, row, 4)  # invoke the parent class init method and change its parameters
        self.col = col
        self.row = row
        self.rot = 1
        self.num = num
        self.blocks = None
        self.colOffsets = None
        self.rowOffsets = None
        self.chosen_image = None
        self.rotate()

    def rotate(self):

        # offsets are assigned starting from the farthest block in reference to the anchor block

        if self.num == 1:  # (default rotation)
            #   o
            #   x               x o            x            o x
            #                                  o
            self.blocks = [Spaces()] * 2
            self.colOffsets = [[0, 0], [0, 1], [0, 0], [-1, 0]]
            self.rowOffsets = [[-1, 0], [0, 0], [0, 1], [0, 0]]
            self.chosen_image = ship1
        elif self.num == 2:  #
            #                   o                              o
            # o x o             x            o x o             x
            #                   o                              o
            self.blocks = [Spaces()] * 3
            self.colOffsets = [[-1, -0, 1], [0, 0, 0], [-1, 0, 1], [0, 0, 0]]
            self.rowOffsets = [[0, 0, 0], [-1, 0, 1], [0, 0, 0], [-1, 0, 1]]
            self.chosen_image = ship2
        elif self.num == 3:  #
            #                   o                              o
            # o x o             x            o x o             x
            #                   o                              o
            self.blocks = [Spaces()] * 3
            self.colOffsets = [[-1, -0, 1], [0, 0, 0], [-1, 0, 1], [0, 0, 0]]
            self.rowOffsets = [[0, 0, 0], [-1, 0, 1], [0, 0, 0], [-1, 0, 1]]
            self.chosen_image = ship3
        elif self.num == 4:  #
            #   o                              o
            #   x            o x o o           x           o x o o
            #   o                              o
            #   o                              o
            self.blocks = [Spaces()] * 4
            self.colOffsets = [[0, 0, 0, 0], [-1, 0, 1, 2], [0, 0, 0, 0], [-1, 0, 1, 2]]
            self.rowOffsets = [[-1, 0, 1, 2], [0, 0, 0, 0], [-1, 0, 1, 2], [0, 0, 0, 0]]
            self.chosen_image = ship4
        elif self.num == 5:
            #   o                              o
            #   o                              o
            #   x            o x o o o         x          o x o o o
            #   o                              o
            #   o                              o
            self.blocks = [Spaces()] * 5
            self.colOffsets = [[0, 0, 0, 0, 0], [3, 2, 1, 0, -1], [0, 0, 0, 0, 0], [-1, 0, 1, 2, 3]]
            self.rowOffsets = [[-2, -1, 0, 1, 2], [0, 0, 0, 0, 0], [2, 1, 0, -1, -2], [0, 0, 0, 0, 0]]
            self.chosen_image = ship5
        elif self.num == 6:  # highlight square
            self.blocks = [Spaces()]
            self.colOffsets = [[0], [0], [0], [0]]
            self.rowOffsets = [[0], [0], [0], [0]]
            self.chosen_image = yellow
        elif self.num == 7:  # miss square
            self.blocks = [Spaces()]
            self.colOffsets = [[0], [0], [0], [0]]
            self.rowOffsets = [[0], [0], [0], [0]]
            self.chosen_image = black
        elif self.num == 8:  # hit square
            self.blocks = [Spaces()]
            self.colOffsets = [[0], [0], [0], [0]]
            self.rowOffsets = [[0], [0], [0], [0]]
            self.chosen_image = green
        self.colOffsets = self.colOffsets[self.rot]
        self.rowOffsets = self.rowOffsets[self.rot]
        self.update()

    def draw(self, surface, gridsize):  # draw function
        for block in self.blocks:
            block.draw(surface, gridsize)

    def move_left(self):  # moves ship left
        self.col -= 1
        self.update()

    def move_right(self):  # moves ship right
        self.col += 1
        self.update()

    def move_down(self):  # moves ship down
        self.row += 1
        self.update()

    def move_up(self):  # moves ship up
        self.row -= 1
        self.update()

    def record_location(self):
        for i in range(len(self.blocks)):

            blockCOL = self.col + self.colOffsets[i]  # looks at every block
            blockROW = self.row + self.rowOffsets[i]

            x = int(blockCOL - 13.75)  # subtracts by the value of how far to the right to get an index
            y = int(blockROW - 0.5)  # subtracts by the value of how far down to get an index

            player_ships[y].pop(x)  # gets rid of the 0
            player_ships[y].insert(x, 1)  # adds in a 1 to indicate a ship is here

    def return_x(self):
        return self.col  # returns the x coordinate

    def return_y(self):
        return self.row  # returns the y coordinate

    def update(self):  # recalculates all the offsets and create blocks with the new attributes
        for i in range(len(self.blocks)):
            blockCOL = self.col + self.colOffsets[i]
            blockROW = self.row + self.rowOffsets[i]
            blockNUM = self.num
            blockImage = self.chosen_image  # fourth parameter to add in image
            self.blocks[i] = Spaces(blockCOL, blockROW, blockNUM, blockImage)


class Enemy(Ship):

    """ There are 5 ships and they all are different lengths (2 blocks, 3 blocks, 4 blocks, 5 blocks)
        data:               behaviour:
            col - column        move left/right/up/down
            row - row           draw
            num - number        rotate
            rot - rotation
        auxiliary data:
            colOffsets - list of horizontal offsets for each block, in reference to the anchor block
            rowOffsets - list of vertical offsets for each block, in reference to the anchor block
    """

    def __init__(self, col=1, row=1, num=1):  # initializing the variables from the Cluster class
        Ship.__init__(self, col, row, 4)  # invoke the parent class init method and change its parameters
        self.col = col
        self.row = row
        self.rot = 1
        self.num = num
        self.blocks = None
        self.colOffsets = None
        self.rowOffsets = None
        self.chosen_image = None
        self.rotate()

    def rotate(self):

        # offsets are assigned starting from the farthest block in reference to the anchor block

        if self.num == 1:  # (default rotation)
            #   o
            #   x               x o            x            o x
            #                                  o
            self.blocks = [Spaces()] * 2
            self.colOffsets = [[0, 0], [0, 1], [0, 0], [-1, 0]]
            self.rowOffsets = [[-1, 0], [0, 0], [0, 1], [0, 0]]
            self.chosen_image = ship6
        elif self.num == 2:  #
            #                   o                              o
            # o x o             x            o x o             x
            #                   o                              o
            self.blocks = [Spaces()] * 3
            self.colOffsets = [[-1, -0, 1], [0, 0, 0], [-1, 0, 1], [0, 0, 0]]
            self.rowOffsets = [[0, 0, 0], [-1, 0, 1], [0, 0, 0], [-1, 0, 1]]
            self.chosen_image = ship7
        elif self.num == 3:  #
            #                   o                              o
            # o x o             x            o x o             x
            #                   o                              o
            self.blocks = [Spaces()] * 3
            self.colOffsets = [[-1, -0, 1], [0, 0, 0], [-1, 0, 1], [0, 0, 0]]
            self.rowOffsets = [[0, 0, 0], [-1, 0, 1], [0, 0, 0], [-1, 0, 1]]
            self.chosen_image = ship8
        elif self.num == 4:  #
            #   o                              o
            #   x            o x o o           x           o x o o
            #   o                              o
            #   o                              o
            self.blocks = [Spaces()] * 4
            self.colOffsets = [[0, 0, 0, 0], [-1, 0, 1, 2], [0, 0, 0, 0], [-1, 0, 1, 2]]
            self.rowOffsets = [[-1, 0, 1, 2], [0, 0, 0, 0], [-1, 0, 1, 2], [0, 0, 0, 0]]
            self.chosen_image = ship9
        elif self.num == 5:
            #   o                              o
            #   o                              o
            #   x            o x o o o         x          o x o o o
            #   o                              o
            #   o                              o
            self.blocks = [Spaces()] * 5
            self.colOffsets = [[0, 0, 0, 0, 0], [3, 2, 1, 0, -1], [0, 0, 0, 0, 0], [-1, 0, 1, 2, 3]]
            self.rowOffsets = [[-2, -1, 0, 1, 2], [0, 0, 0, 0, 0], [2, 1, 0, -1, -2], [0, 0, 0, 0, 0]]
            self.chosen_image = ship10
        self.colOffsets = self.colOffsets[self.rot]
        self.rowOffsets = self.rowOffsets[self.rot]
        self.update()

    def draw(self, surface, gridsize):  # draw function
        for block in self.blocks:
            block.draw(surface, gridsize)

    def update(self):  # recalculates all the offsets and create blocks with the new attributes
        for i in range(len(self.blocks)):
            blockCOL = self.col + self.colOffsets[i]
            blockROW = self.row + self.rowOffsets[i]
            blockNUM = self.num
            blockImage = self.chosen_image  # fourth parameter to add in image
            self.blocks[i] = Spaces(blockCOL, blockROW, blockNUM, blockImage)


class Board(Ship):  # so that the blocks will stay when their placement is selected

    def __init__(self, col=0, row=0, blocksNo=0):  # initializing the variables from the Ship class
        Ship.__init__(self, col, row,
                      blocksNo)  # initially the playing field is empty(no shapes are left inside the field)

    def draw(self, surface, gridsize):  # draw function
        for block in self.blocks:
            block.draw(surface, gridsize)


class Floor(Ship):

    """ Horizontal line of blocks
        data:
            col - column where the anchor block is located
            row - row where the anchor block is located
            blocksNo - number of blocks
        auxiliary data:
            blocksXoffset - list of horizontal offsets for each block, in reference to the anchor block
    """

    def __init__(self, col=1, row=1, blocksNo=1):  # initializes the variables from the Ship class
        Ship.__init__(self, col, row, blocksNo)
        for i in range(blocksNo):  # creates floor
            self.blocksXoffset[i] = i
        self.update()


class Wall(Ship):

    """ Vertical line of blocks
        data:
            col - column where the anchor block is located
            row - row where the anchor block is located
            blocksNo - number of blocks
        auxiliary data:
            blocksYoffset - list of vertical offsets for each block, in reference to the anchor block
    """

    def __init__(self, col=1, row=1, blocksNo=1):  # initializes the variables from the Ship class
        Ship.__init__(self, col, row, blocksNo)
        for i in range(blocksNo):  # creates walls
            self.blocksYoffset[i] = i
        self.update()


class Button:  # button class

    def __init__(self, colour, opacity, stroke_weight, x, y, width, height):  # variables to create button
        self.colour = colour
        self.opacity = opacity
        self.x = x
        self.y = y
        self.width = width
        self.height = height
        self.stroke_weight = stroke_weight

    def create_button(self):  # code to create physical button
        pygame.draw.rect(screen, self.colour, (self.x, self.y, self.width, self.height), self.stroke_weight)

    def fade(self, opacity):  # to change the fill of button
        fade = pygame.Surface((self.width, self.height))
        fade.fill((0, 0, 0))
        fade.set_alpha(opacity, opacity)  # changes shade of filling
        screen.blit(fade, (self.x, self.y))

    def activate(self, mouse):
        if self.x + self.width > mouse[0] > self.x:  # checks if in x boundary
            if self.y + self.height > mouse[1] > self.y:  # checks if in y boundary
                return True
        return False
