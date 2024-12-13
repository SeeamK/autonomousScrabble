# Scrabble Maximizing Agent using MCTS

## Motivation and explanation for project
The goal of this project was to use Monte Carlo Tree Search to make a Scrabble agent that simulates and chooses the best move, maximizing its score. Scrabble seemed like an interesting challenge as there are a lot of different things that nned to be tracked and taken into account. We ran into some issues with the MCTS implementation, and as a result it doesnt maximize its score all the time. This repo includes two versions of the project:
  - scrabbleGreedy.py: a working version using a basic algortihtm, filtering out words based on the ai agents rack and available anchor points on the board. It then chooses the word that gives the highest score. This implementation always maximizes the ai agents score.
  - scrabbleMCTS.py: the attempted MCTS implementation. It works sometimes, but doesnt others. The ai agent always finds a word to play (if one exists), but its not guranteed to be the maximizing word. The agent also ignores the rule that its word should build off a letter already on the board, this is becuase before each simulation, a deep copy of the board is made. This deepcopy has words the ai agent has simulated before, so the final word it chooses builds off one of the existing letters from the simulated board, not the actual game state.

## Links to any data sources:
The base Scrabble game (with no autonomous agent) was taken from an existing github: https://github.com/fayrose/Scrabble/blob/master/scrabble.py

## Software and hardware requirements:
Please ensure that you have `python` in your `PATH`, and that it refers to Python 3.5 or higher.

## Explanation of what we accomplished
Both versions build off the existing scrabble game mentioned above.
scrabbleMCTS.py:
  - There two added classes, node and MCTS
  - node is a simple tree node containing the game_state: The state of the game at this point, move: The move taken to reach this state, parent: The parent node in the tree, children: A list of child nodes (possible states reached from the current node), visits: The number of times this node has been visited during the search, and win_score: The cumulative score accumulated from simulations passing through this node.
  - node also has methods to check if all moves from the current node have been explored, generate all possible moves from that node, and to select the best child based on the UCT function.
  - The MCTS class has selection, expansion, simulation, and backpropgation functions.
    - Selection: goes through the tree and selects best node based on UCT
    - Expansion: If the current node isnt fully expanded, it genreates a new child node, then simulates a move for it
    - Simulation: Simulates a game till terminal state (score reached or rack and bag are empty) and returns the an evaluation of the state
    - backpropgation: after the evaluation in simulation step, the result is sent back up the tree updating every node along the path
  - We use functions filterByRack() and findAnchorPoints() to find the valid words to randomly simulate on.
scrabbleGreedy.py:
  - The main class added here is the aiAgent class
  - simulateMove(): checks the validity of a word generated
  - generateMoves(): uses the same filterByRack() and findAnchorPoints() mentioned earlier to find words that can be used. We then itertate over the filtered words and check them over the different positions and orientations
  - chooseBestMove(): uses words from generateMoves() and chooses highest scoring one with simulateMove()
For both versions, the turn() function was altered so that the player named "AI" uses the proper autonomous agent. 

## How we measured success (or failure)
  - Success was measured by playing multiple games. Each of us (team of 3) played a minimum of 5 games to see if the autonomous agent won everytime.
  - As mentioned earlier, the MCTS agent doesnt win everytime, but the greedy agent does.
  - The MCTS agent also takes a lot more time than the greedy agent to make a decesion, sometimes taking up to 2min.
  - We ended up measuring failure for the MCTS agent since it didnt work, we found that arounf half the time it would win and the other half it wouldn't.

## To run the game (part of description taken from original github)
To play any version of the game:
  1) run `python scrabbleGreedy.py` or `python scrabbleMCTS.py`
  2) when prompted enter 2 as the number of players
  3) enter a name for player 1, and give player 2 the name 'AI'
  4) the game will then start
From here, you will be able to see the scrabble board. The board has 15 rows, and 15 columns, each numbered from 0 to 14. 
The round number, current player's turn and the current player's rack of letters (each player's rack is made up of 7 tiles) will be displayed. You will then be able to create a word from these tiles and place it upon the board. There will also be a bag of 100 starter tiles in the background, which will replenish each player's rack of tiles as you use them. The first turn made by the first player *must* place their word on the center of the board: 7, 7. Afterwards, the player will be asked whether they would like their word to go down or right. Finally, assuming that all information was inputted correctly, and that the word is in the official scrabble dictionary, the user's total score will be displayed, as well as the new board, commencing the next player's turn.

Note: After the first turn, all plays must have at least one letter that interlock with previous words. 

If there are no words that you can think to create using your current tile rack, simply submit a blank word, enter a random (but valid) column and row number, and a direction. You'll then be prompted to skip. However, after 6 skipped turns in a row (as a whole, not by one particular player), the game will come to an end. The game will also end if the bag runs out of tiles and a player has run out of tiles. 
