# Application of Reinforcement Learning to a Simplified Two-Player version of Dhumbal


## Introduction

__Dhumbal__ is a Nepalese card game. It is a draw and discard game in which players discard before drawing a new card and attempt to have the lowest value of cards in hand. The game requires a minimum of two players but is typically played between a group of two to five players.

For the purpose of the current research, we will fix the number of players to two, and use the variant of game with the rules that are considerably simplified compared to the original game.

### Gameplay

The game is played with a 52-card deck composed of standard playing cards. The game is divided into multiple rounds, with a total score tally kept between rounds. Each card in the deck is assigned a value. An ace is worth a single point while cards two through ten are worth their face value. Jack, Queen and King are worth 11, 12, and 13 points, respectively.

Each round in the game ends when one of the players calls. You can only call when your score is equal or below 5. Each player's score is calculated from their remaining cards. The player with the lowest score wins the round and receives no points for the round. Other players record their corresponding scores for the round.

If you call while the your score is above 5, the round immediately ends. In that case, you record your current score plus extra 30 points of penalty, the rest of the players receive a score of 0.

When a player's point total (the sum of the totals for each round) crosses a set threshold of 80 points, that player is eliminated from the game. Once all players but one have been eliminated, the remaining player is declared the winner.

### Structure of a round

At the beginning of a round, each player in the game is dealt five cards face down.

The starting player in the first round is chosen at random.

Players have two options for their turn: They may either play one or more cards or call. When playing cards, the player may discard a single card or a single set of cards, placing them into the discard pile. The player must then draw a card from the draw pile. Alternatively, the player may choose to take the card played by the previous player from the discard pile.

If the drawing deck is empty and no one has yet called, then all cards of the free stack, excluding the last player's discard, are shuffled and placed face down as a new deck.

A player may discard any of the following sets of cards:
- Any single card
- Two or more cards of the same rank
- Three or more cards of consecutive ranks in the same suit (An ace can be used before a 2, but not after a king).


## Approach

### State space

In the games with imperfect information each player is not aware of the cards that are held by the other players. In reality, players would be able to memorize at least some of the cards previously discarded/drawn by them or their opponents. To account for that, we would need to include this information into the current game state. However, for the scope of this research we will pretend players have no memory of the previous events.

The naive approach to modeling the state would be directly encoding the cards in the player's hand. This, however, would lead to an extremely large state space as there exist 2893163 possible player hands, and we would still need to account for all the additional information which would multiply this number manyfold.

Instead, we will design an input state that consists of a small number of hand-selected features, namely:

- __Number of cards you have__ (1..5);
- __Number of cards the opponent has__ (1..5);
- __The combined score of your hand__ (1..64).

This approach reduces the state space to 1600 possible states.

### Action space

We will apply the same approach for the possible actions, namely, we will consider only 3 possible actions:

- __GREEDY_DECREASE:__ Discard the set with the highest score and pick up the card from the previous player with the lowest score;

- __DELAY_COLLECT:__ Discard the set with the highest score; but only consider sets that have score higher than the maximum score of the set that can be obtained after picking the card discarded by the opponent; pick the card discarded by the opponent so to maximize the set that can be discarded at the next turn;

- __CALL:__ Call and end the round.

### Learning Algorithm

TODO:

- Q-learning?

- Monte-Carlo to learn [(state, action) -> state'] transition probabilities (under current policy)? That would allow evaluating policy and do policy iteration. The would also allow doing value iteration. State space is reasonably small to do DP.


TODO: virtual environment to simulate the game

TODO: explore vs exploit
TODO: hyperparameters


## Results

TODO: