# Klassendiagramm des Backend
*Info zum <ins>Zwischenstand</ins>: User und Player sind aktuell unabhängig voneinander implementiert, sollen aber noch zusammengeführt werden. Player ist daher gleichbedeutend oder als Erweiterung von User zu betrachten.*
```mermaid
classDiagram
    class Application{
        +main()
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

    class AppConfig{
        -initialCapacity: int
        -playerRepository: PlayerRepository
        +userDetailsService()
        +authenticationProvider()
        +authenticationManager()
        +passwordEncoder()
    }

    class AccountController{
        -service: AuthenticationService
        +login(request: LoginRequest)
        +register(request: RegisterRequest)
        +guest(request: LoginRequest?)
    }

    class PlayerRepository{
        +findByEmail(email: String)
    }

    class LoginRequest{
        -email: String
        -password: String
    }

    class RegisterRequest{
        -email: String
        -password: String
    }

    class AuthenticationResponse{
        -token: String
    }

    class AuthenticationService{
        -playerRepository: PlayerRepository
        -passwordEncoder: PasswordEncoder
        -jwtService: JwtService
        -authenticationManager: AuthenticationManager
        +register(request: RegisterRequest)
        +login(request: LoginRequest)
    }

    class JwtAuthenticationFilter{
        -doFilterInternal(request: HttpServletRequest, reponse: HttpServletResponse, filterChain: FilterChain)
    }

    class JwtService{
        -SECRET_KEY: String
        +extractUsername(token: String)
        +extractClaim(token:String, claimsResolver: Function<Claims, T>)
        +generateToken(userDetails: UserDetails)
        +generateToken(userDetails: UserDetails, extractClaims: Map<String, Object>)
        +isTokenValid(token: String, userDetails: UserDetails)
        -isTokenExpired(token: String)
        -extractExpiration(token: String)
        -extractAllClaims(token: String)
        -getSecretKey()
    }

    class SecurityConfig{
        -frontendHost: String
        -frontendBasePath: String
        -jwtAuthFilter: JwtAuthenticationFilter
        -authenticationProvider: AuthenticationProvider
        +corsConfigurationSource()
        +securityFilterChain(http: HttpSecurity)
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
    PlayerRepository ..> Player: use
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
    LobbyManager ..> AppConfig: use
    LobbyManager ..> LobbyCodeGenerator: use
    AppConfig ..> PlayerRepository: use
    
    GameController ..> BuildActionDTO: use
    GameController ..> TradeBankDTO: use
    GameController ..> TradePlayerDTO: use
    GameController ..> PickDicePairDTO: use

    AccountController ..> AuthenticationService: use
    AccountController ..> LoginRequest: use
    AccountController ..> RegisterRequest: use
    AuthenticationService ..> PlayerRepository: use
    AuthenticationService ..> JwtService: use
    AuthenticationService ..> AuthenticationResponse: use
    AuthenticationService ..> AppConfig: use
    SecurityConfig ..> JwtAuthenticationFilter: use
    SecurityConfig ..> AppConfig: use
    JwtAuthenticationFilter ..> JwtService: use
    JwtAuthenticationFilter ..> AppConfig: use
```
