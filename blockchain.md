# Blockchain

Warmancer Black Friday is deliberately designed to showcase meaningful applications of blockchain technology in gaming, extending beyond mere monetization.


## Protocols

Warmancer is intended to be as accessible as possible for players within the Stellar ecosystem, players familiar with DeFi, and players without wallets. To enable cross-platform play and reach the widest possible audience, the Warmancer client and multiplayer API are configured to operate on these three protocols.

### Serverless Protocol (Fully On-Chain)
The Serverless protocol is the strictest option, intended for fully on-chain games between wallet holders. When the Warmancer client is set to this protocol, matchmaking is done by sending challenges on-chain via smart contracts. Players submit moves using smart contracts, and the game log is stored on-chain permanently. As its name implies, the Serverless protocol does not require a matchmaking server, enabling Warmancer Black Friday to be played forever.

- **Matchmaking**: On-chain, wallet-to-wallet.
- **Gameplay**: On-chain, requires transactions.
- **Features**: Requires gas from both players. All features are available, but in-game communication between players (e.g., emotes and chat) requires transaction authorization. NFT assets can be used. Best suited for slower-paced matches, akin to "play-by-email."

### Server Protocol (Fallback)
The Server protocol is designed for players who do not have wallets and are not yet part of the Stellar ecosystem. This protocol serves as the default fallback if a participant in a game does not have a connected wallet. Players send game requests through the Warmancer matchmaking server and play by sending socket requests instead of transactions.

- **Matchmaking**: Off-chain, through the matchmaking server.
- **Gameplay**: Off-chain; actions are securely sent via WebSocket to the matchmaking server.
- **Features**: Does not require gas to play. NFT assets cannot be used. Matches are not logged on-chain, and no persistent user data is stored.

### Hybrid Protocol (Recommended)
The Hybrid protocol is the default for matches where at least one player has a connected wallet. This protocol combines all blockchain features with the advantages of a central server, minimizing the number of transactions required for gameplay.

- **Matchmaking**: On-chain or off-chain.
- **Gameplay**: On-chain for record-keeping and verification; off-chain during gameplay.
- **Features**: Requires gas for recording and verifying matches. NFT assets can be used by players with connected wallets. Suitable for most situations.

## NFT Assets
Warmancer Black Friday will be launched alongside a collection of NFT assets created by the game's artists. In addition to being genuinely cool and unique art pieces, these assets can be used to add playable custom skins in-game.


## Smart Contracts

Warmancer leverages smart contracts to ensure secure, transparent, and verifiable gameplay while adhering to blockchain principles. When played fully on-chain, the smart contract manages all game logic, from player registration to move validation and game recording. Below is a brief explanation of the core transaction types:

- **Registration Transaction**:  
  Players register a username linked to their wallet in a one-time transaction. The smart contract writes user data to the blockchain, associating the username with the wallet.

- **Data Transfer Transaction**:  
  Players can transfer their game data to another wallet, such as when switching accounts. The smart contract securely updates the user data with the new wallet address.

- **Send Challenge Transaction**:  
  A host sends a game invitation to an opponent’s wallet or username, including the game setup parameters. The smart contract writes the challenge data and triggers a push notification to alert the opponent.

- **Accept Challenge Transaction**:  
  The opponent confirms the game invitation. The smart contract initializes the game using the setup parameters and emits an event to both players with the game details.

- **Submit Setup Hash Transaction**:  
  Players submit a hashed version of their pawn setup data to confirm that their setup is complete. The smart contract stores the hash and waits for both players to submit. Once both submissions are received, the contract emits an event to both players to start the game.

- **Move Hash Transaction**:  
  Players submit a hashed version of their planned move. The smart contract stores the hash and waits for both players to submit. Once both hashes are received, the contract emits an event for players to reveal their moves.

- **Move Reveal Transaction**:  
  After the move hash event, players reveal their moves and accompanying salt. The smart contract verifies the revealed data against the stored hash and applies both moves to the game state. If a conflict arises (e.g., two pawns move to the same tile), the contract emits an event requesting the ranks of the conflicting pawns. If no conflict exists but a win condition is detected, the contract emits an event for players to submit their initial pawn setup data. Otherwise, it emits resolution data to update the game state.

- **Conflict Reveal Transaction**:  
  When a conflict event is triggered, players submit the ranks of all pawns involved. The smart contract verifies the ranks and resolves the conflict according to the game rules. It then emits resolution data to both players.

- **End Game Transaction**:  
  When a game ends, players submit their initial pawn setup data and salt. The smart contract verifies the submitted data against the initial hashes to confirm the validity of the entire game. Once verified, the contract writes the game history and updates player statistics. An event is sent to both players with the final results.


## Smart Contract Procedure
<details>
<summary> Click to see procedure </summary>

```
	Register tx:
		User registers a username to their wallet as a one time tx
		Contract writes user data
	Transfer tx:
		User transfers data to another wallet
		Contract writes new user data
	Send challenge tx:
		Host sends a invite to opponent wallet or username containing game setup parameters
		Contract writes challenge
		Opponent is alerted via push notification
	Accept challenge tx:
		Opponent sends a confirmation message
		Contract starts game from setup parameters 
		Contract sends event to both players containing game setup parameters
	Submit setup hash tx:
		Player submits hash of their pawn setup data confirming that they are done placing pawns
		Contract writes hash
		Contract waits for both players to submit
		Contract sends event to both players to start game
	Move hash tx:
		Player submits hash of their move
		Contract writes hash
		Contract waits for both players to submit
		Contract sends event to both players to reveal moves
	Move reveal tx:
		On move hash event, Player submits move + salt
		Contract waits for both players to submit
		Contract checks hash and applies both moves
		If conflict, Contract sends event to both players to submit ranks of pawns involved
		Else if game end, Contract sends event to both players to submit initial pawn setup data
		Else Contract sends event to both players with resolution data
	Conflict reveal tx:
		On conflict event, Player submits ranks for all pawns involved
		Contract waits for both players to submit
		Contract sends event to both players with resolution data
	End game tx:
		On game end event, Player submits initial pawn data setup + salt
		Contract waits for both players to submit
		Contract checks hash and verifies entire game
		Contract writes game history and player history
		Client sends event to both players
```
</details>