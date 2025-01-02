# Glossary

## Lobby
A lobby is a pre-match state that bridges players before a game begins. On the server side, it represents an unfinalized match, holding two players and match parameters, while on the client side, it serves as a waiting area where players can view match parameters, ready up, and chat.

## Match
A match is a gameplay session with two players. Matches end when at least one player has been defeated. 

## Board
A board is the set of tiles that a match is played on. Boards are defined with simple JSON. Custom board layouts can be imported and played in multiplayer. Boards can contain square or hexagonal tiles.
## Tile
A tile is a cell that can hold one pawn. Tiles can be traversable or impassable. A tile's setupTeam property determines if a pawn can be placed on it during the setup phase. Tiles can be square or hexagonal.

## Pawn
A pawn is a movable agent belonging to a team. Pawns have a combat power property that determines its strength and a rank. 

## Rank
A rank determines a pawn's base combat power and any special properties it may have. 

### 0. Throne

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/0 throne red.png" alt="Red Throne" style="width: 10%;"/>
  <img src="./assets/pawns/0 throne blue.png" alt="Blue Throne" style="width: 10%;"/>
</div>

- Combat Power: 0
- Count: 1
- Immobile
- If the Throne is destroyed, the player is defeated and the game ends.

### 1. Assassin

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/1 assassin red.png" alt="Red Assassin" style="width: 10%;"/>
  <img src="./assets/pawns/1 assassin blue.png" alt="Blue Assassin" style="width: 10%;"/>
</div>

- Combat Power: 1, or 11 vs Warlord
- Count: 1
- The Assassin is the only pawn besides Traps capable of defeating the War Lord.

### 2. Scout

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/2 scout red.png" alt="Red Scout" style="width: 10%;"/>
  <img src="./assets/pawns/2 scout blue.png" alt="Blue Scout" style="width: 10%;"/>
</div>

- Combat Power: 2
- Count: 8
- Can teleport to tiles in a straight line like a rook, but is blocked by allied pawns and immobile tiles.
- If opposing scouts teleport to the same position, both are destroyed (telefragged!)

### 3. Seer

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/3 seer red.png" alt="Red Seer" style="width: 10%;"/>
  <img src="./assets/pawns/3 seer blue.png" alt="Blue Seer" style="width: 10%;"/>
</div>

- Combat Power: 3
- Count: 5
- Can disarm traps, removing them from play

### 4. Grunt

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/4 grunt red.png" alt="Red Grunt" style="width: 10%;"/>
  <img src="./assets/pawns/4 grunt blue.png" alt="Blue Grunt" style="width: 10%;"/>
</div>

- Combat Power: 4
- Count: 4
- Unremarkable, but useful as fodder

### 5. Knight

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/5 knight red.png" alt="Red Knight" style="width: 10%;"/>
  <img src="./assets/pawns/5 knight blue.png" alt="Blue Knight" style="width: 10%;"/>
</div>

- Combat Power: 5
- Count: 4
- More powerful than Grunts

### 6. Wraith

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/6 wraith red.png" alt="Red Wraith" style="width: 10%;"/>
  <img src="./assets/pawns/6 wraith blue.png" alt="Blue Wraith" style="width: 10%;"/>
</div>

- Combat Power: 6
- Count: 4

### 7. Reaver

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/7 reaver red.png" alt="Red Reaver" style="width: 10%;"/>
  <img src="./assets/pawns/7 reaver blue.png" alt="Blue Reaver" style="width: 10%;"/>
</div>

- Combat Power: 7
- Count: 3

### 8. Herald

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/8 herald red.png" alt="Red Herald" style="width: 10%;"/>
  <img src="./assets/pawns/8 herald blue.png" alt="Blue Herald" style="width: 10%;"/>
</div>

- Combat Power: 8
- Count: 2

### 9. Champion

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/9 champion red.png" alt="Red Champion" style="width: 10%;"/>
  <img src="./assets/pawns/9 champion blue.png" alt="Blue Champion" style="width: 10%;"/>
</div>

- Combat Power: 9
- Count: 1
- Can only be defeated by Traps and War Lords

### 10. War Lord

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/10 warlord red.png" alt="Red Warlord" style="width: 10%;"/>
  <img src="./assets/pawns/10 warlord blue.png" alt="Blue Warlord" style="width: 10%;"/>
</div>

- Combat Power: 10
- Count: 1
- Most powerful mobile pawn. Can only be defeated by Traps and Assassins.

### 11. Trap

<div style="display: flex; justify-content: flex-start;">
  <img src="./assets/pawns/11 trap red.png" alt="Red Trap" style="width: 10%;"/>
  <img src="./assets/pawns/11 trap blue.png" alt="Blue Trap" style="width: 10%;"/>
</div>

- Combat Power: 13
- Count: 6
- Immobile
- Stationary tile hazards that consume all that fall into it's maw. 