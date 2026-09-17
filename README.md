# BetterCommands API
*A universal framework for building chat commands in SM64 Coop DX*

---

## What is it

BetterCommands replaces the vanilla `hook_chat_command` with a declarative API. Instead of manually parsing strings, splitting on commas, checking `tonumber`, handling empty args, and copy-pasting the same colored error message every time — you just describe **what arguments the command expects**, and get ready-to-use values in the callback.

---

## Why it's worth using

- **Less code** — typical commands shrink 3–5x.
- **Fewer bugs** — type checks, bounds, missing args, and selectors are already handled.
- **Consistent style** — every command in your project looks the same; descriptions are auto-generated.
- **Crash protection** — the command body is wrapped in `pcall`; a bad input can't take down your mod.
- **Sane UX** — every error message includes a `Usage:` line with the command signature.
- **Built-in visuals** — ready-made success/error/warning/log functions with sounds and color coding.
- **Extensible** — new argument types are easy to add; registration is decoupled from parsing.

---

## Core features

- **Declarative arguments.** Types are declared in a table at registration time. The parser splits on commas, trims whitespace, converts to the right type, and validates the result — all for you.
- **Automatic documentation.** The command description is generated from the argument list. The signature is visible in `/help` and every error message.
- **Nine argument types.** Bool, int, number, string, player, player list, duration, color, rest-of-line.
- **Optional args and `...`.** Mark an argument as `optional = true` with a `default`, or explicitly skip any of them with `...` in chat.
- **Enum via `choices`.** Provide a list of allowed values; matching is case-insensitive and the result is normalized.
- **Player selectors.** `@s` (self), `@p` (nearest), `@r` (random), `@a` (all, list only). Enabled with `selectors = true`.
- **Permissions.** Gate commands by `anyone` / `moderator` / `host`. Checked automatically before parsing.
- **`pcall` protection with argument dump.** If `func` throws, the player sees a short message and the console gets the stack trace plus the parsed arguments.
- **Automatic usage line on errors.** Every parse failure prints the exact signature the player should follow.

---

## Argument types

- `CMD_ARG_BOOLEAN` — `true` / `false`, `1` / `0`.
- `CMD_ARG_INTEGER` — integer, with optional `min` / `max`.
- `CMD_ARG_NUMBER` — any number, with optional `min` / `max`.
- `CMD_ARG_STRING` — a string without commas.
- `CMD_ARG_PLAYER` — index, name, or selector (`@s`, `@p`, `@r`).
- `CMD_ARG_PLAYER_LIST` — a single player or `@a` (array of all connected).
- `CMD_ARG_DURATION` — `500ms`, `5s`, `2m`, `1h30m` → converted to frames.
- `CMD_ARG_COLOR` — `#rrggbb`, `rrggbb`, or a name (`red`, `cyan`) → `{r, g, b, hex}` with components in 0..255.
- `CMD_ARG_STRING_REST` — everything after the current token, commas preserved.

---

## Argument fields

- `[1]` — argument name (key in the `args` table).
- `[2]` — type (one of the constants above).
- `optional` — boolean, allows skipping with `...`.
- `default` — fallback value when the argument is skipped.
- `min` / `max` — numeric bounds for `INTEGER` and `NUMBER`.
- `choices` — list of allowed values (enum behavior).
- `selectors` — boolean, enables `@s` / `@p` / `@r` for player types.

---

## Permission levels

- `"anyone"` or omitted — everyone.
- `"moderator"` / `"mod"` — moderators and host.
- `"host"` / `"server"` / `"admin"` — host only.

---

## Installation

1. Download `better-commands-lib.lua`.
2. Drop it into your mod folder: `mods/MyMod/libs/better-commands-lib.lua`.
3. Require it (no `.lua` extension):

```lua
local BC = require("libs/better-commands-lib")
```

---

## License and feedback
The library is built for use in SM64 Coop DX mods. Free to integrate, modify, and redistribute alongside your mod.

Questions, bug reports, and suggestions for new argument types — drop them in this thread. Happy modding!
