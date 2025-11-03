# Klassendiagramm des Backend
```mermaid
classDiagram
    class Application{
        +main()
    }

    class LobbyManager{
        +createLobby()
    }

    class Lobby{

    }
    
    class Match{
        
    }

    class AccountController{
        -login()
        -register()
        -guest()
    }

    class LobbyController{
        -createLobby()
        -joinLobby()
    }

	class StructureType{
	<<enumeration>>
	TOWN
	HARBOUR
	}

	class RessourceType{
	<<enumeration>>
	DynamischErzeugenHier
	}

    class PlayerActionType{
        <<enumeration>>
	BUILD
	TRADE_BANK
	TRADE_PLAYER
	PICK_DICE_PAIR
    }

    class PlayerActionDTO{
        -PlayerActionType type
	    -String sessionId
    }

	%%{"type": "BUILD", "sessionId":..., "coords": [[0,0],[1,0]], "structure": "HARBOUR"}
    class BuildActionDTO{
		-Pair[] coords
		-StructureType structureType
	}

	class TradeBankDTO{

	}

	%%{"type":.., "sessionId":..., "destPublicId":..., "offered": [{"ressource": "STONE", "amount": 2}, {"ressource": "WOOD", "amount": 1}], "requested": [{"ressource": "GOLD", "amount": 1}]}
	class TradePlayerDTO{
		-String destPublicId
		-List offered
        -List requested
	}

	class PickDicePairDTO{

    }

    class GameController{
        -playerAction(PlayerActionDTO request)
        -endTurn()
        -buildStructure(BuildActionDTO dto)
        -tradeWithBank(TradeBankDTO dto)
        -tradeWithPlayer(TradePlayerDTO dto)
        -pickDicePair(PickDicePairDTO dto)
    }

    class Player{
	-int id
        -String username
        -boolean isAccount
    }

    class Structure{
        -String name
        -Pair[] coords
    }

    class Ressource{
        -String name
    }

    class Field{
        -int number
    }
    
    class PlayerRepresentation{
	-int publicId
        -String sessionId
	-Color color
    }

    LobbyController --o "1" LobbyManager : lobbyManager
    LobbyManager --> "0..*" Lobby: freeLobbies
    LobbyManager --> "0..*" Lobby: occupiedLobbies
    Match "0..1" --* Lobby
    Lobby --> "1..*" Player
    Structure "0..*" --* Match
    Match --* Field
    Match --* "2..*" PlayerRepresentation
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

    GameController ..> BuildActionDTO: use
    GameController ..> TradeBankDTO: use
    GameController ..> TradePlayerDTO: use
    GameController ..> PickDicePairDTO: use

```
