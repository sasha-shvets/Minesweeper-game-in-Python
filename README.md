This is my educational project - Minesweeper game in Python

The game logic:
 
 flowchart TD
    %% Global Styles
    classDef startEnd fill:#f9f,stroke:#333,stroke-width:2px;
    classDef process fill:#bbf,stroke:#333,stroke-width:1px;
    classDef condition fill:#ffb,stroke:#333,stroke-width:1px;
    classDef event fill:#fbf,stroke:#333,stroke-width:1px;
    classDef terminal fill:#f99,stroke:#333,stroke-width:2px;

    %% Game Initialization
    Start([Launch Game]) --> InitGlobals[Initialize Globals:<br>game_over = False<br>score = 0<br>squares_to_clear = 0]
    InitGlobals --> CallPlay[Call play_bombdodger]
    
    subgraph Build Phase
        CallPlay --> CallCreateField[Call create_bombfield]
        CallCreateField --> GenGrid{Iterate 10x10 Grid}
        GenGrid -->|Roll < 20%| AddBomb[Append 1 to row_list]
        GenGrid -->|Roll >= 20%| AddSafe[Append 0 to row_list<br>Increment squares_to_clear]
        AddBomb --> FieldDone{Grid Complete?}
        AddSafe --> FieldDone
        FieldDone -->|No| GenGrid
        FieldDone -->|Yes| CreateWindow[Create Tkinter Window]
        CreateWindow --> CallLayout[Call layout_window]
        CallLayout --> CreateLabels[Generate 10x10 Grid of Labels]
        CreateLabels --> AssignColor[Randomly assign green shading]
        AssignColor --> BindClick[Bind Left-Click Event to on_click]
    end

    BindClick --> Loop[window.mainloop]
    
    %% Runtime Interaction
    Loop --> ClickEvent(((User clicks a square)))
    
    subgraph Event Handling
        ClickEvent --> CheckOver{Is game_over == True?}
        CheckOver -->|Yes| Ignore[Do Nothing]
        CheckOver -->|No| GetCoords[Get square grid coordinates & text]
        GetCoords --> CheckBomb{Is bombfield row, col == 1?}
        
        %% Bomb Hit Path
        CheckBomb -->|Yes| BombHit[Set game_over = True<br>Turn square RED<br>Print Game Over & Score]
        BombHit --> EndLost([Game Ends - Loss])
        
        %% Safe Square Path
        CheckBomb -->|No| CheckClicked{Is text == '    '?}
        CheckClicked -->|No| Ignore
        CheckClicked -->|Yes| TurnBrown[Turn square BROWN]
        TurnBrown --> CheckNeighbors[Scan 8 neighboring squares for bombs]
        CheckNeighbors --> DisplayCount[Update square text with bomb count]
        DisplayCount --> AdjustScore[Decrement squares_to_clear<br>Increment score]
        AdjustScore --> CheckWin{Is squares_to_clear == 0?}
        CheckWin -->|Yes| WinGame[Set game_over = True<br>Print Victory & Score]
        CheckWin -->|No| Loop
        WinGame --> EndWon([Game Ends - Win])
    end

    %% Apply Styles
    class Start startEnd;
    class EndLost,EndWon terminal;
    class InitGlobals,CallPlay,CallCreateField,CreateWindow,CallLayout,CreateLabels,AssignColor,BindClick,Loop,GetCoords,TurnBrown,CheckNeighbors,DisplayCount,AdjustScore process;
    class GenGrid,FieldDone,CheckOver,CheckBomb,CheckClicked,CheckWin condition;
    class ClickEvent event;
