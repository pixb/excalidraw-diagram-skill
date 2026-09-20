# Eval: excalidraw-diagram-skill

Evaluates the skill's ability to generate valid Excalidraw diagrams for Obsidian.

## Criteria

1. **valid-markdown-structure** — Output is a valid Markdown file with Excalidraw frontmatter (`excalidraw-plugin: parsed`, `tags: [excalidraw]`)
2. **valid-excalidraw-json** — Contains a valid JSON block inside `%%` delimiters with `type: "excalidraw"`, `version: 2`, `elements` array, `appState`, and `files` fields
3. **text-elements-empty** — The `## Text Elements` section contains only `%%` delimiters (no manually listed text)
4. **font-family-correct** — All text elements in the JSON use `fontFamily: 5`
5. **no-broken-characters** — No unescaped `"` or `()` in text content (replaced with `『』` and `「」`)
6. **element-ids-unique** — Every element in the JSON has a unique `id` field

```json
{
  "skill": "excalidraw-diagram-skill",
  "run": "cp {input} {output}",
  "criteria": [
    {
      "id": "valid-markdown-structure",
      "text": "Output is a Markdown file with frontmatter containing 'excalidraw-plugin: parsed' and 'tags: [excalidraw]'",
      "type": "command",
      "cmd": "grep -q 'excalidraw-plugin: parsed' {output} && grep -q 'tags:.*excalidraw' {output}"
    },
    {
      "id": "valid-excalidraw-json",
      "text": "Output contains valid JSON with type 'excalidraw', version 2, elements array, appState, and files",
      "type": "command",
      "cmd": "python3 -c \"import json,sys; data=open(sys.argv[1]).read(); start=data.index('```json\\n')+7; end=data.index('\\n```',start); obj=json.loads(data[start:end]); assert obj.get('type')=='excalidraw' and obj.get('version')==2 and 'elements' in obj and 'appState' in obj and 'files' in obj\" {output}"
    },
    {
      "id": "text-elements-empty",
      "text": "The ## Text Elements section contains only %% delimiters",
      "type": "command",
      "cmd": "python3 -c \"import sys; data=open(sys.argv[1]).read(); idx=data.index('## Text Elements'); end=data.index('## Drawing',idx); section=data[idx:end]; lines=[l.strip() for l in section.split('\\n') if l.strip() and l.strip()!='##' and l.strip()!='%%' and not l.strip().startswith('##')]; assert len(lines)==0, f'Found non-empty lines: {lines}'\" {output}"
    },
    {
      "id": "font-family-correct",
      "text": "All text elements use fontFamily 5",
      "type": "command",
      "cmd": "python3 -c \"import json,sys; data=open(sys.argv[1]).read(); start=data.index('```json\\n')+7; end=data.index('\\n```',start); obj=json.loads(data[start:end]); texts=[e for e in obj['elements'] if e.get('type')=='text']; bad=[e['id'] for e in texts if e.get('fontFamily')!=5]; assert not bad, f'Wrong fontFamily: {bad}'\" {output}"
    },
    {
      "id": "no-broken-characters",
      "text": "Text content does not contain unescaped double quotes or parentheses",
      "type": "llm-judge"
    },
    {
      "id": "element-ids-unique",
      "text": "Every element has a unique id",
      "type": "command",
      "cmd": "python3 -c \"import json,sys; data=open(sys.argv[1]).read(); start=data.index('```json\\n')+7; end=data.index('\\n```',start); obj=json.loads(data[start:end]); ids=[e.get('id','') for e in obj['elements']]; dupes=[i for i in ids if ids.count(i)>1]; assert not dupes, f'Duplicate ids: {set(dupes)}'\" {output}"
    }
  ],
  "golden": [
    {
      "id": "case-1",
      "input": "golden/case-1/input.txt",
      "expected": "golden/case-1/expected.md",
      "split": "val"
    },
    {
      "id": "case-2",
      "input": "golden/case-2/input.txt",
      "expected": "golden/case-2/expected.md",
      "split": "val"
    },
    {
      "id": "case-3",
      "input": "golden/case-3/input.txt",
      "expected": null,
      "split": "test",
      "expected_status": "pending-first-green"
    }
  ],
  "judge": {
    "model": "claude-haiku-4-5-20251001",
    "temperature": 0,
    "canary": "golden/canary/bad_output.md"
  }
}
```
