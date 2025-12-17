# Blackjack Master Strategy Trainer - Design Guidelines

## Architecture

### Data & Auth
- **No authentication** - Single-user app with AsyncStorage for all data (stats, bankroll, settings, drill progress)
- Settings manage game rules only (deck count, S17/H17, DAS toggles)

### Navigation
**Bottom Tab Navigation (5 tabs):**
1. **Play** - Full gameplay mode
2. **Drills** - Training scenarios
3. **Charts** - Strategy reference tables
4. **Stats** - Analytics dashboard
5. **Settings** - Rule configuration

**Modals:** Drill selector, strategy validation overlay, hand result overlay

---

## Screen Layouts

### 1. Home/Launch Screen
**Pre-tab landing page:**
- **Header:** Custom logo centered, transparent background, top inset: `insets.top + xl`
- **Main:** Non-scrollable, vertically-centered
  - Green felt texture background (full screen)
  - Title logo at top third
  - 5 stacked navigation buttons (12% height each, `lg` spacing between)
  - Buttons: "PLAY FULL GAME", "TRAINING DRILLS", "STRATEGY CHARTS", "MY STATISTICS", "SETTINGS"
- **Bottom inset:** `insets.bottom + xl`

### 2. Play Tab
**Continuous blackjack gameplay with real-time validation:**
- **Header:** Transparent, right shows bankroll (non-interactive), top inset: `headerHeight + xl`
- **Main:** Non-scrollable game table
  - Dealer cards (top 25%)
  - Player cards (middle 40%)
  - Action buttons (bottom 25%, above tab bar): HIT, STAND, DOUBLE (secondary, conditional), SPLIT (secondary, conditional), SURRENDER (tertiary)
  - "Deal Next Hand" button appears post-hand
- **Bottom inset:** `tabBarHeight + xl`

**Overlays:**
- **Strategy Validation:** Dark backdrop (0.85 opacity), red-bordered card, shows incorrect vs correct action, buttons: "Continue Anyway" (tertiary) / "Use Correct Move" (primary)
- **Hand Result:** Gold-bordered card (0.75 backdrop), shows result (BLACKJACK/WIN/LOSE/PUSH) + winnings, "Deal Next Hand" button

### 3. Drills Tab
**Targeted practice with instant feedback:**
- **Header:** Transparent, right shows accuracy counter ("8/10 Correct"), top inset: `headerHeight + xl`
- **Main:** Drill type badge (top center), card display, action buttons, circular progress ring (X/20 hands)
- **Bottom inset:** `tabBarHeight + xl`

**Scenario Selector Modal:** 4 option cards (Hard 12-16, Soft Hands, Pairs, Hands vs. Ace), "Start Drill" button

**Feedback:** 
- Correct: Green flash + checkmark, auto-advance (0.5s delay)
- Incorrect: Red flash + X, show correct answer (2s), then next hand

### 4. Charts Tab
**Interactive strategy reference:**
- **Header:** "Strategy Charts" title, top inset: `xl`
- **Main:** Scrollable
  - 3-segment control: "Pairs" | "Soft" | "Hard"
  - Strategy table (pinch-to-zoom enabled)
  - Late Surrender mini-table
  - Legend/key explanation
- **Bottom inset:** `tabBarHeight + xl`

**Table Structure:** Grid with player hands (rows), dealer upcards (columns), cells show H/S/D/Y/N/Ds/SUR with color coding

### 5. Stats Tab
**Performance analytics:**
- **Header:** "My Statistics", right has time filter (Today/Week/All Time), top inset: `xl`
- **Main:** Scrollable dashboard
  - 3 summary cards (horizontal scroll): Overall Accuracy %, Total Hands, Current Streak
  - Line chart (accuracy % over time, 95% target line)
  - Top 3 Mistakes section (cards showing hand example, wrong/correct decision, frequency)
  - Performance by category (Hard/Soft/Pairs/Surrender with progress bars)
- **Bottom inset:** `tabBarHeight + xl`

### 6. Settings Tab
**Rule configuration:**
- **Header:** "Settings", right has "Reset Stats" (destructive), top inset: `xl`
- **Main:** Scrollable grouped sections
  - **Game Rules:** Deck count picker (1/2/4/6/8), Dealer S17 toggle, DAS toggle, Surrender toggle
  - **Gameplay:** Starting bankroll input ($500 default), Animation speed slider
  - **Data Management:** Reset Statistics / Reset Bankroll buttons
