# Story Node Map

**Design branching stories. Test every path. Find every ending.**

Story Node Map is a browser-based tool for building and analyzing event-driven stories. Define events, connect them through logical rules, and explore how different choices affect the outcome.

Instead of manually tracing every branch in a flowchart, Story Node Map evaluates every possible combination of choices and reports the endings, routes, conflicts, and dead ends it discovers.

**[🚀 Open Story Node Map](https://siddharthbhaumik.github.io/CYOA-Story-map-tool/)**

> Replace the link above with your deployed GitHub Pages URL.

---

## Table of Contents

- [What Can You Do With It?](#what-can-you-do-with-it)
- [Getting Started](#getting-started)
- [Understanding the Building Blocks](#understanding-the-building-blocks)
- [Tutorial: Build Your First Branching Story](#tutorial-build-your-first-branching-story)
- [Playing Through Your Story](#playing-through-your-story)
- [Understanding the Results](#understanding-the-results)
- [How the Logic Works](#how-the-logic-works)
- [Saving and Loading Your Work](#saving-and-loading-your-work)
- [Tips for Designing Better Stories](#tips-for-designing-better-stories)
- [Limitations](#limitations)
- [Technology](#technology)

---

## What Can You Do With It?

Story Node Map is useful for writers, game designers, interactive fiction creators, and anyone working with systems that have multiple possible outcomes.

- **Design branching narratives:** Represent story events and their dependencies without drawing a huge flowchart by hand.
- **Test choices:** Toggle starting events and optional choices to see what happens.
- **Discover endings:** Find which endings are reachable and which are impossible.
- **Trace story routes:** Understand which rules contributed to an ending.
- **Detect logical conflicts:** Identify situations where competing rules cannot be resolved.
- **Find dead ends:** Discover combinations of choices that fail to reach an ending.
- **Analyze every combination:** Examine the full choice space rather than relying on a few manually tested playthroughs.
- **Save and reuse frameworks:** Keep your work in the browser or export it as JSON text.

The tool can also model event-driven systems beyond fiction, provided they can be expressed using the same sequential, rule-based structure.

---

## Getting Started

You do not need to install anything.

1. Open the [Story Node Map website](YOUR_GITHUB_PAGES_URL_HERE).
2. Click **Load sample story** to explore a working example.
3. Examine the **Build** section to see how events and rules are defined.
4. Scroll to the **Play Board** and click starting events to change the scenario.
5. Open the **Results** section to explore all possible outcomes.
6. When you are ready, click **Start empty** to create your own framework.

The application runs directly in your browser.

---

## Understanding the Building Blocks

Before creating a story, it helps to understand five basic concepts.

### 1. Sections

Sections represent successive stages of a story.

For example:

| Section | Purpose | Example |
|---|---|---|
| Section 1 | Starting conditions | You have a key |
| Section 2 | Intermediate events | You enter the cellar |
| Section 3 | Later events | You retrieve the treasure |
| Section 4 | Endings | You escape with the treasure |

You can add more sections using **Add time section**.

The tool processes sections in order. A node in one section can use events from that section to produce an event in the next section.

### 2. Events

An event represents a condition, possession, action, or occurrence in your story.

Examples:

- Cellar key
- Secret map
- Guard alerted
- Treasure acquired
- Gate unlocked
- Escaped successfully

Give each event a descriptive name so that the resulting map remains readable.

Events fall into three categories:

- **Starting events:** Events in Section 1. These are the initial switches you can turn on or off.
- **Derived events:** Events produced by nodes in earlier sections.
- **Isolated choices:** Optional events in intermediate sections that are switched on or off directly rather than produced by a node.

The final section contains your endings.

**Important:** An event does not automatically happen merely because you have created it. Intermediate events normally need a rule that produces them, unless you explicitly mark them as isolated choices.

### 3. Nodes

A node represents a decision point or a logical rule-processing step.

For example, a node called `Cellar Door` might contain these rules:

- If you have the key, you enter the cellar.
- If you have both the key and the map, you discover the treasure room.
- If you have both the key and the map, you also discover a hidden passage only when that rule is not superseded by a stronger rule at the same node.

Each node contains one or more formulas.

Use **Add node** to create a node in a section.

### 4. Formulas

A formula defines which events are required and which event is produced.

Every formula has two parts:

- **Needs:** The events that must all be active.
- **Gives:** The event that becomes active when the formula wins.

For example:

`Cellar Key + Lantern → Cellar Explored`

This means that both the key and the lantern must be active for the rule to match.

A formula with multiple required events uses **AND logic**. Having only one of the required events is not sufficient.

To configure a formula:

1. Create a node.
2. Click the event names under **Needs** to select the required events.
3. Choose the output event under **Gives**.
4. Add additional formulas if the node needs more rules.

A complete formula must have at least one required event and an output event.

### 5. Endings

Endings are the events in the final section.

Examples:

- Rich and free
- Rich but hunted
- Free, unnoticed
- Free, empty-handed

An ending is reached when its event becomes active through the chain of rules.

A single combination of choices can reach multiple endings. If no ending becomes active, that combination is a dead end.

---

## Tutorial: Build Your First Branching Story

Let's model a simple adventure in which a character tries to escape a locked building.

### Step 1: Create the sections

Start with the default two sections, then click **Add time section** twice to create four sections.

Your structure will look like this:

| Section | Role |
|---|---|
| 1 | Starting events |
| 2 | Exploration |
| 3 | Escape |
| 4 | Endings |

### Step 2: Add starting events

In Section 1, create these events:

- `Key`
- `Lantern`

These are the starting conditions. On the Play Board, starting events are active by default. You can switch them off when testing scenarios.

### Step 3: Add intermediate events

In Section 2, create:

- `Cellar Explored`
- `Treasure Found`

In Section 3, create:

- `Escape Successful`
- `Escape Failed`

These events will be generated by rules in the preceding sections.

### Step 4: Add endings

In Section 4, create:

- `Escaped with Treasure`
- `Escaped Empty-Handed`

### Step 5: Create the first node

In Section 1, click **Add node** and name it `Explore Cellar`.

Create two formulas:

| Needs | Gives |
|---|---|
| Key | Cellar Explored |
| Key + Lantern | Treasure Found |

The second formula requires both events. Because it contains all the requirements of the first formula plus an additional one, it takes precedence when both formulas match.

Consequently:

- With the key but no lantern, the character explores the cellar.
- With both the key and lantern, the character finds the treasure.

### Step 6: Create the escape rules

In Section 2, add a node called `Escape Route`.

Create these formulas:

| Needs | Gives |
|---|---|
| Cellar Explored | Escape Successful |
| Treasure Found | Escape Failed |

This is a deliberately simple example of a rule system, not necessarily a sensible narrative. The important point is that events in Section 2 produce events in Section 3.

### Step 7: Connect the endings

In Section 3, add a node called `Final Outcome`.

Create these formulas:

| Needs | Gives |
|---|---|
| Escape Successful | Escaped Empty-Handed |
| Escape Failed | Escaped with Treasure |

The example intentionally illustrates how rules determine outcomes, rather than prescribing a particular story.

### Step 8: Test the story

Scroll down to the Play Board.

- Turn `Key` on and `Lantern` off.
- Observe the active rules and resulting events.
- Turn `Lantern` on.
- Watch the node outputs and ending change.

The map updates as you change the switches.

**Experiment:** Change the rules to make the narrative more sensible. For example, make finding the treasure lead to a successful escape with the treasure. Then use the Results section to check whether every intended ending is reachable.

---

## Playing Through Your Story

The Play Board provides a visual representation of the framework.

### Reading the board

The board displays events, nodes, and endings in columns connected by lines.

- **Starting events and isolated choices:** Click these to turn them on or off.
- **Active connections:** Highlighted connections show the currently active flow.
- **Node status:** A node can fire a rule, remain silent, or report a conflict.
- **Formula status:** The node shows which rules match, which rule fires, and which matching rules are outranked.
- **Current ending:** The result at the top of the board displays the ending or endings reached in the current playthrough.

### Board controls

- **Reset:** Restore the default switch configuration. Starting events are on and isolated choices are off.
- **Switch all off:** Turn off every starting event and isolated choice.
- **Horizontal scrolling:** Follow the connections across the board, particularly for larger stories.

The Play Board represents one combination of switches at a time. For the complete analysis of all combinations, use the Results section.

---

## Understanding the Results

The Results section analyzes the entire choice space of your framework.

### 1. Ending groups

Combinations are grouped according to the ending events they reach.

This helps answer questions such as:

- How many combinations lead to each ending?
- Can a character reach multiple endings simultaneously?
- Are there combinations that reach no ending?

### 2. Routes

A route describes the set of rules that contributed to an ending.

The tool traces the chain of producing rules backward from the ending to identify the relevant dependencies.

Different combinations of starting choices may produce the same ending through different routes. The analysis groups combinations with the same ending set and route.

### 3. Conflicts

A conflict occurs when multiple formulas in the same node match, but none can be selected as the unique winning rule under the precedence system.

A conflicted node remains silent for that playthrough, and the conflict is reported.

To resolve one:

1. Locate the conflicted node.
2. Examine the requirements of its matching formulas.
3. Decide which rule should take precedence, or redesign the rules so that the conflict cannot occur.
4. Run the analysis again.

### 4. Unreachable endings

An ending is unreachable if no combination of switches causes its event to become active.

Possible reasons include:

- No formula produces the ending.
- Its producing formula requires an event that cannot occur.
- Earlier rules prevent the necessary chain from completing.

Check the dependency chain from the ending backward through the preceding sections.

### 5. Formulas that never fire

The tool reports formulas that never win during the exhaustive analysis.

A formula might never fire because its requirements cannot be satisfied, or because another formula consistently outranks it.

This can reveal redundant rules, unreachable conditions, or mistakes in the framework.

### 6. Dead ends

A dead end is a combination of choices that reaches no ending.

The analysis groups dead-end combinations and shows how far their rules progressed.

Dead ends are not always undesirable. In a game, they might represent failure states or intentional consequences. The important thing is to know that they exist.

### 7. Choice patterns

The results also compress combinations into patterns that indicate which switches must be on, which must be off, and which can vary.

This helps summarize large numbers of combinations without listing every combination separately.

Selecting a pattern or route lets you inspect the corresponding configuration on the Play Board.

---

## How the Logic Works

The application evaluates the story using a deterministic rule system.

### Rule matching

A formula matches when all its required events are active.

For example:

`Key + Lantern → Treasure Found`

The formula matches only if both `Key` and `Lantern` are on.

### Rule precedence

Within a node, a formula requiring a strict superset of another formula's requirements outranks the smaller formula when both match.

For example:

- `Key → Cellar Explored`
- `Key + Lantern → Treasure Found`

When both the key and lantern are active, the second formula outranks the first because it requires all the same events plus an additional event.

If two matching formulas cannot be ranked in this way and compete at the top, the node reports a conflict and produces no output for that evaluation.

### Sequential evaluation

Nodes are processed in section order. Each node reads events from its own section and can produce an event in the next section.

This makes the framework a staged event system rather than an unrestricted graph in which rules can repeatedly feed back into earlier stages.

### Exhaustive analysis

Every starting event and every isolated choice is a binary switch.

With `n` switches, there are:

`2ⁿ possible combinations`

For example:

| Number of switches | Combinations |
|---:|---:|
| 4 | 16 |
| 8 | 256 |
| 10 | 1,024 |
| 12 | 4,096 |
| 16 | 65,536 |

The application evaluates every combination within its supported limit, rather than estimating which outcomes are likely.

---

## Saving and Loading Your Work

Story Node Map saves your framework in the browser automatically as you edit it.

### Automatic saving

Your framework is stored in browser local storage. You can normally return to the same site in the same browser and continue working.

However, browser storage is not a permanent backup. Clearing site data, switching browsers, or using a different device may mean your work is unavailable.

### Export a backup

1. Scroll to **Save and load**.
2. Find the text area containing your framework.
3. Click **Copy text** to copy the framework.
4. Save the copied text in a file with a `.json` extension, or keep it somewhere safe.

### Import a framework

1. Open the **Save and load** section.
2. Paste your saved framework text into the text area.
3. Click **Load this text**.
4. Check the Build section and Play Board to confirm the framework loaded as expected.

The exported text is JSON, a structured text format suitable for saving, sharing, and versioning.

**Recommendation:** Export a backup before making major structural changes. You can also keep different versions of the same story to experiment with alternative rules.

---

## Tips for Designing Better Stories

### Start with a small framework

Begin with two or three starting events, a few intermediate events, and two endings. Add complexity only after the rules behave as expected.

### Give events clear names

Prefer `Guard Alerted` over `Guard`, or `Treasure Acquired` over `Treasure`. A name should describe the condition that is true, not merely the object involved.

### Make every intermediate event intentional

For each intermediate event, decide whether it is produced by a node or represents an independent choice.

If it is produced by a node, ensure that at least one complete formula gives it.

If it is an independent choice, mark it as an isolated choice.

### Keep formulas readable

Each formula should express a clear causal relationship.

For example:

`Has Disguise + Guard Distracted → Passes Checkpoint`

This is easier to reason about than a formula with many unrelated requirements.

### Test edge cases

Try scenarios where:

- All starting events are off.
- Only one starting event is on.
- Several optional choices are on simultaneously.
- A crucial event is missing.
- Two competing formulas match at once.

These scenarios often expose problems that are easy to overlook in a normal playthrough.

### Treat results as a debugging tool

A useful workflow is:

1. Build a minimal framework.
2. Run the analysis.
3. Inspect unreachable endings and dead ends.
4. Resolve unintended conflicts.
5. Refine the story.
6. Export a backup before making major changes.

---

## Limitations

- **Maximum of 16 switches:** The exhaustive analyzer supports up to 16 starting and isolated switches, corresponding to 65,536 combinations. Larger frameworks can exceed the analysis limit.
- **Sequential rules:** Each node reads events from one section and produces an event in the next. The tool is designed for staged dependencies.
- **Deterministic behavior:** The same switch configuration and rules produce the same results. The application does not simulate randomness or probabilities.
- **Logical rather than narrative validation:** The tool checks event dependencies and outcomes. It cannot determine whether a story is emotionally satisfying, well written, or narratively consistent in a broader literary sense.
- **Browser-based persistence:** Automatic saving uses local storage. Keep external backups for important work.
- **No account-based synchronization:** Browser storage does not automatically synchronize your framework across devices.

---

## Technology

Story Node Map is a client-side web application built with:

- **HTML** for structure.
- **CSS** for layout, styling, responsive design, and theme support.
- **Vanilla JavaScript** for the editor, rule engine, visual board, and exhaustive analysis.
- **Browser local storage** for automatic persistence.
- **JSON** for exporting and importing frameworks.
- **GitHub Pages** for hosting the deployed application.

No installation or application account is required to use the hosted tool.

---

## Contributing and Feedback

Found a bug, have an idea for a feature, or want to improve the documentation?

You can open an issue or submit a pull request in the repository where the project is hosted.

Useful contributions include:

- Bug reports with a reproducible example.
- Suggestions for improving the editing workflow.
- Improvements to the analysis or visualization.
- Documentation fixes and clearer examples.

---

**Start with a few events, connect them with rules, and see where every possible choice leads.**
