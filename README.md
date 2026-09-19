# Excalidraw Diagram Skill

An AI skill definition that teaches an AI assistant to generate **Excalidraw diagram files** compatible with the [Obsidian Excalidraw Plugin](https://github.com/zsviczian/obsidian-excalidraw-plugin).

## Overview

This skill enables an AI agent to automatically create visual diagrams from text content. When triggered by keywords like "Excalidraw", "diagram", "flowchart", or their Chinese equivalents, the AI will:

1. Analyze the provided content
2. Select the most appropriate diagram type
3. Generate valid Excalidraw JSON
4. Save an Obsidian-ready `.md` file to the current directory

## Supported Diagram Types

| Type | Use Case |
|------|----------|
| **Flowchart** | Step-by-step processes, workflows, task sequences |
| **Mind Map** | Concept brainstorming, topic classification, idea capture |
| **Hierarchy** | Organizational structures, content grading, system breakdown |
| **Relationship** | Influence, dependency, and interaction between elements |
| **Comparison** | Side-by-side analysis of multiple options or viewpoints |
| **Timeline** | Event progression, project milestones, model evolution |
| **Matrix** | Two-dimensional classification, task prioritization |
| **Freeform** | Scattered content, quick notes, initial information gathering |

## Trigger Keywords

The skill activates on the following keywords:

- `Excalidraw`, `diagram`, `flowchart`, `mind map`
- `画图`, `流程图`, `思维导图`, `可视化`

## Installation

Copy `SKILL.md` and the `references/` directory into your AI agent's skill directory.

```
your-agent-skills/
├── excalidraw-diagram-skill/
│   ├── SKILL.md
│   └── references/
│       └── excalidraw-schema.md
```

## Output Format

The generated files use a strict Obsidian-compatible Markdown structure:

```markdown
---
excalidraw-plugin: parsed
tags: [excalidraw]
---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== ...

# Excalidraw Data

## Text Elements
%%
## Drawing
```json
{Excalidraw JSON}
```
%%
```

## File Naming Convention

Files are saved as `[Topic].[DiagramType].md` (Chinese preferred for clarity):

- `ContentCreationProcess.flowchart.md`
- `商业模式.relationship.md`
- `ProjectTimeline.timeline.md`

## Design Rules

### Fonts

- All text elements must use `fontFamily: 5` (Excalifont)
- Title: 24-28px
- Subtitle: 18-20px
- Body text: 14-16px
- Line height: `1.25` for all text

### Color Palette

| Purpose | Hex | Description |
|---------|-----|-------------|
| Title | `#1e40af` | Deep Blue |
| Subtitle / Connectors | `#3b82f6` | Medium Blue |
| Body Text | `#374151` | Dark Gray |
| Emphasis | `#f59e0b` | Orange/Gold |
| Success | `#10b981` | Green |
| Warning | `#ef4444` | Red |

**Background colors:**

| Color | Hex |
|-------|-----|
| Light Blue | `#dbeafe` |
| Light Gray | `#f3f4f6` |
| Light Orange | `#fef3c7` |
| Light Green | `#d1fae5` |
| Light Purple | `#ede9fe` |

### Text Substitution Rules

- Replace `"` with `『』` in text content
- Replace `()` with `「」` in text content

### Layout

- Canvas range: 0-1200 x 0-800 pixels
- Coordinate origin: top-left (0,0)
- Ensure adequate spacing between elements

## JSON Schema

See [`references/excalidraw-schema.md`](references/excalidraw-schema.md) for the complete Excalidraw JSON schema, including element types, binding rules, and fill styles.

## Usage in Obsidian

1. Generate a diagram file (via the AI agent)
2. Open the `.md` file in Obsidian
3. Click **MORE OPTIONS** in the top-right corner
4. Select **Switch to EXCALIDRAW VIEW**

## Version

1.1.0
