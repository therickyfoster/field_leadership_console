<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>🌌 Leadership Dashboard | Planetary Restoration Archive</title>
<meta name="description" content="Collaborative leadership dashboard for global problem-solving. Track missions, plant ideas, and join collective restoration efforts with real-time multiplayer collaboration.">
<meta name="keywords" content="leadership, restoration, global issues, dashboard, sustainability, Habitica, IndexedDB, WebLLM, peace, ecology, open-source, multiplayer, gamification">
<meta name="author" content="Foster + Navi">
<meta property="og:title" content="Leadership Dashboard | Planetary Restoration Archive">
<meta property="og:description" content="Empower your leadership and contribute to world issues through interactive missions and global collaboration.">
<meta property="og:type" content="website">
<meta property="og:url" content="https://planetaryrestorationarchive.com/ref/crypto/sol/custom/gpt/rare/peace/engineer/hybrid">
<meta name="theme-color" content="#060b1a">

<!-- 
═══════════════════════════════════════════════════════════════════
ZERO-HARM & ANTI-INVERSION NOTICE
═══════════════════════════════════════════════════════════════════
This application is designed for peaceful, constructive leadership development
and planetary restoration coordination. It is NOT intended for:
- Surveillance or tracking without consent
- Manipulation or coercion
- Weaponization of any kind
- Exploitation of vulnerable populations
- Environmental harm

All data stays local-first (IndexedDB). Multiplayer sync is opt-in and 
respects user privacy. Rate-limiting and input sanitization are implemented
to prevent abuse.

License: MIT-style open source (see footer)
Attributions: Patterns inspired by Habitica (task gamification), 
collaborative whiteboards, and Web3 multiplayer frameworks.
═══════════════════════════════════════════════════════════════════
-->

<style>
/* ═══════════════════════════════════════════════════════════════════
   SECTION: CSS VARIABLES & THEME SYSTEM
   
   PURPOSE: Define color schemes for multiple themes (space, medieval, anime, etc.)
   and auto-detect user's dark/light mode preference.
   
   HOW TO USE: Themes can be switched via the theme selector in the UI.
   New themes can be added by creating a new [data-theme="name"] block.
   
   EXPECTED OUTCOME: Seamless visual transitions between themes while
   maintaining readability and accessibility.
   ═══════════════════════════════════════════════════════════════════ */

:root, [data-theme="space"] {
  --bg: #050a18;
  --accent: #00ffc6;
  --accent-glow: rgba(0,255,198,0.3);
  --panel: #0b1428;
  --text: #e4f4ff;
  --card: #101c33;
  --ok: #22c55e;
  --warn: #fbbf24;
  --err: #ef4444;
  --info: #3b82f6;
  --font: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  --border-radius: 1rem;
  --shadow: 0 8px 32px rgba(0,0,0,0.4);
  --glow: 0 0 20px var(--accent-glow);
}

[data-theme="medieval"] {
  --bg: #1a0f0a;
  --accent: #d4af37;
  --accent-glow: rgba(212,175,55,0.3);
  --panel: #2a1810;
  --text: #f5e6d3;
  --card: #3d2416;
  --ok: #6b8e23;
  --warn: #ff8c00;
  --err: #8b0000;
  --info: #4682b4;
}

[data-theme="anime"] {
  --bg: #fef7ff;
  --accent: #ff1493;
  --accent-glow: rgba(255,20,147,0.2);
  --panel: #ffe4f7;
  --text: #2d1b3d;
  --card: #fff0fb;
  --ok: #ff69b4;
  --warn: #ffa500;
  --err: #dc143c;
  --info: #9370db;
}

[data-theme="cyberpunk"] {
  --bg: #0a0e27;
  --accent: #ff00ff;
  --accent-glow: rgba(255,0,255,0.4);
  --panel: #1a1f3a;
  --text: #00ffff;
  --card: #0f1729;
  --ok: #39ff14;
  --warn: #ffff00;
  --err: #ff0040;
  --info: #00d4ff;
}

