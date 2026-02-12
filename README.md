# Sanguine - Strategic Card Game Implementation

A comprehensive Java implementation of Sanguine, a tactical 2-player card game featuring territory control mechanics on a rectangular board. This project demonstrates advanced MVC architecture, GUI development, AI strategy implementation, and file-based configuration systems through a complete gaming application with both textual and graphical interfaces.

## Project Overview

Sanguine is a strategic card game where players compete for territorial dominance by placing cards with unique influence patterns on a rectangular board. Players gain control of territory through pawns, accumulating points to achieve victory. This implementation provides a complete gaming experience with:

- **Full MVC Architecture**: Separate model, view, and controller components
- **Dual Interface Support**: Both GUI and textual representations
- **AI Player System**: Multiple strategy implementations for computer opponents
- **Flexible Configuration**: File-based deck and game setup
- **Extensible Design**: Plugin-ready architecture for new features

## Project Structure

```
src/main/java/
├── player/
│   ├── HumanPlayer.java                 # Human player implementation
│   ├── MachinePlayer.java               # AI player with strategy system
│   └── PlayerInterface.java             # Player abstraction interface
├── sanguine/
│   ├── controller/
│   │   ├── ConfigFileParser.java        # Deck configuration file parser
│   │   ├── ControllerInterface.java     # Controller abstraction
│   │   └── SanguineController.java      # Main game controller
│   ├── model/
│   │   ├── Card.java                    # Card interface definition
│   │   ├── Cell.java                    # Board cell implementation
│   │   ├── CellInterface.java           # Cell abstraction
│   │   ├── Cost.java                    # Card cost enumeration
│   │   ├── InfluenceType.java           # Influence pattern enumeration
│   │   ├── ModelStatusListener.java     # Model state observer interface
│   │   ├── MutableModelInterface.java   # Mutable model operations
│   │   ├── PawnAmount.java              # Pawn quantity enumeration
│   │   ├── ReadOnlyModelInterface.java  # Read-only model interface
│   │   ├── SanguineCard.java            # Concrete card implementation
│   │   └── SanguineModel.java           # Core game logic implementation
│   ├── strategies/
│   │   ├── FillFirstStrategy.java       # Aggressive territory expansion AI
│   │   ├── MaxRowScoreStrategy.java     # Point-optimization AI strategy
│   │   ├── Move.java                    # Move representation class
│   │   └── Strategy.java                # AI strategy interface
│   └── view/
│       ├── AbstractPanel.java           # Base panel for GUI components
│       ├── GamePanel.java               # Main game display panel
│       ├── GameView.java                # GUI view interface
│       ├── PlayerActions.java           # Player interaction handler
│       ├── SanguineBoardPanel.java      # Game board visual component
│       ├── SanguineHandPanel.java       # Player hand display component
│       ├── SanguineTextualView.java     # Text-based game representation
│       ├── SanguineView.java            # Complete GUI implementation
│       ├── TextualView.java             # Textual view interface
│       └── ViewFeatures.java            # View capability interface
├── Player.java                          # Player enumeration (RED/BLUE)
├── SanguineGame.java                    # Game session manager
└── SanguineStorm.java                   # Main application entry point

docs/
└── *.deck                               # Card deck configuration files

images/
└── screenshots/                         # GUI documentation and test images

test/java/sanguine/
├── mocks/                               # Mock implementations for testing
│   ├── MockModel.java                   # Model mock for controller testing
│   ├── MockView.java                    # View mock for controller testing
│   └── MockStrategy.java                # Strategy mock for AI testing
├── ConfigFileParserTests.java           # Configuration parsing tests
├── SanguineControllerTests.java         # Controller behavior tests
├── SanguineModelTests.java              # Core game logic tests
├── SanguineViewTests.java               # GUI component tests
├── CellTests.java                       # Board cell functionality tests
├── StrategyTests.java                   # AI strategy validation tests
├── MoveTests.java                       # Move validation tests
└── IntegrationTests.java                # End-to-end system tests
```

## Game Features

### Core Mechanics
- **Territory Control**: Strategic card placement influences board control
- **Influence Patterns**: Each card has unique influence grids affecting adjacent cells
- **Pawn Management**: Players accumulate pawns in controlled territories for scoring
- **Resource Management**: Cards have costs that must be managed strategically
- **Win Conditions**: Victory achieved through territorial dominance and point accumulation

### Game Variants & Modes
- **Standard Play**: Traditional 2-player competitive mode
- **AI Opponents**: Multiple difficulty levels with different strategies
- **Custom Boards**: Configurable board sizes and starting conditions
- **Deck Customization**: File-based card configuration system

### Interface Options
- **GUI Mode**: Full graphical interface with mouse and keyboard controls
- **Textual Mode**: Command-line interface for console-based play
- **Mixed Mode**: GUI with textual debugging output

