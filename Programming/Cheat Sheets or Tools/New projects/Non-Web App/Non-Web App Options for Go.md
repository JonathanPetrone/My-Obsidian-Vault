### Desktop GUI Applications:
- **Fyne** (fyne.io) - Native-looking cross-platform (Windows/Mac/Linux), material design, easiest to start
- **Wails** (wails.io) - Embed web frontend (your HTMX/Tailwind skills transfer), Go backend, feels like Electron but smaller
- **Gio** (gioui.org) - Immediate mode GUI, renders with GPU, highly portable but steeper learning curve
- Tradeoff: Fyne = quickest native feel, Wails = reuse web skills, Gio = maximum control/performance

### Terminal/CLI Applications:
- **Bubble Tea** (github.com/charmbracelet/bubbletea) - Build interactive TUIs (Elm architecture), rich components via Lip Gloss
- **Cobra** (github.com/spf13/cobra) - Standard for CLI tools (used by kubectl, hugo), handles subcommands/flags
- **Survey** (github.com/AlecAivazis/survey) - Interactive prompts (select, confirm, input)
- Tradeoff: Bubble Tea = full TUI apps, Cobra = traditional CLI, Survey = simple interactive scripts

### Mobile Applications:
- **Fyne** (also does mobile) - Same codebase for desktop + mobile, OpenGL rendering
- **Go Mobile** (golang.org/x/mobile) - Lower-level bindings to iOS/Android, more friction
- Reality check: Go mobile ecosystem is weak. React Native or Flutter are better bets for serious mobile work.

### Backend Services (non-HTTP):
- **gRPC** (grpc.io) - High-performance RPC, protocol buffers, bidirectional streaming
- **NATS** (nats.io) - Message bus, pub/sub, microservices communication
- **WebSockets** (gorilla/websocket) - Real-time bidirectional communication
- Pattern: These are backend primitives, not end-user apps

### System Tools/Automation:
- Pure Go stdlib - file manipulation, process spawning, network tools
- **Viper + Cobra** - Config-driven automation tools
- Examples: Database backup scripts, log parsers, deployment tools

#### Mental Model:
- **Desktop GUI**: Wails if you want web skills to transfer, Fyne for pure native simplicity
- **Terminal**: Bubble Tea for rich TUIs, Cobra for traditional CLI tools
- **Mobile**: Go is weak here; consider switching to Flutter/React Native
- **Services**: gRPC/NATS for distributed systems

#### Your Pattern Fit: 
Wails and Bubble Tea both maintain your "minimal, learn fundamentals" approach. Wails lets you reuse HTMX+Tailwind knowledge. Bubble Tea is pure Go with excellent docs.