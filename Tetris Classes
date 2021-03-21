import pygame

# Colours and Figures
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
COLOURS = [BLACK, RED, GREEN, BLUE, ORANGE, CYAN, MAGENTA, YELLOW, WHITE]
CLR_names = ['black', 'red', 'green', 'blue', 'orange', 'cyan', 'magenta', 'yellow', 'white']
FIGURES = [None, 'Z', 'S', 'J', 'L', 'I', 'T', 'O', None]

# Defining Images
image1 = pygame.image.load('red.png')
image2 = pygame.image.load('lime.png')
image3 = pygame.image.load('blue.png')
image4 = pygame.image.load('orange.png')
image5 = pygame.image.load('cyan.png')
image6 = pygame.image.load('pink.png')
image7 = pygame.image.load('yellow.png')

# Rendering Images
image1 = pygame.transform.scale(image1, (25, 25))
image2 = pygame.transform.scale(image2, (25, 25))
image3 = pygame.transform.scale(image3, (25, 25))
image4 = pygame.transform.scale(image4, (25, 25))
image5 = pygame.transform.scale(image5, (25, 25))
image6 = pygame.transform.scale(image6, (25, 25))
image7 = pygame.transform.scale(image7, (25, 25))

# Screen Dimensions
HEIGHT = 600
WIDTH = 824
screen = pygame.display.set_mode((WIDTH, HEIGHT))


class Block(object):

    def __init__(self, col=1, row=1, clr=1, chosen_image=image1):  # initializes all the variables
        self.player = True
        self.borders = False
        self.col = col
        self.row = row
        self.clr = clr
        self.chosen_image = chosen_image

    def __str__(self):
        return '(' + str(self.col) + ',' + str(self.row) + ') ' + CLR_names[self.clr]  # for printing purposes

    def __eq__(self, other):
        if self.col == other.col and self.row == other.row:
            return True
        else:
            return False

    def draw(self, surface, gridsize=20):  # drawing the image on the board
        x = self.col * gridsize
        y = self.row * gridsize
        surface.blit(self.chosen_image, (x, y))

    def draw_boarder(self, surface, gridsize=20):  # drawing the boarder around the game
        x = self.col * gridsize  # makes it so it's a square
        y = self.row * gridsize
        CLR = COLOURS[self.clr]
        pygame.draw.rect(surface, CLR, (x, y, gridsize, gridsize), 0)

    def draw_shadow(self, screen, GRIDSIZE):  # drawing the shadows of the blocks

        x = self.col * GRIDSIZE  # x length of shadow block
        y = self.row * GRIDSIZE  # y length of shadow block

        CLR = COLOURS[self.clr]

        pygame.draw.rect(screen, CLR, (x, y, GRIDSIZE, GRIDSIZE), 1)  # drawing the shadow
        pygame.draw.rect(screen, CLR, (x, y, GRIDSIZE + 1, GRIDSIZE + 1), 1)

    def move_down(self):  # moves shape down
        self.row = self.row + 1

    def move_up(self):  # moves shape up
        self.col = self.col + 1