## Quick Start Guide

### Basic Textual Gameplay
```java
// Initialize game with configuration
List<Card> exampleBigDeck = new ConfigFileParser().parseDeck("docs"
    + File.separator + "exampleBig.deck");
MutableModelInterface model = new SanguineModel(3, 5, exampleBigDeck, exampleBigDeck, 5, false);

// Execute game moves
model.playCard(model.getHand(Player.RED).get(4), 2, 0);
model.playCard(model.getHand(Player.BLUE).get(1), 0, 4);
model.playCard(model.getHand(Player.RED).get(2), 0, 0);

// Display current state
TextualView view = new SanguineTextualView(model);
System.out.println(view.toString());
```

### GUI Application Launch
```java
// Initialize components
List<Card> exampleBigDeck = new ConfigFileParser().parseDeck("docs"
    + File.separator + "exampleBig.deck");

MutableModelInterface model = new SanguineModel(5, 7,
    exampleBigDeck, exampleBigDeck, 5, false);
GameView view = new SanguineView((ReadOnlyModelInterface) model);
ControllerInterface controller = new SanguineController(view, model);

// Launch GUI and execute moves
view.display(true);
model.playCard(model.getHand(Player.RED).getFirst(), 1, 0);
view.repaint();
model.playCard(model.getHand(Player.BLUE).getFirst(), 1, 6);
view.repaint();
```

## Game Rules & Mechanics

### Board Structure
- Rectangular grid with configurable dimensions
- Cells can contain cards, pawns, or remain empty
- Each cell tracks controlling player and pawn count
- Border cells provide strategic positioning advantages

### Card System
- **Unique Cards**: Each card has distinct name, cost, and point value
- **Influence Patterns**: 3x3 grids defining area of effect
- **Placement Rules**: Cards affect adjacent cells based on influence patterns
- **Resource Costs**: Strategic resource management required for optimal play

### Influence Types
- **'X'**: No influence (neutral position)
- **'I'**: Active influence (affects target cells)
- **'C'**: Card center position (placement location)

### Victory Conditions
- Game ends when both players consecutively pass
- Victory determined by territorial control and point accumulation
- Each row winner contributes to overall score
- Tie-breaking rules ensure decisive outcomes

## Example Gameplay

### Board Representation
```
Enhanced Textual View:
2 || R R _ _ 3 || 0
0 || 2 _ _ 1 B || 1  
0 || 1 1 _ 1 B || 3

Legend:
- Letters (R/B): Player control
- Numbers: Pawn counts
- _: Empty cells
- ||: Board boundaries
```

### Turn Sequence
1. **Card Selection**: Choose card from hand
2. **Placement**: Select valid board position
3. **Influence Application**: Card affects surrounding cells
4. **Pawn Updates**: Controlled territories gain pawns
5. **Score Calculation**: Points awarded based on control
6. **Next Player**: Turn alternates until game end

## Technical Implementation

### Design Patterns & Architecture

#### Model-View-Controller (MVC)
- **Model**: `SanguineModel` manages game state and rule enforcement
- **View**: Multiple implementations (`SanguineView`, `SanguineTextualView`)
- **Controller**: `SanguineController` coordinates user interactions

#### Strategy Pattern
- **AI Implementation**: Pluggable strategy system for computer players
- **FillFirstStrategy**: Aggressive territorial expansion approach
- **MaxRowScoreStrategy**: Point-optimization focused gameplay
- **Extensible Framework**: Easy addition of new AI behaviors

#### Observer Pattern
- **ModelStatusListener**: Real-time game state updates
- **View Synchronization**: Automatic UI updates on model changes
- **Event-Driven Architecture**: Loose coupling between components

#### Factory Pattern
- **ConfigFileParser**: Dynamic card creation from configuration files
- **Player Creation**: Flexible player type instantiation
- **Game Initialization**: Parameterized game setup

### Advanced Features

#### Configuration System
- **Deck Files**: External card definitions in structured format
- **Board Customization**: Variable grid sizes and starting positions  
- **Rule Variations**: Configurable game mechanics and win conditions
- **Extensibility**: Plugin architecture for custom rules

#### AI System Architecture
- **Strategy Interface**: Uniform AI behavior definition
- **Move Evaluation**: Sophisticated position assessment algorithms
- **Difficulty Scaling**: Multiple AI intelligence levels
- **Performance Optimization**: Efficient move calculation and caching

#### Testing Framework
- **Mock Objects**: Comprehensive mock system for isolated testing
- **Unit Tests**: Full coverage of individual components
- **Integration Tests**: End-to-end system validation
- **GUI Testing**: Image-based verification for visual components

## Dependencies & Build

### Requirements
- Java 17 or higher
- Maven 3.8+ or Gradle 7.0+
- JUnit 5 for testing
- JavaFX 17+ for GUI components

