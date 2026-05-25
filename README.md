<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Nouri — Nutrition Tracker</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display:ital@0;1&family=DM+Sans:ital,opsz,wght@0,9..40,300;0,9..40,400;0,9..40,500;0,9..40,600;1,9..40,300&display=swap" rel="stylesheet">
<style>
:root {
  --sage: #7aab8a;
  --sage-light: #a8c9b4;
  --sage-pale: #e8f2ec;
  --sage-dark: #4a7a5c;
  --mint: #c8e6d4;
  --cream: #faf9f6;
  --white: #ffffff;
  --stone: #f2efea;
  --pebble: #e0dbd2;
  --ink: #1a1f1c;
  --ink-soft: #3d4740;
  --muted: #8a9490;
  --warn: #e07b54;
  --warn-pale: #fdf0eb;
  --danger: #d44f4f;
  --calorie-color: #7aab8a;
  --protein-color: #5b9bd5;
  --fat-color: #e8b84b;
  --carb-color: #c97dd0;
  --water-color: #6bb5d6;
  --shadow-sm: 0 1px 3px rgba(26,31,28,0.06), 0 1px 2px rgba(26,31,28,0.04);
  --shadow: 0 4px 16px rgba(26,31,28,0.08), 0 2px 6px rgba(26,31,28,0.04);
  --shadow-lg: 0 16px 48px rgba(26,31,28,0.12), 0 4px 16px rgba(26,31,28,0.06);
  --radius: 16px;
  --radius-sm: 10px;
  --radius-lg: 24px;
}

[data-theme="dark"] {
  --cream: #121614;
  --white: #1a1f1c;
  --stone: #242b27;
  --pebble: #3d4740;
  --ink: #faf9f6;
  --ink-soft: #e0dbd2;
  --muted: #8a9490;
  --sage-pale: #1e2923;
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.2);
  --shadow: 0 4px 16px rgba(0,0,0,0.3);
}

* { margin: 0; padding: 0; box-sizing: border-box; }

body {
  font-family: 'DM Sans', sans-serif;
  background: var(--cream);
  color: var(--ink);
  min-height: 100vh;
  overflow-x: hidden;
  transition: background .2s, color .2s;
}

/* ── LAYOUT ── */
.app { display: flex; flex-direction: column; min-height: 100vh; max-width: 480px; margin: 0 auto; position: relative; background: var(--cream); }

/* ── TOP BAR ── */
.topbar {
  background: var(--white);
  padding: 16px 20px 0;
  position: sticky; top: 0; z-index: 100;
  border-bottom: 1px solid var(--pebble);
  box-shadow: var(--shadow-sm);
}
.topbar-header { display: flex; align-items: center; justify-content: space-between; margin-bottom: 14px; }
.app-logo { font-family: 'DM Serif Display', serif; font-size: 22px; color: var(--sage-dark); letter-spacing: -0.5px; }
.app-logo span { color: var(--sage); }
.topbar-actions { display: flex; gap: 8px; }
.icon-btn { width: 36px; height: 36px; border-radius: 50%; border: none; background: var(--stone); color: var(--ink-soft); cursor: pointer; display: flex; align-items: center; justify-content: center; transition: all .2s; font-size: 16px; }
.icon-btn:hover { background: var(--sage-pale); color: var(--sage-dark); }

/* Calendar strip */
.calendar-strip { display: flex; gap: 6px; overflow-x: auto; padding-bottom: 14px; scrollbar-width: none; }
.calendar-strip::-webkit-scrollbar { display: none; }
.cal-day {
  flex: 0 0 44px; height: 62px; border-radius: var(--radius-sm); display: flex; flex-direction: column; align-items: center; justify-content: center; gap: 2px; cursor: pointer; transition: all .2s; border: 2px solid transparent;
  background: var(--stone); color: var(--ink);
}
.cal-day:hover { background: var(--sage-pale); border-color: var(--sage-light); }
.cal-day.active { background: var(--sage); border-color: var(--sage); color: white; box-shadow: 0 4px 12px rgba(122,171,138,0.4); }
.cal-day.today:not(.active) { border-color: var(--sage-light); }
.cal-day .day-name { font-size: 10px; font-weight: 500; opacity: 0.7; letter-spacing: 0.5px; text-transform: uppercase; }
.cal-day .day-num { font-size: 17px; font-weight: 600; }
.cal-day.active .day-name, .cal-day.active .day-num { color: white; opacity: 1; }

/* ── MAIN CONTENT ── */
.main { flex: 1; padding: 0 16px 100px; overflow-y: auto; }

/* ── TABS ── */
.tabs { display: flex; gap: 4px; padding: 14px 0 2px; }
.tab { flex: 1; padding: 8px 4px; border: none; background: none; font-family: 'DM Sans', sans-serif; font-size: 13px; font-weight: 500; color: var(--muted); cursor: pointer; border-radius: var(--radius-sm); transition: all .2s; position: relative; }
.tab.active { color: var(--sage-dark); background: var(--sage-pale); }
.tab-indicator { position: absolute; bottom: 0; left: 50%; transform: translateX(-50%); width: 20px; height: 2px; background: var(--sage); border-radius: 2px; display: none; }
.tab.active .tab-indicator { display: block; }

/* ── PAGE SECTIONS ── */
.page { display: none; animation: fadeIn .3s ease; }
.page.active { display: block; }
@keyframes fadeIn { from { opacity: 0; transform: translateY(6px); } to { opacity: 1; transform: translateY(0); } }

/* ── SECTION HEADERS ── */
.section-label { font-size: 11px; font-weight: 600; letter-spacing: 1px; text-transform: uppercase; color: var(--muted); margin: 20px 0 10px; }

/* ── CALORIE RING ── */
.summary-card {
  background: var(--white); border-radius: var(--radius-lg); padding: 20px; margin-top: 10px;
  box-shadow: var(--shadow); display: flex; align-items: center; gap: 20px;
}
.ring-container { position: relative; width: 110px; height: 110px; flex-shrink: 0; }
.ring-svg { width: 110px; height: 110px; transform: rotate(-90deg); }
.ring-track { fill: none; stroke: var(--pebble); stroke-width: 8; }
.ring-fill { fill: none; stroke-width: 8; stroke-linecap: round; transition: stroke-dashoffset 1s cubic-bezier(0.4,0,0.2,1); }
.ring-center { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); text-align: center; }
.ring-cal { font-family: 'DM Serif Display', serif; font-size: 22px; color: var(--ink); line-height: 1; }
.ring-label { font-size: 9px; font-weight: 600; letter-spacing: 0.8px; text-transform: uppercase; color: var(--muted); margin-top: 2px; }
.summary-details { flex: 1; }
.summary-title { font-family: 'DM Serif Display', serif; font-size: 16px; color: var(--ink); margin-bottom: 6px; }
.cal-remaining { font-size: 12px; color: var(--muted); margin-bottom: 12px; }
.cal-remaining strong { color: var(--sage-dark); }

/* Macro mini bars */
.macro-bars { display: flex; flex-direction: column; gap: 7px; }
.macro-row { display: flex; align-items: center; gap: 8px; }
.macro-dot { width: 7px; height: 7px; border-radius: 50%; flex-shrink: 0; }
.macro-name { font-size: 11px; color: var(--muted); width: 14px; flex-shrink: 0; }
.macro-bar-track { flex: 1; height: 5px; background: var(--pebble); border-radius: 3px; overflow: hidden; }
.macro-bar-fill { height: 100%; border-radius: 3px; transition: width 1s cubic-bezier(0.4,0,0.2,1); }
.macro-val { font-size: 11px; font-weight: 600; color: var(--ink-soft); width: 32px; text-align: right; }

