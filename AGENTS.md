# excalidraw-diagram-skill

Generate Excalidraw diagrams from text content for Obsidian. Use when user asks to create diagrams, flowcharts, mind maps, or visual representations in Excalidraw format.

## Activation Triggers

Activate when the user mentions any of:
- `Excalidraw`, `diagram`, `flowchart`, `mind map`
- `画图`, `流程图`, `思维导图`, `可视化`

## Usage

Invoke with `/excalidraw-diagram-skill` or describe a diagram task. The skill analyzes content, selects the best diagram type, generates Excalidraw JSON, and saves an Obsidian-ready `.md` file to the current directory.

## Supported Diagram Types

Flowchart, Mind Map, Hierarchy, Relationship, Comparison, Timeline, Matrix, Freeform.

## Gotchas

- Text must replace `"` with `『』` and `()` with `「」` — these characters break Excalidraw rendering
- `## Text Elements` section MUST be left empty (only `%%` delimiters) — the plugin auto-fills from JSON
- All text elements MUST use `fontFamily: 5` (Excalifont)
- JSON must be valid — malformed JSON silently breaks the diagram
- Canvas coordinates should stay within 0-1200 x 0-800 range
- Each element requires a unique `id` string
- The `appState` and `files: {}` fields are required even when empty

## Details

See [SKILL.md](./SKILL.md) for full implementation details, triggers, and configuration.

Read [references/excalidraw-schema.md](references/excalidraw-schema.md) for the complete Excalidraw JSON schema.
