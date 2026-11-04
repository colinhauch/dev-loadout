# Common tools

Tools that are useful across projects but aren't part of the SDLC itself.

## Agentation

Visual feedback for agents. Annotate elements in a running app, and it turns the annotations into structured markdown an agent can act on. Desktop only. Docs: https://www.agentation.com/

Install (dev dependency):

```bash
npm install agentation -D
```

Use:

1. Click the icon in the bottom-right corner to open the toolbar.
2. Hover to see element names, then click an element to annotate it.
3. Write the feedback and click "Add".
4. Click to copy the markdown, then paste it into Claude Code or Codex.

Agents can also read feedback live over MCP (see https://www.agentation.com/mcp).
