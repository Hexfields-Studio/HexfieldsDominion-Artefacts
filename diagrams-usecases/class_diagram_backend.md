# Klassendiagramm des Backend
```mermaid
classDiagram
    class HexfieldsDominionApplication{
        +main()
    }

    class RootController{
        +root()
    }

    class SseSender~T~{
        <<abstract>>
        #groupsEmitters: Map
        #createEmitter(username: String, group: T)
        #sendEvent(emitters: Map<String, SseEmitter>, name: String, data: Object, group: T)
        #allEmitters(group: T)
        #allEmittersExcept(group: T, user: User)
        #emittersOfOnly(group: T, user: User)
        -unsubscribe(group: T, username: String)
        #onUnsubscribe(group: T, username: String)
        +[abstract] subscribe(group: T, username: String)
    }

    %% lobby
    class Lobby{
        -heartbeatHandler: HeartbeatHandler
        -players: List<Player>
        -hasAccountPlayer: boolean
        -nextPlayerId: int
        -lobbyCode: String
        -match: Match
        -owner: String
        +addPlayer(user: User, lobbyManager: LobbyManager)
        +removePlayer(username: String)
        +removePlayer(id: int)
        +isOwner(username: String)
        +onNoHeartbeat(lobby: Lobby, playerId: int)
    }

    class LobbyCodeGenerator{
        -CHARACTERS: String
        -CODE_LENGTH: int
        -random: Random
        +generateCode()
    }

    class LobbyController{
        -lobbyManager: LobbyManager
        +createLobby(dto: CreateLobbyDTO)
        +joinLobby(lobbyCode: String)
        +doesLobbyWithCodeExist(lobbyCode: String, response: HttpServletResponse)
        +heartbeat(lobbyCode: String, dto: HeartbeatDTO, response: HttpServletResponse)
        +lobbyEvents(lobbyCode: String)
        +match(lobbyCode: String)
    }

    class LobbyManager{
        -occupiedLobbies: HashMap<String, Lobby>
        -freeLobbies: List<Lobby>
        +createLobby(configs: String[], owner: String)
        +joinLobby(lobbyCode: String, User user)
        +findOccupiedLobbyOrThrow(lobbyCode: String)
        -checkLobbyCleanup(lobbyCode: String, lobby: Lobby)
        -notifyLobbyUpdate(lobby: Lobby)
        +createMatchForLobby(lobby: Lobby, user: User)
        +findLobbyByMatch(matchUUID: UUID)
        +subscribe(lobbyCode: String, username: String)
        #onUnsubscribe(lobbyCode: String, username: String)
        +onNoHeartbeat(lobby: Lobby, playerId: int)
    }

    %% lobby/dto
    class CreateLobbyDTO{
		-configs: String[]
    }

    class HeartbeatDTO{
        -playerId: int
    }

    class LobbyDTO{
		-lobbyId: int
        -players: Player[]
    }

    %% lobby/error
    class InvalidRadiusException{
    }
    
    class LobbyNotFoundException{
    }

    class NotOwnerOfLobbyException{
    }

    %% lobby/heartbeat
    class HeartbeatHandler{
        -lobby: Lobby
        -heartbeatCheckIntervalSeconds: long
        -executorService: ExecutorService
        -playerIdsExecutors: Map<Integer, HeartbeatExecutor>
        +registerNoHeartbeat(player: Player, listener: NoHeartbeatListener)
        +resetTimer(playerId: int)
    }

    class NoHeartbeatListener{
        <<Interface>>
        +onNoHeartbeat(lobby: Lobby, playerId: int)
    }

    %% game
    class AxialPosition{
        -q: int
        -r: int
        +of(q: int, r: int)
    }

    class BuildingABuildingValidator{
        -corners: Set
        -cornerOffsetToAdjacentFields: List
        -edges: Set
        -edgeOffsetToAdjacentField: List
        -edgeNeighboursOffsets: Map
        +validate(user: User, match: Match, buildActionDTO: BuildActionDTO)
        -canUpgradeASettlementHere(user: User, match: Match, buildActionDTO: BuildActionDTO)
        -playerHasEnoughResourcesToBuild(user: User, match: Match, buildActionDTO: BuildActionDTO)
        -canPlayerBuildHere(user: User, match: Match, buildActionDTO: BuildActionDTO)
        -getNeighboursToCorner(match: Match, buildActionDTO: BuildActionDTO)
        -getNeighboursToEdge(match: Match, buildActionDTO: BuildActionDTO)
        -isBuildingSpotFree(match: Match, buildActionDTO: BuildActionDTO)
        -precomputeUniqueEdges(fields: List)
        -getSortedPosition(pos: List)
    }

    class GameController{
        -lobbyManager: LobbyManager
        -gameManager: GameManager
        -lobby(gameUUID: UUID)
        +fields(gameUUID: UUID)
        +recipes()
        +gameEvents(gameUUID: UUID)
        +rollDice(gameUUID: UUID)
        +endTurn(gameUUID: UUID)
        +grantedResources(gameUUID: UUID, response: HttpServletResponse)
        -playerAction(gameUUID: UUID, request: PlayerActionDTO)
    }

    class GameManager{
        -DICE_MIN_VALUE: int
        -DICE_MAX_VALUE: int
        -POINTS_REQUIRED_TO_WIN: int
        -lobbyManager: LobbyManager
        +rollDice(gameUUID: UUID, user: User)
        +nextPlayersTurn(gameUUID: UUID, user: User)
        +addPoints(match: Match, user: User, points: int)
        +handlePlayerAction(gameUUID: UUID, user: User, request: PlayerActionDTO)
        +buildBuilding(user: User, match: Match, buildActionDTO: BuildActionDTO)
        +getGrantedResources(gameUUID: UUID, user: User)
        +subscribe(gameUUID: UUID, username: String)
        +onUnsubscribe(matchUUID: UUID, username: String)
        -sendMatchData(emitters: Map, match: Match)
        -sendPlayerTrades(emitters: Map, match: Match)
    }

    class Match{
        -AMOUNT_GRANTED_RESOURCES_PER_STRUCTURE_AND_FIELD: int
        -uuid: UUID
        -gameBoard: GameBoard
        -players: GamePlayers
        -grantedResourcesThisTurn: Map
        -currentDiceResult: Integer[]
        -rolledDiceThisTurn: boolean
        -validator: BuildingABuildingValidator
        -tradingHandler: TradingHandler
        +nextPlayersTurn()
        -grantInitialResources()
        +grantResourcesForDiceResult(diceResult: int)
        -setOrAddResource(resources: Map, fields: Field)
        +buildBuilding(player: PlayerRepresentation, buildActionDTO: BuildActionDTO)
        +buildBuilding(user: User, buildActionDTO: BuildActionDTO)
        +upgradeSettlementToTown(user: User, buildActionDTO: BuildActionDTO)
        +upgradeSettlementToTown(player: PlayerRepresentation, buildActionDTO: BuildActionDTO)
        +letPlayerPayRecipe(user: User, recipe: Map)
    }

    %% game/board
    class Field{
		-pos: AxialPosition
        -numberChip: int
        -resource: ResourceType
    }

    class FieldFactory{
        +generateFields(boardRadius: int, ratios: Map)
        -calculateTotalResourceFields(boardRadius: int)
        -generateNumberChips(boardRadius: int)
        -generateAvailableResourceTypes(boardRadius: int, ratios: Map)
    }

    class GameBoard{
        -RATIOS: Map
        -fields: List
        -structures: List
        +addStructure(player: PlayerRepresentation, buildActionDTO: BuildActionDTO)
        +upgradeSettlementToTown(player: PlayerRepresentation, buildActionDTO: BuildActionDTO)
        +getFieldsAt(positions: List)
        +getStructureAt(pos: List)
        +getFieldsByNumberChip(numberChip: int)
    }

    class Structure{
        -type: StructureType
        -pos: List
        -ownerId: int
        -recipe: Map
    }

    class StructureFactory{
        -settlementRecipe: EnumMap
        -streetRecipe: EnumMap
        -townRecipe: EnumMap
        +randomlyBuildInitialStructures(match: Match, validator: BuildingABuildingValidator)
        +getRecipeForStructureType(type: StructureType)
        +buildStructureFromDTO(player: PlayerRepresentation, dto: BuildActionDTO)
    }

    %% game/dto
    class BuildActionDTO{
		-structureType: StructureType
        -pos: List
	}

	class PickDicePairDTO{
		-index: int
    }
    
    class PlayerActionDTO{
        <<abstract>>
        -type: PlayerActionType
    }

	class TradeBankDTO{
        -resourceOffered: ResourceType
        -amountOffered: int
        -resourceRequested: ResourceType
        -amountRequested: int
    }

	class TradePlayerDTO{
		-id: Integer
        -status: TradingStatus
        -target: TradingTarget
        -offered: Map
        -requested: Map
	}

    %% game/error

    class InvalidBuildRequestException{
    }

    class MatchNotFoundException{
    }

    class MissingAxialPositionsException{
    }

    class MoveHasntBeenImplementedException{
    }

    class NotEnoughResourcesException{
    }

    class NotPlayersTurnException{
    }

    class TooLittleSpaceException{
    }

    %% game/player
    class GamePlayers{
        -players: List
        -playersTurnOrder: List
        -winner: PlayerRepresentation
        -createPlayerRepresentationsForLobby(lobby: Lobby)
        -generatePlayersTurnOrder()
        +rotateNextPlayer()
        +getPlayerCurrentTurn()
        +isPlayersTurn(user: User)
        +getPlayerForUser(user: User)
        +getPlayerById(id: int)
    }

    class Player{
		-id: int
        -isAccount: boolean
        -user: User
        +getUsername()
    }

    class PlayerHueFactory{
        +generateHueFromHash(username: String)
    }

    class PlayerRepresentation{
		-player: Player
        ~username: String
        -publicId: int
        -sessionId: String
		-playerHue: int
        -resources: Map
        -chosenPortrait: String
        -points: int
        +addPoints(points: int)
    }

    %% game/trading

    class PlayerTrade{
        -id: int
        -predecessorId: Integer
        -status: TradingStatus
        -target: TradingTarget
        -createdBy: int
        -offered: Map
        -requested: Map
    }

    class TradingHandler{
        +GIVE_GET_RATIO: int
        -playerTrades: Map
        -nextId: int
        +handlePlayerTrade(user: User, match: Match, dto: TradePlayerDTO)
        +createTrade(user: User, match: Match, dto: TradePlayerDTO)
        -createTradeForPlayerId(predecessorId: Integer, status: TradingStatus, target: TradingTarget, createdBy: int, offered: Map, requested: Map)
        +editTrade(user: User, match: Match, dto: TradePlayerDTO)
        +acceptTrade(user: User, match: Match, dto: TradePlayerDTO)
        +denyTrade(dto: TradePlayerDTO)
        +cancelTrade(dto: TradePlayerDTO)
        +tradeBank(user: User, match: Match, dto: TradeBankDTO)
        +clearTrades()
    }

    class TradingTarget{
        -allPlayers: boolean
        -playerId: Integer
        +ofPlayer(playerId: Integer)
    }

    %% game/types
    class PlayerActionType{
        <<enumeration>>
        BUILD
        TRADE_BANK
        TRADE_PLAYER
        PICK_DICE_PAIR
    }

	class ResourceType{
        <<enumeration>>
        WHEAT
        WOOD
        BRICK
        SHEEP
        DUNES
	}

    class StructureType{
        <<enumeration>>
        -posAmount: int
        SETTLEMENT(3)
        TOWN(3)
        HARBOUR(2)
	}

    class TradingStatus{
        <<enumeration>>
        OFFERED
        CHANGED
        ACCEPTED
        DENIED
        CANCELLED
	}

    %% config
    class AppConfig{
        -initialCapacity: int
        -heartbeatCheckIntervalSeconds: long
        +passwordEncoder()
    }

    class SecurityConfig{
        -frontendHost: String
        -accessTokenAuthenticationFilter: AccessTokenAuthenticationFilter
        -refreshTokenAuthenticationFilter: RefreshTokenAuthenticationFilter
        -sseTokenAuthenticationFilter: SseTokenAuthenticationFilter
        ~corsConfigurationSource()
        +securityFilterChain(http: HttpSecurity)
    }

    %% config/filter
    class AccessTokenAuthenticationFilter{
        -jwtService: JwtService
        -userRepository: AllUserRepository
        #shouldNotFilter(request: HttpServletRequest)
        #doFilterInternal(request: HttpServletRequest, reponse: HttpServletResponse, filterChain: FilterChain)
    }

    class RefreshTokenAuthenticationFilter{
        -jwtService: JwtService
        -refreshTokensService: RefreshTokensService
        -userRepository: AllUserRepository
        +doesFilter(path: String)
        #shouldNotFilter(request: HttpServletRequest)
        #doFilterInternal(request: HttpServletRequest, reponse: HttpServletResponse, filterChain: FilterChain)
    }

    class SseTokenAuthenticationFilter{
        -jwtService: JwtService
        -sseTokenService: SseTokenService
        -userRepository: AllUserRepository
        +doesFilter(path: String)
        #shouldNotFilter(request: HttpServletRequest)
        #doFilterInternal(request: HttpServletRequest, reponse: HttpServletResponse, filterChain: FilterChain)
        -extractSseToken(queryString: String)
    }

    %% error
    class ControllerExceptionHandler{
        +handleNotFound(exception: Exception)
        +handleForbidden(exception: Exception)
        +handleBadRequest(exception: Exception)
        -responseOf(status: int, exception: Exception)
    }

    class BadRequestException{
    }

    class ForbiddenException{
    }

    class InvalidDtoException{
    }

    class NotFoundException{
    }

    %% account
    class AccountController{
        -authenticationService: AuthenticationService
        -sseTokenService: SseTokenService
        +guest(response: HttpServletResponse)
        +register(request: RegisterDTO, response: HttpServletResponse)
        +login(request: LoginDTO, response: HttpServletResponse)
        +refresh(oldRefreshToken: String, response: HttpServletResponse)
        +logout(oldRefreshToken: String, response: HttpServletResponse)
        +sseToken()
    }

    class AuthenticationResponse{
        -accessToken: String
    }

    class AuthenticationResult{
        -authenticationResponse: AuthenticationResponse
        -refreshTokenCookie: Cookie
    }

    class AuthenticationService{
        -userRepository: AllUserRepository
        -passwordEncoder: PasswordEncoder
        -jwtService: JwtService
        -cookieService: CookieService
        -refreshTokensService: RefreshTokensService
        +guest()
        +register(request: RegisterDTO)
        +login(request: LoginDTO)
        +refresh(refreshToken: String)
        +logout(oldRefreshToken: String)
        -createNewTokensAndGetResult(user: User)
    }

    class AuthUtils{
        +getAuthenticatedUser()
    }

    %% account/dto
    class LoginDTO{
        -email: String
        -password: String
    }

    class RegisterDTO{
        -email: String
        -password: String
    }

    %% account/error
    class InvalidCharactersException{
    }

    class InvalidCredentialsException{
    }

    class UserAlreadyExistsException{
    }

    %% account/token
    class AuthTokens{
        +ACCESS_TOKEN_MAX_AGE: int
        +REFRESH_TOKEN_NAME: String
        +REFRESH_TOKEN_MAX_AGE: int
        +SSE_TOKEN_MAX_AGE: int
    }

    class CookieService{
        -jwtService: JwtService
        +createRefreshTokenCookie(user: User)
        +createDeleteRefreshTokenCookie()
    }

    class JwtService{
        -SECRET_KEY: String
        +extractUsername(token: String)
        +extractClaim(token: String, claimsResolver: Function<Claims, T>)
        +generateToken(user: User, maxAgeSeconds: int)
        +generateToken(extraClaims: Map<String,Object>, user: User, maxAgeSeconds: int)
        +isTokenValid(token: String)
        -extractExpiration(token: String)
        -extractAllClaims(token: String)
        -getSecretKey()
    }

    class RefreshTokensService{
        -passwordEncoder: PasswordEncoder
        +store(user: User, refreshTokenCookie: Cookie, userRepository: UserRepository)
        +invalidate(user: User, userRepository: UserRepository)
        +isValid(user: User, refreshToken: String)
    }

    class SseTokenService{
        -jwtService: JwtService
        -usernamesTokens: Map<String, String>
        +createToken(user: User)
        +getValidTokenAndInvalidate(user: User)
    }

    %% account/user
    class AccountUserRepository{
        <<Interface>>
        +save(user: User)
    }

    class AllUserRepository{
        -accountUserRepository: AccountUserRepository
        -guestUserRepository: GuestUserRepository
        +save(user: User)
        +findByUsername(username: String)
        +findByUsernameIgnoreCase(username: String)
        -getRepositoryByUser(user: User)
        +deleteAll()
    }

    class GuestUserRepository{
        +guestUsers: Map<String, User>
        +save(user: User)
        +findByUsername(username: String)
        +findByUsernameIgnoreCase(username: String)
        +deleteAll()
    }

    class Role{
        <<enumeration>>
        PLAYER
        GUEST
	}

    class User{
        -id: Integer
        -username: String
        -password: String
        -email: String
        -role: Role
        -refreshToken: String
        +getAuthorities()
        +getUsername()
        +setPassword(password: String, passwordEncoder: PasswordEncoder)
        +setRefreshToken(refreshToken: String, passwordEncoder: PasswordEncoder)
    }

    class UserRepository{
        <<Interface>>
        +save(user: User)
        +findByUsername(username: String)
        +findByUsernameIgnoreCase(username: String)
        +deleteAll()
    }

    %% RELATIONS
    SseSender ..> User: use

    Lobby ..> HeartbeatHandler: use
    Lobby ..> Player: use
    Lobby ..> Match: use
    Lobby ..> AppConfig: use
    Lobby ..> LobbyManager: use
    Lobby ..> User: use
    Lobby ..|> NoHeartbeatListener
    LobbyController ..> LobbyManager : use
    LobbyController ..> CreateLobbyDTO : use
    LobbyController ..> AuthUtils : use
    LobbyController ..> LobbyNotFoundException : use
    LobbyController ..> HeartbeatDTO : use
    LobbyController ..> NotOwnerOfLobbyException : use
    LobbyController ..> TooLittleSpaceException : use
    LobbyController ..> Match : use
    LobbyManager ..> Lobby: use
    LobbyManager ..> AppConfig: use
    LobbyManager ..> LobbyCodeGenerator: use
    LobbyManager ..> LobbyNotFoundException: use
    LobbyManager ..> Player: use
    LobbyManager ..> User: use
	LobbyManager ..> TooLittleSpaceException: use
    LobbyManager ..> InvalidRadiusException: use
    LobbyManager ..> NotOwnerOfLobbyException: use
    LobbyManager ..> Match: use
    LobbyManager ..> MatchNotFoundException: use
    LobbyManager --|> SseSender
    LobbyManager ..|> NoHeartbeatListener
    LobbyDTO ..> Player: use
    InvalidRadiusException --|> BadRequestException
    LobbyNotFoundException --|> NotFoundException
    NotOwnerOfLobbyException --|> ForbiddenException
    HeartbeatHandler ..> Lobby: use
    HeartbeatHandler ..> Player: use
    HeartbeatHandler ..> NoHeartbeatListener: use
    NoHeartbeatListener ..> Lobby: use

    BuildingABuildingValidator ..> AxialPosition: use
    BuildingABuildingValidator ..> Field: use
    BuildingABuildingValidator ..> User: use
    BuildingABuildingValidator ..> Match: use
    BuildingABuildingValidator ..> BuildActionDTO: use
    BuildingABuildingValidator ..> MissingAxialPositionsException: use
    BuildingABuildingValidator ..> StructureType: use
    BuildingABuildingValidator ..> PlayerRepresentation: use
    BuildingABuildingValidator ..> Structure: use
    BuildingABuildingValidator ..> ResourceType: use
    BuildingABuildingValidator ..> StructureFactory: use
    GameController ..> LobbyManager: use
    GameController ..> GameManager: use
    GameController ..> MatchNotFoundException: use
    GameController ..> Field: use
    GameController ..> StructureType: use
    GameController ..> ResourceType: use
    GameController ..> StructureFactory: use
    GameController ..> AuthUtils: use
    GameController ..> NotPlayersTurnException: use
    GameController ..> PlayerActionDTO: use
    GameController ..> InvalidBuildRequestException: use
    GameController ..> MoveHasntBeenImplementedException: use
    GameController ..> Lobby: use
    GameManager ..> LobbyManager: use
    GameManager ..> MatchNotFoundException: use
    GameManager ..> NotPlayersTurnException: use
    GameManager ..> User: use
    GameManager ..> Match: use
    GameManager ..> ForbiddenException: use
    GameManager ..> PlayerActionDTO: use
    GameManager ..> InvalidBuildRequestException: use
    GameManager ..> MoveHasntBeenImplementedException: use
    GameManager ..> BuildActionDTO: use
    GameManager ..> TradeBankDTO: use
    GameManager ..> TradePlayerDTO: use
    GameManager ..> StructureType: use
    GameManager ..> StructureFactory: use
    GameManager ..> ResourceType: use
    GameManager --|> SseSender
    Match ..> GameBoard: use
    Match ..> GamePlayers: use
    Match ..> ResourceType: use
    Match ..> BuildingABuildingValidator: use
    Match ..> TradingHandler: use
    Match ..> Lobby: use
    Match ..> TooLittleSpaceException: use
    Match ..> StructureFactory: use
    Match ..> StructureType: use
    Match ..> PlayerRepresentation: use
    Match ..> Field: use
    Match ..> User: use
    Match ..> BuildActionDTO: use
    Field ..> AxialPosition: use
    Field ..> ResourceType: use
    FieldFactory ..> Field: create
    FieldFactory ..> ResourceType: use
    FieldFactory ..> AxialPosition: use
    GameBoard ..> ResourceType: use
    GameBoard ..> Field: use
    GameBoard ..> Structure: use
    GameBoard ..> FieldFactory: use
    GameBoard ..> PlayerRepresentation: use
    GameBoard ..> BuildActionDTO: use
    GameBoard ..> StructureFactory: use
    GameBoard ..> AxialPosition: use
    Structure ..> StructureType: use
    Structure ..> AxialPosition: use
    Structure ..> ResourceType: use
    StructureFactory ..> Structure: create
    StructureFactory ..> ResourceType: use
    StructureFactory ..> Match: use
    StructureFactory ..> BuildingABuildingValidator: use
    StructureFactory ..> TooLittleSpaceException: use
    StructureFactory ..> AxialPosition: use
    StructureFactory ..> PlayerRepresentation: use
    StructureFactory ..> BuildActionDTO: use
    StructureFactory ..> Field: use
    StructureFactory ..> StructureType: use
    BuildActionDTO ..> StructureType: use
    BuildActionDTO ..> AxialPosition: use
    BuildActionDTO --|> PlayerActionDTO
    PickDicePairDTO --|> PlayerActionDTO
    PlayerActionDTO ..> PlayerActionType: use
    TradeBankDTO ..> ResourceType: use
    TradeBankDTO --|> PlayerActionDTO
    TradePlayerDTO ..> ResourceType: use
    TradePlayerDTO ..> TradingStatus: use
    TradePlayerDTO ..> TradingTarget: use
    TradePlayerDTO --|> PlayerActionDTO
    InvalidBuildRequestException --|> BadRequestException
    MatchNotFoundException --|> NotFoundException
    MissingAxialPositionsException ..> StructureType: use
    MissingAxialPositionsException --|> InvalidDtoException
    MoveHasntBeenImplementedException ..> PlayerActionType: use
    MoveHasntBeenImplementedException --|> BadRequestException
    NotEnoughResourcesException --|> BadRequestException
    NotPlayersTurnException --|> ForbiddenException
    TooLittleSpaceException --|> BadRequestException
    GamePlayers ..> PlayerRepresentation: use
    GamePlayers ..> Lobby: use
    GamePlayers ..> User: use
    Player ..> User: use
    Player ..> Role: use
    PlayerRepresentation ..> Player: use
    PlayerRepresentation ..> ResourceType: use
    PlayerRepresentation ..> PlayerHueFactory: use
    PlayerTrade ..> TradingStatus: use
    PlayerTrade ..> TradingTarget: use
    PlayerTrade ..> ResourceType: use
    TradingHandler ..> PlayerTrade: use
    TradingHandler ..> User: use
    TradingHandler ..> Match: use
    TradingHandler ..> TradePlayerDTO: use
    TradingHandler ..> TradingStatus: use
    TradingHandler ..> PlayerRepresentation: use
    TradingHandler ..> TradingTarget: use
    TradingHandler ..> ResourceType: use
    TradingHandler ..> NotEnoughResourcesException: use
    TradingHandler ..> BadRequestException: use

    SecurityConfig ..> AccessTokenAuthenticationFilter: use
    SecurityConfig ..> RefreshTokenAuthenticationFilter: use
    SecurityConfig ..> SseTokenAuthenticationFilter: use
    AccessTokenAuthenticationFilter ..> JwtService: use
    AccessTokenAuthenticationFilter ..> AllUserRepository: use
    AccessTokenAuthenticationFilter ..> RefreshTokenAuthenticationFilter: use
    AccessTokenAuthenticationFilter ..> SseTokenAuthenticationFilter: use
    RefreshTokenAuthenticationFilter ..> JwtService: use
    RefreshTokenAuthenticationFilter ..> RefreshTokensService: use
    RefreshTokenAuthenticationFilter ..> AllUserRepository: use
    RefreshTokenAuthenticationFilter ..> AuthTokens: use
    SseTokenAuthenticationFilter ..> JwtService: use
    SseTokenAuthenticationFilter ..> AllUserRepository: use
    SseTokenAuthenticationFilter ..> SseTokenService: use

    ControllerExceptionHandler ..> NotFoundException: use
    ControllerExceptionHandler ..> ForbiddenException: use
    ControllerExceptionHandler ..> BadRequestException: use
    InvalidDtoException --|> BadRequestException

    AuthUtils ..> User: use
    AuthenticationService ..> AllUserRepository: use
    AuthenticationService ..> JwtService: use
    AuthenticationService ..> CookieService: use
    AuthenticationService ..> RefreshTokensService: use
    AuthenticationService ..> AuthenticationResult: use
    AuthenticationService ..> User: use
    AuthenticationService ..> InvalidCharactersException: use
    AuthenticationService ..> UserAlreadyExistsException: use
    AuthenticationService ..> Role: use
    AuthenticationService ..> InvalidCredentialsException: use
    AuthenticationService ..> AuthenticationResponse: use
    AuthenticationService ..> AuthTokens: use
	AuthenticationService ..> LoginDTO: use
	AuthenticationService ..> RegisterDTO: use
    AuthenticationResult ..> AuthenticationResponse: use
    AccountController ..> AuthenticationService: use
    AccountController ..> SseTokenService: use
    AccountController ..> AuthenticationResponse: use
    AccountController ..> AuthenticationResult: use
	AccountController ..> LoginDTO: use
	AccountController ..> RegisterDTO: use
    AccountController ..> User: use
    AccountController ..> AuthUtils: use
    InvalidCharactersException --|> BadRequestException
    InvalidCredentialsException --|> BadRequestException
    UserAlreadyExistsException --|> BadRequestException
    CookieService ..> JwtService: use
    CookieService ..> AuthTokens: use
    JwtService ..> User: use
    RefreshTokensService ..> User: use
    RefreshTokensService ..> UserRepository: use
    SseTokenService ..> JwtService: use
    SseTokenService ..> AuthTokens: use
    SseTokenService ..> User: use
    AccountUserRepository ..> User: use
    AccountUserRepository --|> UserRepository
    AllUserRepository ..> AccountUserRepository: use
    AllUserRepository ..> GuestUserRepository: use
    AllUserRepository ..> User: use
    AllUserRepository ..> Role: use
    AllUserRepository ..|> UserRepository
    GuestUserRepository ..> User: use
    GuestUserRepository ..|> UserRepository
    User ..> Role: use
    UserRepository ..> User: use
```