/* ── WATER WIDGET ── */
.water-card {
  background: linear-gradient(135deg, #e8f4fa 0%, #d0eaf6 100%);
  border-radius: var(--radius); padding: 16px 18px; margin-top: 12px;
  display: flex; align-items: center; gap: 14px; box-shadow: var(--shadow-sm);
}
[data-theme="dark"] .water-card {
  background: linear-gradient(135deg, #1e2d35 0%, #15252e 100%);
}
.water-icon { font-size: 26px; }
.water-info { flex: 1; }
.water-title { font-size: 13px; font-weight: 600; color: #2c6b8a; margin-bottom: 4px; }
[data-theme="dark"] .water-title { color: #6bb5d6; }
.water-glasses { display: flex; gap: 5px; flex-wrap: wrap; margin-top: 6px; }
.glass-btn {
  width: 28px; height: 28px; border-radius: 8px; border: 2px solid rgba(107,181,214,0.3);
  background: transparent; cursor: pointer; transition: all .15s; display: flex; align-items: center; justify-content: center; font-size: 14px;
}
.glass-btn.filled { background: var(--water-color); border-color: var(--water-color); }
.glass-btn:hover:not(.filled) { border-color: var(--water-color); background: rgba(107,181,214,0.15); }
.water-ml { font-size: 20px; font-weight: 700; color: #2c6b8a; font-family: 'DM Serif Display', serif; }
[data-theme="dark"] .water-ml { color: #6bb5d6; }
.water-target { font-size: 11px; color: #5a9ab8; margin-left: 2px; }

/* ── VITAMIN SECTION ── */
.vitamins-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 0; }
.vitamin-pill {
  background: var(--white); border-radius: var(--radius-sm); padding: 12px 14px;
  box-shadow: var(--shadow-sm); display: flex; flex-direction: column; gap: 6px;
}
.vitamin-header { display: flex; justify-content: space-between; align-items: center; }
.vitamin-name { font-size: 12px; font-weight: 600; color: var(--ink-soft); }
.vitamin-pct { font-size: 12px; font-weight: 700; }
.vitamin-bar-track { height: 4px; background: var(--pebble); border-radius: 2px; overflow: hidden; }
.vitamin-bar-fill { height: 100%; border-radius: 2px; transition: width 1s ease; }
.vitamin-amounts { font-size: 10px; color: var(--muted); display: flex; justify-content: space-between; }

/* ── MEAL SECTIONS ── */
.meal-block { background: var(--white); border-radius: var(--radius); margin-top: 12px; overflow: hidden; box-shadow: var(--shadow-sm); }
.meal-header { padding: 14px 16px; display: flex; align-items: center; justify-content: space-between; cursor: pointer; user-select: none; }
.meal-header-left { display: flex; align-items: center; gap: 10px; }
.meal-emoji { font-size: 18px; }
.meal-name { font-size: 14px; font-weight: 600; color: var(--ink); }
.meal-cal-badge { font-size: 11px; color: var(--muted); background: var(--stone); padding: 3px 8px; border-radius: 20px; }
.meal-chevron { color: var(--muted); font-size: 12px; transition: transform .2s; }
.meal-block.expanded .meal-chevron { transform: rotate(180deg); }
.meal-body { display: none; border-top: 1px solid var(--pebble); }
.meal-block.expanded .meal-body { display: block; }
.food-item { display: flex; align-items: center; padding: 11px 16px; gap: 10px; border-bottom: 1px solid var(--stone); }
.food-item:last-child { border-bottom: none; }
.food-item-info { flex: 1; }
.food-item-name { font-size: 13px; font-weight: 500; color: var(--ink); }
.food-item-meta { font-size: 11px; color: var(--muted); margin-top: 2px; }
.food-item-cal { font-size: 13px; font-weight: 600; color: var(--ink-soft); }
.food-item-actions { display: flex; gap: 4px; }
.food-item-del, .food-item-edit { width: 24px; height: 24px; border-radius: 50%; border: none; background: none; color: var(--muted); cursor: pointer; font-size: 13px; display: flex; align-items: center; justify-content: center; transition: all .15s; }
.food-item-del:hover { background: #fde8e8; color: var(--danger); }
.food-item-edit:hover { background: var(--sage-pale); color: var(--sage-dark); }
.add-food-btn {
  width: 100%; padding: 11px 16px; border: none; background: var(--sage-pale); color: var(--sage-dark);
  font-family: 'DM Sans', sans-serif; font-size: 13px; font-weight: 500; cursor: pointer;
  display: flex; align-items: center; justify-content: center; gap: 6px; transition: all .15s;
}
.add-food-btn:hover { background: var(--mint); }

/* ── LIBRARY PAGE ── */
.library-search { display: flex; gap: 8px; margin-top: 10px; }
.search-input {
  flex: 1; padding: 10px 14px; border-radius: var(--radius-sm); border: 2px solid var(--pebble);
  font-family: 'DM Sans', sans-serif; font-size: 14px; background: var(--white); color: var(--ink); outline: none;
  transition: border-color .2s;
}
.search-input:focus { border-color: var(--sage); }
.primary-btn {
  padding: 10px 16px; background: var(--sage); color: white; border: none; border-radius: var(--radius-sm);
  font-family: 'DM Sans', sans-serif; font-size: 13px; font-weight: 600; cursor: pointer; transition: all .2s; white-space: nowrap;
}
.primary-btn:hover { background: var(--sage-dark); transform: translateY(-1px); box-shadow: 0 4px 12px rgba(122,171,138,0.4); }

.food-cards { display: flex; flex-direction: column; gap: 8px; margin-top: 10px; }
.food-card {
  background: var(--white); border-radius: var(--radius-sm); padding: 14px 16px;
  box-shadow: var(--shadow-sm); display: flex; align-items: center; gap: 12px;
  transition: all .2s; border: 2px solid transparent;
}
.food-card:hover { border-color: var(--sage-light); }
.food-card-clickable { flex: 1; display: flex; align-items: center; gap: 12px; cursor: pointer; }
.food-card-emoji { font-size: 24px; }
.food-card-info { flex: 1; }
.food-card-name { font-size: 14px; font-weight: 600; color: var(--ink); }
.food-card-macros { font-size: 11px; color: var(--muted); margin-top: 3px; }
.macro-chips { display: flex; gap: 4px; margin-top: 5px; }
.macro-chip { font-size: 10px; font-weight: 600; padding: 2px 7px; border-radius: 20px; }
.chip-p { background: #dbedf8; color: #3a7ab8; }
.chip-f { background: #fef3d8; color: #b8892a; }
.chip-c { background: #f5e8f9; color: #9a50a8; }
.food-card-cal { font-size: 15px; font-weight: 700; color: var(--ink); font-family: 'DM Serif Display', serif; text-align: right; }
.food-card-cal-unit { font-size: 10px; color: var(--muted); }
.food-card-actions { display: flex; gap: 4px; }

/* ── ANALYTICS PAGE ── */
.analytics-header { display: flex; justify-content: space-between; align-items: center; margin-top: 14px; margin-bottom: 4px; }
.analytics-title { font-family: 'DM Serif Display', serif; font-size: 18px; color: var(--ink); }
.period-toggle { display: flex; background: var(--stone); border-radius: 8px; padding: 3px; gap: 2px; }
.period-btn { padding: 5px 10px; border-radius: 6px; border: none; background: none; font-size: 12px; font-weight: 500; color: var(--muted); cursor: pointer; transition: all .15s; }
.period-btn.active { background: var(--white); color: var(--ink); box-shadow: var(--shadow-sm); }

.chart-card { background: var(--white); border-radius: var(--radius); padding: 16px; margin-top: 12px; box-shadow: var(--shadow-sm); }
.chart-title { font-size: 13px; font-weight: 600; color: var(--ink-soft); margin-bottom: 12px; }
.chart-area { position: relative; height: 120px; }
canvas { width: 100% !important; }

.insights-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 12px; }
.insight-card { background: var(--white); border-radius: var(--radius-sm); padding: 14px; box-shadow: var(--shadow-sm); }
.insight-val { font-family: 'DM Serif Display', serif; font-size: 22px; color: var(--ink); }
.insight-label { font-size: 11px; color: var(--muted); margin-top: 2px; }
.insight-trend { font-size: 11px; font-weight: 600; margin-top: 4px; }
.trend-up { color: var(--sage); }
.trend-down { color: var(--warn); }

/* ── MODAL ── */
.modal-overlay {
  position: fixed; inset: 0; background: rgba(26,31,28,0.45); z-index: 200;
  display: none; align-items: flex-end; justify-content: center;
  backdrop-filter: blur(4px);
}
.modal-overlay.open { display: flex; animation: overlayIn .25s ease; }
@keyframes overlayIn { from { opacity: 0; } to { opacity: 1; } }

.modal {
  background: var(--white); border-radius: var(--radius-lg) var(--radius-lg) 0 0;
  width: 100%; max-width: 480px; max-height: 90vh; overflow-y: auto;
  padding: 0 0 40px; animation: slideUp .3s cubic-bezier(0.34,1.56,0.64,1);
}
@keyframes slideUp { from { transform: translateY(60px); opacity: 0; } to { transform: translateY(0); opacity: 1; } }

.modal-handle { width: 40px; height: 4px; background: var(--pebble); border-radius: 2px; margin: 12px auto 0; }
.modal-header { padding: 16px 20px 4px; display: flex; align-items: center; justify-content: space-between; }
.modal-title { font-family: 'DM Serif Display', serif; font-size: 20px; color: var(--ink); }
.modal-close { width: 32px; height: 32px; border-radius: 50%; border: none; background: var(--stone); color: var(--muted); cursor: pointer; font-size: 16px; display: flex; align-items: center; justify-content: center; transition: all .15s; }
.modal-close:hover { background: var(--pebble); }
.modal-body { padding: 16px 20px; display: flex; flex-direction: column; gap: 14px; }

.form-field { display: flex; flex-direction: column; gap: 5px; }
.form-label { font-size: 12px; font-weight: 600; color: var(--muted); letter-spacing: 0.5px; text-transform: uppercase; }
.form-input, .form-select {
  padding: 11px 14px; border: 2px solid var(--pebble); border-radius: var(--radius-sm);
  font-family: 'DM Sans', sans-serif; font-size: 14px; background: var(--white); outline: none;
  transition: border-color .2s; color: var(--ink);
}
.form-input:focus, .form-select:focus { border-color: var(--sage); }
.form-row { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 10px; }
.form-hint { font-size: 11px; color: var(--muted); }

.vitamin-entry { display: flex; gap: 8px; align-items: center; }
.vitamin-entry .form-select { flex: 1.5; }
.vitamin-entry .form-input { flex: 1; }
.remove-vit-btn { width: 28px; height: 28px; border-radius: 50%; border: none; background: var(--stone); color: var(--muted); cursor: pointer; font-size: 16px; flex-shrink: 0; display: flex; align-items: center; justify-content: center; transition: all .15s; }
.remove-vit-btn:hover { background: #fde8e8; color: var(--danger); }
.add-vit-btn { display: flex; align-items: center; gap: 6px; background: none; border: 2px dashed var(--pebble); color: var(--muted); border-radius: var(--radius-sm); padding: 9px 14px; font-family: 'DM Sans', sans-serif; font-size: 13px; cursor: pointer; transition: all .15s; width: 100%; }
.add-vit-btn:hover { border-color: var(--sage-light); color: var(--sage-dark); background: var(--sage-pale); }

.form-divider { height: 1px; background: var(--pebble); margin: 2px 0; }
.form-section-label { font-size: 11px; font-weight: 700; letter-spacing: 1px; text-transform: uppercase; color: var(--sage-dark); padding: 2px 0; }

.saved-items-scroll { display: flex; flex-direction: column; gap: 6px; max-height: 180px; overflow-y: auto; padding: 2px; }
.saved-item-btn {
  display: flex; align-items: center; gap: 10px; padding: 10px 12px;
  background: var(--stone); border: 2px solid transparent; border-radius: var(--radius-sm);
  cursor: pointer; text-align: left; width: 100%; transition: all .15s; font-family: 'DM Sans', sans-serif;
}
.saved-item-btn:hover, .saved-item-btn.selected { border-color: var(--sage); background: var(--sage-pale); }
.saved-item-emoji { font-size: 18px; }
.saved-item-name { font-size: 13px; font-weight: 500; color: var(--ink); flex: 1; }
.saved-item-info { font-size: 11px; color: var(--muted); }

.modal-footer { padding: 0 20px; display: flex; gap: 10px; }
.secondary-btn { flex: 1; padding: 13px; border: 2px solid var(--pebble); border-radius: var(--radius-sm); background: none; font-family: 'DM Sans', sans-serif; font-size: 14px; font-weight: 600; color: var(--muted); cursor: pointer; transition: all .15s; }
.secondary-btn:hover { border-color: var(--ink-soft); color: var(--ink-soft); }
.submit-btn { flex: 2; padding: 13px; background: var(--sage); border: none; border-radius: var(--radius-sm); font-family: 'DM Sans', sans-serif; font-size: 14px; font-weight: 700; color: white; cursor: pointer; transition: all .2s; }
.submit-btn:hover { background: var(--sage-dark); box-shadow: 0 4px 16px rgba(122,171,138,0.4); }

/* Status colors */
.bar-green { background: var(--sage); }
.bar-warn { background: var(--warn); }
.bar-red { background: var(--danger); }
.pct-green { color: var(--sage-dark); }
.pct-warn { color: var(--warn); }

/* ── BOTTOM NAV ── */
.bottom-nav {
  position: fixed; bottom: 0; left: 50%; transform: translateX(-50%);
  width: 100%; max-width: 480px; background: var(--white);
  border-top: 1px solid var(--pebble); padding: 8px 0 20px;
  display: flex; z-index: 100; box-shadow: 0 -4px 20px rgba(26,31,28,0.06);
}
.nav-item { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 3px; cursor: pointer; padding: 4px 0; transition: all .2s; border: none; background: none; }
.nav-icon { font-size: 20px; transition: transform .2s; }
.nav-label { font-size: 10px; font-weight: 500; color: var(--muted); transition: color .2s; }
.nav-item.active .nav-label { color: var(--sage-dark); font-weight: 700; }
.nav-item.active .nav-icon { transform: translateY(-2px); }

/* toast */
.toast {
  position: fixed; top: 20px; left: 50%; transform: translateX(-50%) translateY(-80px);
  background: var(--ink); color: white; padding: 10px 20px; border-radius: 40px;
  font-size: 13px; font-weight: 500; z-index: 999; transition: transform .3s cubic-bezier(0.34,1.56,0.64,1);
  white-space: nowrap;
}
.toast.show { transform: translateX(-50%) translateY(0); }

/* empty state */
.empty-state { text-align: center; padding: 30px 20px; color: var(--muted); }
.empty-state-icon { font-size: 36px; margin-bottom: 10px; }
.empty-state-text { font-size: 13px; }
</style>
</head>
<body>

<div class="app">
  <div class="topbar">
    <div class="topbar-header">
      <div class="app-logo">Nou<span>ri</span></div>
      <div class="topbar-actions">
        <button class="icon-btn" onclick="toggleTheme()" title="Toggle Dark Mode">🌓</button>
        <button class="icon-btn" onclick="openModal('new-food')">＋</button>
        <button class="icon-btn" onclick="openModal('settings')">⚙</button>
      </div>
    </div>
    <div class="calendar-strip" id="calendarStrip"></div>
  </div>

  <div class="main">
    <div class="tabs">
      <button class="tab active" data-tab="dashboard">Today<div class="tab-indicator"></div></button>
      <button class="tab" data-tab="library">Library<div class="tab-indicator"></div></button>
      <button class="tab" data-tab="analytics">Analytics<div class="tab-indicator"></div></button>
    </div>

    <div class="page active" id="page-dashboard">
      <p class="section-label">Daily Overview</p>
      <div class="summary-card">
        <div class="ring-container">
          <svg class="ring-svg" viewBox="0 0 110 110">
            <circle class="ring-track" cx="55" cy="55" r="45"/>
            <circle class="ring-fill" id="calRing" cx="55" cy="55" r="45"
              stroke="var(--sage)" stroke-dasharray="283" stroke-dashoffset="283"/>
          </svg>
          <div class="ring-center">
            <div class="ring-cal" id="calConsumed">0</div>
            <div class="ring-label">kcal</div>
          </div>
        </div>
        <div class="summary-details">
          <div class="summary-title">Calories</div>
          <div class="cal-remaining" id="calRemaining">Start logging your meals today</div>
          <div class="macro-bars">
            <div class="macro-row">
              <div class="macro-dot" style="background:var(--protein-color)"></div>
              <div class="macro-name" style="color:var(--protein-color);font-size:10px;font-weight:700;">P</div>
              <div class="macro-bar-track"><div class="macro-bar-fill" id="pBar" style="background:var(--protein-color);width:0%"></div></div>
              <div class="macro-val" id="pVal">0g</div>
            </div>
            <div class="macro-row">
              <div class="macro-dot" style="background:var(--fat-color)"></div>
              <div class="macro-name" style="color:var(--fat-color);font-size:10px;font-weight:700;">F</div>
              <div class="macro-bar-track"><div class="macro-bar-fill" id="fBar" style="background:var(--fat-color);width:0%"></div></div>
              <div class="macro-val" id="fVal">0g</div>
            </div>
            <div class="macro-row">
              <div class="macro-dot" style="background:var(--carb-color)"></div>
              <div class="macro-name" style="color:var(--carb-color);font-size:10px;font-weight:700;">C</div>
              <div class="macro-bar-track"><div class="macro-bar-fill" id="cBar" style="background:var(--carb-color);width:0%"></div></div>
              <div class="macro-val" id="cVal">0g</div>
            </div>
          </div>
        </div>
      </div>

      <div class="water-card">
        <div class="water-icon">💧</div>
        <div class="water-info">
          <div class="water-title">Hydration</div>
          <div>
            <span class="water-ml" id="waterMl">0</span>
            <span class="water-target">/ 2000 ml</span>
          </div>
          <div class="water-glasses" id="waterGlasses"></div>
        </div>
      </div>

      <p class="section-label">Vitamins & Micronutrients</p>
      <div class="vitamins-grid" id="vitaminsGrid"></div>

      <p class="section-label">Meals</p>
      <div id="mealsContainer"></div>
    </div>

    <div class="page" id="page-library">
      <div class="library-search">
        <input class="search-input" id="librarySearch" placeholder="Search saved foods…" oninput="filterLibrary()">
        <button class="primary-btn" onclick="openCreateFoodModal()">＋ New</button>
      </div>
      <p class="section-label">Saved Foods</p>
      <div class="food-cards" id="libraryCards"></div>
    </div>

    <div class="page" id="page-analytics">
      <div class="analytics-header">
        <div class="analytics-title">Insights</div>
        <div class="period-toggle">
          <button class="period-btn active" onclick="setPeriod(this,'week')">Week</button>
          <button class="period-btn" onclick="setPeriod(this,'month')">Month</button>
          <button class="period-btn" onclick="setPeriod(this,'year')">Year</button>
        </div>
      </div>

      <div class="insights-grid">
        <div class="insight-card">
          <div class="insight-val" id="avgCal">—</div>
          <div class="insight-label">Avg. Calories</div>
          <div class="insight-trend trend-up" id="calTrend"></div>
        </div>
        <div class="insight-card">
          <div class="insight-val" id="avgProtein">—</div>
          <div class="insight-label">Avg. Protein</div>
          <div class="insight-trend trend-up" id="proteinTrend"></div>
        </div>
        <div class="insight-card">
          <div class="insight-val" id="streakDays">—</div>
          <div class="insight-label">Logging Days</div>
          <div class="insight-trend trend-up">🔥 Active history</div>
        </div>
        <div class="insight-card">
          <div class="insight-val" id="goalDays">—</div>
          <div class="insight-label">Goal Met Days</div>
          <div class="insight-trend trend-up" id="goalTrend"></div>
        </div>
      </div>

      <div class="chart-card">
        <div class="chart-title" id="calChartTitle">Calorie Intake</div>
        <div class="chart-area"><canvas id="calChart"></canvas></div>
      </div>

      <div class="chart-card">
        <div class="chart-title">Macronutrient Split (avg)</div>
        <div style="display:flex;align-items:center;justify-content:center;gap:24px;padding:8px 0">
          <canvas id="pieChart" width="130" height="130"></canvas>
          <div id="pieLegend" style="display:flex;flex-direction:column;gap:8px;"></div>
        </div>
      </div>

      <div class="chart-card">
        <div class="chart-title">Macronutrient Trend</div>
        <div class="chart-area"><canvas id="macroChart"></canvas></div>
      </div>

      <div class="chart-card">
        <div class="chart-title">Micro & Vitamin Balance Analytics</div>
        <div id="vitaminAnalytics" style="display:flex;flex-direction:column;gap:12px;margin-top:4px;"></div>
      </div>
    </div>
  </div>
</div>

<div class="bottom-nav">
  <button class="nav-item active" data-nav="dashboard">
    <span class="nav-icon">🏠</span><span class="nav-label">Today</span>
  </button>
  <button class="nav-item" data-nav="library">
    <span class="nav-icon">📚</span><span class="nav-label">Library</span>
  </button>
  <button class="nav-item" data-nav="analytics">
    <span class="nav-icon">📊</span><span class="nav-label">Analytics</span>
  </button>
</div>

<div class="modal-overlay" id="modal-new-food">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-header">
      <div class="modal-title" id="nfModalTitle">Log Food</div>
      <button class="modal-close" onclick="closeModal('new-food')">✕</button>
    </div>
    <div class="modal-body">
      <input type="hidden" id="nf-edit-index">
      <div class="form-field">
        <label class="form-label">Meal</label>
        <select class="form-select" id="nf-meal">
          <option value="breakfast">🌅 Breakfast</option>
          <option value="lunch">☀️ Lunch</option>
          <option value="dinner">🌙 Dinner</option>
          <option value="snacks">🍎 Snacks</option>
        </select>
      </div>

      <div id="nf-saved-section">
        <div class="form-divider"></div>
        <div class="form-section-label">From Saved Foods</div>
        <div class="saved-items-scroll" id="savedItemsList"></div>
      </div>

      <div class="form-field" id="weightField" style="display:none">
        <label class="form-label">Weight (g)</label>
        <input class="form-input" type="number" id="nf-weight" placeholder="e.g. 80" oninput="recalcFromSaved()">
        <div class="form-hint" id="weightHint"></div>
      </div>

      <div class="form-divider"></div>
      <div class="form-section-label" id="manualSectionLabel">Or Enter Manually</div>

      <div class="form-field">
        <label class="form-label">Food Name</label>
        <input class="form-input" type="text" id="nf-name" placeholder="e.g. Greek Yogurt">
      </div>
      <div class="form-field">
        <label class="form-label">Calories (kcal)</label>
        <input class="form-input" type="number" id="nf-cal" placeholder="e.g. 250">
      </div>
      <div class="form-field">
        <label class="form-label">Macros (g)</label>
        <div class="form-row">
          <div>
            <div style="font-size:11px;color:var(--protein-color);font-weight:600;margin-bottom:4px;">Protein</div>
            <input class="form-input" type="number" id="nf-p" placeholder="0" step="0.1">
          </div>
          <div>
            <div style="font-size:11px;color:var(--fat-color);font-weight:600;margin-bottom:4px;">Fat</div>
            <input class="form-input" type="number" id="nf-f" placeholder="0" step="0.1">
          </div>
          <div>
            <div style="font-size:11px;color:var(--carb-color);font-weight:600;margin-bottom:4px;">Carbs</div>
            <input class="form-input" type="number" id="nf-c" placeholder="0" step="0.1">
          </div>
        </div>
      </div>
      <div class="form-field">
        <label class="form-label">Vitamins</label>
        <div id="nf-vitamins"></div>
        <button class="add-vit-btn" onclick="addVitaminEntry('nf-vitamins')">＋ Add Vitamin</button>
      </div>
    </div>
    <div class="modal-footer">
      <button class="secondary-btn" onclick="closeModal('new-food')">Cancel</button>
      <button class="submit-btn" onclick="logFood()" id="nfSubmitBtn">Log Food</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="modal-create-food">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-header">
      <div class="modal-title" id="cfModalTitle">Save Food (per 100g)</div>
      <button class="modal-close" onclick="closeModal('create-food')">✕</button>
    </div>
    <div class="modal-body">
      <input type="hidden" id="cf-edit-id">
      <div class="form-field">
        <label class="form-label">Food Name</label>
        <input class="form-input" type="text" id="cf-name" placeholder="e.g. Chicken Breast">
      </div>
      <div class="form-field">
        <label class="form-label">Emoji</label>
        <input class="form-input" type="text" id="cf-emoji" placeholder="🍗" maxlength="2">
      </div>
      <div class="form-field">
        <label class="form-label">Calories per 100g</label>
        <input class="form-input" type="number" id="cf-cal" placeholder="e.g. 165">
      </div>
      <div class="form-field">
        <label class="form-label">Macros per 100g</label>
        <div class="form-row">
          <div>
            <div style="font-size:11px;color:var(--protein-color);font-weight:600;margin-bottom:4px;">Protein</div>
            <input class="form-input" type="number" id="cf-p" placeholder="0" step="0.1">
          </div>
          <div>
            <div style="font-size:11px;color:var(--fat-color);font-weight:600;margin-bottom:4px;">Fat</div>
            <input class="form-input" type="number" id="cf-f" placeholder="0" step="0.1">
          </div>
          <div>
            <div style="font-size:11px;color:var(--carb-color);font-weight:600;margin-bottom:4px;">Carbs</div>
            <input class="form-input" type="number" id="cf-c" placeholder="0" step="0.1">
          </div>
        </div>
      </div>
      <div class="form-field">
        <label class="form-label">Vitamins per 100g</label>
        <div id="cf-vitamins"></div>
        <button class="add-vit-btn" onclick="addVitaminEntry('cf-vitamins')">＋ Add Vitamin</button>
      </div>
    </div>
    <div class="modal-footer">
      <button class="secondary-btn" onclick="closeModal('create-food')">Cancel</button>
      <button class="submit-btn" onclick="saveFood()" id="cfSubmitBtn">Save to Library</button>
    </div>
  </div>
</div>

<div class="modal-overlay" id="modal-settings">
  <div class="modal">
    <div class="modal-handle"></div>
    <div class="modal-header">
      <div class="modal-title">Daily Goals</div>
      <button class="modal-close" onclick="closeModal('settings')">✕</button>
    </div>
    <div class="modal-body">
      <div class="form-section-label">Calories & Macros</div>
      <div class="form-field">
        <label class="form-label">Calories (kcal)</label>
        <input class="form-input" type="number" id="sg-cal" placeholder="2000">
      </div>
      <div class="form-field">
        <label class="form-label">Macros (g/day)</label>
        <div class="form-row">
          <div>
            <div style="font-size:11px;color:var(--protein-color);font-weight:600;margin-bottom:4px;">Protein</div>
            <input class="form-input" type="number" id="sg-p" placeholder="150">
          </div>
          <div>
            <div style="font-size:11px;color:var(--fat-color);font-weight:600;margin-bottom:4px;">Fat</div>
            <input class="form-input" type="number" id="sg-f" placeholder="65">
          </div>
          <div>
            <div style="font-size:11px;color:var(--carb-color);font-weight:600;margin-bottom:4px;">Carbs</div>
            <input class="form-input" type="number" id="sg-c" placeholder="250">
          </div>
        </div>
      </div>
      <div class="form-field">
        <label class="form-label">Water (ml/day)</label>
        <input class="form-input" type="number" id="sg-water" placeholder="2000">
      </div>

      <div class="form-divider"></div>
      <div class="form-section-label">Vitamins & Minerals (RDA / UL)</div>
      <div id="sg-micros" style="display:flex;flex-direction:column;gap:10px;"></div>
    </div>
    <div class="modal-footer">
      <button class="secondary-btn" onclick="closeModal('settings')">Cancel</button>
      <button class="submit-btn" onclick="saveSettings()">Save Goals</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
// ── DATA ──
const DEFAULT_GOALS = { cal: 2000, p: 150, f: 65, c: 250, water: 2000 };
let GOALS = JSON.parse(localStorage.getItem('nouri_goals') || 'null') || { ...DEFAULT_GOALS };
function saveGoals() { localStorage.setItem('nouri_goals', JSON.stringify(GOALS)); }

const MICRONUTRIENT_DEFAULTS = {
  A: 900, D: 20, E: 15, K: 120, C: 90,
  B1: 1.2, B2: 1.3, B3: 16, B5: 5, B6: 1.7, B7: 30, B9: 400, B12: 2.4,
  Calcium: 1000, Phosphorus: 700, Iron: 18, Magnesium: 400, Potassium: 3500,
  Zinc: 11, Sodium: 2300, Iodine: 150
};
const MICRONUTRIENT_LIMITS = {
  A: 3000, D: 100, E: 1000, K: 0, C: 2000,
  B1: 0, B2: 0, B3: 35, B5: 0, B6: 100, B7: 100, B9: 1000, B12: 0,
  Calcium: 2500, Phosphorus: 4000, Iron: 45, Magnesium: 350, Potassium: 0,
  Zinc: 40, Sodium: 2300, Iodine: 1100
};
const MICRONUTRIENT_UNITS = {
  A: 'μg', D: 'μg', E: 'mg', K: 'μg', C: 'mg',
  B1: 'mg', B2: 'mg', B3: 'mg', B5: 'mg', B6: 'mg', B7: 'μg', B9: 'μg', B12: 'μg',
  Calcium: 'mg', Phosphorus: 'mg', Iron: 'mg', Magnesium: 'mg', Potassium: 'mg',
  Zinc: 'mg', Sodium: 'mg', Iodine: 'μg'
};
const MICRONUTRIENT_LABELS = {
  A: 'Vit. A', D: 'Vit. D', E: 'Vit. E', K: 'Vit. K', C: 'Vit. C',
  B1: 'Vit. B1', B2: 'Vit. B2', B3: 'Vit. B3', B5: 'Vit. B5',
  B6: 'Vit. B6', B7: 'Vit. B7 (Biotin)', B9: 'Vit. B9', B12: 'Vit. B12',
  Calcium: 'Calcium', Phosphorus: 'Phosphorus', Iron: 'Iron', Magnesium: 'Magnesium',
  Potassium: 'Potassium', Zinc: 'Zinc', Sodium: 'Sodium', Iodine: 'Iodine'
};

// --- FIX: SMART MERGE FOR NEW MICRONUTRIENTS ---
const savedMicros = JSON.parse(localStorage.getItem('nouri_micros') || '{}');
let VITAMIN_GOALS = { ...MICRONUTRIENT_DEFAULTS, ...savedMicros };
localStorage.setItem('nouri_micros', JSON.stringify(VITAMIN_GOALS));

function saveMicroGoals() { localStorage.setItem('nouri_micros', JSON.stringify(VITAMIN_GOALS)); }

const VITAMIN_UNITS = MICRONUTRIENT_UNITS;
const VITAMIN_COLORS = [
  '#ff7e5f','#f9c74f','#90be6d','#4cc9f0','#7b2d8b','#f77f00',
  '#d62828','#023e8a','#2d6a4f','#e76f51','#457b9d','#a8dadc',
  '#6d6875','#b5838d','#e9c46a','#264653','#2a9d8f','#e9c46a','#f4a261','#3f51b5','#9c27b0'
];

const MEAL_CONFIG = [
  { id: 'breakfast', name: 'Breakfast', emoji: '🌅' },
  { id: 'lunch', name: 'Lunch', emoji: '☀️' },
  { id: 'dinner', name: 'Dinner', emoji: '🌙' },
  { id: 'snacks', name: 'Snacks', emoji: '🍎' },
];

let foodLibrary = JSON.parse(localStorage.getItem('nouri_lib') || 'null') || [
  { id: 'f1', name: 'Chicken Breast', emoji: '🍗', cal: 165, p: 31, f: 3.6, c: 0, vitamins: [{type:'B12',qty:0.3},{type:'Phosphorus',qty:228}] },
  { id: 'f2', name: 'Greek Yogurt', emoji: '🥛', cal: 59, p: 10, f: 0.4, c: 3.6, vitamins: [{type:'D',qty:1.2},{type:'Phosphorus',qty:144}] },
  { id: 'f3', name: 'Banana', emoji: '🍌', cal: 89, p: 1.1, f: 0.3, c: 23, vitamins: [{type:'B6',qty:0.4}] },
  { id: 'f4', name: 'Brown Rice', emoji: '🍚', cal: 112, p: 2.3, f: 0.9, c: 24, vitamins: [{type:'Phosphorus',qty:83}] },
  { id: 'f5', name: 'Broccoli', emoji: '🥦', cal: 34, p: 2.8, f: 0.4, c: 7, vitamins: [{type:'C',qty:89},{type:'K',qty:102}] },
  { id: 'f6', name: 'Salmon', emoji: '🐟', cal: 208, p: 20, f: 13, c: 0, vitamins: [{type:'D',qty:11},{type:'B12',qty:3.2},{type:'Phosphorus',qty:250}] },
  { id: 'f7', name: 'Oats', emoji: '🥣', cal: 389, p: 17, f: 7, c: 66, vitamins: [{type:'B7',qty:20},{type:'Phosphorus',qty:523}] },
  { id: 'f8', name: 'Egg', emoji: '🥚', cal: 155, p: 13, f: 11, c: 1.1, vitamins: [{type:'D',qty:2},{type:'B12',qty:1.1},{type:'B7',qty:10},{type:'Phosphorus',qty:198}] },
];

let dailyLogs = JSON.parse(localStorage.getItem('nouri_logs') || '{}');
let waterLogs = JSON.parse(localStorage.getItem('nouri_water') || '{}');
let selectedDate = todayStr();
let selectedSavedFood = null;
let currentTheme = localStorage.getItem('nouri_theme') || 'light';

function todayStr() { return new Date().toISOString().slice(0, 10); }
function saveLogs() { localStorage.setItem('nouri_logs', JSON.stringify(dailyLogs)); }
function saveLib() { localStorage.setItem('nouri_lib', JSON.stringify(foodLibrary)); }
function saveWater() { localStorage.setItem('nouri_water', JSON.stringify(waterLogs)); }

function getDay(d) {
  if (!dailyLogs[d]) dailyLogs[d] = { breakfast: [], lunch: [], dinner: [], snacks: [] };
  return dailyLogs[d];
}
function getWater(d) { return waterLogs[d] || 0; }

// ── THEME ENGINE ──
document.documentElement.setAttribute('data-theme', currentTheme);
function toggleTheme() {
  currentTheme = currentTheme === 'light' ? 'dark' : 'light';
  document.documentElement.setAttribute('data-theme', currentTheme);
  localStorage.setItem('nouri_theme', currentTheme);
  if (document.getElementById('page-analytics').classList.contains('active')) {
    renderAnalytics();
  }
}

// ── CALENDAR STRIP ──
function buildCalendar() {
  const strip = document.getElementById('calendarStrip');
  if(!strip) return;
  strip.innerHTML = '';
  const today = new Date();
  const days = ['S','M','T','W','T','F','S'];
  for (let i = -3; i <= 3; i++) {
    const d = new Date(today); d.setDate(d.getDate() + i);
    const ds = d.toISOString().slice(0, 10);
    const el = document.createElement('div');
    el.className = 'cal-day' + (ds === selectedDate ? ' active' : '') + (i === 0 ? ' today' : '');
    el.innerHTML = `<span class="day-name">${days[d.getDay()]}</span><span class="day-num">${d.getDate()}</span>`;
    el.onclick = () => { selectedDate = ds; buildCalendar(); renderDashboard(); };
    strip.appendChild(el);
  }
}

// ── DASHBOARD ──
function renderDashboard() {
  const day = getDay(selectedDate);
  let totCal = 0, totP = 0, totF = 0, totC = 0;
  const vitTotals = {};
  Object.keys(VITAMIN_GOALS).forEach(v => vitTotals[v] = 0);

  MEAL_CONFIG.forEach(m => {
    (day[m.id] || []).forEach(item => {
      totCal += item.cal || 0; totP += item.p || 0; totF += item.f || 0; totC += item.c || 0;
      (item.vitamins || []).forEach(v => { vitTotals[v.type] = (vitTotals[v.type] || 0) + v.qty; });
    });
  });

  // Ring
  const pct = Math.min(totCal / GOALS.cal, 1);
  const circ = 2 * Math.PI * 45;
  const offset = circ * (1 - pct);
  const ring = document.getElementById('calRing');
  if(ring) {
    ring.style.strokeDashoffset = offset;
    ring.style.stroke = totCal > GOALS.cal ? 'var(--danger)' : totCal / GOALS.cal >= 0.9 ? 'var(--warn)' : 'var(--sage)';
  }
  document.getElementById('calConsumed').textContent = Math.round(totCal);
  const rem = GOALS.cal - totCal;
  document.getElementById('calRemaining').innerHTML =
    rem > 0 ? `<strong>${Math.round(rem)} kcal</strong> remaining` :
    totCal === 0 ? 'Start logging your meals today' :
    `<span style="color:var(--danger)">+${Math.round(-rem)} kcal over goal</span>`;

  // Macro bars
  function setBar(id, val, goal, valId, unit='g') {
    const pct = Math.min(val / goal * 100, 100);
    const bar = document.getElementById(id);
    if(bar) {
      bar.style.width = pct + '%';
      bar.style.background = pct >= 100 ? 'var(--sage)' : pct > 80 ? 'var(--warn)' : '';
    }
    document.getElementById(valId).textContent = Math.round(val) + unit;
  }
  setBar('pBar', totP, GOALS.p, 'pVal'); setBar('fBar', totF, GOALS.f, 'fVal'); setBar('cBar', totC, GOALS.c, 'cVal');

  // Vitamins
  const grid = document.getElementById('vitaminsGrid');
  if(grid) {
    grid.innerHTML = '';
    Object.entries(VITAMIN_GOALS).forEach(([name, goal], i) => {
      const val = vitTotals[name] || 0;
      const pct = Math.min(val / goal * 100, 100);
      const color = VITAMIN_COLORS[i % VITAMIN_COLORS.length];
      const unit = VITAMIN_UNITS[name];
      const limit = MICRONUTRIENT_LIMITS[name] || 0;
      const limitText = limit > 0 ? `max: ${limit}${unit}` : 'no max';
      
      let fillStyle = `width:${pct}%;background:${pct >= 100 ? 'var(--sage)' : color}`;
      if(limit > 0 && val > limit) {
        fillStyle = `width:100%;background:var(--danger)`;
      }

      const pill = document.createElement('div');
      pill.className = 'vitamin-pill';
      pill.innerHTML = `
        <div class="vitamin-header">
          <span class="vitamin-name">${MICRONUTRIENT_LABELS[name] || name}</span>
          <span class="vitamin-pct" style="color:${(limit > 0 && val > limit) ? 'var(--danger)' : pct >= 100 ? 'var(--sage-dark)' : pct > 70 ? 'var(--warn)' : 'var(--muted)'}">${Math.round(pct)}%</span>
        </div>
        <div class="vitamin-bar-track"><div class="vitamin-bar-fill" style="${fillStyle}"></div></div>
        <div class="vitamin-amounts">
          <span>${val.toFixed(1)} / ${goal}${unit}</span>
          <span style="opacity:0.6; font-size:9px;">${limitText}</span>
        </div>`;
      grid.appendChild(pill);
    });
  }

  // Water
  const water = getWater(selectedDate);
  document.getElementById('waterMl').textContent = water;
  const glasses = document.getElementById('waterGlasses');
  if(glasses) {
    glasses.innerHTML = '';
    const total = 8;
    const filled = Math.min(Math.floor(water / 250), total);
    for (let i = 0; i < total; i++) {
      const btn = document.createElement('button');
      btn.className = 'glass-btn' + (i < filled ? ' filled' : '');
      btn.textContent = i < filled ? '💧' : '○';
      btn.onclick = () => addWater(selectedDate);
      glasses.appendChild(btn);
    }
  }

  // Meals
  const container = document.getElementById('mealsContainer');
  if(container) {
    container.innerHTML = '';
    MEAL_CONFIG.forEach(m => {
      const items = day[m.id] || [];
      const mealCal = items.reduce((s, i) => s + (i.cal || 0), 0);
      const block = document.createElement('div');
      block.className = 'meal-block expanded';
      block.id = 'meal-' + m.id;

      const itemsHtml = items.length === 0 ? '' : items.map((item, idx) => `
        <div class="food-item">
          <div class="food-item-info">
            <div class="food-item-name">${item.name}</div>
            <div class="food-item-meta">P ${item.p}g · F ${item.f}g · C ${item.c}g${item.weight ? ' · ' + item.weight + 'g' : ''}</div>
          </div>
          <div class="food-item-cal">${Math.round(item.cal)} kcal</div>
          <div class="food-item-actions">
            <button class="food-item-edit" onclick="openEditLoggedFoodModal('${m.id}', ${idx})" title="Edit entry">✏️</button>
            <button class="food-item-del" onclick="deleteItem('${m.id}',${idx})" title="Delete entry">✕</button>
          </div>
        </div>`).join('');

      block.innerHTML = `
        <div class="meal-header" onclick="if(!event.target.classList.contains('add-food-btn')) this.parentElement.classList.toggle('expanded')">
          <div class="meal-header-left">
            <span class="meal-emoji">${m.emoji}</span>
            <span class="meal-name">${m.name}</span>
          </div>
          <div style="display:flex;align-items:center;gap:8px">
            <span class="meal-cal-badge">${Math.round(mealCal)} kcal</span>
            <span class="meal-chevron">▼</span>
          </div>
        </div>
        <div class="meal-body">
          ${itemsHtml || '<div class="empty-state" style="padding:14px"><div style="font-size:11px;color:var(--muted)">No items yet</div></div>'}
          <button class="add-food-btn" onclick="openModal('new-food','${m.id}')">＋ Add Food</button>
        </div>`;
      container.appendChild(block);
    });
  }
}

function deleteItem(mealId, idx) {
  const day = getDay(selectedDate);
  day[mealId].splice(idx, 1);
  saveLogs(); renderDashboard();
}

function addWater(d) {
  waterLogs[d] = (waterLogs[d] || 0) + 250;
  if (waterLogs[d] > GOALS.water) waterLogs[d] = GOALS.water;
  saveWater(); renderDashboard();
  showToast('💧 Glass added!');
}

// ── LIBRARY MGMT ──
function renderLibrary() {
  const q = (document.getElementById('librarySearch')?.value || '').toLowerCase();
  const filtered = foodLibrary.filter(f => f.name.toLowerCase().includes(q));
  const container = document.getElementById('libraryCards');
  if (!container) return;
  container.innerHTML = filtered.length === 0
    ? '<div class="empty-state"><div class="empty-state-icon">🔍</div><div class="empty-state-text">No foods found</div></div>'
    : filtered.map(f => `
      <div class="food-card">
        <div class="food-card-clickable" onclick="quickAddFromLib('${f.id}')">
          <div class="food-card-emoji">${f.emoji || '🍽'}</div>
          <div class="food-card-info">
            <div class="food-card-name">${f.name}</div>
            <div class="macro-chips">
              <span class="macro-chip chip-p">P ${f.p}g</span>
              <span class="macro-chip chip-f">F ${f.f}g</span>
              <span class="macro-chip chip-c">C ${f.c}g</span>
            </div>
          </div>
          <div>
            <div class="food-card-cal">${f.cal}</div>
            <div class="food-card-cal-unit">kcal/100g</div>
          </div>
        </div>
        <div class="food-card-actions">
          <button class="food-item-edit" onclick="openEditLibraryModal('${f.id}')">✏️</button>
          <button class="food-item-del" onclick="deleteLibraryItem('${f.id}')">✕</button>
        </div>
      </div>`).join('');
}
function filterLibrary() { renderLibrary(); }

function quickAddFromLib(id) {
  const food = foodLibrary.find(f => f.id === id);
  if (!food) return;
  openModal('new-food');
  setTimeout(() => selectSavedFood(id), 100);
}

function openCreateFoodModal() {
  document.getElementById('cf-edit-id').value = '';
  document.getElementById('cfModalTitle').textContent = "Save Food (per 100g)";
  document.getElementById('cfSubmitBtn').textContent = "Save to Library";
  ['cf-name','cf-emoji','cf-cal','cf-p','cf-f','cf-c'].forEach(id => document.getElementById(id).value = '');
  document.getElementById('cf-vitamins').innerHTML = '';
  openModal('create-food');
}

function openEditLibraryModal(id) {
  const f = foodLibrary.find(item => item.id === id);
  if(!f) return;
  document.getElementById('cf-edit-id').value = f.id;
  document.getElementById('cfModalTitle').textContent = "Edit Library Food";
  document.getElementById('cfSubmitBtn').textContent = "Update Food";
  
  document.getElementById('cf-name').value = f.name;
  document.getElementById('cf-emoji').value = f.emoji || '🍽';
  document.getElementById('cf-cal').value = f.cal;
  document.getElementById('cf-p').value = f.p;
  document.getElementById('cf-f').value = f.f;
  document.getElementById('cf-c').value = f.c;
  
  const vitDiv = document.getElementById('cf-vitamins');
  vitDiv.innerHTML = '';
  (f.vitamins || []).forEach(v => {
    addVitaminEntry('cf-vitamins', v.type, v.qty);
  });
  openModal('create-food');
}

function deleteLibraryItem(id) {
  if(confirm('Are you sure you want to remove this item from your library?')) {
    foodLibrary = foodLibrary.filter(f => f.id !== id);
    saveLib();
    renderLibrary();
    showToast('🗑️ Food removed from library');
  }
}

// ── EDIT ALREADY LOGGED FOOD ──
function openEditLoggedFoodModal(mealId, idx) {
  const day = getDay(selectedDate);
  const item = day[mealId][idx];
  if(!item) return;

  openModal('new-food');
  
  document.getElementById('nfModalTitle').textContent = "Edit Logged Food";
  document.getElementById('nfSubmitBtn').textContent = "Update Entry";
  document.getElementById('nf-saved-section').style.display = 'none';
  document.getElementById('manualSectionLabel').textContent = "Food Metrics Data";
  
  document.getElementById('nf-edit-index').value = idx;
  document.getElementById('nf-meal').value = mealId;
  document.getElementById('nf-name').value = item.name;
  document.getElementById('nf-cal').value = item.cal;
  document.getElementById('nf-p').value = item.p;
  document.getElementById('nf-f').value = item.f;
  document.getElementById('nf-c').value = item.c;
  
  if(item.weight) {
    document.getElementById('weightField').style.display = 'block';
    document.getElementById('nf-weight').value = item.weight;
    document.getElementById('weightHint').textContent = "Manually modified or baseline loaded weight config.";
  } else {
    document.getElementById('weightField').style.display = 'none';
  }

  const vitDiv = document.getElementById('nf-vitamins');
  vitDiv.innerHTML = '';
  (item.vitamins || []).forEach(v => {
    addVitaminEntry('nf-vitamins', v.type, v.qty);
  });
}

// ── MODAL INTERACTION CONTROLS ──
function openModal(name, mealPreset) {
  const overlay = document.getElementById('modal-' + name);
  if (!overlay) return;
  overlay.classList.add('open');
  document.body.style.overflow = 'hidden';

  if (name === 'new-food' && !document.getElementById('nf-edit-index').value) {
    document.getElementById('nfModalTitle').textContent = "Log Food";
    document.getElementById('nfSubmitBtn').textContent = "Log Food";
    document.getElementById('nf-saved-section').style.display = 'block';
    document.getElementById('manualSectionLabel').textContent = "Or Enter Manually";
    populateSavedList();
    selectedSavedFood = null;
    document.getElementById('weightField').style.display = 'none';
    if (mealPreset) document.getElementById('nf-meal').value = mealPreset;
    clearNFForm();
  }

  if (name === 'settings') {
    document.getElementById('sg-cal').value = GOALS.cal;
    document.getElementById('sg-p').value = GOALS.p;
    document.getElementById('sg-f').value = GOALS.f;
    document.getElementById('sg-c').value = GOALS.c;
    document.getElementById('sg-water').value = GOALS.water;
    const microsDiv = document.getElementById('sg-micros');
    microsDiv.innerHTML = '';
    Object.entries(VITAMIN_GOALS).forEach(([key, val]) => {
      const unit = MICRONUTRIENT_UNITS[key];
      const limit = MICRONUTRIENT_LIMITS[key] || 0;
      const limitStr = limit > 0 ? ` / Max UL: ${limit}${unit}` : '';
      const row = document.createElement('div');
      row.style.cssText = 'display:flex;align-items:center;gap:10px; margin-bottom:4px;';
      row.innerHTML = `
        <label style="font-size:12px;font-weight:500;color:var(--ink-soft);flex:1;min-width:100px">${MICRONUTRIENT_LABELS[key]}</label>
        <input class="form-input" type="number" data-key="${key}" value="${val}" style="width:80px;text-align:right; padding:6px 8px;">
        <span style="font-size:11px;color:var(--muted);width:110px">${unit}${limitStr}</span>`;
      microsDiv.appendChild(row);
    });
  }
}

function closeModal(name) {
  document.getElementById('modal-' + name).classList.remove('open');
  document.body.style.overflow = '';
  if(name === 'new-food') {
    setTimeout(() => {
      document.getElementById('nf-edit-index').value = '';
    }, 200);
  }
}

function saveSettings() {
  GOALS.cal = parseFloat(document.getElementById('sg-cal').value) || GOALS.cal;
  GOALS.p = parseFloat(document.getElementById('sg-p').value) || GOALS.p;
  GOALS.f = parseFloat(document.getElementById('sg-f').value) || GOALS.f;
  GOALS.c = parseFloat(document.getElementById('sg-c').value) || GOALS.c;
  GOALS.water = parseFloat(document.getElementById('sg-water').value) || GOALS.water;
  document.querySelectorAll('#sg-micros input[data-key]').forEach(input => {
    const key = input.dataset.key;
    const val = parseFloat(input.value);
    if (key && val > 0) VITAMIN_GOALS[key] = val;
  });
  saveGoals(); saveMicroGoals();
  closeModal('settings');
  renderDashboard();
  showToast('✓ Goals saved!');
}

function clearNFForm() {
  ['nf-name','nf-cal','nf-p','nf-f','nf-c','nf-edit-index'].forEach(id => {
    const el = document.getElementById(id); if (el) el.value = '';
  });
  document.getElementById('nf-vitamins').innerHTML = '';
  document.getElementById('nf-weight').value = '';
  selectedSavedFood = null;
}

function populateSavedList() {
  const list = document.getElementById('savedItemsList');
  if(!list) return;
  list.innerHTML = foodLibrary.map(f => `
    <button class="saved-item-btn" id="si-${f.id}" onclick="selectSavedFood('${f.id}')">
      <span class="saved-item-emoji">${f.emoji || '🍽'}</span>
      <span class="saved-item-name">${f.name}</span>
      <span class="saved-item-info">${f.cal} kcal/100g</span>
    </button>`).join('');
}

function selectSavedFood(id) {
  selectedSavedFood = foodLibrary.find(f => f.id === id);
  if (!selectedSavedFood) return;
  document.querySelectorAll('.saved-item-btn').forEach(b => b.classList.remove('selected'));
  const btn = document.getElementById('si-' + id);
  if (btn) btn.classList.add('selected');

  document.getElementById('weightField').style.display = 'block';
  document.getElementById('nf-name').value = selectedSavedFood.name;
  document.getElementById('nf-weight').value = 100;
  recalcFromSaved();
}

function recalcFromSaved() {
  if (!selectedSavedFood) return;
  const w = parseFloat(document.getElementById('nf-weight').value) || 0;
  const scale = w / 100;
  document.getElementById('nf-cal').value = (selectedSavedFood.cal * scale).toFixed(1);
  document.getElementById('nf-p').value = (selectedSavedFood.p * scale).toFixed(1);
  document.getElementById('nf-f').value = (selectedSavedFood.f * scale).toFixed(1);
  document.getElementById('nf-c').value = (selectedSavedFood.c * scale).toFixed(1);
  document.getElementById('weightHint').textContent =
    `Auto-calculated from ${w}g (baseline: ${selectedSavedFood.cal} kcal / 100g)`;

  const vitDiv = document.getElementById('nf-vitamins');
  vitDiv.innerHTML = '';
  (selectedSavedFood.vitamins || []).forEach(v => {
    addVitaminEntry('nf-vitamins', v.type, (v.qty * scale).toFixed(2));
  });
}

function addVitaminEntry(containerId, typeVal = '', qtyVal = '') {
  const container = document.getElementById(containerId);
  const entry = document.createElement('div');
  entry.className = 'vitamin-entry';
  entry.style.marginBottom = '8px';
  entry.innerHTML = `
    <select class="form-select" style="padding:6px 10px;">
      ${Object.keys(VITAMIN_GOALS).map(v => `<option value="${v}" ${v === typeVal ? 'selected' : ''}>${MICRONUTRIENT_LABELS[v]}</option>`).join('')}
    </select>
    <input class="form-select" type="number" placeholder="qty" value="${qtyVal}" style="width:80px; padding:6px 8px;" step="0.01">
    <button class="remove-vit-btn" onclick="this.parentElement.remove()">✕</button>`;
  container.appendChild(entry);
}

function logFood() {
  const name = document.getElementById('nf-name').value.trim();
  const cal = parseFloat(document.getElementById('nf-cal').value) || 0;
  const p = parseFloat(document.getElementById('nf-p').value) || 0;
  const f = parseFloat(document.getElementById('nf-f').value) || 0;
  const c = parseFloat(document.getElementById('nf-c').value) || 0;
  const meal = document.getElementById('nf-meal').value;
  const weight = parseFloat(document.getElementById('nf-weight').value) || null;
  const editIdx = document.getElementById('nf-edit-index').value;

  if (!name) { showToast('⚠️ Please enter a food name'); return; }

  const vitamins = [];
  document.querySelectorAll('#nf-vitamins .vitamin-entry').forEach(entry => {
    const type = entry.querySelector('select').value;
    const qty = parseFloat(entry.querySelector('input').value) || 0;
    if (qty > 0) vitamins.push({ type, qty });
  });

  const day = getDay(selectedDate);
  const foodObject = { name, cal, p, f, c, vitamins, weight };

  if(editIdx !== '') {
    day[meal][parseInt(editIdx)] = foodObject;
    showToast(`✓ ${name} updated!`);
  } else {
    day[meal].push(foodObject);
    showToast(`✓ ${name} logged!`);
  }
  
  saveLogs(); renderDashboard(); closeModal('new-food');
}

function saveFood() {
  const name = document.getElementById('cf-name').value.trim();
  const emoji = document.getElementById('cf-emoji').value || '🍽';
  const cal = parseFloat(document.getElementById('cf-cal').value) || 0;
  const p = parseFloat(document.getElementById('cf-p').value) || 0;
  const f = parseFloat(document.getElementById('cf-f').value) || 0;
  const c = parseFloat(document.getElementById('cf-c').value) || 0;
  const editId = document.getElementById('cf-edit-id').value;
  
  if (!name) { showToast('⚠️ Please enter a food name'); return; }

  const vitamins = [];
  document.querySelectorAll('#cf-vitamins .vitamin-entry').forEach(entry => {
    const type = entry.querySelector('select').value;
    const qty = parseFloat(entry.querySelector('input').value) || 0;
    if (qty > 0) vitamins.push({ type, qty });
  });

  if(editId) {
    const idx = foodLibrary.findIndex(item => item.id === editId);
    if(idx !== -1) {
      foodLibrary[idx] = { id: editId, name, emoji, cal, p, f, c, vitamins };
      showToast(`✓ Updated library item!`);
    }
  } else {
    foodLibrary.push({ id: 'f' + Date.now(), name, emoji, cal, p, f, c, vitamins });
    showToast(`✓ Saved to library!`);
  }
  
  saveLib(); renderLibrary(); closeModal('create-food');
}

// ── EXTENDED ANALYTICS ENG ──
let analyticsPeriod = 'week';
function setPeriod(btn, period) {
  document.querySelectorAll('.period-btn').forEach(b => b.classList.remove('active'));
  btn.classList.add('active');
  analyticsPeriod = period;
  renderAnalytics();
}

function renderAnalytics() {
  let days = 7;
  if(analyticsPeriod === 'month') days = 30;
  if(analyticsPeriod === 'year') days = 365;
  
  document.getElementById('calChartTitle').textContent = `Calorie Intake — Last ${days} Days`;
  
  const labels = [], calData = [], pData = [], fData = [], cData = [];
  let totalCal = 0, totalP = 0, goalMetDays = 0, loggedDays = 0;
  const vitAvgTotals = {};
  Object.keys(VITAMIN_GOALS).forEach(k => vitAvgTotals[k] = 0);

  for (let i = days - 1; i >= 0; i--) {
    const d = new Date(); d.setDate(d.getDate() - i);
    const ds = d.toISOString().slice(0, 10);
    const day = dailyLogs[ds];
    let cal = 0, p = 0, f = 0, c = 0;
    
    if (day) {
      let loggedAtAll = false;
      MEAL_CONFIG.forEach(m => {
        if(day[m.id] && day[m.id].length > 0) loggedAtAll = true;
        (day[m.id] || []).forEach(item => { 
          cal += item.cal || 0; p += item.p || 0; f += item.f || 0; c += item.c || 0; 
          (item.vitamins || []).forEach(v => {
            if(vitAvgTotals[v.type] !== undefined) vitAvgTotals[v.type] += v.qty;
          });
        });
      });
      if (cal > 0 || loggedAtAll) { loggedDays++; totalCal += cal; totalP += p; }
      if (cal >= GOALS.cal * 0.8 && cal <= GOALS.cal * 1.2) goalMetDays++;
    }
    
    if(days === 7) {
      labels.push(d.getDate() + '/' + (d.getMonth() + 1));
    } else if(days === 30) {
      labels.push(i % 5 === 0 ? d.getDate() + '/' + (d.getMonth() + 1) : '');
    } else {
      labels.push(i % 50 === 0 ? d.getDate() + '/' + (d.getMonth() + 1) : '');
    }
    
    calData.push(Math.round(cal)); pData.push(Math.round(p)); fData.push(Math.round(f)); cData.push(Math.round(c));
  }

  const avgCal = loggedDays ? Math.round(totalCal / loggedDays) : 0;
  const avgP = loggedDays ? Math.round(totalP / loggedDays) : 0;
  
  if (loggedDays > 0) {
    Object.keys(vitAvgTotals).forEach(k => vitAvgTotals[k] /= loggedDays);
  }

  document.getElementById('avgCal').textContent = avgCal || '—';
  document.getElementById('avgProtein').textContent = avgP ? avgP + 'g' : '—';
  document.getElementById('streakDays').textContent = `${loggedDays}/${days}d`;
  document.getElementById('goalDays').textContent = goalMetDays;
  document.getElementById('calTrend').textContent = avgCal ? (avgCal >= GOALS.cal * 0.9 && avgCal <= GOALS.cal * 1.1 ? '✓ Balanced' : avgCal > GOALS.cal ? '↑ Surplus' : '↓ Deficit') : '';
  document.getElementById('proteinTrend').textContent = avgP ? (avgP >= GOALS.p * 0.8 ? '✓ Optimum' : '↑ Low Protein') : '';
  document.getElementById('goalTrend').textContent = goalMetDays > 0 ? `${Math.round(goalMetDays/days*100)}% of period` : '0%';

  drawCalChart(labels, calData);
  drawPieChart(pData, fData, cData);
  drawMacroChart(labels, pData, fData, cData);
  renderVitaminAnalyticsExtended(vitAvgTotals);
}

function drawPieChart(pData, fData, cData) {
  const canvas = document.getElementById('pieChart');
  const ctx = canvas.getContext('2d');
  const size = 130;
  canvas.width = size; canvas.height = size;
  ctx.clearRect(0, 0, size, size);

  const avgP = pData.reduce((a, b) => a + b, 0) / pData.length || 0;
  const avgF = fData.reduce((a, b) => a + b, 0) / fData.length || 0;
  const avgC = cData.reduce((a, b) => a + b, 0) / cData.length || 0;

  const calP = avgP * 4, calF = avgF * 9, calC = avgC * 4;
  const total = calP + calF + calC;

  const legend = document.getElementById('pieLegend');
  legend.innerHTML = '';

  if (total === 0) {
    ctx.fillStyle = 'var(--pebble)';
    ctx.beginPath(); ctx.arc(size/2, size/2, size/2 - 10, 0, Math.PI * 2); ctx.fill();
    ctx.fillStyle = 'var(--muted)'; ctx.font = '10px DM Sans'; ctx.textAlign = 'center';
    ctx.fillText('No data', size/2, size/2 + 4);
    return;
  }

  const slices = [
    { label: 'Protein', val: calP, avg: avgP, unit: 'g', color: '#5b9bd5' },
    { label: 'Fat', val: calF, avg: avgF, unit: 'g', color: '#e8b84b' },
    { label: 'Carbs', val: calC, avg: avgC, unit: 'g', color: '#c97dd0' },
  ];

  const cx = size / 2, cy = size / 2, r = size / 2 - 8, innerR = r * 0.52;
  let startAngle = -Math.PI / 2;

  slices.forEach(s => {
    const sweep = (s.val / total) * Math.PI * 2;
    ctx.beginPath();
    ctx.moveTo(cx, cy);
    ctx.arc(cx, cy, r, startAngle, startAngle + sweep);
    ctx.closePath();
    ctx.fillStyle = s.color;
    ctx.fill();
    ctx.strokeStyle = currentTheme === 'dark' ? '#1a1f1c' : '#fff';
    ctx.lineWidth = 2;
    ctx.stroke();
    startAngle += sweep;
  });

  ctx.beginPath(); ctx.arc(cx, cy, innerR, 0, Math.PI * 2);
  ctx.fillStyle = varColor('--white'); ctx.fill();

  ctx.fillStyle = varColor('--ink'); ctx.font = `bold 13px DM Sans`; ctx.textAlign = 'center';
  ctx.fillText(Math.round(total), cx, cy - 2);
  ctx.fillStyle = 'var(--muted)'; ctx.font = '9px DM Sans';
  ctx.fillText('avg kcal', cx, cy + 10);

  slices.forEach(s => {
    const pct = total > 0 ? Math.round(s.val / total * 100) : 0;
    const item = document.createElement('div');
    item.style.cssText = 'display:flex;align-items:center;gap:6px;';
    item.innerHTML = `
      <div style="width:10px;height:10px;border-radius:3px;background:${s.color};flex-shrink:0"></div>
      <div>
        <div style="font-size:12px;font-weight:600;color:var(--ink-soft)">${s.label}</div>
        <div style="font-size:11px;color:var(--muted)">${Math.round(s.avg)}${s.unit} · ${pct}%</div>
      </div>`;
    legend.appendChild(item);
  });
}

function drawCalChart(labels, data) {
  const canvas = document.getElementById('calChart');
  canvas.height = 120;
  const ctx = canvas.getContext('2d');
  const w = canvas.parentElement.offsetWidth || 320;
  canvas.width = w;
  ctx.clearRect(0, 0, w, 120);

  const pad = { l: 34, r: 10, t: 10, b: 24 };
  const cw = w - pad.l - pad.r, ch = 120 - pad.t - pad.b;
  const max = Math.max(...data, GOALS.cal) * 1.1 || 2200;
  const n = data.length;

  const gy = pad.t + ch * (1 - GOALS.cal / max);
  ctx.strokeStyle = 'rgba(122,171,138,0.4)'; ctx.lineWidth = 1; ctx.setLineDash([4, 4]);
  ctx.beginPath(); ctx.moveTo(pad.l, gy); ctx.lineTo(pad.l + cw, gy); ctx.stroke();
  ctx.setLineDash([]);

  ctx.strokeStyle = currentTheme === 'dark' ? 'rgba(255,255,255,0.05)' : 'rgba(0,0,0,0.05)';
  [0.25, 0.5, 0.75, 1].forEach(f => {
    const y = pad.t + ch * f;
    ctx.beginPath(); ctx.moveTo(pad.l, y); ctx.lineTo(pad.l + cw, y); ctx.stroke();
  });

  const grad = ctx.createLinearGradient(0, pad.t, 0, pad.t + ch);
  grad.addColorStop(0, 'rgba(122,171,138,0.25)'); grad.addColorStop(1, 'rgba(122,171,138,0)');
  ctx.beginPath();
  data.forEach((v, i) => {
    const x = pad.l + (i / (n - 1)) * cw;
    const y = pad.t + ch * (1 - v / max);
    i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
  });
  ctx.lineTo(pad.l + cw, pad.t + ch); ctx.lineTo(pad.l, pad.t + ch); ctx.closePath();
  ctx.fillStyle = grad; ctx.fill();

  ctx.beginPath(); ctx.strokeStyle = 'var(--sage)'; ctx.lineWidth = 2.5; ctx.lineJoin = 'round';
  data.forEach((v, i) => {
    const x = pad.l + (i / (n - 1)) * cw;
    const y = pad.t + ch * (1 - v / max);
    i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
  });
  ctx.stroke();

  ctx.font = '9px DM Sans'; ctx.fillStyle = 'var(--muted)'; ctx.textAlign = 'center';
  data.forEach((v, i) => {
    if (labels[i] === '') return;
    const x = pad.l + (i / (n - 1)) * cw;
    ctx.beginPath(); ctx.arc(x, y, 2.5, 0, Math.PI * 2);
    ctx.fillStyle = 'var(--sage)'; ctx.fill();
    ctx.fillStyle = 'var(--muted)';
    ctx.fillText(labels[i], x, pad.t + ch + 14);
  });

  ctx.fillStyle = 'var(--muted)'; ctx.textAlign = 'right'; ctx.font = '8px DM Sans';
  [0, 0.5, 1].forEach(f => {
    ctx.fillText(Math.round(max * (1 - f)), pad.l - 4, pad.t + ch * f + 3);
  });
}

function drawMacroChart(labels, pData, fData, cData) {
  const canvas = document.getElementById('macroChart');
  canvas.height = 120;
  const ctx = canvas.getContext('2d');
  const w = canvas.parentElement.offsetWidth || 320;
  canvas.width = w;
  ctx.clearRect(0, 0, w, 120);

  const pad = { l: 30, r: 10, t: 10, b: 24 };
  const cw = w - pad.l - pad.r, ch = 120 - pad.t - pad.b;
  const max = Math.max(...pData, ...fData, ...cData, 50) * 1.1;
  const n = pData.length;

  const series = [
    { data: pData, color: 'var(--protein-color)' },
    { data: fData, color: 'var(--fat-color)' },
    { data: cData, color: 'var(--carb-color)' },
  ];

  series.forEach(s => {
    ctx.beginPath(); ctx.strokeStyle = s.color; ctx.lineWidth = 1.8; ctx.lineJoin = 'round';
    s.data.forEach((v, i) => {
      const x = pad.l + (i / (n - 1)) * cw;
      const y = pad.t + ch * (1 - v / max);
      i === 0 ? ctx.moveTo(x, y) : ctx.lineTo(x, y);
    });
    ctx.stroke();
  });

  ctx.fillStyle = 'var(--muted)'; ctx.textAlign = 'center'; ctx.font = '9px DM Sans';
  pData.forEach((_, i) => {
    if (labels[i] === '') return;
    const x = pad.l + (i / (n - 1)) * cw;
    ctx.fillText(labels[i], x, pad.t + ch + 14);
  });

  let lx = pad.l;
  const legend = [['P', 'var(--protein-color)'], ['F', 'var(--fat-color)'], ['C', 'var(--carb-color)']];
  legend.forEach(([label, color]) => {
    ctx.fillStyle = color; ctx.fillRect(lx, pad.t, 8, 8);
    ctx.fillStyle = 'var(--muted)'; ctx.textAlign = 'left'; ctx.font = '9px DM Sans';
    ctx.fillText(label, lx + 11, pad.t + 7);
    lx += 28;
  });
}

function varColor(cssVar) {
  return getComputedStyle(document.documentElement).getPropertyValue(cssVar).trim();
}

function renderVitaminAnalyticsExtended(vitAverages) {
  const container = document.getElementById('vitaminAnalytics');
  if(!container) return;
  container.innerHTML = '';

  Object.entries(VITAMIN_GOALS).forEach(([name, goal], i) => {
    const avg = vitAverages[name] || 0;
    const pct = goal > 0 ? (avg / goal * 100) : 0;
    const limit = MICRONUTRIENT_LIMITS[name] || 0;
    const unit = VITAMIN_UNITS[name];
    
    let stateLabel = '';
    let stateColor = 'var(--muted)';
    let barColor = VITAMIN_COLORS[i % VITAMIN_COLORS.length];

    if (limit > 0 && avg > limit) {
      stateLabel = `⚠️ OVER TOXICITY LIMIT (+${Math.round((avg-limit)/limit*100)}%)`;
      stateColor = 'var(--danger)';
      barColor = 'var(--danger)';
    } else if (pct >= 100) {
      const surplus = pct - 100;
      stateLabel = surplus > 0 ? `Optimal (+${Math.round(surplus)}% surplus)` : '100% Met';
      stateColor = 'var(--sage-dark)';
      barColor = 'var(--sage)';
    } else {
      const deficit = 100 - pct;
      stateLabel = `Deficit (-${Math.round(deficit)}%)`;
      stateColor = 'var(--warn)';
      barColor = 'var(--pebble)';
    }

    const row = document.createElement('div');
    row.style.cssText = 'border-bottom: 1px solid var(--stone); padding-bottom: 8px;';
    row.innerHTML = `
      <div style="display:flex; justify-content:space-between; margin-bottom:4px; font-size:12px;">
        <span style="font-weight:600; color:var(--ink-soft)">${MICRONUTRIENT_LABELS[name] || name}</span>
        <span style="color:${stateColor}; font-weight:700;">${stateLabel}</span>
      </div>
      <div style="height:6px; background:var(--stone); border-radius:3px; overflow:hidden; margin-bottom:2px;">
        <div style="height:100%; width:${Math.min(pct, 100)}%; background:${barColor}; border-radius:3px; transition:width 0.5s ease"></div>
      </div>
      <div style="display:flex; justify-content:space-between; font-size:10px; color:var(--muted)">
        <span>Avg intake: ${avg.toFixed(1)}${unit} / target: ${goal}${unit}</span>
        <span>${limit > 0 ? 'UL Max: ' + limit + unit : 'No Upper Limit'}</span>
      </div>`;
    container.appendChild(row);
  });
}

document.querySelectorAll('.tab').forEach(tab => {
  tab.onclick = () => {
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    tab.classList.add('active');
    const name = tab.dataset.tab;
    document.getElementById('page-' + name).classList.add('active');
    document.querySelector(`[data-nav="${name}"]`)?.classList.add('active');
    if (name === 'library') renderLibrary();
    if (name === 'analytics') setTimeout(renderAnalytics, 50);
  };
});

document.querySelectorAll('.nav-item').forEach(nav => {
  nav.onclick = () => {
    document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
    document.querySelectorAll('.page').forEach(p => p.classList.remove('active'));
    nav.classList.add('active');
    const name = nav.dataset.nav;
    document.getElementById('page-' + name).classList.add('active');
    document.querySelector(`[data-tab="${name}"]`)?.classList.add('active');
    if (name === 'library') renderLibrary();
    if (name === 'analytics') setTimeout(renderAnalytics, 50);
  };
});

document.querySelectorAll('.modal-overlay').forEach(overlay => {
  overlay.addEventListener('click', e => { if (e.target === overlay) { overlay.classList.remove('open'); document.body.style.overflow = ''; } });
});

function showToast(msg) {
  const t = document.getElementById('toast');
  if(!t) return;
  t.textContent = msg; t.classList.add('show');
  setTimeout(() => t.classList.remove('show'), 2200);
}

buildCalendar();
renderDashboard();
</script>
</body>
</html>
