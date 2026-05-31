This is my educational project - Minesweeper game in Python

The game logic:
 
 ```mermaid
flowchart TD
    Start([Launch Game]) --> Init[Initialize Variables & Grid]
    Init --> DrawWindow[Build 10x10 Tkinter Window]
    DrawWindow --> MainLoop[Wait for Player Input]

    MainLoop --> Click[Player clicks a green square]
    Click --> CheckOver{Is Game Over?}
    
    CheckOver -->|Yes| MainLoop
    CheckOver -->|No| CheckBomb{Is it a bomb?}

    CheckBomb -->|Yes| GameOver[Turn square RED<br>Show Game Over<br>Print Score]
    GameOver --> End[End Game]

    CheckBomb -->|No| Safe[Turn square BROWN<br>Count 8 neighboring bombs<br>Show bomb count]
    Safe --> Score[Update score<br>Reduce remaining squares]
    Score --> CheckWin{Are all safe squares clear?}
    
    CheckWin -->|Yes| Win[Show Victory<br>Print Final Score]
    CheckWin -->|No| MainLoop
    Win --> End
```

