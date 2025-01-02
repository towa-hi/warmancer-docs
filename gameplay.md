# Gameplay
Warmancer Black Friday is a turn-based strategy board game based around the concept of imperfect information where two players compete to dominate the battlefield with their hidden pawns and destroy each other's thrones.

## Game Parameters
The creator of the challenge can custom rules for their game.
- **Custom board**:
Challenger can select which board they want to play the game on.
- **Casual game**:
When true, results of the game are not recorded and encryption is disabled when on chain. Reduces the number of transactions to once per turn at the cost of security.
- **Buy pawns on setup**:
When true, players are given a set budget to buy the pawns they wish to use leading to a more asymmetrical gameplay experience.
- **Always Hidden pawns**:
When true, surviving pawns are not revealed after battle. Disables battle cutscenes.
- **Sequential moves**:
When true, players take turns making moves instead of simultaneously resolving moves.


## Game Flow

A match is structured into distinct phases: Pre-Game, Setup Phase, Move Phase, Resolve Phase, and End Phase on the client side. In this context, the term "Server" is interchangeable with "Smart Contract," as the client-facing API is designed to function the same for both.

### Pre-Game

Before the match begins, the Challenger sets the game parameters, such as the board and special rules. Once configured, the Challenger sends an invitation to the Defender through the Server, which relays the invitation. When the Defender receives and accepts the invite, the Server acknowledges their confirmation, and both players are brought into the game lobby to review the game settings.

In the lobby, both players indicate their readiness by clicking a "Ready" button. Once both are ready, the Server uses the predefined parameters to initiate the game. The initial game state is sent to both players, marking the transition to the Setup Phase.

### Setup Phase

During the Setup Phase, both players arrange their pawns on their respective sides of the board. All pawns must be placed before proceeding. Once satisfied, players submit a hash of their configurations to the Server, serving as an encrypted record of the initial positions and ranks of their pawns to ensure game integrity.

After receiving both players' hashed configurations, the Server generates a neutral game state. This state includes an array of pawns identified by their IDs, initial positions, and hashed ranks. The neutral game state is then sent to both players in preparation for the Move Phase.

### Move Phase

In the Move Phase, players simultaneously plan their moves. Each player selects one pawn and chooses a valid destination tile. Normal pawns move one tile orthogonally, while scouts can move multiple tiles in a straight line, similar to a rook in chess. Pawns cannot cross or land on impassable tiles. They may occupy tiles with opponent pawns, triggering an attack, but cannot land on tiles occupied by their own pawns. Players with no valid moves must resign.
After choosing a move, each player submits a hashed version of their move to the Server. When both moves are received, the Server requests the keys to decrypt the move details, processes the moves, and resolves any conflicts. Conflicts arise when two pawns attempt to occupy the same tile or move into each other’s initial positions.

Conflict resolution follows these rules:
- Moves to unoccupied positions are executed first.
- If both players move to the same tile, a battle is triggered. The higher-ranked pawn wins and occupies the tile, while the losing pawn is removed.
- If pawns swap positions by moving into each other’s starting tiles, a battle determines the winner, who claims the losing pawn’s position.
- Moves into occupied tiles result in battles, with the winner claiming the tile and the loser removed.
- Battles are resolved before finalizing moves to ensure no two pawns share the same tile.

After resolving moves and conflicts, the Server checks for win conditions. A player wins if their opponent’s heart pawn is destroyed or if the opponent cannot make valid moves. A tie occurs if both hearts are destroyed simultaneously or if neither player has valid moves. The Server records the sequence of operations (e.g., MOVE, REVEAL, BATTLE, KILL) required to update the board and sends this data to both players. If a win condition is met, the Server requests the key to decrypt the initial setup configurations.

### Resolve Phase

In the Resolve Phase, players watch the recorded sequence of operations play out on their boards. Pawns move, battles occur, and defeated pawns are removed automatically. This phase provides a visual summary of how the moves and battles unfolded. If no win condition is met, the game transitions back to the Move Phase for the next turn. If a winner is declared or the game ends in a tie, the game proceeds to the End Phase.

### End Phase

In the final phase, players submit the keys to their original setup configurations, allowing the Server to verify the game’s integrity. The Server replays the game step by step, ensuring all moves and outcomes align with the original setups and rules. Once verified, the game history is recorded, and player data, such as stats or rankings, is updated.

Players are presented with a detailed end-game summary, including a replay option to analyze the match move by move. The match is stored locally for future reference. Finally, players are returned to the main menu, where they can choose to start a new game or explore other options.
