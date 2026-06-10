# Retirement Calculator - Node-Based Web App

## Project Overview
- **Project name:** Retirement Calculator
- **Type:** Single-page web application (HTML/CSS/JS)
- **Core functionality:** A visual node-based calculator where users connect financial nodes to model retirement scenarios
- **Target users:** Individuals planning retirement who want a visual way to model different financial scenarios

## UI/UX Specification

### Layout Structure
- **Full viewport canvas** - Dark theme with infinite-feel pan/zoom canvas
- **Toolbar** - Fixed top bar with node addition buttons and calculate action
- **Node panel** - Floating panel on left side for adding new nodes
- **Results panel** - Floating panel on right side showing calculation results

### Responsive Breakpoints
- Desktop-first (1200px+)
- Tablet (768px-1199px): Slightly smaller nodes
- Mobile (< 768px): Not primary target, basic support

### Visual Design

#### Color Palette
- **Background (canvas):** `#0d1117` (deep dark blue-black)
- **Panel background:** `#161b22` (dark gray)
- **Node background:** `#21262d` (slightly lighter gray)
- **Node border default:** `#30363d` (subtle border)
- **Node border selected:** `#58a6ff` (bright blue)
- **Accent primary:** `#238636` (green - for Starting Data node)
- **Accent expense:** `#da3633` (red - for One Time Expense)
- **Accent income:** `#3fb950` (bright green - for One Time Income)
- **Accent monthly:** `#a371f7` (purple - for Monthly Finances)
- **Text primary:** `#e6edf3` (off-white)
- **Text secondary:** `#8b949e` (muted gray)
- **Connection lines:** `#58a6ff` (blue)

#### Typography
- **Font family:** `"JetBrains Mono", "Fira Code", monospace` for a developer/terminal aesthetic
- **Node title:** 14px, bold, uppercase, letter-spacing 1px
- **Field labels:** 11px, uppercase, letter-spacing 0.5px
- **Field values:** 14px, regular

#### Spacing System
- **Node padding:** 16px
- **Node gap:** 12px between fields
- **Canvas grid:** 20px snap grid
- **Toolbar height:** 56px

#### Visual Effects
- **Node shadow:** `0 4px 12px rgba(0,0,0,0.4)`
- **Node hover:** Slight scale (1.02) with transition 0.15s
- **Connection lines:** Bezier curves, 2px stroke, animated dash on hover
- **Panel backdrop blur:** 12px

### Components

#### Toolbar
- Logo/title on left
- "Calculate" button (prominent green)
- "Clear All" button (subtle red)
- Zoom controls (+/- and fit)

#### Node Types

1. **Starting Data Node** (always present by default, green accent)
   - Fields: Starting Capital ($), Age (years)
   - One output port (to connect to other nodes)
   - Delete button (X) - cannot delete the last one or first one

2. **One Time Expense Node** (red accent)
   - Field: Description (text), Amount ($)
   - Input port (from Starting Data or other nodes)
   - Output port (to next node)
   - Delete button

3. **One Time Income Node** (green accent)
   - Field: Description (text), Amount ($)
   - Input port, Output port
   - Delete button

4. **Monthly Finances Node** (purple accent)
   - Fields: Monthly Income ($), Monthly Expenses ($), Annual Return (%), Inflation (%), Years (number)
   - Input port, Output port
   - Delete button

#### Node States
- **Default:** Standard colors
- **Hover:** Slight glow, elevated shadow
- **Selected:** Blue border glow, elevated z-index
- **Dragging:** Even more elevated (shadow increases), slight opacity

#### Connection Ports
- Small circles (12px) on node edges
- Input on left edge, Output on right edge
- Hover: Scale up to 16px, brighter color
- Connected: Filled with connection color

#### Results Panel
- Shows calculated retirement projections
- Year-by-year breakdown table
- Key metrics: Final Balance, Total Incomes, Total Expenses, Net Worth trajectory

### Animations
- Node drag: Smooth with slight lag
- Connection creation: Animated line drawing
- Panel open/close: Slide + fade
- Calculate button: Pulse animation when results update

## Functionality Specification

### Core Features

1. **Canvas Interaction**
   - Pan canvas by dragging background
   - Zoom with scroll wheel or buttons
   - Grid snapping for node placement

2. **Node Management**
   - Add nodes from toolbar panel
   - Drag nodes to reposition
   - Delete nodes with X button or keyboard (Delete/Backspace)
   - Double-click node title to edit

3. **Node Connections**
   - Click and drag from output port to input port to connect
   - Click connection line to delete
   - Visual feedback during connection drag

4. **Data Calculation**
   - "Calculate" button runs the retirement projection
   - Chain calculation through connected nodes
   - Display results in results panel

### Calculation Logic

1. **Starting Data** provides base capital and starting age
2. **One Time Expenses** subtract from capital at the age specified (or immediate if not specified - we'll add an "Age" field to expense/income nodes)
3. **One Time Incomes** add to capital
4. **Monthly Finances** runs the compound projection:
   - For each month: balance = balance * (1 + annualReturn/12) + monthlyIncome - monthlyExpenses
   - Apply inflation adjustment to expenses annually
   - Project for specified years from starting age

Let me add an "Age" field to One Time Expense and One Time Income nodes so they can be scheduled at the right time.

### User Interactions
- Click node to select
- Shift+click for multi-select
- Drag selected nodes together
- Right-click node for context menu (delete)
- Keyboard: Delete key removes selected nodes

### Edge Cases
- No Starting Data node: Show error, require one
- Disconnected nodes: Warn user or ignore in calculation
- Circular connections: Prevent, show error
- Empty fields: Default to 0 or show validation error

## Acceptance Criteria

1. ✅ Canvas loads with one Starting Data node by default
2. ✅ Can add all 4 node types via toolbar buttons
3. ✅ Nodes are draggable and snap to grid
4. ✅ Can delete any node except the last Starting Data
5. ✅ Can connect nodes by dragging between ports
6. ✅ Connections render as smooth bezier curves
7. ✅ Calculate button produces meaningful retirement projection
8. ✅ Results display in a clear, readable format
9. ✅ UI is dark-themed and visually polished
10. ✅ Works smoothly on desktop browsers