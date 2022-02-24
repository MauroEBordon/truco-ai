# Class specification & Diagrams

To get started i listed some candidates for posible classes, and then selected the keywords that had really different functionality and isnt just a cluster of data.

* ## `Room` (Sala)   
    A posible `Game` (with at least 1 human `Player`), its url works both as an ID & the gateway for other human `Players` to enter the  `Game`.
    The room is terminated after 1 minute without the button 'Start' being pressed.
    A user cannot host 2 rooms/games at the same time

* ## `Game` / `RoundLoop` (Partida)  
    The rules of `Truco` vary by region & time period but basically the game has a deterministic `State` each `Round`, the players are not aware of all the factors in play, but the system can know everything (I'm not saying my AI will cheat though).  
    The game ends when a `Team` gets first to 30 points.
    Games inactive for 1 minute (without any request from any human player) are terminated.
    
* ## `Round` (Mano)  
    Once a Game starts, the round loop is on the loose. It first starts dealing 3 cards and then the players orderly take their `Turns` to play. The Round ends when:  
    * All players had shown their cards  
    * A Player has decided to give up  
    * There is a clear winner and no other state can change the outcome of the Round.   
  
* ## `Turn` (Turno) -> Broke into two:  
    * ## `GameState` Relational Model (Estado del juego). 
        - Id: Hash
        - Score: Tuple(Int, Int)
        - NRound: Int
        - PlayerOrder{0,5}: List(Int)
        - TrickBet: Int
        - EnvyBet: Int
        - Signals{0,5}: List(Hash) (this is a special data which can be corrupted and integrity isnt really appreciated, if fact i want to have an stochastic factor)
        - Cards{0,5}: List(3-uple(Hash))
        - `Desition`: Enum (listed below)
    * ## Individual `Desition` (Desición)
        0.  Scheme on a Trick (cantar sobre los trucos) (if team has the priority) [0-4 Points]  
        1.  Scheme on Envy (cantar sobre los envidos) (if team has the priority) [0-15 Points]  
        2.  Just play a card (no cantar)  
        3.  Give up the Round (rendirse)  
* ## `SchemeCicle` (Team Desitions) (Ciclo de Respuesta a Cantos)   
    When a Scheme is excecuted, the game enters in a new internal state, the `SchemeCicle`, in which the tams vote* to answer Yes or No to the given scheme.
    In case of a tie, the vote of player with fewer priority is answered.  

* ## `Player` (Jugador)  
    We need to track IP, and not much more i guess.

As you can see, this Class system has some relation inside, so lets try to diagram this with a classic UML Class Diagram.

![Class Diagrams]()

I'm tempted to translate `Truco` as `Trick&Scheme`. 