class Cluster(object):
    """ Collection of blocks
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
        self.clr = 0  # this is an abstract class, so the colour is not important (invisible)
        self.blocks = [Block()] * blocksNo  # LEARN HOW TO...create clusters with different length
        self.blocksXoffset = [0] * blocksNo
        self.blocksYoffset = [0] * blocksNo

    def update(self):
        for i in range(len(self.blocks)):
            blockCOL = self.col + self.blocksXoffset[
                i]  # LEARN HOW TO...calculate the coordinates of each block in a cluster
            blockROW = self.row + self.blocksYoffset[i]
            blockCLR = self.clr
            blockImage = image1
            self.blocks[i] = Block(blockCOL, blockROW, blockCLR,
                                   blockImage)  # LEARN HOW TO...generate new block objects to form a cluster

    def draw_obstacles(self, surface, gridsize):  # separate draw function for obstacles only
        for block in self.blocks:
            block.draw(surface, gridsize)

    def draw_walls(self, surface, gridsize):  # separate draw function for walls only
        for block in self.blocks:
            block.draw_boarder(surface, gridsize)

    def draw_shadow(self, surface, gridsize):  # separate draw function for shadows only
        for block in self.blocks:
            block.draw_shadow(surface, gridsize)

    def collides(self, other):  # checking collisions

        """ Compare each block from a cluster to all blocks from another cluster.
            Return True only if there is a location conflict.
        """
        # LEARN HOW...two objects are compared. Open the link below and pay attention to the if statement.
        # http://goo.gl/ARrHWD. Use the same procedure to compare blocks from two clusters. The for loops are already done.

        for block in self.blocks:
            for obstacle in other.blocks:
                if block == obstacle:  # if the block collides with the obstacle block
                    return True
        return False

    def append(self, other):  # sticking the obstacles together

        """ Append all blocks from another cluster to this one.
        """
        for blocks in other.blocks:
            # print(other.blocks[blocks])
            self.blocks.append(blocks)


class Obstacles(Cluster):
    """ Collection of tetrominoe blocks on the playing field, left from previous shapes.

    """

    def __init__(self, col=0, row=0, blocksNo=0):  # initializing the variables from the Cluster class
        Cluster.__init__(self, col, row,
                         blocksNo)  # initially the playing field is empty(no shapes are left inside the field)

    def show(self):  # showing  which obstacles there are
        print("\nObstacle: ")
        for block in self.blocks:
            print(block)

    def find_full_rows(self, top, bottom, columns):

        fullRows = []  # which rows are full
        rows = []

        for block in self.blocks:
            rows.append(block.row)  # make a list with only the row numbers of all blocks

        for row in range(top, bottom):  # starting from the top (row 0), and down to the bottom
            if rows.count(row) == columns:  # if the number of blocks with certain row number
                fullRows.append(row)  # equals to the number of columns -> the row is full
        return fullRows  # return a list with the full rows' numbers

    def remove_full_rows(self, fullRows):
        for row in fullRows:  # for each full row, STARTING FROM THE TOP (fullRows are in order)
            for i in reversed(range(len(self.blocks))):  # check all obstacle blocks in REVERSE ORDER,
                # so when popping them the index doesn't go out of range !!!
                if self.blocks[i].row == row:
                    self.blocks.pop(i)  # remove each block that is on this row
                elif self.blocks[i].row < row:
                    self.blocks[i].move_down()  # move down each block that is above this row

    def clear_board(self):
        for row in range(22):  # for each full row, STARTING FROM THE TOP (fullRows are in order)
            for i in reversed(range(len(self.blocks))):  # check all obstacle blocks in REVERSE ORDER,
                # so when popping them the index doesn't go out of range !!!
                self.blocks.pop(i)  # remove each block that is on this row


class Shape(Cluster):
    """ A tetrominoe in one of the shapes: Z, S, J, L, I, T, O; consists of 4 x Block() objects
        data:               behaviour:
            col - column        move left/right/up/down
            row - row           draw
            clr - colour        rotate
                * figure/shape is defined by the colour
            rot - rotation
        auxiliary data:
            colOffsets - list of horizontal offsets for each block, in reference to the anchor block
            rowOffsets - list of vertical offsets for each block, in reference to the anchor block
    """

    def __init__(self, col=1, row=1, clr=1):  # initializing the variables from the Cluster class
        Cluster.__init__(self, col, row, 4)  # invoke the parent class init method and change its parameters
        self.col = col
        self.row = row
        self.clr = clr
        self.rot = 1
        self.blocks = [Block()] * 4  # creates four blocks
        self.colOffsets = [-1, 0, 0, 1]
        self.rowOffsets = [-1, -1, 0, 0]
        self.chosen_image = None
        self.rotate()

    def __str__(self):
        return FIGURES[self.clr] + ' (' + str(self.col) + ',' + str(self.row) + ') ' + CLR_names[self.clr]

    def rotate(self):

        # offsets are assigned starting from the farthest block in reference to the anchor block

        if self.clr == 1:  # (default rotation)
            #   o             o o                o
            # o x               x o            x o          o x
            # o                                o              o o
            self.colOffsets = [[-1, -1, 0, 0], [-1, 0, 0, 1], [1, 1, 0, 0], [1, 0, 0, -1]]
            self.rowOffsets = [[1, 0, 0, -1], [-1, -1, 0, 0], [-1, 0, 0, 1], [1, 1, 0, 0]]
            self.chosen_image = image1
        elif self.clr == 2:  #
            # o                 o o           o
            # o x             o x             x o             x o
            #   o                               o           o o
            self.colOffsets = [[-1, -1, 0, 0], [1, 0, 0, -1], [1, 1, 0, 0], [-1, 0, 0, 1]]
            self.rowOffsets = [[-1, 0, 0, 1], [-1, -1, 0, 0], [1, 0, 0, -1], [1, 1, 0, 0]]
            self.chosen_image = image2
        elif self.clr == 3:  #
            #   o             o                o o
            #   x             o x o            x           o x o
            # o o                              o               o
            self.colOffsets = [[-1, 0, 0, 0], [-1, -1, 0, 1], [1, 0, 0, 0], [1, 1, 0, -1]]
            self.rowOffsets = [[1, 1, 0, -1], [-1, 0, 0, 0], [-1, -1, 0, 1], [1, 0, 0, 0]]
            self.chosen_image = image3
        elif self.clr == 4:  #
            # o o                o             o
            #   x            o x o             x           o x o
            #   o                              o o         o
            self.colOffsets = [[-1, 0, 0, 0], [-1, 0, 1, 1], [0, 0, 0, 1], [-1, -1, 0, 1]]
            self.rowOffsets = [[-1, -1, 0, 1], [0, 0, 0, -1], [-1, 0, 1, 1], [1, 0, 0, 0]]
            self.chosen_image = image4
        elif self.clr == 5:
            #   o                              o
            #   o                              x
            #   x            o x o o           o          o o x o
            #   o                              o
            self.colOffsets = [[0, 0, 0, 0], [2, 1, 0, -1], [0, 0, 0, 0], [-2, -1, 0, 1]]
            self.rowOffsets = [[-2, -1, 0, 1], [0, 0, 0, 0], [2, 1, 0, -1], [0, 0, 0, 0]]
            self.chosen_image = image5
        elif self.clr == 6:  #
            #   o              o                o
            # o x            o x o              x o         o x o
            #   o                               o             o
            self.colOffsets = [[0, -1, 0, 0], [-1, 0, 0, 1], [0, 1, 0, 0], [1, 0, 0, -1]]
            self.rowOffsets = [[1, 0, 0, -1], [0, -1, 0, 0], [-1, 0, 0, 1], [0, 1, 0, 0]]
            self.chosen_image = image6
        elif self.clr == 7:  #
            # o o            o o               o o          o o
            # o x            o x               o x          o x
            #
            self.colOffsets = [[-1, -1, 0, 0], [-1, -1, 0, 0], [-1, -1, 0, 0], [-1, -1, 0, 0]]
            self.rowOffsets = [[0, -1, 0, -1], [0, -1, 0, -1], [0, -1, 0, -1], [0, -1, 0, -1]]
            self.chosen_image = image7
        self.colOffsets = self.colOffsets[self.rot]
        self.rowOffsets = self.rowOffsets[self.rot]
        self.update()

    def rotate_counter_clockwise(self):  # makes it go through backwards instead of forwards
        self.rot = (self.rot - 1) % 4  # 4 possible values for rotation - 0,1,2,3
        self.rotate()

    def draw(self, surface, gridsize):  # draw function
        for block in self.blocks:
            block.draw(surface, gridsize)

    def move_left(self):  # moves shape left
        self.col -= 1
        self.update()

    def move_right(self):  # moves shape right
        self.col += 1
        self.update()

    def move_down(self):  # moves shape down
        self.row += 1
        self.update()

    def move_up(self):  # moves shape up
        self.row -= 1
        self.update()

    def update(self):  # recalculates all the offsets and create blocks with the new attributes
        for i in range(len(self.blocks)):
            blockCOL = self.col + self.colOffsets[i]
            blockROW = self.row + self.rowOffsets[i]
            blockCLR = self.clr
            blockImage = self.chosen_image  # fourth parameter to add in image
            self.blocks[i] = Block(blockCOL, blockROW, blockCLR, blockImage)

    def shadow_create(self):  # Creates the shadow
        self.update()

    def rotate_shadow(self, other):  # Rotates the shadow
        self.rot = (self.rot + 1) % 4
        if other.clr == 1:  # image 1
            self.clr = 1
            self.rotate()
        elif other.clr == 2:  # image 2
            self.clr = 2
            self.rotate()
        elif other.clr == 3:  # image 3
            self.clr = 3
            self.rotate()
        elif other.clr == 4:  # image 4
            self.clr = 4
            self.rotate()
        elif other.clr == 5:  # image 5
            self.clr = 5
            self.rotate()
        elif other.clr == 6:  # image 6
            self.clr = 6
            self.rotate()
        elif other.clr == 7:  # image 7
            self.clr = 7
            self.rotate()


class Floor(Cluster):

    """ Horizontal line of blocks
        data:
            col - column where the anchor block is located
            row - row where the anchor block is located
            blocksNo - number of blocks
        auxiliary data:
            blocksXoffset - list of horizontal offsets for each block, in reference to the anchor block
    """

    def __init__(self, col=1, row=1, blocksNo=1):  # initializes the variables from the Cluster class
        Cluster.__init__(self, col, row, blocksNo)
        for i in range(blocksNo):  # creates floor
            self.blocksXoffset[i] = i
        self.update()


class Wall(Cluster):

    """ Vertical line of blocks
        data:
            col - column where the anchor block is located
            row - row where the anchor block is located
            blocksNo - number of blocks
        auxiliary data:
            blocksYoffset - list of vertical offsets for each block, in reference to the anchor block
    """

    def __init__(self, col=1, row=1, blocksNo=1):   # initializes the variables from the Cluster class
        Cluster.__init__(self, col, row, blocksNo)
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
        fade.fill((255, 255, 255))
        fade.set_alpha(opacity, opacity)  # changes shade of filling
        screen.blit(fade, (self.x, self.y))

    def activate(self, mouse):
        if self.x + self.width > mouse[0] > self.x:  # checks if in x boundary
            if self.y + self.height > mouse[1] > self.y:  # checks if in y boundary
                return True
        return False
