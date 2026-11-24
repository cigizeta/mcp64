# Rename Aio64 to mcp64 -- plan v1

*Status: plan, awaiting approval.*

The app, the GitHub repository, and the local folder are renamed from
Aio64 to **mcp64**. The name `aio64` goes to another of the developer's
apps, so nothing in this repository may keep claiming it.

## Decisions

- The name is written `mcp64` everywhere, prose included. Identifiers:
  `mcp64`, `MCP64_TOKEN`, `__MCP64_VERSION__`, `MCP64_VERSION`,
  `ui://mcp64/...`, app name `mcp64-board`.
- Historical docs in `claude/done/` and `claude/archived/` stay unchanged.
  `TODO.md` gets one line: "Formerly named Aio64."
- All three tags (`v0.1.0`, `v0.1.1`, `v2026.11.0`) are deleted, locally
  and on GitHub. `v2026.11.0` is recreated on the rename commit. The
  version in `package.json` stays `2026.11.0`.
- The `mentes-final` tag is not touched.

## Step 1 -- edits in the repository (Claude)

Code:

- `package.json`, `package-lock.json`: `"name": "mcp64"`.
- `src/server.ts`: server name, `MCP64_VERSION`, `MCP64_TOKEN` check and
  its error message, startup log, every `ui://aio64/...` URI (board and
  engine resources).
- `widget/engine.ts`: `ui://mcp64/engine/...` reads, console prefix,
  header comment.
- `widget/board.ts`, `vite.config.ts`: `__MCP64_VERSION__`, app name
  `mcp64-board`.
- `widget/board.html`: `<title>`.
- `src/http.ts`: User-Agent `mcp64 (personal chess analysis server)`.
- `src/db.ts`, `src/hash.ts`: comments.

Docs:

- `README.md` (title, prose, clone command, `MCP64_TOKEN`).
- `CLAUDE.md` (title, prose, `<MCP64_TOKEN>` placeholder).
- `.env.example`: `MCP64_TOKEN=`.
- `claude/TODO.md` and every doc in `claude/active/`.
- `stockfish-analysis-v1.md` and `hello-board-spike-v1.md` refer to
  `aio64-design-v1.md` by filename; change them to `mcp64-design-v1.md`.

Check afterwards: a case-insensitive search for `aio64` outside
`claude/done/` and `claude/archived/` finds only this plan.

## Step 2 -- rename the design doc, build, test (developer)

```
git mv claude/active/aio64-design-v1.md claude/active/mcp64-design-v1.md
```

In `.env`, rename `AIO64_TOKEN` to `MCP64_TOKEN` (value unchanged). Then
build and start the server as usual, and check the startup log shows
`mcp64` and a `ui://mcp64/board-...` URI.

## Step 3 -- reconnect the connector (developer)

The server name and the `ui://` URIs changed, so claude.ai's cached tool
list is stale. In Settings > Connectors, remove the Aio64 connector and
add one named **mcp64** with the same `$PUBLIC_URL/mcp` and the same
`X-Api-Key`. In a new chat, ask to show the mcp64 board and check that
analysis runs.

## Step 4 -- commit (developer)

```
git add -A
```

```
git commit -m "Rename Aio64 to mcp64"
```

## Step 5 -- replace the tags (developer)

Delete the three tags locally:

```
git tag -d v0.1.0 v0.1.1 v2026.11.0
```

Delete them on GitHub:

```
git push origin --delete v0.1.0 v0.1.1 v2026.11.0
```

Recreate `v2026.11.0` on the rename commit:

```
git tag -a v2026.11.0 -m "mcp64 2026.11.0"
```

If GitHub has Releases attached to the old tags, deleting a tag turns
them into drafts; delete those drafts in the web UI.

## Step 6 -- rename the GitHub repository (developer)

Done first, before step 1, with `gh` (which also updates the local
`origin` remote):

```
gh repo rename mcp64
```

Later, after steps 1-5:

```
git push origin main
```

```
git push origin v2026.11.0
```

## Step 7 -- rename the local folder (developer)

Close VS Code (and any terminal inside the folder), then:

```
mv /c/dev/aio64 /c/dev/mcp64
```

Reopen `c:\dev\mcp64` in VS Code. Claude Code keeps project memory and
session history by folder path, so earlier sessions stay under the old
path.

## Step 8 -- only now, free the name (developer)

GitHub redirects `aio64` to `mcp64` until a new repository named `aio64`
exists; then the redirect stops for good. Before creating the other app's
`aio64` repository, make sure every clone of this one (other machines
included) points at `mcp64` (step 6's `set-url`).

## Done when

- No `aio64` outside `claude/done/`, `claude/archived/`, and this plan.
- The server runs from `c:\dev\mcp64`, the mcp64 connector shows the
  board and analyzes.
- GitHub shows the repository `mcp64` with exactly the tags
  `v2026.11.0` (on the rename commit) and `mentes-final`.
