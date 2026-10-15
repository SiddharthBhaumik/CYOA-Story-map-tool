# Story Node Map

**Design branching stories. Test every path. Find every ending.**

Story Node Map is a browser-based tool for planning and analyzing branching narratives. Model story events as switches, connect them with logical rules, play through different starting conditions, and inspect the possible outcomes.

Instead of tracing every branch manually, use the built-in analyzer to explore combinations and surface reachable endings, dead ends, conflicting rules, and formulas that never fire.

<p align="center">
  <a href="https://siddharthbhaumik.github.io/CYOA-Story-map-tool/"><strong>Open Story Node Map →</strong></a>
  &nbsp; · &nbsp;
  <a href="https://siddharthbhaumik.github.io/CYOA-Story-map-tool/tutorial.html">Follow the interactive tutorial</a>
</p>

---

## Start here

- **[Launch the editor](https://siddharthbhaumik.github.io/CYOA-Story-map-tool/)**
- **[Open the step-by-step tutorial](https://siddharthbhaumik.github.io/CYOA-Story-map-tool/tutorial.html)**
- **[Browse the source code](https://github.com/SiddharthBhaumik/CYOA-Story-map-tool)**

No installation, account, or build step is required. The app runs in your browser.

### Your first few minutes

1. Open the editor.
2. Click **Load sample story** to explore *The Midnight Archive*, a small escape adventure.
3. Look through the sections to see the events and rules that make the story work.
4. Scroll to the **Play Board** and toggle starting events or isolated choices.
5. Inspect **Results** to see what happens across all supported combinations.
6. Click **Start empty** when you are ready to build your own story.

Want a guided introduction first? The [interactive tutorial](https://siddharthbhaumik.github.io/CYOA-Story-map-tool/tutorial.html) explains the building blocks and includes its own small, playable version of the sample story.

## What you can do

- **Plan branching narratives** using ordered sections, events, nodes, and formulas.
- **Try different choices** in the Play Board and see which events and endings activate.
- **Trace routes** to understand which rules contributed to an outcome.
- **Find unreachable endings** and discover combinations that do not reach any ending.
- **Detect rule conflicts** when competing formulas cannot be resolved by the precedence rules.
- **Find formulas that never fire** during exhaustive analysis.
- **Save and share frameworks** by copying and loading their JSON representation.
- **Use the same logic for other systems** whose behavior can be represented as sequential, event-driven rules.

## How the model works

A framework is built from five main concepts.

### 1. Sections

Sections are the ordered stages of a story. The first section contains starting events, the last section contains endings, and the sections in between represent the unfolding story.

A node reads events from its own section and can produce an event in the **next** section. This staged structure keeps the logic easier to follow than a large, unrestricted web of branches.

### 2. Events

Events describe facts, conditions, choices, or outcomes.

Examples include `Cellar key`, `Secret map`, `Treasure in hand`, and `Gate unlocked`.

The app supports three event roles:

- **Starting events:** switches in Section 1 that define the initial scenario.
- **Derived events:** events produced by formulas in earlier sections.
- **Isolated choices:** optional events in intermediate sections that can be switched directly rather than produced by a node.

Creating an event does not automatically activate it. Intermediate events generally need a formula that produces them, unless they are intentionally marked as isolated choices.

### 3. Nodes

A node groups formulas that evaluate related conditions at a particular stage. For example, a `Cellar door` node might decide whether the character explores the cellar, finds a treasure room, or discovers a hidden passage.

### 4. Formulas

Each formula has two parts:

- **Needs:** events that must all be active.
- **Gives:** the event produced if that formula wins.

For example:

```text
Cellar key + Lantern → Cellar explored
Cellar key + Lantern + Secret map → Treasure room
```

All listed requirements must be active (AND logic). When multiple formulas in the same node match, a formula whose requirements are a strict superset of another matching formula's requirements outranks the less specific one. If the matching formulas cannot be ranked to identify a unique winner, the node reports a conflict and produces no output for that evaluation.

### 5. Endings

Endings are events in the final section. The sample story includes outcomes such as **Rich and free**, **Rich but hunted**, **Free, unnoticed**, and **Free, empty-handed**.

A playthrough can activate more than one ending. A combination that reaches no ending is reported as a dead end.

## Playing and analyzing

### Play Board

The Play Board displays the framework as a visual sequence of events and rules.

- Click starting events and isolated choices to change the current scenario.
- Follow the highlighted connections to see the active route.
- Check node and formula states to understand which rules matched, fired, or conflicted.
- Use **Reset** to return to the default switch configuration.
- Scroll horizontally to follow larger frameworks.

The Play Board represents one combination at a time.

### Results

The analyzer evaluates the supported combinations of starting events and isolated choices, then summarizes what it finds.

| Result | What it tells you |
|---|---|
| Ending groups | Which endings are reached by different combinations |
| Routes | Which rule chains contributed to outcomes |
| Conflicts | Where a node cannot choose a unique winning formula |
| Unreachable endings | Which endings no combination can activate |
| Formulas that never fire | Which formulas never win in the tested choice space |
| Dead ends | Which combinations reach no ending |
| Choice patterns | Which switches must be on or off, and which can vary |

These reports are useful for checking whether your story logic behaves as intended. They do not judge the quality of the writing or whether a narrative is emotionally satisfying.

### Analysis limit

The exhaustive analyzer supports up to **16 switches**, for a maximum of **65,536 combinations**. If a framework exceeds that limit, the app reports that it cannot complete the exhaustive analysis for that configuration. You can still work on the framework; reduce the number of switches to run the exhaustive check.

## Save and load

Your work is saved automatically in the current browser using local storage. That makes it convenient to return to the same framework in the same browser, but it is not a backup or cross-device sync.

To keep a portable copy:

1. Open **Save and load** in the editor.
2. Click **Copy text** to copy the framework's JSON text.
3. Save it in a file or another safe location.

To restore a framework, paste its JSON text into the same area and click **Load this text**.

**Tip:** Export a backup before making major changes. You can save multiple JSON versions to experiment with alternative story structures.

## Tips for building a clear framework

- **Start small.** Use a few starting switches, a handful of intermediate events, and two or three endings.
- **Name events as facts.** `Guard alerted` is usually clearer than `Guard`; `Treasure acquired` is clearer than `Treasure`.
- **Give every intermediate event a role.** Decide whether a node produces it or whether it should be an isolated choice.
- **Keep formulas causal.** A formula should express a relationship that makes sense in your story.
- **Test edge cases.** Try all switches off, one switch on, several optional choices on, and combinations where a crucial event is missing.
- **Use Results as a debugger.** Inspect conflicts, unreachable endings, dead ends, and formulas that never fire, then adjust the rules and test again.

## Project structure

```text
.
├── index.html          # Main Story Node Map application
├── story-node-map.html # Standalone editor page
├── tutorial.html       # Guided tutorial with a sample story
└── README.md           # Project documentation
```

The GitHub Pages site is published from this repository. The root page launches the editor, and `tutorial.html` provides the guided learning experience.

## Technology

- **HTML** — page structure
- **CSS** — layout, responsive styling, and light/dark theme support
- **Vanilla JavaScript** — framework editor, rule evaluation, Play Board, and exhaustive analysis
- **Browser local storage** — automatic saving in the current browser
- **JSON** — portable framework export and import
- **GitHub Pages** — hosting

The application does not require a server-side runtime to use the hosted editor.

## Contributing and feedback

Found a bug or have an idea for improving the editor, tutorial, or analysis? Open an issue or submit a pull request in this repository.

Helpful contributions include:

- Bug reports with steps to reproduce the issue
- Clearer examples and documentation
- Improvements to the editing or play-testing workflow
- Improvements to the rule engine, analysis, or visualization

---

**Build the rules, explore the branches, and see where every possible choice leads.**
