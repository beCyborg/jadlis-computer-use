English · [Русский](README.md)

# Part of the work lives in an application window that has neither an API nor a command line

`jadlis-computer-use` does not install the screen-control server — it already ships inside Claude
Code — it adds the discipline around it: what replaces a screen frame, what goes into a single
batch, and why a pixel click is last in the queue rather than first.

```
claude plugin marketplace add https://github.com/beCyborg/jadlis-hub
claude plugin install jadlis-computer-use@jadlis
```

No keys needed, but the `computer-use` server itself is switched on once through `/mcp` and requires
a Pro or Max plan.

![Three cheaper routes to the task, and only past them — clicks on the screen](docs/img/hero-jadlis-computer-use.webp)

In words: on the left a ladder of methods — the application's own interface, system commands, the
browser, and only at the top clicks on the screen — on the right the native window nothing else can
reach.

This is my workbench published as it is, not a product: whatever I stopped using, I removed.

## Before → after

| By hand | With an AI chat | With this plugin |
|---|---|---|
| **How a task reaches an app without an API.** You click it yourself, window after window. | It explains where to press, and you still press it. | The agent drives the native window itself — but only after it has checked the cheaper routes: the app's own interface, system commands and URL schemes, the browser. |
| **What it costs to look at the screen.** You look with your eyes; there is nothing to pay. | You capture the frame yourself and attach it to the message. | A frame is not taken on every step: the result is pulled as text — a Copy button or a save-to-file — and a predictable chain goes out as one batch with a trailing frame at the end. |
| **How access to apps is granted.** No question arises: it is your computer. | — | The route is planned whole and access is requested in a single dialog for the entire list of apps; a refusal means stop and report, with no workarounds. |
| **What counts as "done".** You see the result on screen. | It answers "it should work". | Done is verified by the live state: the file exists, the command printed output, the final frame shows the end screen. The absence of an error does not count as success. |
| **When the chain stops moving.** You give up and do it by hand. | — | Two or three iterations without progress mean stop and ask, not another round of clicking; an app once worked out is written down in `references/app-recipes.md` and costs little the next time. |

## How it works

![A desktop task, the choice of route, one access request, a batch of actions, verification by the live state](docs/img/how-jadlis-computer-use.webp)

Going in — a task in a native window and your permission: the access dialog is confirmed by you, not the agent.
Inside — the cheaper surfaces first, then actions in batches and text channels instead of frames.
Coming out — a saved result verified by the live state, not a self-report saying "done".

In words: desktop task → choice of route → one access request → batch of actions → verification by
the live state.

The surface hierarchy is the same for every task: the app's own API, MCP or CLI → structural access
through `osascript` and `open` with a URL scheme → the browser → and only then clicks on the screen.
Electron apps — Slack, Obsidian, VS Code, Discord — are a browser in a wrapper, and their road runs
through their own MCP or CLI, not through pixels.

A screenshot is the most expensive operation in the loop: the frame enters the context as an image
and stays in the history, which is re-sent on every iteration. So the skill names the target before
the frame, takes one after opening an app, after navigation and before an irreversible action — and
replaces it with text wherever it can: the clipboard, a save-to-file, a zoom into a region of the
frame already taken.

Mechanical chains go out as a single `computer_batch` call: actions run in order, stopping at the
first error, coordinates inside the batch are measured against the pre-batch frame, and the trailing
screenshot verifies the outcome and becomes the coordinate base for the next call. Exploration and
error recovery are not batched.

Diagnostics has its own section because the symptoms mislead: an empty or black frame is a capture
failure, not "the app is not running"; the permissions belong to the application Claude Code was
started from, and it has to start after they were granted; part of the system menus is drawn in a
layer outside the capture, and no amount of aiming precision reaches it — only the keyboard.

## Installing and the first run

**a) Text to paste to the agent.** Copy it whole into a Claude Code chat:

```
You are the installer. Put the jadlis-computer-use plugin from the jadlis marketplace on this Mac.
This plugin needs no keys, there is nothing secret to ask for.
Run exactly these commands, verbatim, abbreviating nothing:
1. claude plugin marketplace add https://github.com/beCyborg/jadlis-hub
2. claude plugin install jadlis-computer-use@jadlis
3. claude plugin list — show me the jadlis-computer-use line and its version.
Then tell me in one line: run /mcp, enable the computer-use server and restart Claude Code.
Before each command show it to me in full and wait for a yes. If I say no, do not run it,
tell me what you skipped and move on.
If a command returns an error — stop, show the output, do not go on to the next one.
```

**b) The commands by hand.**

```
claude plugin marketplace add https://github.com/beCyborg/jadlis-hub
claude plugin install jadlis-computer-use@jadlis
claude plugin list
```

The first command installs nothing — it adds the marketplace. Only the second installs, and it is
removed by one line: `claude plugin uninstall jadlis-computer-use@jadlis --keep-data`.

**c) The short command.** Open Claude Code and type:

```
/computer-use
```

Not found — check the name with `claude plugin list`. The skill also triggers on a plain sentence:
"take a screenshot of the desktop and tell me which apps are open".

**Enabling the server — once per project.** Run `/mcp`, find `computer-use` in the list and enable
it. On first use macOS asks for two permissions: Accessibility (clicks, typing, scrolling) and
Screen Recording (frames) — both are required. The server is missing from the list — check the
requirements: macOS, an interactive session, a Pro or Max plan.

## Limits, cost, updating

**What it does not do.** It works nowhere but macOS, and only in an interactive session: headless
(`claude -p`) has no server. It does not install the server — that one is built in; the plugin adds
the skill. It does not go to the web or to logged-in sites: that is the neighbouring plugin
`jadlis-browser`, and it is cheaper than pixels. It does not reach into Electron apps with pixels
while they have their own MCP or CLI. It does not route around your refusal in the access dialog by
scripting. It does not count the absence of an error as success and does not report "done" without a
saved result. It does not hold two sessions at once: there is one machine lock and it lives until
the session exits, not until the task ends.

**What you need.** No keys and no third-party subscriptions. You need a Claude **Pro or Max** plan:
screen control is a research preview and is not available on Team and Enterprise. You need macOS and
two permissions in System Settings — Accessibility and Screen Recording — granted to the application
Claude Code is started from; after granting them the application is restarted, otherwise the rights
are not picked up. For long tasks Do Not Disturb helps: a notification banner that pops up takes the
click for itself.

**How tokens get spent.** A run is heavier than an ordinary text one: every screen frame enters the
context as an image and stays in the history, and the history is re-sent on every iteration of the
loop. So the skill's discipline is not about tidiness, it is about the bill: a text channel instead
of a frame, a batch instead of scattered calls, one frame at the end instead of a frame after every
step.

**Verified where I work:** my Mac, my subscription. macOS only — outside it the server simply does
not exist. On macOS beta builds the anchors of system dialogs and deep links drift away from the
recipes: the skill says so itself, and the differences are written into
`references/app-recipes.md` on the spot.

**Terms of use.** There is no license: all rights reserved by the author. You may read it and use it
personally. Commercial use, republishing and bundling it into your own products — by arrangement
with me.

**Updating.** With a third-party marketplace, auto-update is off on your side: until you run the
first command you keep the version you installed.

```
claude plugin marketplace update jadlis
claude plugin update jadlis-computer-use@jadlis
claude plugin list
```

A reinstall, if something landed wrong:

```
claude plugin uninstall jadlis-computer-use@jadlis --keep-data && claude plugin install jadlis-computer-use@jadlis
```

Version history — [CHANGELOG.md](CHANGELOG.md).
