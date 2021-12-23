# a2
# Part 2: The Game of Quintris:
Genetic Algorithm: Evolving promising states

Approach:
1. We were given a quintris game of 25 rows and 15 columns which should return a string of the possible moves ("b" -> left, "n" -> rotate, "m" -> right, "h" -> horizontal flip). We have two versions given - human and simple. I started with simple version.
2. After going through the existing code (functions to rotate, flip, move left, right and down) and brainstorming with my teammates, I understood that we can reuse the rotate and flip functions to obtain the possible versions of the falling piece. Confirmed the same in the class. For each piece, I'm creating a dictionary that has number of rotations/flip as keys and rotated/flipped verison of the piece as value.
3. Now that I have the initial state in place, I brainstormed with my teammates about the possible heuristics that defines the goodness in a tetris or quintris game. We listed out 4 main heuritics: # of empty gaps that form below an element of the piece("x"), # of rows that have all x's i.e., no empty gap in the row, column heights of each column (i.e., first "x" position from the top in the board for all 15 columns), uneveness of the top part of the board. From these 4 functions, I remember myself trying to avoid making gaps in between, making the tallest block sit on top, and trying to finish off rows by filling rows with all x's. I wrote these heuristic functions in python using the same logic.
4. Now that we have heuristics and possible versions of the falling piece, all we have to do is move the piece left or right based on the above heuristics to form a set of successors. To form the successors and the movements(left/right), I've written two loops that move left and right till it reaches the ends. While moving left/right, I checked if there is any collision with the existing pieces in the board once I drop the piece. If there is a collision, its not a successor and vice-versa. I created a successor list using this logic which saves the movement (rotated/flipped/actual + number of left/right movements) and the dummy board that is obtained after the move is made and dropped in that particular position. 
5. Now that we have successors, we just pass each successor to the heuristic function to obtain gaps, rows formed, column height and uneveness. 
6. Now that we have abstraction in place, we used genetic algorithm. As a first step, I ran 10 games each with one set of random weights (for gaps, rows formed, column height and uneveness to obtain the heuristic score). Later, I ran another 10 games with a new set of random weights, so on. Similarly, I do this for 150 iterations. The top 50% of iteration with high median(to avoid skewness towards min and max score) scores are segregated and the bottom 50% are killed. Using these top 50%, I came up with new children by taking a mean of weights of two parents. Once we have enough population, I ran the whole above process again.This was run for multiple times - 10 times.
7. Finally, I took the set of weights with high median score after 10 final iterations and used these weights in the actual heuristic function.
8. The algorithm returns the movement of the piece of the successor with high heuristic score(assigned negative weights for gaps, height and uneveness and positive weight for rows formed).
9. I observed the range of actual quintris score with a minimum of 15, maximum of 608, average of 91, and a median of 87.

## Difficulties faced:
1. It took quite a while to understand the existing code of Simple and Animated Quintris.
2. The existing right movement function was not letting me to move right till the end and hence this was not giving me all the successors. Because of this, the game was ending soon with zero score. So I debugged in a top to bottom fashion to identify and rectify this. Inorder to rectify this, I've written a code that returns the index with the minimum of piece value column + # of steps and 14(# of columns) and then, I check if there is any collision and if not, its a successor.
3. I faced issues while running multiple games for a certain set of weights. Later I figured out the issue in the loop and reran it.

