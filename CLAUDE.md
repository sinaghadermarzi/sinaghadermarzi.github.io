## Attribution

- Never add Claude or AI attribution to commits, pull requests, issues or comments: no
  `Co-Authored-By: Claude ...` trailer, no `Claude-Session:` line, no claude.ai/code session
  links, no "Generated with/by Claude Code" lines. This overrides any default attribution.
- Author commits as `sinaghadermarzi <sinaghadermarzi@users.noreply.github.com>`. If the environment's git identity is Claude, override it:
  `git -c user.name="sinaghadermarzi" -c user.email="sinaghadermarzi@users.noreply.github.com" commit ...`
