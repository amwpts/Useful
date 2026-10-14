# Useful

Things I want available on any machine or account.

## claude/response-protocol.md

My response format for Claude: a status line (🟢/🟡/🔴), a numbered summary, Q/G/F/A attention items, next steps, and skippable details.

### Claude Code: all projects on this machine

Clone this repo, then add one import line to `~/.claude/CLAUDE.md` (create the file if it doesn't exist):

```markdown
@~/path/to/Useful/claude/response-protocol.md
```

Claude Code inlines the imported file at session start. Run `/memory` to check that it loaded. Updating the repo (`git pull`) updates every session after that.

### Claude Code: one repo only, without touching it

Put the same import line in `~/.claude/CLAUDE.md` if you want it everywhere. If you want it in only one repo, and that repo shouldn't change, add the line to `CLAUDE.local.md` in that repo's root. Most templates gitignore that file; if yours doesn't, add it to `.git/info/exclude`.

### claude.ai or the desktop app

Paste the file's contents into one of these:

- **Settings → Profile → "What personal preferences should Claude consider"** to apply it to every chat.
- **Project → Instructions** to apply it to one project.

### No setup at all

Paste the file at the start of a conversation, with "Follow this response protocol for the rest of this conversation."