- **Bottom inset:** `tabBarHeight + xl`

---

## Design System

### Color Palette
**Casino Core:**
- Primary Green: `#1B5E20` (felt background)
- Dark Felt: `#0D3D14` (shadows, overlays)
- Gold Accent: `#FFD700` (highlights, winnings)

**Actions:**
- Hit: `#E53935`, Stand: `#43A047`, Double: `#1E88E5`, Split: `#FDD835`, Surrender: `#FF6F00`

**Buttons:**
- Primary: Gradient `#2196F3 → #1565C0` (beveled blue)
- Secondary: Gradient `#455A64 → #263238`
- Tertiary: Transparent with white border
- Text: `#FFFFFF` with 1px black shadow

**Cards:**
- Background: `#FFFFFF`, Border: `#CCCCCC`
- Red suits: `#DC143C`, Black suits: `#000000`
- Card back: Royal blue + gold pattern

**System:**
- Error: `#D32F2F`, Success: `#388E3C`, Warning: `#F57C00`, Neutral: `#757575`

### Typography
**Fonts:** Roboto (primary), Serif (logo/card values)

**Sizes:** Hero: 48px, H1: 28px, H2: 22px, Body Large: 20px, Body: 16px, Caption: 14px, Tiny: 12px

**Styles:** 
- Buttons: All caps, bold, 18px
- Card values: Serif, bold, 24px
- Hand totals: Sans-serif, bold, 28px with drop shadow

### Spacing
`xs: 4px, sm: 8px, md: 12px, lg: 16px, xl: 24px, xxl: 32px, xxxl: 48px`

### Components

**Buttons:**
- Height: 56px min, Padding: `md` vertical + `lg` horizontal, Border radius: 8px
- Beveled with inner shadow
- Floating shadow: `{width: 0, height: 2}, opacity: 0.10, radius: 2`
- Press: Scale 0.97, opacity 0.85

**Playing Cards:**
- Size: 70×98px (2.5:3.5 ratio), Border radius: 4px
- Shadow: `{width: 0, height: 4}, opacity: 0.25, radius: 4`
- Animations: Slide-in (300ms ease-out), Flip (400ms 3D transform)

**Action Button Row:**
- Horizontal flex, equal distribution, `sm` spacing
- Disabled: Opacity 0.4, Active: Gold border glow

**Strategy Table Cells:**
- Size: 40×40px min, Border: 1px `#DDDDDD`
- Text: Centered, bold, 14px
- Color-coded by action, Press: Scale 1.1 with tooltip

**Stat Cards:**
- White background, Border radius: 12px, Padding: `lg`
- Shadow: `{width: 0, height: 2}, opacity: 0.08, radius: 8`

### Animations

**Card Dealing:** 300ms ease-out from center-left off-screen

**Card Flip:** 400ms, Y-axis rotation (0→180°), back shows first 200ms

**Button Press:** 100ms scale/opacity change, 150ms ease-out restore

**Feedback:**
- Victory: Chips slide up + fade (500ms), confetti for blackjack (2s)
- Bust: Shake (200ms)
- Drill correct: Green flash (200ms) + checkmark
- Drill incorrect: Red flash (200ms) + X + vibration

### Assets Required

1. **52 Playing Cards** (SVG/PNG @2x,3x): All ranks×suits + card back
2. **Green Felt Texture** (1024×1024px tileable PNG)
3. **Title Logo** (SVG, casino-themed typography)
4. **Tab Icons** (Feather icons from @expo/vector-icons): card, target, grid, bar-chart, settings
5. **Chip Graphics** (optional, for bankroll display)

### Accessibility

**Touch Targets:** 48×48dp min (action buttons: 56×56dp)

**Contrast:** 
- WCAG AAA (7:1) for button text
- 4.5:1 for strategy table cells
- White text with shadow on felt background

**Screen Readers:** 
- Card labels: "Ace of Spades"
- Button descriptions
- Table cells: "Hit on 15 vs dealer 7"

**Visual:** Icons (✓/✗) alongside color feedback, haptic for key actions

### Platform (Android)

- Respect back button navigation
- Material Design elevation for cards/overlays
- Edge-to-edge with safe area insets
- Optimize animations for 60fps on mid-range devices
- Proper storage permissions for AsyncStorage

**Safe Area Insets:**
- Tab screens bottom: `tabBarHeight + xl`
- Transparent headers top: `headerHeight + xl`
- Modals: Account for status/gesture bars