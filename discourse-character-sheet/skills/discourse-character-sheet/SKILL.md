---
name: discourse-character-sheet
description: This skill should be used when the user asks to "generate a character sheet", "build a character", "RPG character", "character page", or wants to turn Discourse forum activity into an RPG-style character with stats, abilities, and an interactive HTML page.
version: 20260514
argument-hint: <username> <site-url> [timeframe]
---

# Character Sheet Skill

Generate an RPG character sheet from Discourse forum activity. Analyzes a user's
posts to derive stats, abilities, debuffs, inventory, quests, and party dynamics,
then outputs an interactive HTML character page.

**Target**: Build character for user `$0` on `$1`
**Timeframe**: `$2` (if not specified, use last 6 months)

## Instructions

Follow these phases IN ORDER. Each phase must complete before the next begins.

---

### Phase 1: Data Gathering

Use the discourse MCP tools to collect data. IMPORTANT: `has_more: false` can be
reported prematurely — keep requesting pages until you get an empty array.

1. Call `discourse_select_site` with site URL: `$1`
2. Call `discourse_get_user` to get the profile for: `$0`
3. Call `discourse_list_user_posts` to get their posts, filtering to the specified
   timeframe. Gather ALL pages — do not stop at `has_more: false`.
4. Call `discourse_read_post` on 10-15 substantive posts (longer content,
   interesting topics, strategic discussions, disagreements)

Summarize what you gathered: total post count, date range, main categories of
activity, and notable topics before proceeding.

---

### Phase 2: Analysis

Analyze the gathered data through these lenses. Take notes with specific post
examples for each:

- **Output patterns**: Volume, consistency, what categories dominate
- **Signature moves**: Recurring rhetorical or strategic patterns (reframing,
  building prototypes, connecting threads, storytelling, etc.)
- **Strongest contributions**: 5-7 posts that had the most impact or engagement
- **Gaps and limitations**: What's missing, what gets started but not finished,
  where do they defer
- **Collaboration style**: How they engage with others — build on, disagree,
  defer, lead
- **Evolution**: How their activity changed over the period
- **The deeper question**: What is this person trying to do beyond the surface tasks

Present a brief summary of your analysis to the user before proceeding.

---

### Phase 3: Collaborative Trait Mapping

This phase is INTERACTIVE. Do not skip ahead. Present your proposals and let the
user choose.

#### 3a. Character Identity

Propose a **Name**, **Family**, **Trade**, and **Temper** based on the analysis.

The class system has three parts:
- **Family**: Where they come from / how they're positioned (e.g., Borderlander,
  Guildmaster, Wayfinder, Sentinel, Chronicler)
- **Trade**: What they build / their craft (e.g., Artificer, Architect, Forgehand,
  Scribe, Cartographer)
- **Temper**: How they influence others (e.g., Field Bard, Truthsayer,
  Tactician, Hearthkeeper, Signalfire)

Each should have a one-line description explaining why it fits. Offer 2-3 options
for each and let the user pick or remix.

Also propose:
- **Level** (1-20, based on tenure and impact)
- **Alignment** (D&D style but with a workplace twist, e.g., "Pragmatic Good",
  "Chaotic Builder", "Lawful Questioner")

#### 3b. Stats

Propose six stats on a 1-20 scale with a one-line justification for each:

- **STR**: Raw output / delivery volume
- **DEX**: Versatility / ability to context-switch
- **CON**: Endurance / sustained load over time
- **INT**: Systems thinking / pattern recognition
- **WIS**: Judgment / knowing when and where to act
- **CHA**: Influence / ability to shift others' thinking

Present all six with your reasoning. The user may adjust.

#### 3c. Abilities

Propose abilities based on the signature patterns found in Phase 2.

Each ability needs:
- **Name**: Evocative, 1-2 words
- **Type**: Passive or Active
- **For active**: Range (Melee/Ranged/Ritual/Utility/Bardic), Cooldown (None/Medium/Long/Days), Cost
- **Description**: What it does, when it works, when it fails
- **Example quote**: A real quote from their posts that demonstrates the ability

Aim for 1 passive + 3-5 active abilities. Present them and let the user adjust.

#### 3d. Debuffs

Propose 2-4 debuffs based on the gaps found in Phase 2.

Each debuff needs:
- **Name**: Evocative
- **Type**: persistent (structural, can't self-remove), removable (habitual, can
  be worked on), or environmental (caused by context, not character)
