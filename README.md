# One Piece Card Game PWA - Architecture and Technical Stack

## 1. Overview

This document outlines the architecture and technical stack for the One Piece Card Game Progressive Web App (PWA). The application will be a client-side game, installable on Android devices via the browser, and will function offline.

## 2. Key Requirements (from PRD)

-   **Installable PWA:** Must be installable via browser ("Add to Home Screen") and function in full-screen standalone mode.
-   **Offline Support:** The app should work fully offline after installation using Service Worker caching.
-   **Client-Side Only:** No backend or server-side logic is required. No data persistence between sessions.
-   **Asset Management:** Card images (PNG) are stored locally and loaded dynamically.
-   **Target Platform:** Android (installable PWA) and Web (via browser).
-   **Responsive Design:** Optimized for Android screens, adaptable to browser sizes.

## 3. Technical Stack

-   **Frontend Framework:** React.js
    -   Chosen for its component-based architecture, efficient rendering, and suitability for single-page applications (SPAs).
    -   We will use the `create_react_app` template which includes Tailwind CSS, shadcn/ui, Lucide icons, and Recharts, providing a good starting point for UI development and styling.
-   **Programming Language:** JavaScript (ES6+)
-   **Styling:** Tailwind CSS (included with the chosen React template)
    -   A utility-first CSS framework that will help in creating a responsive and themed UI quickly, aligning with the One Piece visual style.
-   **PWA Features:**
    -   **Web App Manifest (`manifest.json`):** To define app properties like name, icons, start URL, display mode (standalone), and orientation.
    -   **Service Worker (`service-worker.js`):** To cache app assets (HTML, CSS, JS, images) for offline functionality and to manage app updates.
-   **Build Tool:** The `create_react_app` template uses `pnpm` for package management and provides scripts for development and production builds.
-   **Version Control:** Git (will be initialized by `create_react_app`).

## 4. Application Architecture

### 4.1. Project Structure (based on `create_react_app`)

```
/one_piece_pwa
|-- public/
|   |-- index.html
|   |-- manifest.json
|   |-- assets/
|   |   |-- app-background.png
|   |   |-- cards/  (Directory for card images, e.g., card1.png, card2.png, ...)
|   |   |-- icons/ (App icons for manifest)
|-- src/
|   |-- components/         # Reusable UI components (e.g., Card, Button, WinBlock)
|   |   |-- StartScreen.js
|   |   |-- BattleScreen.js
|   |   |-- CardDisplay.js
|   |   |-- WinBlock.js
|   |-- App.js              # Main application component, routing
|   |-- index.js            # Entry point, renders App component
|   |-- service-worker.js   # Custom service worker logic
|   |-- setupTests.js
|   |-- reportWebVitals.js
|   |-- styles/             # Global styles, Tailwind config
|   |   |-- global.css
|-- .gitignore
|-- package.json
|-- pnpm-lock.yaml
|-- README.md
```

### 4.2. Core Components

-   **`App.js`:** Main container, handles routing between Start Screen and Battle Screen.
-   **`StartScreen.js`:** Displays the initial screen with the "Play" button.
-   **`BattleScreen.js`:** Manages the main game state and logic.
    -   Displays the current card.
    -   Manages the three Win Blocks.
    -   Handles Shuffle and Win button interactions.
    -   Tracks shuffle count.
    -   Implements game logic as per PRD (shuffling, winning, new match).
-   **`CardDisplay.js`:** Component to display a single card image and its details (if any, though designs show only image).
-   **`WinBlock.js`:** Component to display a card in a win slot.
-   **Button Components:** Reusable button components for "Play", "Shuffle", "Win", "New Match".

### 4.3. State Management

-   Given the simplicity of the game (no backend, no session persistence), React's built-in state management (useState, useContext, or useReducer hooks) will be sufficient.
-   Game state (current card, shuffle count, win block contents) will be managed within the `BattleScreen.js` component or lifted to a shared context if necessary.

### 4.4. Asset Handling

-   The `app-background.png` will be used as the main background for the application.
-   Other provided design screen images (`Start Screen.png`, `Battle Screen - Initial.png`, etc.) are for reference and will not be directly used as assets in the app, but their styles and layouts will be replicated.
-   Card images (not provided yet, but PRD states they are PNGs in `assets/cards/`) will be placed in `public/assets/cards/`. The application will dynamically list and load these images.
-   App icons for the PWA manifest will need to be created or sourced, adhering to One Piece branding.

### 4.5. Service Worker and Offline Caching

-   A service worker will be implemented to cache the application shell (HTML, CSS, JS) and all static assets (background image, card images, icons).
-   This will ensure the app loads quickly on subsequent visits and works offline.
-   The `create_react_app` template provides a basic service worker setup that can be customized.

## 5. UI/UX Considerations (from PRD and Designs)

-   **Theme:** Adhere to the One Piece pirate theme using the provided background and emulating the style from design screens.
-   **Colors:** Use colors from the design screens (e.g., green for Play/Win buttons, purple for Shuffle button).
-   **Layout:** Replicate the layouts shown in the design screens for Start and Battle screens, including button placements, card display area, and win blocks.
-   **Responsiveness:** Ensure the layout adapts well to different Android screen sizes and common browser dimensions.
-   **Animations:** Implement subtle animations for card transitions and button states as suggested in the PRD.
-   **Feedback:** Provide clear visual feedback for actions like filling win blocks or starting a new match.

## 6. Development Workflow

1.  **Setup:** Initialize React project using `create_react_app one_piece_pwa`.
2.  **Asset Integration:** Copy `app-background.png` to `public/assets/`. Create `public/assets/cards/` and `public/assets/icons/`.
3.  **Component Development:** Create React components for each UI element and screen.
4.  **Styling:** Apply Tailwind CSS to match the design screens.
5.  **Game Logic:** Implement the card shuffling, winning, and new match logic in `BattleScreen.js`.
6.  **PWA Configuration:** Create `manifest.json` and configure the service worker for offline support.
7.  **Testing:** Thoroughly test on target devices (Android via browser) and different web browsers.
8.  **Deployment:** Deploy as a static website.

This architecture provides a solid foundation for developing the One Piece Card Game PWA as per the requirements.