### Build Instructions

#### Using Maven
```bash
# Clean and compile
mvn clean compile

# Run tests
mvn test

# Package application
mvn package

# Run GUI version
mvn exec:java -Dexec.mainClass="SanguineGame"

# Run textual version
mvn exec:java -Dexec.mainClass="SanguineStorm"
```

#### Using Gradle
```bash
# Clean and build
./gradlew clean build

# Run tests
./gradlew test

# Run GUI application
./gradlew run -PmainClass=SanguineGame

# Run textual application  
./gradlew run -PmainClass=SanguineStorm
```

#### Manual Compilation
```bash
# Compile source
javac -d bin -sourcepath src/main/java src/main/java/**/*.java

# Run applications
java -cp bin SanguineGame        # GUI version
java -cp bin SanguineStorm      # Textual version

# With custom deck
java -cp bin SanguineGame docs/customDeck.deck
```

### Configuration Files

#### Deck Format Example
```
# Card configuration format
CardName,Cost,Points,InfluencePattern
Warrior,2,3,X I X / I C I / X I X
Archer,1,2,I I I / X C X / X X X  
Wizard,3,4,X I X / I C I / I I I
```

#### Game Parameters
- Board dimensions: `width x height`
- Hand size: Number of cards per player
- Deck composition: Card types and quantities
- Victory conditions: Scoring thresholds

## Development Highlights

### Code Quality & Standards
- **Clean Architecture**: Clear separation of concerns across layers
- **Interface-Driven Design**: Extensive use of abstractions for flexibility
- **Defensive Programming**: Comprehensive input validation and error handling
- **Documentation**: Full Javadoc coverage for all public APIs
- **Testing**: High test coverage with mock-based isolation

### Performance Optimizations
- **Efficient Algorithms**: Optimized influence calculation and board evaluation
- **Memory Management**: Minimal object creation during gameplay
- **GUI Responsiveness**: Asynchronous operations for smooth user experience
- **Caching Strategies**: Strategic caching of expensive computations

### Extensibility Features
- **Plugin Architecture**: Easy addition of new game variants
- **Strategy Framework**: Pluggable AI implementations
- **Configuration System**: External game setup without code changes
- **Interface Abstractions**: Support for multiple UI implementations

## AI Strategy Details

### FillFirstStrategy
- **Objective**: Maximize territorial coverage
- **Approach**: Prioritize empty cell occupation
- **Strengths**: Strong early game presence
- **Weaknesses**: May sacrifice point optimization

### MaxRowScoreStrategy  
- **Objective**: Optimize point accumulation per row
- **Approach**: Strategic placement for maximum scoring
- **Strengths**: Efficient resource utilization
- **Weaknesses**: Vulnerable to aggressive expansion

### Custom Strategy Development
```java
public class CustomStrategy implements Strategy {
    @Override
    public Move chooseMove(ReadOnlyModelInterface model, Player player) {
        // Implement custom decision logic
        return new Move(card, row, col);
    }
}
```

## Future Enhancements

### Planned Features
- **Network Multiplayer**: Online gameplay with matchmaking
- **Tournament Mode**: Multi-round competitive play
- **Replay System**: Game recording and analysis tools
- **Advanced AI**: Machine learning-based opponents
- **Mobile Version**: Android/iOS application
- **Spectator Mode**: Real-time game observation

### Technical Improvements
- **Performance Profiling**: Optimization of critical paths
- **Database Integration**: Persistent game state and statistics
- **WebGL Renderer**: Browser-based 3D visualization
- **Voice Commands**: Accessibility improvements
- **Internationalization**: Multi-language support
- **Cloud Saves**: Cross-device game synchronization

### Community Features
- **Deck Sharing**: Community-created card collections
- **Strategy Guides**: Built-in tutorials and tips
- **Leaderboards**: Global and local player rankings
- **Custom Tournaments**: User-organized competitions
- **Modding Support**: Community-developed game modifications

## Contributing

This project demonstrates advanced Java development practices including:

- **Software Architecture**: Professional MVC implementation with clear separation of concerns
- **Design Patterns**: Strategic use of Observer, Strategy, Factory, and MVC patterns
- **GUI Development**: Comprehensive JavaFX application with custom components
- **AI Implementation**: Sophisticated strategy system with pluggable algorithms
- **Testing Methodologies**: Extensive unit and integration testing with mock frameworks
- **Configuration Management**: External file-based system configuration
- **Code Quality**: Clean code principles with comprehensive documentation

The implementation showcases expertise in:
- Object-oriented design and programming
- User interface development and user experience design
- Artificial intelligence and game theory implementation
- Software testing and quality assurance
- Configuration and build system management
- Performance optimization and scalability considerations

---