- **Effect**: Mechanical description of the penalty
- **Description**: What causes it and how it manifests

#### 3e. Inventory, Quests, Party

Propose:
- **Equipped items** (3): Major tools/assets with tags and descriptions
- **Pack items** (4-6): Smaller artifacts, documents, prototypes
- **Active quest** (1): The big thing they're working toward, with progress steps
- **Side quests** (2-4): Parallel efforts
- **Completed quests** (2-3): Things they've shipped
- **Allies** (4-6): People they collaborate with and how
- **Relationship style**: One sentence
- **Party role**: One sentence

Present all of these together. The user may adjust.

#### 3f. Origin Story

Write a 3-4 sentence origin story in third person, past tense, slightly mythic
tone. It should cover: arrival, early work, the moment of noticing something
deeper, and current state.

---

### Phase 4: Generate Output

Once the user approves the character (or says to proceed), generate two files:

#### 4a. Markdown Character Sheet

Save to the current working directory as `{username}-character-sheet.md`.

Use this structure:

```markdown
# Character Sheet

**Name:** [Character Name]
**Family:** [Family] — [description]
**Trade:** [Trade] — [description]
**Temper:** [Temper] — [description]

**Level:** [N]
**Alignment:** [Alignment]

---

## STATS (1–20 scale)

[For each stat: abbreviation, value, bar visualization, description]

---

## PASSIVE ABILITY

**[Name]** *(always active)*
[Description]

---

## ACTIVE ABILITIES

### [Ability Name]
*[Type] / Cooldown: [X] / Cost: [Y]*
[Description]

---

## DEBUFFS

### [Debuff Name] *([type])*
**Effect:** [Description]

---

## INVENTORY

### Equipped
- **[Item]** *([tags])*
  [Description]

### In Pack
- [Item] *([tags])*

---

## QUEST LOG

### Active Quest: [Name]
[Description]
- [x] [Completed step]
- [ ] [Pending step]

### Side Quests
- **[Quest]:** [Description]

### Completed Quests
- [Quest]: [Description]

---

## PARTY DYNAMICS

**Natural allies:** [Names and roles]
**Relationship style:** [Description]
**Role in party:** [Description]

---

## ORIGIN STORY

*[Origin text]*
```

#### 4b. Interactive HTML Character Page

Save to the current working directory as `{username}-character-sheet.html`.

Generate a complete, self-contained HTML file using the template below. Replace
ALL placeholder values with the character data from Phase 3. The HTML should work
when opened directly in a browser with no build step.

Portrait: The HTML references `{username}-character-portrait.png` in the same
directory. Tell the user they can generate pixel art for it separately (Might and
Magic III style works well) and save it with that filename.