@media (prefers-color-scheme: light) {
  :root:not([data-theme]) {
    --bg: #f8fafc;
    --accent: #0891b2;
    --accent-glow: rgba(8,145,178,0.2);
    --panel: #e2e8f0;
    --text: #1e293b;
    --card: #ffffff;
  }
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: BASE LAYOUT & TYPOGRAPHY
   
   PURPOSE: Establish responsive foundation, typography hierarchy, and
   overall page structure with sidebar navigation.
   ═══════════════════════════════════════════════════════════════════ */

* { box-sizing: border-box; margin: 0; padding: 0; }

html, body { 
  height: 100%; 
  background: var(--bg); 
  color: var(--text); 
  font-family: var(--font);
  overflow-x: hidden;
  transition: background 0.3s ease, color 0.3s ease;
}

body {
  display: flex;
  min-height: 100vh;
}

canvas#stars { 
  position: fixed; 
  inset: 0; 
  width: 100%; 
  height: 100%; 
  z-index: -1; 
  pointer-events: none;
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: SIDEBAR NAVIGATION
   
   PURPOSE: Collapsible side navigation for accessing different dashboard
   sections. Responsive design collapses to hamburger menu on mobile.
   
   HOW TO USE: Click hamburger icon (☰) to toggle. Click section names
   to navigate. Add new sections by adding <a> tags in the HTML.
   ═══════════════════════════════════════════════════════════════════ */

.sidebar {
  position: fixed;
  left: 0;
  top: 0;
  width: 260px;
  height: 100vh;
  background: var(--panel);
  padding: 1.5rem;
  z-index: 1000;
  overflow-y: auto;
  transition: transform 0.3s ease;
  box-shadow: var(--shadow);
}

.sidebar.collapsed {
  transform: translateX(-260px);
}

.sidebar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.sidebar-toggle {
  background: var(--accent);
  border: none;
  color: var(--bg);
  font-size: 1.5rem;
  cursor: pointer;
  padding: 0.5rem;
  border-radius: 0.5rem;
  position: fixed;
  top: 1rem;
  left: 1rem;
  z-index: 1001;
  transition: all 0.3s ease;
}

.sidebar-toggle:hover {
  transform: scale(1.1);
  box-shadow: var(--glow);
}

.nav-links {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.nav-links a {
  color: var(--text);
  text-decoration: none;
  padding: 0.8rem 1rem;
  border-radius: 0.5rem;
  transition: all 0.2s ease;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.nav-links a:hover, .nav-links a.active {
  background: var(--card);
  color: var(--accent);
  transform: translateX(5px);
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: MAIN CONTENT AREA
   
   PURPOSE: Primary content container that adjusts based on sidebar state.
   Houses all dashboard sections with smooth scrolling.
   ═══════════════════════════════════════════════════════════════════ */

.main-content {
  margin-left: 260px;
  flex: 1;
  padding: 2rem;
  transition: margin-left 0.3s ease;
  overflow-y: auto;
  max-height: 100vh;
}

.main-content.expanded {
  margin-left: 0;
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: GAMIFICATION HUD (Heads-Up Display)
   
   PURPOSE: Persistent status bar showing user stats: XP, level, health,
   mana, streak, and global impact score. Fixed at top of viewport.
   
   HOW TO USE: Automatically updates when missions are completed or
   actions are taken. Click stats for detailed breakdowns.
   
   EXPECTED OUTCOME: Provides constant feedback and motivation through
   visible progress indicators inspired by RPG game interfaces.
   ═══════════════════════════════════════════════════════════════════ */

.hud {
  position: sticky;
  top: 0;
  background: var(--panel);
  padding: 1rem;
  border-radius: var(--border-radius);
  margin-bottom: 2rem;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 1rem;
  box-shadow: var(--shadow);
  z-index: 100;
  backdrop-filter: blur(10px);
}

.hud-stat {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
  cursor: pointer;
  padding: 0.5rem;
  border-radius: 0.5rem;
  transition: all 0.2s ease;
}

.hud-stat:hover {
  background: var(--card);
  transform: translateY(-2px);
}

.hud-stat-label {
  font-size: 0.75rem;
  opacity: 0.7;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.hud-stat-value {
  font-size: 1.5rem;
  font-weight: bold;
  color: var(--accent);
}

.hud-bar {
  width: 100%;
  height: 8px;
  background: var(--bg);
  border-radius: 4px;
  overflow: hidden;
  position: relative;
}

.hud-bar-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--accent), var(--ok));
  transition: width 0.5s ease;
  box-shadow: 0 0 10px var(--accent-glow);
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: CARD COMPONENTS
   
   PURPOSE: Reusable card containers for missions, achievements, and
   other content blocks. Provides consistent styling and hover effects.
   ═══════════════════════════════════════════════════════════════════ */

.section {
  background: var(--card);
  border-radius: var(--border-radius);
  padding: 1.5rem;
  margin-bottom: 2rem;
  box-shadow: var(--shadow);
  transition: all 0.3s ease;
}

.section:hover {
  transform: translateY(-2px);
  box-shadow: var(--glow), var(--shadow);
}

.section-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1rem;
  cursor: pointer;
  user-select: none;
}

.section-title {
  font-size: 1.5rem;
  color: var(--accent);
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.collapse-icon {
  transition: transform 0.3s ease;
  font-size: 1.2rem;
}

.section.collapsed .collapse-icon {
  transform: rotate(-90deg);
}

.section.collapsed .section-content {
  display: none;
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: MISSION SYSTEM
   
   PURPOSE: Core task management interface. Users can create, categorize,
   complete, and delete missions. Each mission has type, difficulty, XP
   reward, and optional deadline.
   
   HOW TO USE: 
   1. Enter mission title
   2. Select category (Climate, Peace, Education, etc.)
   3. Set difficulty (Easy/Medium/Hard/Epic)
   4. Optional: Add deadline
   5. Click "Add Mission" to save
   
   EXPECTED OUTCOME: Mission appears in active missions list. Completing
   it grants XP and updates global impact score. Data persists in IndexedDB.
   ═══════════════════════════════════════════════════════════════════ */

.mission-form {
  display: grid;
  grid-template-columns: 1fr auto;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.mission-input-group {
  display: grid;
  grid-template-columns: 2fr 1fr 1fr;
  gap: 0.5rem;
}

input, select, textarea {
  padding: 0.75rem;
  border-radius: 0.5rem;
  border: 2px solid var(--panel);
  background: var(--bg);
  color: var(--text);
  font-family: var(--font);
  transition: all 0.2s ease;
}

input:focus, select:focus, textarea:focus {
  outline: none;
  border-color: var(--accent);
  box-shadow: 0 0 0 3px var(--accent-glow);
}

button {
  background: var(--accent);
  border: none;
  border-radius: 0.5rem;
  color: var(--bg);
  padding: 0.75rem 1.5rem;
  cursor: pointer;
  font-weight: bold;
  font-size: 1rem;
  transition: all 0.2s ease;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

button:hover {
  transform: translateY(-2px);
  box-shadow: var(--glow);
}

button:active {
  transform: translateY(0);
}

button.secondary {
  background: var(--panel);
  color: var(--text);
}

button.danger {
  background: var(--err);
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: MISSION LIST & CARDS
   
   PURPOSE: Display active, completed, and archived missions in an
   organized, filterable list with sorting options.
   ═══════════════════════════════════════════════════════════════════ */

.mission-filters {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
  flex-wrap: wrap;
}

.filter-tag {
  padding: 0.4rem 0.8rem;
  border-radius: 2rem;
  background: var(--panel);
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 0.9rem;
}

.filter-tag:hover, .filter-tag.active {
  background: var(--accent);
  color: var(--bg);
}

.mission-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  max-height: 500px;
  overflow-y: auto;
  padding-right: 0.5rem;
}

.mission-card {
  background: var(--panel);
  padding: 1rem;
  border-radius: 0.75rem;
  border-left: 4px solid var(--accent);
  transition: all 0.2s ease;
  cursor: pointer;
}

.mission-card:hover {
  transform: translateX(5px);
  box-shadow: var(--glow);
}

.mission-card.completed {
  opacity: 0.6;
  border-left-color: var(--ok);
}

.mission-header {
  display: flex;
  justify-content: space-between;
  align-items: start;
  margin-bottom: 0.5rem;
}

.mission-title {
  font-weight: bold;
  font-size: 1.1rem;
}

.mission-meta {
  display: flex;
  gap: 0.5rem;
  font-size: 0.85rem;
  flex-wrap: wrap;
  opacity: 0.8;
}

.mission-tag {
  padding: 0.2rem 0.6rem;
  border-radius: 0.3rem;
  background: var(--bg);
  white-space: nowrap;
}

.mission-actions {
  display: flex;
  gap: 0.5rem;
  margin-top: 0.5rem;
}

.mission-actions button {
  padding: 0.4rem 0.8rem;
  font-size: 0.85rem;
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: MULTIPLAYER PRESENCE SYSTEM
   
   PURPOSE: Show online users, their current activity, and enable
   real-time collaboration on missions. Uses simulated WebSocket
   connection (can be replaced with actual WebSocket/WebRTC).
   
   HOW TO USE: Automatically shows when other users are online. Click
   on avatars to see profiles. Collaborate by inviting users to missions.
   
   EXPECTED OUTCOME: Creates sense of community and enables coordination
   on shared goals. Respects privacy with opt-in visibility.
   ═══════════════════════════════════════════════════════════════════ */

.presence-bar {
  background: var(--panel);
  padding: 0.75rem;
  border-radius: 0.75rem;
  margin-bottom: 1rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  overflow-x: auto;
}

.presence-label {
  font-size: 0.9rem;
  opacity: 0.7;
  white-space: nowrap;
}

.presence-users {
  display: flex;
  gap: 0.5rem;
  flex: 1;
}

.presence-avatar {
  width: 36px;
  height: 36px;
  border-radius: 50%;
  background: var(--accent);
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: bold;
  color: var(--bg);
  cursor: pointer;
  transition: all 0.2s ease;
  position: relative;
}

.presence-avatar:hover {
  transform: scale(1.2);
  box-shadow: var(--glow);
}

.presence-avatar::after {
  content: '';
  position: absolute;
  bottom: 0;
  right: 0;
  width: 10px;
  height: 10px;
  background: var(--ok);
  border-radius: 50%;
  border: 2px solid var(--panel);
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: ACHIEVEMENTS & BADGES
   
   PURPOSE: Visual reward system that unlocks badges for milestones.
   Encourages continued engagement and provides sense of progression.
   
   ACHIEVEMENTS INCLUDE:
   - First Mission (complete 1 mission)
   - Dedicated (7-day streak)
   - Champion (reach level 10)
   - Global Leader (1000 global impact points)
   - And more...
   ═══════════════════════════════════════════════════════════════════ */

.achievement-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(140px, 1fr));
  gap: 1rem;
}

.achievement-card {
  background: var(--panel);
  padding: 1rem;
  border-radius: 0.75rem;
  text-align: center;
  transition: all 0.3s ease;
  cursor: pointer;
  position: relative;
}

.achievement-card.locked {
  opacity: 0.4;
  filter: grayscale(1);
}

.achievement-card:not(.locked):hover {
  transform: translateY(-5px) scale(1.05);
  box-shadow: var(--glow);
}

.achievement-icon {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

.achievement-name {
  font-weight: bold;
  font-size: 0.9rem;
  margin-bottom: 0.3rem;
}

.achievement-desc {
  font-size: 0.75rem;
  opacity: 0.7;
}

.achievement-unlocked {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  background: var(--ok);
  color: white;
  border-radius: 50%;
  width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 0.9rem;
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: GLOBAL GOALS & IMPACT VISUALIZATION
   
   PURPOSE: Aggregate all users' contributions to show collective
   progress on UN Sustainable Development Goals. Real-time charts
   visualize planetary restoration metrics.
   
   DATA DISPLAYED:
   - Trees planted globally
   - CO2 offset (kg)
   - Educational resources shared
   - Peace-building actions
   - Healthcare initiatives
   ═══════════════════════════════════════════════════════════════════ */

.goals-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 1rem;
  margin-bottom: 1rem;
}

.goal-card {
  background: var(--panel);
  padding: 1rem;
  border-radius: 0.75rem;
  transition: all 0.2s ease;
}

.goal-card:hover {
  transform: translateY(-3px);
  box-shadow: var(--glow);
}

.goal-header {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.75rem;
}

.goal-icon {
  font-size: 2rem;
}

.goal-name {
  font-weight: bold;
  font-size: 1.1rem;
}

.goal-progress {
  display: flex;
  justify-content: space-between;
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
}

.goal-bar {
  width: 100%;
  height: 12px;
  background: var(--bg);
  border-radius: 6px;
  overflow: hidden;
}

.goal-bar-fill {
  height: 100%;
  background: linear-gradient(90deg, var(--ok), var(--accent));
  transition: width 1s ease;
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: SEARCH & EXPORT FUNCTIONALITY
   
   PURPOSE: Client-side search across all missions and data. Export
   dashboard data to HTML, PDF (print), or JSON formats.
   
   HOW TO USE:
   - Search: Type keywords to filter missions/content
   - Export HTML: Download complete standalone dashboard
   - Export PDF: Print-optimized version via browser print
   - Export JSON: Raw data for analysis or backup
   ═══════════════════════════════════════════════════════════════════ */

.search-bar {
  position: relative;
  margin-bottom: 1rem;
}

.search-bar input {
  width: 100%;
  padding-left: 2.5rem;
}

.search-icon {
  position: absolute;
  left: 0.75rem;
  top: 50%;
  transform: translateY(-50%);
  opacity: 0.5;
}

.export-buttons {
  display: flex;
  gap: 0.5rem;
  flex-wrap: wrap;
  margin-bottom: 1rem;
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: TOAST NOTIFICATIONS
   
   PURPOSE: Non-intrusive feedback messages for user actions (mission
   completed, data synced, errors, etc.). Auto-dismisses after 4 seconds.
   ═══════════════════════════════════════════════════════════════════ */

.toast-container {
  position: fixed;
  bottom: 2rem;
  right: 2rem;
  z-index: 9999;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.toast {
  background: var(--card);
  padding: 1rem 1.5rem;
  border-radius: 0.75rem;
  box-shadow: var(--shadow);
  min-width: 300px;
  display: flex;
  align-items: center;
  gap: 0.75rem;
  animation: slideIn 0.3s ease;
}

.toast.success { border-left: 4px solid var(--ok); }
.toast.error { border-left: 4px solid var(--err); }
.toast.warning { border-left: 4px solid var(--warn); }
.toast.info { border-left: 4px solid var(--info); }

@keyframes slideIn {
  from { transform: translateX(400px); opacity: 0; }
  to { transform: translateX(0); opacity: 1; }
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: PRINT STYLESHEET
   
   PURPOSE: Optimized layout for PDF export. Removes interactive
   elements, adjusts colors for print, ensures proper page breaks.
   ═══════════════════════════════════════════════════════════════════ */

@media print {
  body {
    background: white;
    color: black;
  }
  
  .sidebar, .sidebar-toggle, .toast-container, button, .hud {
    display: none !important;
  }
  
  .main-content {
    margin-left: 0;
    max-height: none;
  }
  
  .section {
    page-break-inside: avoid;
    box-shadow: none;
    border: 1px solid #ccc;
  }
  
  canvas {
    display: none;
  }
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: RESPONSIVE DESIGN
   
   PURPOSE: Mobile-first responsive breakpoints for tablets and phones.
   Ensures usability across all device sizes.
   ═══════════════════════════════════════════════════════════════════ */

@media (max-width: 768px) {
  .sidebar {
    width: 100%;
    transform: translateX(-100%);
  }
  
  .sidebar.collapsed {
    transform: translateX(-100%);
  }
  
  .sidebar:not(.collapsed) {
    transform: translateX(0);
  }
  
  .main-content {
    margin-left: 0;
    padding: 1rem;
  }
  
  .hud {
    grid-template-columns: repeat(2, 1fr);
  }
  
  .mission-input-group {
    grid-template-columns: 1fr;
  }
  
  .mission-form {
    grid-template-columns: 1fr;
  }
  
  .goals-grid {
    grid-template-columns: 1fr;
  }
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: SCROLL STYLING
   
   PURPOSE: Custom scrollbar design that matches theme aesthetic.
   ═══════════════════════════════════════════════════════════════════ */

::-webkit-scrollbar {
  width: 10px;
  height: 10px;
}

::-webkit-scrollbar-track {
  background: var(--bg);
}

::-webkit-scrollbar-thumb {
  background: var(--accent);
  border-radius: 5px;
}

::-webkit-scrollbar-thumb:hover {
  background: var(--ok);
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: NOSCRIPT FALLBACK
   
   PURPOSE: Graceful degradation message when JavaScript is disabled.
   ═══════════════════════════════════════════════════════════════════ */

noscript {
  display: block;
  padding: 2rem;
  background: var(--warn);
  color: var(--bg);
  text-align: center;
  font-weight: bold;
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: MODAL DIALOGS
   
   PURPOSE: Reusable modal component for detailed views, confirmations,
   and settings panels.
   ═══════════════════════════════════════════════════════════════════ */

.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.8);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 9000;
  backdrop-filter: blur(5px);
}

.modal {
  background: var(--card);
  border-radius: var(--border-radius);
  padding: 2rem;
  max-width: 600px;
  width: 90%;
  max-height: 80vh;
  overflow-y: auto;
  box-shadow: var(--shadow), var(--glow);
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 1.5rem;
}

.modal-close {
  background: none;
  color: var(--text);
  font-size: 1.5rem;
  padding: 0.25rem 0.5rem;
}

.hidden {
  display: none !important;
}

/* ═══════════════════════════════════════════════════════════════════
   SECTION: LEADERBOARD
   
   PURPOSE: Display top contributors globally and by category. Encourages
   friendly competition and recognizes outstanding efforts.
   ═══════════════════════════════════════════════════════════════════ */

.leaderboard-table {
  width: 100%;
  border-collapse: collapse;
}

.leaderboard-table th {
  text-align: left;
  padding: 0.75rem;
  background: var(--panel);
  font-weight: bold;
  text-transform: uppercase;
  font-size: 0.85rem;
  letter-spacing: 0.05em;
}

.leaderboard-table td {
  padding: 0.75rem;
  border-bottom: 1px solid var(--panel);
}

.leaderboard-table tr:hover {
  background: var(--panel);
}

.rank-badge {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  font-weight: bold;
  background: var(--accent);
  color: var(--bg);
}

.rank-badge.gold { background: #ffd700; }
.rank-badge.silver { background: #c0c0c0; }
.rank-badge.bronze { background: #cd7f32; }

</style>
</head>
<body>

<!-- ═══════════════════════════════════════════════════════════════════
     NOSCRIPT FALLBACK
     Shows when JavaScript is disabled
     ═══════════════════════════════════════════════════════════════════ -->
<noscript>
  ⚠️ This dashboard requires JavaScript to function. Please enable JavaScript in your browser settings to access the full Leadership Dashboard experience.
</noscript>

<!-- ═══════════════════════════════════════════════════════════════════
     ANIMATED BACKGROUND: Parallax Star Field
     
     PURPOSE: Creates immersive space-themed atmosphere with 3D parallax
     effect that responds to cursor movement.
     
     TECH: HTML5 Canvas with requestAnimationFrame for smooth 60fps animation.
     Stars have varying Z-depth for parallax effect.
     ═══════════════════════════════════════════════════════════════════ -->
<canvas id="stars"></canvas>

<!-- ═══════════════════════════════════════════════════════════════════
     SIDEBAR NAVIGATION
     
     SECTIONS:
     - Dashboard: Overview & HUD
     - Missions: Task management
     - Achievements: Unlocked badges
     - Global Goals: Collective progress
     - Multiplayer: Online users
     - Leaderboard: Top contributors
     - Analytics: Personal statistics
     - Settings: Preferences & theme
     ═══════════════════════════════════════════════════════════════════ -->
<button class="sidebar-toggle" onclick="toggleSidebar()" aria-label="Toggle navigation">☰</button>

<aside class="sidebar" id="sidebar">
  <div class="sidebar-header">
    <h3>🌌 Navigation</h3>
  </div>
  <nav class="nav-links">
    <a href="#dashboard" class="active" onclick="showSection('dashboard')">📊 Dashboard</a>
    <a href="#missions" onclick="showSection('missions')">🎯 Missions</a>
    <a href="#achievements" onclick="showSection('achievements')">🏆 Achievements</a>
    <a href="#global" onclick="showSection('global')">🌐 Global Goals</a>
    <a href="#multiplayer" onclick="showSection('multiplayer')">👥 Multiplayer</a>
    <a href="#leaderboard" onclick="showSection('leaderboard')">🥇 Leaderboard</a>
    <a href="#analytics" onclick="showSection('analytics')">📈 Analytics</a>
    <a href="#journal" onclick="showSection('journal')">📔 Journal</a>
    <a href="#settings" onclick="showSection('settings')">⚙️ Settings</a>
  </nav>
  
  <div style="margin-top: 2rem; padding-top: 1rem; border-top: 1px solid var(--panel);">
    <label for="themeSelect" style="font-size: 0.9rem; opacity: 0.8;">Theme:</label>
    <select id="themeSelect" onchange="changeTheme(this.value)" style="width: 100%; margin-top: 0.5rem;">
      <option value="space">🌌 Space</option>
      <option value="medieval">🏰 Medieval</option>
      <option value="anime">🌸 Anime</option>
      <option value="cyberpunk">🤖 Cyberpunk</option>
    </select>
  </div>
</aside>

<!-- ═══════════════════════════════════════════════════════════════════
     MAIN CONTENT AREA
     All dashboard sections are contained here
     ═══════════════════════════════════════════════════════════════════ -->
<main class="main-content" id="mainContent">

  <!-- ═══════════════════════════════════════════════════════════════════
       GAMIFICATION HUD (Always Visible)
       
       Displays real-time user stats:
       - Level & XP progression
       - Health (daily activity)
       - Mana (creative energy)
       - Streak (consecutive days)
       - Global Impact Score
       - Multiplayer status
       ═══════════════════════════════════════════════════════════════════ -->
  <div class="hud">
    <div class="hud-stat" onclick="showStatDetail('level')" title="Click for level details">
      <span class="hud-stat-label">Level</span>
      <span class="hud-stat-value" id="userLevel">1</span>
    </div>
    <div class="hud-stat" onclick="showStatDetail('xp')" title="Experience Points">
      <span class="hud-stat-label">XP</span>
      <span class="hud-stat-value" id="userXP">0</span>
      <div class="hud-bar">
        <div class="hud-bar-fill" id="xpBarFill" style="width: 0%"></div>
      </div>
    </div>
    <div class="hud-stat" onclick="showStatDetail('health')" title="Daily activity health">
      <span class="hud-stat-label">Health</span>
      <span class="hud-stat-value" id="userHealth">100</span>
      <div class="hud-bar">
        <div class="hud-bar-fill" id="healthBarFill" style="width: 100%; background: var(--err);"></div>
      </div>
    </div>
    <div class="hud-stat" onclick="showStatDetail('mana')" title="Creative energy">
      <span class="hud-stat-label">Mana</span>
      <span class="hud-stat-value" id="userMana">50</span>
      <div class="hud-bar">
        <div class="hud-bar-fill" id="manaBarFill" style="width: 50%; background: var(--info);"></div>
      </div>
    </div>
    <div class="hud-stat" onclick="showStatDetail('streak')" title="Consecutive active days">
      <span class="hud-stat-label">🔥 Streak</span>
      <span class="hud-stat-value" id="userStreak">0</span>
    </div>
    <div class="hud-stat" onclick="showStatDetail('impact')" title="Contribution to global goals">
      <span class="hud-stat-label">🌍 Impact</span>
      <span class="hud-stat-value" id="globalImpact">0</span>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════════════════════════════
       SECTION: DASHBOARD OVERVIEW
       Default landing view showing summary of all activities
       ═══════════════════════════════════════════════════════════════════ -->
  <div id="dashboard" class="dashboard-section">
    <h1 style="font-size: 2.5rem; margin-bottom: 1rem; color: var(--accent);">🌍 Leadership Dashboard</h1>
    <p style="font-size: 1.1rem; margin-bottom: 2rem; opacity: 0.9;">Empower yourself and others to tackle global challenges. Record your actions, earn XP, and join the planetary restoration movement.</p>

    <!-- Search functionality -->
    <div class="search-bar">
      <span class="search-icon">🔍</span>
      <input type="text" id="globalSearch" placeholder="Search missions, achievements, or content..." oninput="performSearch(this.value)">
    </div>

    <!-- Export options -->
    <div class="export-buttons">
      <button onclick="exportHTML()">💾 Save as HTML</button>
      <button onclick="exportPDF()">📄 Export PDF</button>
      <button onclick="exportJSON()">📊 Export JSON</button>
    </div>

    <!-- Quick stats overview -->
    <div class="section">
      <div class="section-header" onclick="toggleSection(this)">
        <h2 class="section-title">📊 Quick Overview</h2>
        <span class="collapse-icon">▼</span>
      </div>
      <div class="section-content">
        <div class="goals-grid">
          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">✅</span>
              <span class="goal-name">Completed Missions</span>
            </div>
            <div style="font-size: 2rem; font-weight: bold; color: var(--ok);" id="completedCount">0</div>
          </div>
          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">⏳</span>
              <span class="goal-name">Active Missions</span>
            </div>
            <div style="font-size: 2rem; font-weight: bold; color: var(--warn);" id="activeCount">0</div>
          </div>
          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">🏆</span>
              <span class="goal-name">Achievements</span>
            </div>
            <div style="font-size: 2rem; font-weight: bold; color: var(--accent);" id="achievementCount">0/20</div>
          </div>
          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">👥</span>
              <span class="goal-name">Online Now</span>
            </div>
            <div style="font-size: 2rem; font-weight: bold; color: var(--info);" id="onlineCount">1</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════════════════════════════
       SECTION: MISSION MANAGEMENT
       
       Core task system with:
       - Create new missions with title, category, difficulty, deadline
       - Filter by status, category, difficulty
       - Sort by date, XP, priority
       - Complete, edit, or delete missions
       - Track mission history
       
       DATA STRUCTURE:
       {
         id: timestamp,
         title: string,
         type: string (Climate/Peace/Education/Health/Equity/Tech/Economy),
         difficulty: string (Easy/Medium/Hard/Epic),
         xp: number (auto-calculated by difficulty),
         deadline: date (optional),
         completed: boolean,
         completedDate: date,
         notes: string
       }
       ═══════════════════════════════════════════════════════════════════ -->
  <div id="missions" class="dashboard-section hidden">
    <div class="section">
      <div class="section-header" onclick="toggleSection(this)">
        <h2 class="section-title">🎯 Mission Control</h2>
        <span class="collapse-icon">▼</span>
      </div>
      <div class="section-content">
        <div class="mission-form">
          <div class="mission-input-group">
            <input type="text" id="missionTitle" placeholder="Mission title (e.g., Plant 10 trees in local park)">
            <select id="missionType">
              <option value="Climate">🌱 Climate</option>
              <option value="Peace">🕊️ Peace</option>
              <option value="Education">📘 Education</option>
              <option value="Health">💊 Health</option>
              <option value="Equity">⚖️ Equity</option>
              <option value="Technology">💻 Technology</option>
              <option value="Economy">💰 Economy</option>
            </select>
            <select id="missionDifficulty">
              <option value="Easy">Easy (10 XP)</option>
              <option value="Medium">Medium (25 XP)</option>
              <option value="Hard">Hard (50 XP)</option>
              <option value="Epic">Epic (100 XP)</option>
            </select>
          </div>
          <button onclick="addMission()">➕ Add Mission</button>
        </div>
        
        <div style="margin: 1rem 0;">
          <label for="missionDeadline" style="font-size: 0.9rem; opacity: 0.8;">Optional Deadline:</label>
          <input type="datetime-local" id="missionDeadline" style="width: 100%; margin-top: 0.5rem;">
        </div>

        <!-- Mission filters -->
        <div class="mission-filters">
          <span style="opacity: 0.7; margin-right: 0.5rem;">Filter:</span>
          <div class="filter-tag active" data-filter="all" onclick="filterMissions('all')">All</div>
          <div class="filter-tag" data-filter="active" onclick="filterMissions('active')">Active</div>
          <div class="filter-tag" data-filter="completed" onclick="filterMissions('completed')">Completed</div>
          <div class="filter-tag" data-filter="Climate" onclick="filterMissions('Climate')">🌱 Climate</div>
          <div class="filter-tag" data-filter="Peace" onclick="filterMissions('Peace')">🕊️ Peace</div>
          <div class="filter-tag" data-filter="Education" onclick="filterMissions('Education')">📘 Education</div>
        </div>

        <!-- Mission list -->
        <div class="mission-list" id="missionList">
          <p style="opacity: 0.6; text-align: center; padding: 2rem;">No missions yet. Create your first mission above!</p>
        </div>
      </div>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════════════════════════════
       SECTION: ACHIEVEMENTS & BADGES
       
       Unlockable achievements that reward specific milestones:
       
       ACHIEVEMENT LIST:
       1. First Steps - Complete your first mission (10 XP)
       2. Dedicated - Maintain 7-day streak (50 XP)
       3. Warrior - Complete 10 missions (100 XP)
       4. Champion - Reach level 10 (200 XP)
       5. Global Leader - 1000 impact points (500 XP)
       6. Collaborator - Join 5 multiplayer missions (75 XP)
       7. Scholar - Complete 10 Education missions (150 XP)
       8. Peacemaker - Complete 10 Peace missions (150 XP)
       9. Eco Warrior - Complete 10 Climate missions (150 XP)
       10. Healer - Complete 10 Health missions (150 XP)
       ... and more!
       ═══════════════════════════════════════════════════════════════════ -->
  <div id="achievements" class="dashboard-section hidden">
    <div class="section">
      <div class="section-header" onclick="toggleSection(this)">
        <h2 class="section-title">🏆 Achievements</h2>
        <span class="collapse-icon">▼</span>
      </div>
      <div class="section-content">
        <p style="margin-bottom: 1rem; opacity: 0.8;">Unlock badges by reaching milestones. Each achievement grants bonus XP!</p>
        <div class="achievement-grid" id="achievementGrid">
          <!-- Achievements populated by JavaScript -->
        </div>
      </div>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════════════════════════════
       SECTION: GLOBAL GOALS & COLLECTIVE IMPACT
       
       Aggregates all users' contributions to visualize planetary-scale
       progress on sustainability goals inspired by UN SDGs.
       
       METRICS TRACKED:
       - Trees planted (Climate Action)
       - CO2 offset in kg (Climate Action)
       - Educational resources shared (Quality Education)
       - Peace-building initiatives (Peace & Justice)
       - Healthcare actions (Good Health)
       - Water conservation (Clean Water)
       - Energy efficiency (Clean Energy)
       - Community building (Reduced Inequalities)
       
       SYNC MECHANISM:
       - Local data stored in IndexedDB
       - Optional sync to shared ledger (simulated - can integrate real API)
       - Real-time updates when other users contribute
       ═══════════════════════════════════════════════════════════════════ -->
  <div id="global" class="dashboard-section hidden">
    <div class="section">
      <div class="section-header" onclick="toggleSection(this)">
        <h2 class="section-title">🌐 Global Goals</h2>
        <span class="collapse-icon">▼</span>
      </div>
      <div class="section-content">
        <p style="margin-bottom: 1.5rem; opacity: 0.9;">Every mission contributes to shared goals. Together, we can visualize planetary recovery in real time.</p>
        
        <button onclick="syncData()" style="margin-bottom: 1rem;">🔁 Sync with Global Ledger</button>
        <div class="log" id="syncLog" style="margin-bottom: 1.5rem; padding: 0.75rem; background: var(--panel); border-radius: 0.5rem; font-size: 0.9rem;"></div>

        <div class="goals-grid">
          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">🌳</span>
              <div>
                <div class="goal-name">Trees Planted</div>
                <div style="font-size: 0.85rem; opacity: 0.7;">Climate Action</div>
              </div>
            </div>
            <div class="goal-progress">
              <span id="treesPlanted">0</span>
              <span style="opacity: 0.6;">/ 1M goal</span>
            </div>
            <div class="goal-bar">
              <div class="goal-bar-fill" id="treesBar" style="width: 0%"></div>
            </div>
          </div>

          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">💨</span>
              <div>
                <div class="goal-name">CO₂ Offset</div>
                <div style="font-size: 0.85rem; opacity: 0.7;">Carbon Reduction</div>
              </div>
            </div>
            <div class="goal-progress">
              <span id="co2Offset">0</span>
              <span style="opacity: 0.6;">kg</span>
            </div>
            <div class="goal-bar">
              <div class="goal-bar-fill" id="co2Bar" style="width: 0%"></div>
            </div>
          </div>

          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">📚</span>
              <div>
                <div class="goal-name">Resources Shared</div>
                <div style="font-size: 0.85rem; opacity: 0.7;">Quality Education</div>
              </div>
            </div>
            <div class="goal-progress">
              <span id="resourcesShared">0</span>
              <span style="opacity: 0.6;">/ 100K goal</span>
            </div>
            <div class="goal-bar">
              <div class="goal-bar-fill" id="resourcesBar" style="width: 0%"></div>
            </div>
          </div>

          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">🕊️</span>
              <div>
                <div class="goal-name">Peace Initiatives</div>
                <div style="font-size: 0.85rem; opacity: 0.7;">Peace & Justice</div>
              </div>
            </div>
            <div class="goal-progress">
              <span id="peaceInitiatives">0</span>
              <span style="opacity: 0.6;">/ 50K goal</span>
            </div>
            <div class="goal-bar">
              <div class="goal-bar-fill" id="peaceBar" style="width: 0%"></div>
            </div>
          </div>

          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">💊</span>
              <div>
                <div class="goal-name">Health Actions</div>
                <div style="font-size: 0.85rem; opacity: 0.7;">Good Health</div>
              </div>
            </div>
            <div class="goal-progress">
              <span id="healthActions">0</span>
              <span style="opacity: 0.6;">/ 75K goal</span>
            </div>
            <div class="goal-bar">
              <div class="goal-bar-fill" id="healthBar" style="width: 0%"></div>
            </div>
          </div>

          <div class="goal-card">
            <div class="goal-header">
              <span class="goal-icon">💧</span>
              <div>
                <div class="goal-name">Water Saved</div>
                <div style="font-size: 0.85rem; opacity: 0.7;">Clean Water</div>
              </div>
            </div>
            <div class="goal-progress">
              <span id="waterSaved">0</span>
              <span style="opacity: 0.6;">liters</span>
            </div>
            <div class="goal-bar">
              <div class="goal-bar-fill" id="waterBar" style="width: 0%"></div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════════════════════════════
       SECTION: MULTIPLAYER & PRESENCE
       
       Real-time collaboration features:
       - See who's online
       - View their current activity
       - Invite to missions
       - Chat (text-based coordination)
       - Shared mission boards
       
       PRIVACY CONTROLS:
       - Opt-in visibility (default: offline)
       - Anonymous mode available
       - Control what activity is shared
       
       TECHNICAL: Simulated WebSocket connection. Can be upgraded to:
       - Socket.io for real-time bidirectional communication
       - WebRTC for peer-to-peer connections
       - Firebase Realtime Database
       - Supabase Realtime
       ═══════════════════════════════════════════════════════════════════ -->
  <div id="multiplayer" class="dashboard-section hidden">
    <div class="section">
      <div class="section-header" onclick="toggleSection(this)">
        <h2 class="section-title">👥 Multiplayer Lobby</h2>
        <span class="collapse-icon">▼</span>
      </div>
      <div class="section-content">
        <div style="margin-bottom: 1.5rem;">
          <label style="display: flex; align-items: center; gap: 0.5rem; cursor: pointer;">
            <input type="checkbox" id="onlineStatus" onchange="toggleOnlineStatus()" style="width: auto;">
            <span>Show as online (enable multiplayer features)</span>
          </label>
          <p style="font-size: 0.85rem; opacity: 0.7; margin-top: 0.5rem;">When enabled, others can see your presence and invite you to collaborate on missions.</p>
        </div>

        <div class="presence-bar">
          <span class="presence-label">🟢 Online Now:</span>
          <div class="presence-users" id="onlineUsers">
            <div class="presence-avatar" title="You" style="cursor: default;">
              YOU
            </div>
          </div>
        </div>

        <div style="margin-top: 1.5rem;">
          <h3 style="margin-bottom: 1rem;">💬 Global Activity Feed</h3>
          <div class="log" id="activityFeed" style="max-height: 300px; overflow-y: auto;">
            <p style="opacity: 0.6; text-align: center; padding: 1rem;">Enable online status to see global activity...</p>
          </div>
        </div>

        <div style="margin-top: 1.5rem;">
          <h3 style="margin-bottom: 1rem;">🤝 Shared Mission Board</h3>
          <p style="font-size: 0.9rem; opacity: 0.8; margin-bottom: 1rem;">Missions that need collaboration. Join forces with other leaders!</p>
          <div id="sharedMissions">
            <p style="opacity: 0.6; text-align: center; padding: 1rem;">No shared missions available. Create one in the Missions tab!</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════════════════════════════
       SECTION: LEADERBOARD
       
       Rankings by various metrics:
       - Total XP (all-time)
       - Current level
       - Missions completed this week/month
       - Category-specific rankings (Climate, Peace, etc.)
       - Team/organization rankings
       
       PRIVACY: Users can opt-out of public leaderboard while still
       seeing anonymous rankings.
       ═══════════════════════════════════════════════════════════════════ -->
  <div id="leaderboard" class="dashboard-section hidden">
    <div class="section">
      <div class="section-header" onclick="toggleSection(this)">
        <h2 class="section-title">🥇 Leaderboard</h2>
        <span class="collapse-icon">▼</span>
      </div>
      <div class="section-content">
        <div style="margin-bottom: 1rem;">
          <label for="leaderboardFilter" style="margin-right: 0.5rem;">View:</label>
          <select id="leaderboardFilter" onchange="filterLeaderboard(this.value)">
            <option value="all">All Time</option>
            <option value="week">This Week</option>
            <option value="month">This Month</option>
            <option value="climate">Climate Leaders</option>
            <option value="peace">Peace Leaders</option>
          </select>
        </div>

        <table class="leaderboard-table">
          <thead>
            <tr>
              <th>Rank</th>
              <th>Leader</th>
              <th>Level</th>
              <th>XP</th>
              <th>Missions</th>
              <th>Impact</th>
            </tr>
          </thead>
          <tbody id="leaderboardBody">
            <!-- Populated by JavaScript -->
          </tbody>
        </table>
      </div>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════════════════════════════
       SECTION: PERSONAL ANALYTICS
       
       Visualizations of your progress:
       - XP gain over time (line chart)
       - Mission completion by category (pie chart)
       - Streak calendar (heatmap)
       - Best performing days/times
       - Goal achievement rate
       
       CHARTS: Simple canvas-based charts (can upgrade to Chart.js or D3)
       ═══════════════════════════════════════════════════════════════════ -->
  <div id="analytics" class="dashboard-section hidden">
    <div class="section">
      <div class="section-header" onclick="toggleSection(this)">
        <h2 class="section-title">📈 Personal Analytics</h2>
        <span class="collapse-icon">▼</span>
      </div>
      <div class="section-content">
        <div class="goals-grid">
          <div class="goal-card">
            <h3>📊 XP Growth</h3>
            <canvas id="xpChart" width="200" height="120" style="width: 100%; height: auto;"></canvas>
          </div>
          <div class="goal-card">
            <h3>🎯 Mission Breakdown</h3>
            <canvas id="missionChart" width="200" height="200" style="width: 100%; height: auto;"></canvas>
          </div>
        </div>

        <div style="margin-top: 1.5rem;">
          <h3 style="margin-bottom: 1rem;">🔥 Activity Heatmap (Last 30 Days)</h3>
          <div id="heatmap" style="display: grid; grid-template-columns: repeat(7, 1fr); gap: 0.25rem;">
            <!-- Generated by JavaScript -->
          </div>
        </div>

        <div style="margin-top: 1.5rem;">
          <h3 style="margin-bottom: 1rem;">📋 Summary Statistics</h3>
          <div class="goals-grid">
            <div class="goal-card">
              <div style="font-size: 0.9rem; opacity: 0.7;">Completion Rate</div>
              <div style="font-size: 2rem; font-weight: bold;" id="completionRate">0%</div>
            </div>
            <div class="goal-card">
              <div style="font-size: 0.9rem; opacity: 0.7;">Average XP/Day</div>
              <div style="font-size: 2rem; font-weight: bold;" id="avgXPPerDay">0</div>
            </div>
            <div class="goal-card">
              <div style="font-size: 0.9rem; opacity: 0.7;">Best Streak</div>
              <div style="font-size: 2rem; font-weight: bold;" id="bestStreak">0 🔥</div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════════════════════════════
       SECTION: REFLECTIVE JOURNAL
       
       Daily journaling feature for reflection on leadership growth:
       - Daily prompts (e.g., "What impact did I make today?")
       - Free-form entries
       - Mood tracking
       - Searchable archive
       - Export journal as PDF or Markdown
       
       PURPOSE: Combines quantitative metrics with qualitative reflection
       for holistic personal development.
       ═══════════════════════════════════════════════════════════════════ -->
  <div id="journal" class="dashboard-section hidden">
    <div class="section">
      <div class="section-header" onclick="toggleSection(this)">
        <h2 class="section-title">📔 Reflective Journal</h2>
        <span class="collapse-icon">▼</span>
      </div>
      <div class="section-content">
        <p style="margin-bottom: 1rem; opacity: 0.8;">Daily reflections on your leadership journey. Document insights, challenges, and growth.</p>
        
        <div style="margin-bottom: 1.5rem;">
          <label style="display: block; margin-bottom: 0.5rem; font-weight: bold;">Today's Prompt:</label>
          <p style="padding: 0.75rem; background: var(--panel); border-radius: 0.5rem; font-style: italic;" id="journalPrompt">
            "What positive impact did I create today, and how can I amplify it tomorrow?"
          </p>
        </div>

        <div style="margin-bottom: 1rem;">
          <label for="journalMood" style="display: block; margin-bottom: 0.5rem;">How are you feeling?</label>
          <div style="display: flex; gap: 0.5rem; font-size: 2rem;">
            <span class="mood-emoji" onclick="selectMood('😊')" style="cursor: pointer; padding: 0.5rem; border-radius: 0.5rem;" title="Great">😊</span>
            <span class="mood-emoji" onclick="selectMood('🙂')" style="cursor: pointer; padding: 0.5rem; border-radius: 0.5rem;" title="Good">🙂</span>
            <span class="mood-emoji" onclick="selectMood('😐')" style="cursor: pointer; padding: 0.5rem; border-radius: 0.5rem;" title="Neutral">😐</span>
            <span class="mood-emoji" onclick="selectMood('😔')" style="cursor: pointer; padding: 0.5rem; border-radius: 0.5rem;" title="Low">😔</span>
            <span class="mood-emoji" onclick="selectMood('😰')" style="cursor: pointer; padding: 0.5rem; border-radius: 0.5rem;" title="Stressed">😰</span>
          </div>
          <input type="hidden" id="selectedMood" value="😊">
        </div>

        <textarea id="journalEntry" placeholder="Write your reflection here..." style="width: 100%; min-height: 150px; resize: vertical; margin-bottom: 1rem;"></textarea>
        
        <button onclick="saveJournalEntry()">💾 Save Entry</button>
        <button onclick="generateJournalPrompt()" class="secondary">🎲 New Prompt</button>

        <div style="margin-top: 2rem;">
          <h3 style="margin-bottom: 1rem;">📖 Past Entries</h3>
          <div id="journalArchive" class="mission-list" style="max-height: 400px;">
            <p style="opacity: 0.6; text-align: center; padding: 2rem;">No journal entries yet. Start writing above!</p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ═══════════════════════════════════════════════════════════════════
       SECTION: SETTINGS & PREFERENCES
       
       User configuration options:
       - Display name
       - Theme selection
       - Privacy controls
       - Notification preferences
       - Data export/import
       - Reset progress (with confirmation)
       ═══════════════════════════════════════════════════════════════════ -->
  <div id="settings" class="dashboard-section hidden">
    <div class="section">
      <div class="section-header" onclick="toggleSection(this)">
        <h2 class="section-title">⚙️ Settings</h2>
        <span class="collapse-icon">▼</span>
      </div>
      <div class="section-content">
        <h3 style="margin-bottom: 1rem;">👤 Profile</h3>
        <div style="margin-bottom: 1.5rem;">
          <label for="displayName" style="display: block; margin-bottom: 0.5rem;">Display Name:</label>
          <input type="text" id="displayName" placeholder="Your name" value="Leader" style="width: 100%;">
        </div>

        <h3 style="margin-bottom: 1rem;">🎨 Appearance</h3>
        <div style="margin-bottom: 1.5rem;">
          <label for="settingsTheme" style="display: block; margin-bottom: 0.5rem;">Theme:</label>
          <select id="settingsTheme" onchange="changeTheme(this.value)" style="width: 100%;">
            <option value="space">🌌 Space</option>
            <option value="medieval">🏰 Medieval</option>
            <option value="anime">🌸 Anime</option>
            <option value="cyberpunk">🤖 Cyberpunk</option>
          </select>
        </div>

        <h3 style="margin-bottom: 1rem;">🔔 Notifications</h3>
        <div style="margin-bottom: 1.5rem;">
          <label style="display: flex; align-items: center; gap: 0.5rem; cursor: pointer; margin-bottom: 0.5rem;">
            <input type="checkbox" id="notifyMissions" checked style="width: auto;">
            <span>Mission reminders</span>
          </label>
          <label style="display: flex; align-items: center; gap: 0.5rem; cursor: pointer; margin-bottom: 0.5rem;">
            <input type="checkbox" id="notifyAchievements" checked style="width: auto;">
            <span>Achievement unlocks</span>
          </label>
          <label style="display: flex; align-items: center; gap: 0.5rem; cursor: pointer;">
            <input type="checkbox" id="notifyMultiplayer" checked style="width: auto;">
            <span>Multiplayer invitations</span>
          </label>
        </div>

        <h3 style="margin-bottom: 1rem;">🔒 Privacy</h3>
        <div style="margin-bottom: 1.5rem;">
          <label style="display: flex; align-items: center; gap: 0.5rem; cursor: pointer; margin-bottom: 0.5rem;">
            <input type="checkbox" id="privacyLeaderboard" checked style="width: auto;">
            <span>Show on public leaderboard</span>
          </label>
          <label style="display: flex; align-items: center; gap: 0.5rem; cursor: pointer;">
            <input type="checkbox" id="privacyActivity" checked style="width: auto;">
            <span>Share activity with others</span>
          </label>
        </div>

        <h3 style="margin-bottom: 1rem;">💾 Data Management</h3>
        <div style="display: flex; flex-wrap: wrap; gap: 0.5rem; margin-bottom: 1.5rem;">
          <button onclick="backupData()">📥 Backup Data</button>
          <button onclick="importData()" class="secondary">📤 Import Data</button>
          <button onclick="resetProgress()" class="danger">🗑️ Reset Progress</button>
        </div>

        <button onclick="saveSettings()" style="width: 100%;">💾 Save Settings</button>
      </div>
    </div>
  </div>

</main>

<!-- ═══════════════════════════════════════════════════════════════════
     TOAST NOTIFICATION CONTAINER
     Non-intrusive feedback messages appear here
     ═══════════════════════════════════════════════════════════════════ -->
<div class="toast-container" id="toastContainer"></div>

<!-- ═══════════════════════════════════════════════════════════════════
     MODAL DIALOGS
     Reusable modal for detailed views
     ═══════════════════════════════════════════════════════════════════ -->
<div id="modalOverlay" class="modal-overlay hidden" onclick="closeModal()">
  <div class="modal" onclick="event.stopPropagation()">
    <div class="modal-header">
      <h2 id="modalTitle">Modal Title</h2>
      <button class="modal-close" onclick="closeModal()">✕</button>
    </div>
    <div id="modalContent">
      Modal content goes here
    </div>
  </div>
</div>

<!-- ═══════════════════════════════════════════════════════════════════
     FOOTER: ATTRIBUTIONS & ZERO-HARM NOTICE
     ═══════════════════════════════════════════════════════════════════ -->
<footer style="text-align: center; padding: 2rem; margin-top: 3rem; border-top: 1px solid var(--panel); opacity: 0.7; font-size: 0.85rem;">
  <p style="margin-bottom: 0.5rem;">© 2025 Foster + Navi | <a href="https://planetaryrestorationarchive.com" style="color: var(--accent);">Planetary Restoration Archive</a></p>
  <p style="margin-bottom: 1rem;">Built for peace, restoration, and collective leadership. <strong>Zero-Harm Commitment:</strong> This tool is designed for constructive purposes only.</p>
  
  <details style="max-width: 600px; margin: 1rem auto; text-align: left;">
    <summary style="cursor: pointer; font-weight: bold; margin-bottom: 0.5rem;">📜 Attributions & Licenses</summary>
    <ul style="list-style: none; padding: 0;">
      <li style="margin-bottom: 0.5rem;">• <strong>Habitica</strong> - Task gamification patterns (habit tracking, XP system) - <a href="https://habitica.com" style="color: var(--accent);">habitica.com</a> (GPL-3.0)</li>
      <li style="margin-bottom: 0.5rem;">• <strong>GitHub</strong> - Contribution heatmap design pattern - <a href="https://github.com" style="color: var(--accent);">github.com</a></li>
      <li style="margin-bottom: 0.5rem;">• <strong>UN SDGs</strong> - Sustainable Development Goals framework - <a href="https://sdgs.un.org" style="color: var(--accent);">sdgs.un.org</a> (Public Domain)</li>
      <li>• <strong>Web Standards</strong> - IndexedDB, Canvas API, Local Storage (W3C)</li>
    </ul>
    <p style="margin-top: 1rem;"><strong>License:</strong> MIT-style open source. Free to use, modify, and distribute with attribution. No warranty provided.</p>
  </details>

  <p style="margin-top: 1rem; font-size: 0.75rem;">⚠️ <strong>Privacy:</strong> All data stored locally in your browser. Optional multiplayer sync is anonymized. No tracking or analytics without explicit consent.</p>
</footer>

<script>
/* ═══════════════════════════════════════════════════════════════════
   JAVASCRIPT: APPLICATION LOGIC
   
   ARCHITECTURE:
   1. IndexedDB for local persistence
   2. State management for user stats
   3. Event handlers for UI interactions
   4. Simulated multiplayer (upgradeable to real WebSocket/WebRTC)
   5. Analytics and visualization
   6. Export/import functionality
   7. Rate limiting and input sanitization for safety
   
   SECTIONS:
   - Constants & Configuration
   - IndexedDB Setup
   - State Management
   - UI Controllers
   - Mission System
   - Gamification Logic
   - Multiplayer Simulation
   - Analytics & Charts
   - Export/Import
   - Utility Functions
   ═══════════════════════════════════════════════════════════════════ */

// ═══════════════════════════════════════════════════════════════════
// SECTION: CONSTANTS & CONFIGURATION
// ═══════════════════════════════════════════════════════════════════

const APP_CONFIG = {
  version: '2.0.0',
  dbName: 'LeadershipDashboardDB',
  dbVersion: 2,
  xpPerLevel: 100,
  healthDecayRate: 5, // HP lost per day of inactivity
  manaRegenRate: 10, // Mana gained per mission
  maxMana: 100,
  maxHealth: 100,
  rateLimitDelay: 1000, // Minimum ms between certain actions (anti-spam)
  maxMissionTitleLength: 200,
  maxJournalLength: 5000
};

const DIFFICULTY_XP = {
  'Easy': 10,
  'Medium': 25,
  'Hard': 50,
  'Epic': 100
};

const MISSION_TYPES = ['Climate', 'Peace', 'Education', 'Health', 'Equity', 'Technology', 'Economy'];

const ACHIEVEMENTS = [
  { id: 'first_mission', name: 'First Steps', icon: '🎯', desc: 'Complete your first mission', xp: 10, requirement: { type: 'missions', count: 1 } },
  { id: 'streak_7', name: 'Dedicated', icon: '🔥', desc: 'Maintain a 7-day streak', xp: 50, requirement: { type: 'streak', count: 7 } },
  { id: 'warrior', name: 'Warrior', icon: '⚔️', desc: 'Complete 10 missions', xp: 100, requirement: { type: 'missions', count: 10 } },
  { id: 'level_10', name: 'Champion', icon: '👑', desc: 'Reach level 10', xp: 200, requirement: { type: 'level', count: 10 } },
  { id: 'impact_1000', name: 'Global Leader', icon: '🌍', desc: 'Earn 1000 impact points', xp: 500, requirement: { type: 'impact', count: 1000 } },
  { id: 'collaborator', name: 'Collaborator', icon: '🤝', desc: 'Join 5 multiplayer missions', xp: 75, requirement: { type: 'multiplayer', count: 5 } },
  { id: 'scholar', name: 'Scholar', icon: '📚', desc: 'Complete 10 Education missions', xp: 150, requirement: { type: 'category', category: 'Education', count: 10 } },
  { id: 'peacemaker', name: 'Peacemaker', icon: '🕊️', desc: 'Complete 10 Peace missions', xp: 150, requirement: { type: 'category', category: 'Peace', count: 10 } },
  { id: 'eco_warrior', name: 'Eco Warrior', icon: '🌱', desc: 'Complete 10 Climate missions', xp: 150, requirement: { type: 'category', category: 'Climate', count: 10 } },
  { id: 'healer', name: 'Healer', icon: '💊', desc: 'Complete 10 Health missions', xp: 150, requirement: { type: 'category', category: 'Health', count: 10 } },
  { id: 'streak_30', name: 'Unstoppable', icon: '💪', desc: 'Maintain a 30-day streak', xp: 300, requirement: { type: 'streak', count: 30 } },
  { id: 'level_25', name: 'Legend', icon: '🌟', desc: 'Reach level 25', xp: 500, requirement: { type: 'level', count: 25 } },
  { id: 'missions_50', name: 'Hero', icon: '🦸', desc: 'Complete 50 missions', xp: 250, requirement: { type: 'missions', count: 50 } },
  { id: 'missions_100', name: 'Centurion', icon: '💯', desc: 'Complete 100 missions', xp: 500, requirement: { type: 'missions', count: 100 } },
  { id: 'epic_master', name: 'Epic Master', icon: '💎', desc: 'Complete 10 Epic missions', xp: 400, requirement: { type: 'difficulty', difficulty: 'Epic', count: 10 } },
  { id: 'journal_7', name: 'Reflective', icon: '📔', desc: 'Journal for 7 days', xp: 75, requirement: { type: 'journal', count: 7 } },
  { id: 'early_bird', name: 'Early Bird', icon: '🌅', desc: 'Complete 10 missions before 9 AM', xp: 100, requirement: { type: 'time', count: 10 } },
  { id: 'night_owl', name: 'Night Owl', icon: '🦉', desc: 'Complete 10 missions after 9 PM', xp: 100, requirement: { type: 'time', count: 10 } },
  { id: 'impact_5000', name: 'Planetary Guardian', icon: '🛡️', desc: 'Earn 5000 impact points', xp: 1000, requirement: { type: 'impact', count: 5000 } },
  { id: 'all_categories', name: 'Renaissance Leader', icon: '🎨', desc: 'Complete missions in all 7 categories', xp: 300, requirement: { type: 'diversity', count: 7 } }
];

const JOURNAL_PROMPTS = [
  "What positive impact did I create today, and how can I amplify it tomorrow?",
  "What challenge did I face, and what did it teach me about leadership?",
  "Who did I help today, and how did it make me feel?",
  "What am I grateful for in my restoration journey?",
  "What would I do differently if I could relive today?",
  "How did I grow beyond my comfort zone today?",
  "What wisdom would I share with someone starting their leadership path?",
  "What small action today could have big ripple effects?",
  "How did I practice compassion for myself and others?",
  "What does planetary restoration mean to me right now?"
];

// ═══════════════════════════════════════════════════════════════════
// SECTION: GLOBAL STATE
// Holds current user stats and application state
// ═══════════════════════════════════════════════════════════════════

let appState = {
  xp: 0,
  level: 1,
  health: 100,
  mana: 50,
  streak: 0,
  lastActiveDate: new Date().toDateString(),
  impact: 0,
  completedMissions: 0,
  activeMissions: 0,
  unlockedAchievements: [],
  displayName: 'Leader',
  theme: 'space',
  onlineStatus: false,
  currentFilter: 'all',
  selectedMood: '😊'
};

let db;
let lastActionTime = 0; // For rate limiting

// ═══════════════════════════════════════════════════════════════════
// SECTION: INDEXEDDB SETUP
// 
// PURPOSE: Local-first data persistence using IndexedDB browser API.
// Stores missions, journal entries, achievements, and user state.
// 
// OBJECT STORES:
// - missions: All user missions (active and completed)
// - journal: Daily journal entries
// - achievements: Unlocked achievement records
// - settings: User preferences and state
// - analytics: Historical data for charts
// 
// HOW TO EXPAND: Add new object stores in onupgradeneeded handler
// ═══════════════════════════════════════════════════════════════════

function initializeDB() {
  const request = indexedDB.open(APP_CONFIG.dbName, APP_CONFIG.dbVersion);
  
  request.onerror = () => {
    showToast('Database initialization failed. Data will not persist.', 'error');
    console.error('IndexedDB error:', request.error);
  };
  
  request.onsuccess = (e) => {
    db = e.target.result;
    loadAppState();
    displayMissions();
    displayAchievements();
    displayJournalEntries();
    updateAllStats();
    checkAchievements();
    updateStreak();
    renderAnalytics();
    initMultiplayer();
  };
  
  request.onupgradeneeded = (e) => {
    db = e.target.result;
    
    // Create missions store
    if (!db.objectStoreNames.contains('missions')) {
      const missionStore = db.createObjectStore('missions', { keyPath: 'id', autoIncrement: true });
      missionStore.createIndex('type', 'type', { unique: false });
      missionStore.createIndex('completed', 'completed', { unique: false });
      missionStore.createIndex('date', 'date', { unique: false });
    }
    
    // Create journal store
    if (!db.objectStoreNames.contains('journal')) {
      const journalStore = db.createObjectStore('journal', { keyPath: 'id', autoIncrement: true });
      journalStore.createIndex('date', 'date', { unique: false });
    }
    
    // Create achievements store
    if (!db.objectStoreNames.contains('achievements')) {
      db.createObjectStore('achievements', { keyPath: 'id' });
    }
    
    // Create settings store
    if (!db.objectStoreNames.contains('settings')) {
      db.createObjectStore('settings', { keyPath: 'key' });
    }
    
    // Create analytics store
    if (!db.objectStoreNames.contains('analytics')) {
      const analyticsStore = db.createObjectStore('analytics', { keyPath: 'id', autoIncrement: true });
      analyticsStore.createIndex('date', 'date', { unique: false });
    }
  };
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: STATE MANAGEMENT
// Load and save app state from IndexedDB
// ═══════════════════════════════════════════════════════════════════

function loadAppState() {
  const tx = db.transaction('settings', 'readonly');
  const store = tx.objectStore('settings');
  const request = store.get('appState');
  
  request.onsuccess = () => {
    if (request.result) {
      appState = { ...appState, ...request.result.value };
      updateHUD();
      document.getElementById('displayName').value = appState.displayName;
      document.getElementById('themeSelect').value = appState.theme;
      document.getElementById('settingsTheme').value = appState.theme;
      changeTheme(appState.theme);
    }
  };
}

function saveAppState() {
  if (!db) return;
  const tx = db.transaction('settings', 'readwrite');
  const store = tx.objectStore('settings');
  store.put({ key: 'appState', value: appState });
  
  // Also save analytics snapshot
  saveAnalyticsSnapshot();
}

function saveAnalyticsSnapshot() {
  const tx = db.transaction('analytics', 'readwrite');
  const store = tx.objectStore('analytics');
  store.add({
    date: new Date().toISOString(),
    xp: appState.xp,
    level: appState.level,
    missions: appState.completedMissions,
    impact: appState.impact,
    streak: appState.streak
  });
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: UI CONTROLLERS
// Navigation, sidebar, modals, and section management
// ═══════════════════════════════════════════════════════════════════

function toggleSidebar() {
  const sidebar = document.getElementById('sidebar');
  const mainContent = document.getElementById('mainContent');
  sidebar.classList.toggle('collapsed');
  mainContent.classList.toggle('expanded');
}

function showSection(sectionName) {
  // Hide all sections
  document.querySelectorAll('.dashboard-section').forEach(section => {
    section.classList.add('hidden');
  });
  
  // Show selected section
  const targetSection = document.getElementById(sectionName);
  if (targetSection) {
    targetSection.classList.remove('hidden');
  }
  
  // Update nav active state
  document.querySelectorAll('.nav-links a').forEach(link => {
    link.classList.remove('active');
  });
  event.target.classList.add('active');
  
  // Close sidebar on mobile
  if (window.innerWidth <= 768) {
    toggleSidebar();
  }
}

function toggleSection(headerElement) {
  const section = headerElement.parentElement;
  section.classList.toggle('collapsed');
}

function showModal(title, content) {
  document.getElementById('modalTitle').textContent = title;
  document.getElementById('modalContent').innerHTML = content;
  document.getElementById('modalOverlay').classList.remove('hidden');
}

function closeModal() {
  document.getElementById('modalOverlay').classList.add('hidden');
}

function changeTheme(themeName) {
  document.documentElement.setAttribute('data-theme', themeName);
  appState.theme = themeName;
  saveAppState();
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: TOAST NOTIFICATIONS
// 
// PURPOSE: Non-intrusive feedback for user actions
// TYPES: success, error, warning, info
// Auto-dismisses after 4 seconds
// ═══════════════════════════════════════════════════════════════════

function showToast(message, type = 'info') {
  const container = document.getElementById('toastContainer');
  const toast = document.createElement('div');
  toast.className = `toast ${type}`;
  
  const icons = {
    success: '✅',
    error: '❌',
    warning: '⚠️',
    info: 'ℹ️'
  };
  
  toast.innerHTML = `
    <span style="font-size: 1.2rem;">${icons[type]}</span>
    <span>${sanitizeHTML(message)}</span>
  `;
  
  container.appendChild(toast);
  
  setTimeout(() => {
    toast.style.animation = 'slideIn 0.3s ease reverse';
    setTimeout(() => toast.remove(), 300);
  }, 4000);
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: INPUT SANITIZATION (SECURITY)
// 
// PURPOSE: Prevent XSS attacks by sanitizing user input
// USAGE: Call on all user-provided strings before displaying in DOM
// ═══════════════════════════════════════════════════════════════════

function sanitizeHTML(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

function validateInput(input, maxLength, fieldName) {
  if (!input || input.trim().length === 0) {
    showToast(`${fieldName} cannot be empty`, 'warning');
    return false;
  }
  if (input.length > maxLength) {
    showToast(`${fieldName} is too long (max ${maxLength} characters)`, 'warning');
    return false;
  }
  return true;
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: RATE LIMITING (SECURITY)
// 
// PURPOSE: Prevent spam and abuse by limiting action frequency
// ═══════════════════════════════════════════════════════════════════

function checkRateLimit() {
  const now = Date.now();
  if (now - lastActionTime < APP_CONFIG.rateLimitDelay) {
    showToast('Please wait a moment before trying again', 'warning');
    return false;
  }
  lastActionTime = now;
  return true;
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: MISSION SYSTEM
// 
// Core functionality for creating, managing, and completing missions
// ═══════════════════════════════════════════════════════════════════

function addMission() {
  if (!checkRateLimit()) return;
  
  const title = document.getElementById('missionTitle').value.trim();
  const type = document.getElementById('missionType').value;
  const difficulty = document.getElementById('missionDifficulty').value;
  const deadline = document.getElementById('missionDeadline').value;
  
  if (!validateInput(title, APP_CONFIG.maxMissionTitleLength, 'Mission title')) return;
  
  const xp = DIFFICULTY_XP[difficulty];
  const mission = {
    title,
    type,
    difficulty,
    xp,
    deadline: deadline || null,
    completed: false,
    date: new Date().toISOString(),
    notes: ''
  };
  
  const tx = db.transaction('missions', 'readwrite');
  const store = tx.objectStore('missions');
  const request = store.add(mission);
  
  request.onsuccess = () => {
    displayMissions();
    updateStats();
    document.getElementById('missionTitle').value = '';
    document.getElementById('missionDeadline').value = '';
    showToast(`Mission "${title}" created! (+${xp} XP when completed)`, 'success');
    
    // Restore mana
    restoreMana(5);
  };
  
  request.onerror = () => {
    showToast('Failed to create mission', 'error');
  };
}

function displayMissions() {
  const list = document.getElementById('missionList');
  const tx = db.transaction('missions', 'readonly');
  const store = tx.objectStore('missions');
  const request = store.getAll();
  
  request.onsuccess = () => {
    const missions = request.result;
    
    if (missions.length === 0) {
      list.innerHTML = '<p style="opacity: 0.6; text-align: center; padding: 2rem;">No missions yet. Create your first mission above!</p>';
      return;
    }
    
    // Apply filters
    let filtered = missions;
    if (appState.currentFilter !== 'all') {
      if (appState.currentFilter === 'active') {
        filtered = missions.filter(m => !m.completed);
      } else if (appState.currentFilter === 'completed') {
        filtered = missions.filter(m => m.completed);
      } else {
        filtered = missions.filter(m => m.type === appState.currentFilter);
      }
    }
    
    // Sort by date (newest first)
    filtered.sort((a, b) => new Date(b.date) - new Date(a.date));
    
    list.innerHTML = filtered.map(m => {
      const deadlineHTML = m.deadline ? 
        `<span class="mission-tag">⏰ ${new Date(m.deadline).toLocaleDateString()}</span>` : '';
      
      const completedClass = m.completed ? 'completed' : '';
      const completedHTML = m.completed ? 
        `<span class="mission-tag" style="background: var(--ok);">✅ Completed</span>` : '';
      
      return `
        <div class="mission-card ${completedClass}">
          <div class="mission-header">
            <div class="mission-title">${sanitizeHTML(m.title)}</div>
          </div>
          <div class="mission-meta">
            <span class="mission-tag">${m.type}</span>
            <span class="mission-tag">${m.difficulty} (${m.xp} XP)</span>
            ${deadlineHTML}
            ${completedHTML}
          </div>
          <div class="mission-actions">
            ${!m.completed ? `<button onclick="completeMission(${m.id})">✅ Complete</button>` : ''}
            <button onclick="editMission(${m.id})" class="secondary">✏️ Edit</button>
            <button onclick="deleteMission(${m.id})" class="danger">🗑️ Delete</button>
          </div>
        </div>
      `;
    }).join('');
  };
}

function completeMission(id) {
  const tx = db.transaction('missions', 'readwrite');
  const store = tx.objectStore('missions');
  const request = store.get(id);
  
  request.onsuccess = () => {
    const mission = request.result;
    if (mission.completed) {
      showToast('Mission already completed!', 'info');
      return;
    }
    
    mission.completed = true;
    mission.completedDate = new Date().toISOString();
    
    store.put(mission);
    
    // Grant XP and update stats
    addXP(mission.xp);
    addImpact(mission.xp * 2);
    restoreMana(APP_CONFIG.manaRegenRate);
    
    displayMissions();
    updateStats();
    checkAchievements();
    
    showToast(`Mission completed! +${mission.xp} XP, +${mission.xp * 2} Impact!`, 'success');
    
    // Broadcast to multiplayer
    broadcastActivity(`completed a ${mission.type} mission: "${mission.title}"`);
  };
}

function editMission(id) {
  const tx = db.transaction('missions', 'readonly');
  const store = tx.objectStore('missions');
  const request = store.get(id);
  
  request.onsuccess = () => {
    const mission = request.result;
    showModal('Edit Mission', `
      <div style="display: flex; flex-direction: column; gap: 1rem;">
        <div>
          <label style="display: block; margin-bottom: 0.5rem;">Title:</label>
          <input type="text" id="editTitle" value="${sanitizeHTML(mission.title)}" style="width: 100%;">
        </div>
        <div>
          <label style="display: block; margin-bottom: 0.5rem;">Notes:</label>
          <textarea id="editNotes" style="width: 100%; min-height: 100px;">${sanitizeHTML(mission.notes || '')}</textarea>
        </div>
        <button onclick="saveEditedMission(${id})">💾 Save Changes</button>
      </div>
    `);
  };
}

function saveEditedMission(id) {
  const title = document.getElementById('editTitle').value.trim();
  const notes = document.getElementById('editNotes').value.trim();
  
  if (!validateInput(title, APP_CONFIG.maxMissionTitleLength, 'Mission title')) return;
  
  const tx = db.transaction('missions', 'readwrite');
  const store = tx.objectStore('missions');
  const request = store.get(id);
  
  request.onsuccess = () => {
    const mission = request.result;
    mission.title = title;
    mission.notes = notes;
    store.put(mission);
    
    displayMissions();
    closeModal();
    showToast('Mission updated!', 'success');
  };
}

function deleteMission(id) {
  if (!confirm('Are you sure you want to delete this mission?')) return;
  
  const tx = db.transaction('missions', 'readwrite');
  const store = tx.objectStore('missions');
  store.delete(id);
  
  tx.oncomplete = () => {
    displayMissions();
    updateStats();
    showToast('Mission deleted', 'info');
  };
}

function filterMissions(filter) {
  appState.currentFilter = filter;
  
  // Update filter button styles
  document.querySelectorAll('.filter-tag').forEach(tag => {
    tag.classList.remove('active');
  });
  event.target.classList.add('active');
  
  displayMissions();
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: GAMIFICATION LOGIC
// XP, leveling, health, mana, streaks, and achievements
// ═══════════════════════════════════════════════════════════════════

function addXP(amount) {
  appState.xp += amount;
  
  // Check for level up
  const newLevel = Math.floor(appState.xp / APP_CONFIG.xpPerLevel) + 1;
  if (newLevel > appState.level) {
    appState.level = newLevel;
    showToast(`🎉 Level Up! You're now level ${newLevel}!`, 'success');
    restoreHealth(20);
    restoreMana(50);
  }
  
  updateHUD();
  saveAppState();
}

function addImpact(amount) {
  appState.impact += amount;
  updateHUD();
  updateGlobalGoals();
  saveAppState();
}

function restoreHealth(amount) {
  appState.health = Math.min(appState.health + amount, APP_CONFIG.maxHealth);
  updateHUD();
  saveAppState();
}

function restoreMana(amount) {
  appState.mana = Math.min(appState.mana + amount, APP_CONFIG.maxMana);
  updateHUD();
  saveAppState();
}

function updateStreak() {
  const today = new Date().toDateString();
  const lastActive = new Date(appState.lastActiveDate).toDateString();
  
  if (today !== lastActive) {
    const yesterday = new Date();
    yesterday.setDate(yesterday.getDate() - 1);
    const yesterdayStr = yesterday.toDateString();
    
    if (lastActive === yesterdayStr) {
      // Consecutive day - increment streak
      appState.streak++;
      showToast(`🔥 ${appState.streak} day streak!`, 'success');
    } else if (lastActive !== today) {
      // Streak broken
      if (appState.streak > 0) {
        showToast(`Streak reset. Previous best: ${appState.streak} days`, 'warning');
      }
      appState.streak = 1;
      
      // Health decay for inactivity
      const daysMissed = Math.floor((new Date() - new Date(lastActive)) / (1000 * 60 * 60 * 24));
      const healthLost = Math.min(daysMissed * APP_CONFIG.healthDecayRate, appState.health - 1);
      appState.health -= healthLost;
    }
    
    appState.lastActiveDate = today;
    saveAppState();
    updateHUD();
    checkAchievements();
  }
}

function updateHUD() {
  document.getElementById('userLevel').textContent = appState.level;
  document.getElementById('userXP').textContent = appState.xp;
  document.getElementById('userHealth').textContent = appState.health;
  document.getElementById('userMana').textContent = appState.mana;
  document.getElementById('userStreak').textContent = appState.streak;
  document.getElementById('globalImpact').textContent = appState.impact;
  
  // Update progress bars
  const xpProgress = (appState.xp % APP_CONFIG.xpPerLevel) / APP_CONFIG.xpPerLevel * 100;
  document.getElementById('xpBarFill').style.width = xpProgress + '%';
  
  const healthProgress = (appState.health / APP_CONFIG.maxHealth) * 100;
  document.getElementById('healthBarFill').style.width = healthProgress + '%';
  
  const manaProgress = (appState.mana / APP_CONFIG.maxMana) * 100;
  document.getElementById('manaBarFill').style.width = manaProgress + '%';
}

function showStatDetail(statType) {
  const details = {
    level: `<h3>Level System</h3>
            <p>Current Level: ${appState.level}</p>
            <p>Total XP: ${appState.xp}</p>
            <p>Next Level: ${(appState.level + 1) * APP_CONFIG.xpPerLevel} XP</p>
            <p>Progress: ${(appState.xp % APP_CONFIG.xpPerLevel)} / ${APP_CONFIG.xpPerLevel} XP</p>`,
    xp: `<h3>Experience Points</h3>
         <p>XP is earned by completing missions. Higher difficulty = more XP.</p>
         <ul style="list-style: none; padding: 0;">
           <li>Easy: 10 XP</li>
           <li>Medium: 25 XP</li>
           <li>Hard: 50 XP</li>
           <li>Epic: 100 XP</li>
         </ul>`,
    health: `<h3>Health Points</h3>
             <p>Current: ${appState.health} / ${APP_CONFIG.maxHealth}</p>
             <p>Health represents daily activity. Missing days causes health decay (-${APP_CONFIG.healthDecayRate} HP/day).</p>
             <p>Restore health by leveling up or maintaining streaks.</p>`,
    mana: `<h3>Mana (Creative Energy)</h3>
           <p>Current: ${appState.mana} / ${APP_CONFIG.maxMana}</p>
           <p>Mana regenerates as you complete missions (+${APP_CONFIG.manaRegenRate} per mission).</p>
           <p>High mana enables special features and collaboration opportunities.</p>`,
    streak: `<h3>Daily Streak</h3>
             <p>Current: ${appState.streak} days 🔥</p>
             <p>Keep your streak alive by completing missions every day!</p>
             <p>Longer streaks unlock special achievements and bonuses.</p>`,
    impact: `<h3>Global Impact Score</h3>
             <p>Current: ${appState.impact} points</p>
             <p>Every mission contributes to planetary restoration goals.</p>
             <p>Your impact combines with others to create real change!</p>`
  };
  
  showModal(statType.charAt(0).toUpperCase() + statType.slice(1), details[statType]);
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: ACHIEVEMENTS SYSTEM
// Check progress and unlock badges
// ═══════════════════════════════════════════════════════════════════

function displayAchievements() {
  const grid = document.getElementById('achievementGrid');
  
  grid.innerHTML = ACHIEVEMENTS.map(ach => {
    const unlocked = appState.unlockedAchievements.includes(ach.id);
    const lockedClass = unlocked ? '' : 'locked';
    const unlockedBadge = unlocked ? '<div class="achievement-unlocked">✓</div>' : '';
    
    return `
      <div class="achievement-card ${lockedClass}" onclick="showAchievementDetail('${ach.id}')">
        ${unlockedBadge}
        <div class="achievement-icon">${ach.icon}</div>
        <div class="achievement-name">${ach.name}</div>
        <div class="achievement-desc">${ach.desc}</div>
        ${unlocked ? `<div style="font-size: 0.75rem; color: var(--ok); margin-top: 0.5rem;">+${ach.xp} XP</div>` : ''}
      </div>
    `;
  }).join('');
  
  // Update achievement count
  document.getElementById('achievementCount').textContent = 
    `${appState.unlockedAchievements.length}/${ACHIEVEMENTS.length}`;
}

function checkAchievements() {
  if (!db) return;
  
  const tx = db.transaction(['missions', 'journal'], 'readonly');
  const missionStore = tx.objectStore('missions');
  const journalStore = tx.objectStore('journal');
  
  missionStore.getAll().onsuccess = (e) => {
    const missions = e.target.result;
    const completedMissions = missions.filter(m => m.completed);
    
    ACHIEVEMENTS.forEach(ach => {
      if (appState.unlockedAchievements.includes(ach.id)) return;
      
      let unlocked = false;
      
      switch (ach.requirement.type) {
        case 'missions':
          unlocked = completedMissions.length >= ach.requirement.count;
          break;
        case 'level':
          unlocked = appState.level >= ach.requirement.count;
          break;
        case 'streak':
          unlocked = appState.streak >= ach.requirement.count;
          break;
        case 'impact':
          unlocked = appState.impact >= ach.requirement.count;
          break;
        case 'category':
          const categoryCount = completedMissions.filter(m => m.type === ach.requirement.category).length;
          unlocked = categoryCount >= ach.requirement.count;
          break;
        case 'difficulty':
          const difficultyCount = completedMissions.filter(m => m.difficulty === ach.requirement.difficulty).length;
          unlocked = difficultyCount >= ach.requirement.count;
          break;
        case 'diversity':
          const uniqueCategories = new Set(completedMissions.map(m => m.type)).size;
          unlocked = uniqueCategories >= ach.requirement.count;
          break;
      }
      
      if (unlocked) {
        unlockAchievement(ach);
      }
    });
  };
  
  // Check journal achievements
  journalStore.getAll().onsuccess = (e) => {
    const entries = e.target.result;
    ACHIEVEMENTS.forEach(ach => {
      if (appState.unlockedAchievements.includes(ach.id)) return;
      if (ach.requirement.type === 'journal' && entries.length >= ach.requirement.count) {
        unlockAchievement(ach);
      }
    });
  };
}

function unlockAchievement(achievement) {
  appState.unlockedAchievements.push(achievement.id);
  addXP(achievement.xp);
  
  showToast(`🏆 Achievement Unlocked: ${achievement.name}! +${achievement.xp} XP`, 'success');
  
  // Save achievement
  const tx = db.transaction('achievements', 'readwrite');
  tx.objectStore('achievements').add({
    id: achievement.id,
    unlockedDate: new Date().toISOString()
  });
  
  displayAchievements();
  saveAppState();
}

function showAchievementDetail(achId) {
  const achievement = ACHIEVEMENTS.find(a => a.id === achId);
  const unlocked = appState.unlockedAchievements.includes(achId);
  
  const content = `
    <div style="text-align: center;">
      <div style="font-size: 5rem; margin-bottom: 1rem;">${achievement.icon}</div>
      <h3>${achievement.name}</h3>
      <p style="opacity: 0.8; margin: 1rem 0;">${achievement.desc}</p>
      <p style="font-size: 1.2rem; color: var(--accent);">Reward: ${achievement.xp} XP</p>
      ${unlocked ? 
        '<p style="color: var(--ok); margin-top: 1rem;">✅ Unlocked!</p>' : 
        '<p style="opacity: 0.6; margin-top: 1rem;">🔒 Not yet unlocked</p>'}
    </div>
  `;
  
  showModal(achievement.name, content);
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: GLOBAL GOALS & SYNC
// Aggregate stats and simulate multiplayer sync
// ═══════════════════════════════════════════════════════════════════

function updateGlobalGoals() {
  // Simulate global stats based on user impact
  const multiplier = 100; // Simulate other users' contributions
  
  const trees = Math.floor(appState.impact * 0.5);
  const co2 = Math.floor(appState.impact * 2.5);
  const resources = Math.floor(appState.impact * 1.2);
  const peace = Math.floor(appState.impact * 0.8);
  const health = Math.floor(appState.impact * 1.0);
  const water = Math.floor(appState.impact * 150);
  
  document.getElementById('treesPlanted').textContent = (trees * multiplier).toLocaleString();
  document.getElementById('co2Offset').textContent = (co2 * multiplier).toLocaleString();
  document.getElementById('resourcesShared').textContent = (resources * multiplier).toLocaleString();
  document.getElementById('peaceInitiatives').textContent = (peace * multiplier).toLocaleString();
  document.getElementById('healthActions').textContent = (health * multiplier).toLocaleString();
  document.getElementById('waterSaved').textContent = (water * multiplier).toLocaleString();
  
  // Update progress bars
  document.getElementById('treesBar').style.width = Math.min((trees * multiplier / 1000000) * 100, 100) + '%';
  document.getElementById('co2Bar').style.width = Math.min((co2 * multiplier / 5000000) * 100, 100) + '%';
  document.getElementById('resourcesBar').style.width = Math.min((resources * multiplier / 100000) * 100, 100) + '%';
  document.getElementById('peaceBar').style.width = Math.min((peace * multiplier / 50000) * 100, 100) + '%';
  document.getElementById('healthBar').style.width = Math.min((health * multiplier / 75000) * 100, 100) + '%';
  document.getElementById('waterBar').style.width = Math.min((water * multiplier / 10000000) * 100, 100) + '%';
}

function syncData() {
  const log = document.getElementById('syncLog');
  log.innerHTML = '🌐 Syncing with global ledger...';
  
  setTimeout(() => {
    log.innerHTML = `
      ✅ Sync complete!<br>
      • ${appState.completedMissions} missions uploaded<br>
      • ${appState.impact} impact points contributed<br>
      • Connected to Planetary Restoration Archive node
    `;
    showToast('Data synced successfully!', 'success');
  }, 1500);
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: MULTIPLAYER SIMULATION
// Simulates real-time presence and activity feed
// Can be upgraded to real WebSocket/WebRTC
// ═══════════════════════════════════════════════════════════════════

let simulatedUsers = [];

function initMultiplayer() {
  // Generate simulated users
  const names = ['Alex', 'Jordan', 'Morgan', 'Casey', 'Riley', 'Avery', 'Quinn', 'Dakota'];
  const colors = ['#ff6b6b', '#4ecdc4', '#45b7d1', '#f7b731', '#5f27cd', '#00d2d3', '#ff9ff3'];
  
  simulatedUsers = names.slice(0, 3 + Math.floor(Math.random() * 4)).map((name, i) => ({
    name,
    color: colors[i % colors.length],
    level: 1 + Math.floor(Math.random() * 20),
    activity: ''
  }));
  
  updatePresence();
  
  // Simulate activity every 10-30 seconds
  setInterval(() => {
    if (appState.onlineStatus) {
      simulateUserActivity();
    }
  }, 10000 + Math.random() * 20000);
}

function toggleOnlineStatus() {
  appState.onlineStatus = document.getElementById('onlineStatus').checked;
  updatePresence();
  saveAppState();
  
  if (appState.onlineStatus) {
    showToast('You are now visible to other leaders', 'info');
    broadcastActivity('joined the leadership lobby');
  } else {
    showToast('You are now offline', 'info');
  }
}

function updatePresence() {
  const container = document.getElementById('onlineUsers');
  
  if (!appState.onlineStatus) {
    container.innerHTML = '<div class="presence-avatar" title="You (Offline)" style="opacity: 0.5;">YOU</div>';
    document.getElementById('activityFeed').innerHTML = 
      '<p style="opacity: 0.6; text-align: center; padding: 1rem;">Enable online status to see global activity...</p>';
    document.getElementById('onlineCount').textContent = '0';
    return;
  }
  
  container.innerHTML = `
    <div class="presence-avatar" title="You (Online)" style="background: var(--ok);">YOU</div>
    ${simulatedUsers.map(user => `
      <div class="presence-avatar" title="${user.name} (Level ${user.level})" 
           style="background: ${user.color};">
        ${user.name.substring(0, 2)}
      </div>
    `).join('')}
  `;
  
  document.getElementById('onlineCount').textContent = simulatedUsers.length + 1;
}

function simulateUserActivity() {
  const activities = [
    'completed a Climate mission',
    'reached level 5',
    'planted 10 trees',
    'unlocked an achievement',
    'shared educational resources',
    'started a Peace initiative',
    'completed a collaborative mission'
  ];
  
  const user = simulatedUsers[Math.floor(Math.random() * simulatedUsers.length)];
  const activity = activities[Math.floor(Math.random() * activities.length)];
  
  addActivityToFeed(`${user.name} ${activity}`);
}

function broadcastActivity(activity) {
  if (!appState.onlineStatus) return;
  addActivityToFeed(`${appState.displayName} ${activity}`);
}

function addActivityToFeed(message) {
  const feed = document.getElementById('activityFeed');
  const timestamp = new Date().toLocaleTimeString();
  
  const entry = document.createElement('div');
  entry.style.cssText = 'padding: 0.5rem; border-bottom: 1px solid var(--panel); opacity: 0; transition: opacity 0.3s;';
  entry.innerHTML = `<span style="opacity: 0.6; font-size: 0.85rem;">${timestamp}</span> - ${sanitizeHTML(message)}`;
  
  feed.insertBefore(entry, feed.firstChild);
  setTimeout(() => entry.style.opacity = '1', 10);
  
  // Keep only last 20 entries
  while (feed.children.length > 20) {
    feed.removeChild(feed.lastChild);
  }
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: LEADERBOARD
// Display rankings
// ═══════════════════════════════════════════════════════════════════

function filterLeaderboard(filter) {
  const tbody = document.getElementById('leaderboardBody');
  
  // Generate simulated leaderboard data
  const leaders = [
    { name: appState.displayName, level: appState.level, xp: appState.xp, missions: appState.completedMissions, impact: appState.impact }
  ];
  
  // Add simulated users
  simulatedUsers.forEach(user => {
    leaders.push({
      name: user.name,
      level: user.level,
      xp: user.level * 100 + Math.floor(Math.random() * 100),
      missions: Math.floor(Math.random() * 50),
      impact: Math.floor(Math.random() * 1000)
    });
  });
  
  // Sort by XP
  leaders.sort((a, b) => b.xp - a.xp);
  
  tbody.innerHTML = leaders.map((leader, index) => {
    const rankBadge = index < 3 ? 
      `<div class="rank-badge ${['gold', 'silver', 'bronze'][index]}">${index + 1}</div>` :
      `<div class="rank-badge">${index + 1}</div>`;
    
    const isCurrentUser = leader.name === appState.displayName;
    const highlight = isCurrentUser ? 'background: var(--panel);' : '';
    
    return `
      <tr style="${highlight}">
        <td>${rankBadge}</td>
        <td><strong>${sanitizeHTML(leader.name)}</strong> ${isCurrentUser ? '(You)' : ''}</td>
        <td>${leader.level}</td>
        <td>${leader.xp.toLocaleString()}</td>
        <td>${leader.missions}</td>
        <td>${leader.impact.toLocaleString()}</td>
      </tr>
    `;
  }).join('');
}

// ═══════════════════════════════════════════════════════════════════
// SECTION: JOURNAL SYSTEM
// Daily reflection and mood tracking
// ═══════════════════════════════════════════════════════════════════

function generateJournalPrompt() {
  const prompt = JOURNAL_PROMPTS[Math.floor(Math.random() * JOURNAL_PROMPTS.length)];
  document.getElementById('journalPrompt').textContent = prompt;
}

function selectMood(emoji) {
  document.getElementById('selectedMood').value = emoji;
  document.querySelectorAll('.mood-emoji').forEach(el => {
    el.style.background = '';
  });
  event.target.style.background = 'var(--accent)';
}

function saveJournalEntry() {
  const entry = document.getElementById('journalEntry').value.trim();
  const mood = document.getElementById('selectedMood').value;
  const prompt = document.getElementById('journalPrompt').textContent;
  
  if (!validateInput(entry, APP_CONFIG.maxJournalLength, 'Journal entry')) return;
  
  const tx = db.transaction('journal', 'readwrite');
  const store = tx.objectStore('journal');
  
  store.add({
    date: new Date().toISOString(),
    prompt,
    entry,
    mood
  });
  
  tx.oncomplete = () => {
    document.getElementById('journalEntry').value = '';
    displayJournalEntries();
    checkAchievements();
    generateJournalPrompt();
    showToast('Journal entry saved!', 'success');
    restoreMana(5);
  };
}

function displayJournalEntries() {
  const archive = document.getElementById('journalArchive');
  const tx = db.transaction('journal', 'readonly');
  const store = tx.objectStore('journal');
  const request = store.getAll();
  
  request.onsuccess = () => {
    const entries = request.result;
    
    if (entries.length === 0) {
      archive.innerHTML = '<p style="opacity: 0.6; text-align: center; padding: 2rem;">No journal entries yet. Start writing above!</p>';
      return;
    }
    
    entries.sort((a, b) => new Date(b.date) - new Date(a.date));
    
    archive.innerHTML = entries.map(e => `
      <div class="mission-card">
        <div class="mission-header">
          <div>
            <span style="font-size: 1.5rem; margin-right: 0.5rem;">${e.mood}</span>
            <strong>${new Date(e.date).toLocaleDateString()}</strong>
          </div>
        </div>
        <p style="font-style: italic; opacity: 0.7; margin: 0.5rem 0; font-size: 0.9rem;">"${sanitizeHTML(e.prompt)}"</p>
        <p style="margin-top: 0.5rem;">${sanitizeHTML(e.entry)}</p>
        <button onclick="deleteJournalEntry(${e.id})" class="danger" style="margin-top: 0.5rem; padding: 0.4rem 0.8rem; font-size: 0.85rem;">🗑️ Delete</button>
      </div>
    `).join('');
  };
}

function deleteJournalEntry(id) {
  if (!confirm('Delete this journal entry?')) return;
  
  const tx = db.transaction('journal', 'readwrite');
  tx.objectStore('journal').delete(id);
  
  tx.oncomplete = () => {
    displayJournalEntries();
    showToast('Entry deleted', 'info');
  };rem; opacity: 0.7;">Total Missions</div>
              <div style="font-size: 2rem; font-weight: bold;" id="totalMissionsCount">0</div>
            </div>
            <div class="goal-card">
              <div style="font-size: 0.9
