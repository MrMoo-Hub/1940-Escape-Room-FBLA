## 🏗️ System Architecture & Game Flow

The game runs on a continuous **60 FPS loop**, structured into three core layers:

1. **Input & Engine Control**
2. **Game State Machine (Logic Layer)**
3. **Rendering & Persistence (Output Layer)**

This separation mirrors real-world software architecture used in production systems and game engines.

```mermaid
flowchart LR
    %% ========== STYLES ==========
    classDef engine fill:#0f172a,stroke:#38bdf8,stroke-width:2px,color:#e2e8f0;
    classDef logic fill:#111827,stroke:#facc15,stroke-width:2px,color:#fefce8;
    classDef render fill:#020617,stroke:#4ade80,stroke-width:2px,color:#dcfce7;
    classDef decision fill:#1f2937,stroke:#f87171,stroke-width:2px,color:#fee2e2;

    %% ========== ENGINE ==========
    subgraph ENGINE["⚙️ 1. ENGINE LOOP (60 FPS)"]
        LOOP["Main Loop<br/>(while running)"]:::engine
        INPUT["Capture Input<br/>(Keyboard / Mouse)"]:::engine
        TRACE["Trace Hook<br/>(sys.settrace)"]:::engine
        ROUTE["Evaluate Game State"]:::engine

        LOOP --> INPUT --> TRACE --> ROUTE
    end

    %% ========== GAME LOGIC ==========
    subgraph LOGIC["🧠 2. GAME STATE MACHINE"]
        TITLE["Title Screen"]:::logic
        ACT1["Challenge 1<br/>Strange Room"]:::logic
        ACT2["Challenge 2<br/>System Glitch"]:::logic
        ACT3["Challenge 3<br/>Command Post"]:::logic
        ACT4["Challenge 4<br/>Code Analyst"]:::logic
        
        CHECK{"Score ≥ 200?"}:::decision
        END["End Screen<br/>+ Save Results"]:::logic

        TITLE --> ACT1 --> ACT2 --> ACT3 --> ACT4 --> CHECK
        CHECK -->|Yes| END
        CHECK -->|No (Reset)| TITLE
    end

    %% ========== RENDERING ==========
    subgraph RENDER["🎨 3. RENDERING & OUTPUT"]
        DRAW["Render Scene + UI<br/>(Tech Panels)"]:::render
        TIMER["Update Timer / Score"]:::render
        DISPLAY["Display Frame<br/>(pygame.display.flip)"]:::render
        FPS["Frame Sync<br/>(clock.tick 60)"]:::render
        SAVE["Persist Data<br/>(JSON File I/O)"]:::render

        DRAW --> TIMER --> DISPLAY --> FPS
    end

    %% ========== CONNECTIONS ==========
    ROUTE -->|Select Active State| TITLE
    ACT1 --> DRAW
    ACT2 --> DRAW
    ACT3 --> DRAW
    ACT4 --> DRAW

    END --> SAVE
    FPS --> LOOP
