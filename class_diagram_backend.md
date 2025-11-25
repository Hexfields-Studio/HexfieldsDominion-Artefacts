# Klassendiagramm des Backend
```mermaid
classDiagram
    class Application{
        +main()
    }

    class AppConfig{
        -initialCapacity: int
    }



    class LobbyManager{
        -occupiedLobbies: HashMap<String, Lobby>
        -freeLobbies: Lobby[]
        +LobbyManager(config: AppConfig)
        +createLobby(configs: String[]): String
        +joinLobby(lobbyCode: String, res: Map<String, Object>): boolean
    }

    class Lobby{
        +players: Player[]
        +addPlayer(player: Player)
    }
    
    class Match{
        
    }

    class AccountController{
        -login()
        -register()
        -guest()
    }

    class LobbyController{
        -lobbyManager: LobbyManager
        +LobbyController(lobbyManager: LobbyManager)
        +createLobby(configs: CreateLobbyDTO): ResponseEntity<Map<String, String>>
        +joinLobby(lobbyCode: String): ResponseEntity<Map<String, Object>>
		
    }

    class LobbyCodeGenerator{
        -CHARACTERS: String
        -CODE_LENGTH: int
        -random: Random
        +generateCode(): String
    }

	class StructureType{
	<<enumeration>>
	TOWN
	HARBOUR
	}

	class RessourceType{
	<<enumeration>>
	HierDynamischMitDatenAusDBFüllen
	}

    class PlayerActionType{
        <<enumeration>>
        BUILD
        TRADE_BANK
        TRADE_PLAYER
        PICK_DICE_PAIR
    }

    class PlayerActionDTO{
        -type: PlayerActionType
	    -sessionId: String
    }

	class CreateLobbyDTO{
		-configs: String[]
    }

	%%{"type": "BUILD", "sessionId":..., "pos": [[0,0],[1,0]], "structure": "HARBOUR"}
    class BuildActionDTO{
		-pos: Pair[]
		-structureType: StructureType
	}

	class TradeBankDTO{

	}

	%%{"type":.., "sessionId":..., "destPublicId":..., "offered": [{"ressource": "STONE", "amount": 2}, {"ressource": "WOOD", "amount": 1}], "requested": [{"ressource": "GOLD", "amount": 1}]}
	class TradePlayerDTO{
		-destPublicId: String
	}

	class PickDicePairDTO{
		-index: int
    }

    class GameController{
        -playerAction(request: PlayerActionDTO)
        -endTurn()
        -buildStructure(dto: BuildActionDTO)
        -tradeWithBank(dto: TradeBankDTO)
        -tradeWithPlayer(dto: TradePlayerDTO)
        -pickDicePair(dto: PickDicePairDTO)
    }

    class Player{
		-id: int
        -username: String
        -isAccount: boolean
    }

    class Structure{
        -name: String
        -pos: Pair<Integer, Integer>[]
        -recipe: Map<RessourceType, Integer>
    }

    class Ressource{
        -name: String
    }

    class Field{
		-pos: Pair<Integer, Integer>
        -number: int
        -ressource: Ressource
    }
    
    class PlayerRepresentation{
		-player: Player
        -publicId: int
        -sessionId: String
		-color: Color
        -ressources: Map<Ressource, Integer>
    }

    LobbyController --o "1" LobbyManager : lobbyManager
    LobbyManager --> "0..*" Lobby: freeLobbies
    LobbyManager --> "0..*" Lobby: occupiedLobbies
    Match "0..1" --* Lobby
    Lobby --> "1..*" Player
    Structure "0..*" --* Match: structures
    Match --* Field
    Match --* "2..*" PlayerRepresentation: players
    PlayerRepresentation --> "0..1" Player: player
    Ressource "0..*" --* PlayerRepresentation: ressources
    Field --> "1" Ressource: ressource
    Structure --> "1" Ressource: ressourceRecipe
    GameController --> "0..*" Match: matches
    BuildActionDTO --|> PlayerActionDTO
    PickDicePairDTO --|> PlayerActionDTO
    TradeBankDTO --|> PlayerActionDTO
    TradePlayerDTO --|> PlayerActionDTO
    PlayerActionDTO ..> PlayerActionType: use
    BuildActionDTO ..> StructureType: use
    TradeBankDTO --* "1" RessourceType: requested
    TradeBankDTO --* "1" RessourceType: offered
    TradePlayerDTO --* "1..*" RessourceType: requests
    TradePlayerDTO --* "1..*" RessourceType: offers
	LobbyManager ..> CreateLobbyDTO: use
	AccountController ..> Player: use
    LobbyManager ..> AppConfig: use
    LobbyManager ..> LobbyCodeGenerator: use
    
    GameController ..> BuildActionDTO: use
    GameController ..> TradeBankDTO: use
    GameController ..> TradePlayerDTO: use
    GameController ..> PickDicePairDTO: use

```
