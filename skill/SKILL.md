---
name: algo-visualizer
description: Generate interactive HTML animation pages for LeetCode and algorithm problem solutions with step-by-step visual playback. Use when user wants to create algorithm visualization demos, wants animated step-through of their code, needs to explain array/string/tree/graph algorithms visually, or asks for code execution animation in a dark-themed webpage. Supports array-based algorithms (two pointers, sliding window, sorting, dynamic programming), linked list visualizations, and tree/graph traversals. Also use when user references an existing HTML demo and asks to create a similar one for a different algorithm.
---

# Algorithm Visualizer

Generate single-file, self-contained HTML pages that animate algorithm execution step-by-step. The output is a dark-themed interactive webpage with code highlighting, variable state tracking, and visual playback controls.

## Overview

The visualizer follows a proven dual-panel layout:
- **Left panel**: Syntax-highlighted code with active-line indication + status explanation card
- **Right panel**: Animated data structure visualization (arrays, pointers, etc.) + playback controls + progress bar

All CSS and JavaScript are inlined into a single `.html` file for easy sharing.

## Workflow

### Step 1: Understand the Algorithm

1. Read the user's code and understand the core data structures and variables
2. Identify the key loop indices, pointers, and state variables that change during execution
3. Determine the test data to use (use user's example, or generate a small illustrative case)

### Step 2: Plan the Visualization

Decide the visualization pattern based on algorithm type:

| Algorithm Type | Visualization Pattern |
|---------------|----------------------|
| Array + pointers (two pointers, sliding window) | Single or dual array rows with pointer arrows |
| In-place array mutation (sorting, Dutch flag) | Single array with zone coloring and swap animations |
| DP / grid problems | 2D grid with cell highlighting and value transitions |
| Linked list | Node chain with pointer movement |
| Tree / graph | Node tree with traversal path highlighting |

### Step 3: Generate the HTML

Write a complete single-file HTML document. Use the template in `assets/template.html` as the starting boilerplate, then customize:

1. **Replace the code block** in `#codeBlock` with the user's algorithm, maintaining syntax highlighting spans (`<span class="kw">`, `<span class="var">`, `<span class="method">`, `<span class="num">`, `<span class="type">`, `<span class="comment">`)
2. **Implement `buildSteps()`** in the `<script>` section. This function simulates the algorithm and returns an array of step objects
3. **Implement `render()`** to map each step object to DOM updates (cell colors, pointer positions, status text)
4. **Adjust CSS** if the algorithm requires additional visual elements (grids, linked list nodes, trees)

#### Step Object Schema

Each step in the `buildSteps()` return array must be an object with these fields:

```javascript
{
  line: 5,              // number: active code line (matches data-line attribute)
  msg: "描述当前状态",   // string: human-readable explanation shown in status card
  arr: [...],           // optional: current array state for rendering
  highlight: [2, 4],    // optional: array indices to highlight (yellow pulse)
  swap: [2, 4],         // optional: array indices involved in a swap animation
  done: false,          // optional: true on final step to trigger completion animation
  // plus any custom fields needed by render() (pointer positions, ans values, etc.)
}
```

#### Rendering Conventions

- **Code highlighting**: Toggle `.active` class on `.line` elements where `parseInt(line.dataset.line) === step.line`
- **Status text**: Update `#statusMsg.textContent` with `step.msg`
- **Array cells**: Rebuild the array row on each step to ensure clean state; add animation classes (`swapping`, `highlight`, `sorted-pop`) based on step fields
- **Pointers**: Use `positionPtr()` helper to move pointer divs above/below cells based on step data
- **Progress**: Update `#progressFill` width and `#stepInfo` text

### Step 4: Save and Deliver

Save the output as a single `.html` file at the path the user specifies (or ask if not specified). Confirm the file path with the user.

## HTML Structure & Design System

### Color Palette (CSS Variables)

Always use these exact CSS custom properties for consistency:

```css
:root {
  --bg: #0b0f19;         /* page background */
  --panel: #111827;      /* panel background */
  --code-bg: #0d1117;    /* code block background */
  --text: #e2e8f0;       /* primary text */
  --muted: #94a3b8;      /* secondary text */
  --accent: #10b981;     /* active line indicator, progress bar, primary buttons */
  --accent-glow: rgba(16, 185, 129, 0.35);
  --btn: #1f2937;
  --btn-hover: #374151;
}
```

### Syntax Highlighting Classes

| Class | Color | Usage |
|-------|-------|-------|
| `.kw` | `#ff7b72` | keywords (class, public, void, int, for, while, if, return) |
| `.type` | `#ffa657` | types (Solution, String, List) |
| `.var` | `#79c0ff` | variables (nums, i, j, ans) |
| `.method` | `#d2a8ff` | methods (maxDistance, sortColors) |
| `.num` | `#79c0ff` | numeric literals |
| `.comment` | `#8b949e` italic | comments |

### Pointer Styling

Pointers are absolutely-positioned flex columns above/below array cells:
- Arrow triangle using CSS borders (`border-left/right: 8px solid transparent`)
- Colored label pill with background/border at ~15% opacity
- Transition: `all 0.45s cubic-bezier(0.34, 1.56, 0.64, 1)`

### Animation Keyframes

Always include these standard animations:
- `swapPulse`: scale + rotate for swap operations
- `borderPulse`: yellow glow pulse for comparisons
- `sortedPop`: scale-in for final sorted/completed state

## Array Visualization Patterns

### Pattern A: Single Array with Pointers
For in-place sorting, partitioning, sliding window.

Use a single `.array-row` with `.array-cell` children. Pointer `.ptr` elements are absolutely positioned within `.ptr-layer` overlay.

### Pattern B: Dual Arrays (Two Pointers on Separate Arrays)
For problems like LeetCode 1855 (max distance between arrays).

Use two `.array-block` containers stacked vertically, each with its own `.array-row`. Each array gets its own pointer color.

### Pattern C: Grid (2D Array)
For DP, matrix, graph problems.

Use CSS Grid layout. Each cell is a square div. Highlight cells with border-color and box-shadow transitions.

## Build Steps Best Practices

1. **Simulate, don't execute user code directly**: Write a JavaScript simulation that mirrors the algorithm logic to generate the step sequence. Do not use `eval()` or Function constructors.
2. **One conceptual step per array element**: Each step should represent one human-comprehensible algorithm action (e.g., "compare nums1[i] with nums2[j]", "swap elements", "move pointer").
3. **State snapshots**: Every step must include the full current array state so `render()` can rebuild accurately.
4. **Edge cases**: Include steps for loop entry, condition checks that fail, and early termination (break/return).
5. **Chinese language**: Status messages should be in Chinese for consistency with the reference design.

## Keyboard Shortcuts

Always wire these keyboard events:
- `ArrowRight` → next step
- `ArrowLeft` → previous step
- `Space` → toggle play/pause

## Assets

- **`assets/template.html`**: Boilerplate HTML with the full CSS framework, layout structure, and JavaScript playback engine. Copy this and fill in the algorithm-specific code, `buildSteps()`, and `render()` logic.

## Important Notes

- The output must be a **single self-contained HTML file** (no external CSS/JS dependencies except system fonts).
- Use inline `<style>` and `<script>` tags.
- Test data arrays should be small (5-10 elements) so they fit on screen.
- Keep `SKILL.md` instructions token-efficient; rely on `assets/template.html` for the full boilerplate rather than embedding CSS/JS in SKILL.md.