CRITICAL: Use the EXACT HTML template structure below. Fill in all placeholders
marked with [BRACKETS]. Do not change the CSS, class names, or structure.

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>[CHARACTER NAME]</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Cinzel:wght@400;600&family=Crimson+Text:ital,wght@0,400;0,600;1,400&display=swap');
  @import url('https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css');

  * { margin: 0; padding: 0; box-sizing: border-box; }

  body {
    background: #1a1a2e;
    color: #e0d5c1;
    font-family: 'Crimson Text', Georgia, serif;
    font-size: 17px;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    padding: 20px;
  }

  .page {
    max-width: 920px;
    width: 100%;
  }

  .header {
    display: flex;
    gap: 28px;
    padding: 28px;
    background: #16213e;
    border: 2px solid #534320;
    margin-bottom: 2px;
  }

  .portrait {
    width: 240px;
    height: 240px;
    flex-shrink: 0;
    overflow: hidden;
    border: 4px solid #534320;
    outline: 2px solid #1a1a2e;
    box-shadow: 0 0 0 3px #2a2015;
  }

  .portrait img {
    width: 116%;
    height: 116%;
    margin: -8% 0 0 -8%;
    display: block;
    image-rendering: pixelated;
  }

  .identity {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  .name {
    font-family: 'Cinzel', serif;
    font-weight: 600;
    font-size: 26px;
    color: #d4a853;
    margin-bottom: 14px;
    letter-spacing: 1px;
  }

  .tags {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    margin-bottom: 14px;
  }

  .tag {
    background: #534320;
    color: #e0d5c1;
    padding: 5px 12px;
    font-family: 'Crimson Text', serif;
    font-size: 16px;
    border: 1px solid #6b5a30;
  }

  .tag-label {
    color: #8a7a5a;
    margin-right: 4px;
    font-style: italic;
  }

  .level-alignment {
    font-size: 16px;
    color: #8a7a5a;
    font-style: italic;
    letter-spacing: 0.5px;
  }

  .tabs {
    display: flex;
    background: #0f1a30;
    border-left: 2px solid #534320;
    border-right: 2px solid #534320;
  }

  .tab {
    flex: 1;
    padding: 11px 8px;
    text-align: center;
    font-family: 'Cinzel', serif;
    font-weight: 600;
    font-size: 12px;
    letter-spacing: 1.5px;
    color: #6b5a30;
    background: #0f1a30;
    border: none;
    border-bottom: 2px solid #534320;
    cursor: pointer;
    transition: all 0.15s;
  }

  .tab:hover {
    color: #d4a853;
    background: #16213e;
  }

  .tab.active {
    color: #d4a853;
    background: #16213e;
    border-bottom: 2px solid #d4a853;
  }

  .panel {
    display: none;
    background: #16213e;
    border: 2px solid #534320;
    border-top: none;
    padding: 28px;
    min-height: 400px;
  }

  .panel.active { display: block; }

  .panel h2 {
    font-family: 'Cinzel', serif;
    font-weight: 600;
    font-size: 14px;
    letter-spacing: 2px;
    color: #d4a853;
    margin: 28px 0 14px 0;
    padding-bottom: 8px;
    border-bottom: 1px solid #2a2a4a;
    text-transform: uppercase;
  }

  .panel h2:first-child { margin-top: 0; }

  .panel p, .panel li {
    line-height: 1.7;
    margin-bottom: 8px;
  }

  .panel ul {
    list-style: none;
    padding-left: 0;
  }

  .panel ul li::before {
    content: '\f0da  ';
    font-family: 'Font Awesome 6 Free';
    font-weight: 900;
    color: #d4a853;
    font-size: 12px;
  }

  .overview-columns {
    display: flex;
    gap: 32px;
  }

  .overview-left { flex: 1; }
  .overview-right { flex: 1; }

  .origin-intro {
    background: #0f1a30;
    border: 1px solid #2a2a4a;
    padding: 20px;
    line-height: 1.8;
    color: #b0a890;
    font-style: italic;
    font-size: 16px;
    margin-bottom: 16px;
  }

  .class-detail {
    background: #0f1a30;
    border: 1px solid #2a2a4a;
    padding: 14px 18px;
    margin-bottom: 10px;
  }

  .class-detail-label {
    font-family: 'Cinzel', serif;
    font-weight: 600;
    font-size: 13px;
    color: #d4a853;
    letter-spacing: 1px;
    margin-bottom: 4px;
  }

  .class-detail-name {
    color: #e0d5c1;
    font-size: 18px;
    font-weight: 600;
  }

  .class-detail-desc {
    color: #8a7a5a;
    font-size: 16px;
    font-style: italic;
    margin-top: 4px;
    line-height: 1.5;
  }

  .stat-row {
    display: flex;
    align-items: center;
    margin-bottom: 6px;
    gap: 10px;
  }

  .stat-name {
    width: 36px;
    font-family: 'Cinzel', serif;
    font-weight: 600;
    font-size: 12px;
    color: #d4a853;
    letter-spacing: 1px;
  }

  .stat-value {
    width: 24px;
    text-align: center;
    font-family: 'Cinzel', serif;
    font-weight: 600;
    font-size: 14px;
    color: #e0d5c1;
  }

  .stat-bar-bg {
    flex: 1;
    height: 14px;
    background: #0f1a30;
    border: 1px solid #2a2a4a;
  }

  .stat-bar {
    height: 100%;
    transition: width 0.5s;
  }

  .stat-desc {
    font-size: 15px;
    color: #c8a960;
    padding-left: 70px;
    margin-top: -2px;
    margin-bottom: 10px;
    font-style: italic;
  }

  .stat-row-wrap {
    display: flex;
    flex-wrap: wrap;
    align-items: center;
    margin-bottom: 2px;
  }

  .str .stat-bar { background: #a84040; }
  .dex .stat-bar { background: #40a848; }
  .con .stat-bar { background: #c89040; }
  .int .stat-bar { background: #4068c8; }
  .wis .stat-bar { background: #8848b8; }
  .cha .stat-bar { background: #c85888; }

  .ability {
    background: #0f1a30;
    border: 1px solid #2a2a4a;
    padding: 18px;
    margin-bottom: 12px;
  }

  .ability-header {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: 8px;
  }

  .ability-name {
    font-family: 'Cinzel', serif;
    font-weight: 600;
    font-size: 14px;
    color: #d4a853;
    letter-spacing: 0.5px;
  }

  .ability-meta {
    font-size: 16px;
    color: #c8a960;
    font-style: italic;
    letter-spacing: 0.3px;
  }

  .ability-desc { line-height: 1.7; }

  .passive-badge {
    display: inline-block;
    background: #2a4a2a;
    color: #60c060;
    font-family: 'Cinzel', serif;
    font-size: 12px;
    letter-spacing: 1px;
    padding: 2px 8px;
    margin-left: 10px;
    border: 1px solid #3a6a3a;
    text-transform: uppercase;
  }

  .debuff {
    background: #2a1515;
    border: 1px solid #5a2020;
    padding: 18px;
    margin-bottom: 12px;
  }

  .debuff .ability-name { color: #c85050; }

  .debuff-type {
    display: inline-block;
    font-family: 'Cinzel', serif;
    font-size: 12px;
    letter-spacing: 1px;
    padding: 2px 8px;
    margin-left: 10px;
    border: 1px solid;
    text-transform: uppercase;
  }

  .debuff-persistent {
    background: #3a1515;
    color: #c87070;
    border-color: #5a2020;
  }

  .debuff-removable {
    background: #2a2a15;
    color: #c8c070;
    border-color: #5a5a20;
  }

  .debuff-environmental {
    background: #15202a;
    color: #7090c8;
    border-color: #204060;
  }

  .item {
    background: #0f1a30;
    border: 1px solid #2a2a4a;
    padding: 14px 18px;
    margin-bottom: 8px;
    display: flex;
    gap: 14px;
  }

  .item-icon {
    font-size: 18px;
    flex-shrink: 0;
    width: 30px;
    text-align: center;
    color: #d4a853;
  }

  .item-name {
    color: #d4a853;
    font-weight: 600;
  }

  .item-tag {
    font-size: 16px;
    color: #c8a960;
    font-style: italic;
    letter-spacing: 0.3px;
  }

  .item-desc {
    font-size: 15px;
    color: #b0a890;
    margin-top: 4px;
    line-height: 1.6;
  }

  .quest {
    background: #0f1a30;
    border: 1px solid #2a2a4a;
    padding: 18px;
    margin-bottom: 12px;
  }

  .quest-title {
    font-family: 'Cinzel', serif;
    font-weight: 600;
    font-size: 14px;
    color: #d4a853;
    margin-bottom: 8px;
  }

  .quest-desc {
    font-size: 15px;
    color: #c8b890;
    margin-bottom: 12px;
    line-height: 1.6;
  }

  .quest-step { padding: 4px 0; }
  .quest-step.done { color: #6b5a30; text-decoration: line-through; }
  .quest-step.done::before { content: '[x] '; color: #40a848; font-family: monospace; }
  .quest-step.pending::before { content: '[ ] '; color: #6b5a30; font-family: monospace; }

  .quest-side { border-color: #2a2a4a; }
  .quest-complete {
    border-color: #2a4a2a;
    background: #0f1a20;
  }
  .quest-complete .quest-title { color: #60a060; }

  .quest-label {
    font-family: 'Cinzel', serif;
    font-size: 12px;
    letter-spacing: 1px;
    padding: 2px 8px;
    margin-left: 10px;
    border: 1px solid;
    text-transform: uppercase;
  }

  .label-active { background: #2a2a15; color: #c8c070; border-color: #5a5a20; }
  .label-side { background: #15202a; color: #7090c8; border-color: #204060; }
  .label-complete { background: #152a15; color: #60a060; border-color: #205a20; }

  .ally {
    display: flex;
    gap: 14px;
    padding: 10px 0;
    border-bottom: 1px solid #1a1a3e;
  }

  .ally:last-child { border-bottom: none; }

  .ally-name {
    color: #d4a853;
    font-weight: 600;
    width: 100px;
    flex-shrink: 0;
  }

  .ally-role {
    color: #b0a890;
    line-height: 1.5;
  }

  .page-footer {
    max-width: 920px;
    width: 100%;
    margin: 16px auto 0;
    text-align: center;
    font-size: 14px;
    color: #4a4a5a;
    letter-spacing: 0.3px;
  }

  .quote {
    border-left: 3px solid #534320;
    padding: 10px 18px;
    margin: 10px 0;
    color: #b0a890;
    font-style: italic;
    background: #0f1520;
    line-height: 1.7;
  }
</style>
</head>
<body>
<div class="page">

  <!-- HEADER -->
  <div class="header">
    <div class="portrait">
      <img src="[USERNAME]-character-portrait.png" alt="[CHARACTER NAME]">
    </div>
    <div class="identity">
      <div class="name">[CHARACTER NAME]</div>
      <div class="tags">
        <span class="tag"><span class="tag-label">Family:</span> [FAMILY NAME]</span>
        <span class="tag"><span class="tag-label">Trade:</span> [TRADE NAME]</span>
        <span class="tag"><span class="tag-label">Temper:</span> [TEMPER NAME]</span>
      </div>
      <div class="level-alignment">Level [N] &middot; [ALIGNMENT]</div>
    </div>
  </div>

  <!-- TABS -->
  <div class="tabs">
    <button class="tab active" onclick="showTab('overview')">OVERVIEW</button>
    <button class="tab" onclick="showTab('abilities')">ABILITIES</button>
    <button class="tab" onclick="showTab('debuffs')">DEBUFFS</button>
    <button class="tab" onclick="showTab('inventory')">INVENTORY</button>
    <button class="tab" onclick="showTab('quests')">QUESTS</button>
    <button class="tab" onclick="showTab('party')">PARTY</button>
  </div>

  <!-- OVERVIEW PANEL -->
  <div class="panel active" id="panel-overview">
    <div class="overview-columns">
      <div class="overview-left">
        <h2>Class</h2>

        <div class="class-detail">
          <div class="class-detail-label">Family</div>
          <div class="class-detail-name">[FAMILY NAME]</div>
          <div class="class-detail-desc">[FAMILY DESCRIPTION]</div>
        </div>

        <div class="class-detail">
          <div class="class-detail-label">Trade</div>
          <div class="class-detail-name">[TRADE NAME]</div>
          <div class="class-detail-desc">[TRADE DESCRIPTION]</div>
        </div>

        <div class="class-detail">
          <div class="class-detail-label">Temper</div>
          <div class="class-detail-name">[TEMPER NAME]</div>
          <div class="class-detail-desc">[TEMPER DESCRIPTION]</div>
        </div>
      </div>

      <div class="overview-right">
        <h2>Stats</h2>

        <!-- Repeat this block for each stat: STR, DEX, CON, INT, WIS, CHA -->
        <!-- stat-bar width = (value / 20) * 100 percent -->
        <!-- Use class str/dex/con/int/wis/cha on stat-row-wrap for bar color -->
        <div class="stat-row-wrap str">
          <div class="stat-row" style="width:100%">
            <span class="stat-name">STR</span>
            <span class="stat-value">[N]</span>
            <div class="stat-bar-bg"><div class="stat-bar" style="width:[N*5]%"></div></div>
          </div>
          <div class="stat-desc">[SHORT STAT DESCRIPTION]</div>
        </div>
        <!-- ... repeat for DEX, CON, INT, WIS, CHA ... -->

      </div>
    </div>

    <h2>Story</h2>
    <div class="origin-intro">
      [ORIGIN STORY TEXT]
    </div>
  </div>

  <!-- ABILITIES PANEL -->
  <div class="panel" id="panel-abilities">

    <!-- Passive ability -->
    <div class="ability">
      <div class="ability-header">
        <span class="ability-name">[PASSIVE NAME] <span class="passive-badge">passive</span></span>
      </div>
      <div class="ability-desc">[PASSIVE DESCRIPTION]</div>
    </div>

    <h2>Active Abilities</h2>

    <!-- Repeat for each active ability -->
    <div class="ability">
      <div class="ability-header">
        <span class="ability-name">[ABILITY NAME]</span>
        <span class="ability-meta">[Type] &middot; [Cooldown] &middot; Cost: [Cost]</span>
      </div>
      <div class="ability-desc">[ABILITY DESCRIPTION]</div>
      <!-- Include a quote block if there's a representative quote -->
      <div class="quote">"[EXAMPLE QUOTE FROM POSTS]"</div>
    </div>

  </div>

  <!-- DEBUFFS PANEL -->
  <div class="panel" id="panel-debuffs">
    <h2>Active Debuffs</h2>

    <!-- Repeat for each debuff. Use debuff-persistent, debuff-removable, or debuff-environmental -->
    <div class="debuff">
      <div class="ability-header">
        <span class="ability-name">[DEBUFF NAME]</span>
        <span class="debuff-type debuff-[TYPE]">[TYPE]</span>
      </div>
      <div class="ability-desc">[DEBUFF DESCRIPTION WITH MECHANICAL EFFECT]</div>
    </div>

  </div>

  <!-- INVENTORY PANEL -->
  <div class="panel" id="panel-inventory">
    <h2>Equipped</h2>

    <!-- Repeat for each equipped item. Pick a fitting Font Awesome icon. -->
    <div class="item">
      <div class="item-icon"><i class="fa-solid fa-[ICON]"></i></div>
      <div>
        <div class="item-name">[ITEM NAME]</div>
        <div class="item-tag">[tags separated by middots]</div>
        <div class="item-desc">[ITEM DESCRIPTION]</div>
      </div>
    </div>

    <h2>In Pack</h2>

    <!-- Repeat for each pack item -->
    <div class="item">
      <div class="item-icon"><i class="fa-solid fa-[ICON]"></i></div>
      <div>
        <div class="item-name">[ITEM NAME]</div>
        <div class="item-tag">[tags]</div>
      </div>
    </div>

  </div>

  <!-- QUESTS PANEL -->
  <div class="panel" id="panel-quests">
    <h2>Active Quest</h2>

    <div class="quest">
      <div class="quest-title">[QUEST NAME] <span class="quest-label label-active">active</span></div>
      <div class="quest-desc">[QUEST DESCRIPTION]</div>
      <!-- Use quest-step done or quest-step pending -->
      <div class="quest-step done">[COMPLETED STEP]</div>
      <div class="quest-step pending">[PENDING STEP]</div>
    </div>

    <h2>Side Quests</h2>

    <div class="quest quest-side">
      <div class="quest-title">[SIDE QUEST NAME] <span class="quest-label label-side">side</span></div>
      <div class="quest-desc">[SIDE QUEST DESCRIPTION]</div>
    </div>

    <h2>Completed</h2>

    <div class="quest quest-complete">
      <div class="quest-title">[QUEST NAME] <span class="quest-label label-complete">complete</span></div>
      <div class="quest-desc">[QUEST DESCRIPTION]</div>
    </div>

  </div>

  <!-- PARTY PANEL -->
  <div class="panel" id="panel-party">
    <h2>Allies</h2>

    <!-- Repeat for each ally -->
    <div class="ally">
      <span class="ally-name">[NAME]</span>
      <span class="ally-role">[ROLE AND HOW THEY COLLABORATE]</span>
    </div>

    <h2>Relationship Style</h2>
    <p>[RELATIONSHIP STYLE DESCRIPTION]</p>

    <h2>Role in Party</h2>
    <p>[PARTY ROLE DESCRIPTION]</p>
  </div>

  <div class="page-footer">
    ~[N] posts &middot; [SITE URL] &middot; [DATE RANGE] &middot; Generated [DATE]
  </div>

</div>

<script>
function showTab(name) {
  document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
  document.querySelectorAll('.panel').forEach(p => p.classList.remove('active'));
  document.getElementById('panel-' + name).classList.add('active');
  event.target.classList.add('active');
}
</script>
</body>
</html>
```

### Font Awesome Icon Suggestions

Pick contextually appropriate icons for inventory items. Good RPG-fitting options:
- fa-briefcase, fa-hammer, fa-map, fa-scroll, fa-gem, fa-crown, fa-flask,
  fa-compass-drafting, fa-cubes, fa-shield, fa-wand-sparkles, fa-book,
  fa-gavel, fa-wrench, fa-fire, fa-feather, fa-chess-rook, fa-landmark

---

### Phase 5: Wrap Up

After generating both files, tell the user:

1. The two files that were created and where they are
2. To generate a portrait, they can ask for "Might and Magic III style pixel art
   portrait" describing the character, and save it as `{username}-character-portrait.png`
   in the same directory
3. Open the HTML file in a browser to see the interactive character page

Also note: the character sheet can be regenerated or updated as the person's
activity evolves — stats shift, new abilities emerge, debuffs get removed, quests
complete.
