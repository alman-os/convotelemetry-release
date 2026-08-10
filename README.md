![convoTelemetry window](https://github.com/alman-os/convotelemetry-release/blob/main/convoTelemetry_horizontal_brandified.png)

# convoTelemetry

**Turn a long ChatGPT or Claude conversation into a visual map — using your own chat, no API key, nothing leaving your Mac.**

You paste a short prompt into a conversation you're already having. The AI hands back a structured JSON summary of that chat: its topics, how you jumped between them, the tone, and what got built along the way. convoTelemetry parses that JSON into an interactive node graph and a telemetry dashboard, then saves it to a local library you own.

It's a desktop app for people who have long, sprawling AI conversations and want to see the shape of them.

![convoTelemetry window](https://github.com/alman-os/convotelemetry-release/blob/main/convoTelemetry_1_UI-main-view_brandified.png)

<table>
  <tr>
    <td>
      <a href="https://youtu.be/CliYf4wyBLc">
        <img src="https://github.com/alman-os/convotelemetry-release/blob/main/yt-thumbnail.png" alt="Watch the video" width="300">
      </a>
    </td>
    <td>
      <h3>ConvoTelemetry - Turn AI Conversations Into Visual Project Maps
</h3>
      <p> 👈🏻 Watch the Youtube Explainer here!
</p>
    </td>
  </tr>
</table>

## Table of Contents

- [What Does This Do](#what-does-this-do)
- [Installation](#installation)
- [Usage](#usage)
- [What Can You Do With This](#what-can-you-do-with-this)
- [Privacy](#privacy)
- [Tech Details](#tech-details)
- [Contributing](#contributing)
- [License](#license)

## What Does This Do

- **Maps a conversation into a graph** — topics become nodes, transitions become edges. Node size is topic weight, edge color is the "vibe" of the jump, glow scales with emotional temperature.
- **No API key, no background calls** — you run the analysis in your own ChatGPT/Claude/Monday session by copy-pasting one prompt. The app never talks to an LLM itself.
- **Milestone checkpoint protocol** — the first prompt tells the AI to remember that JSON as a checkpoint. Later, you send only what changed and it extends the map, so follow-ups stay short instead of re-pasting the whole thing every time.
- **Local-first library** — every map is a JSON file under `~/Documents/AOS/convoTelemetry/`, one folder per conversation, with a full checkpoint history of each update.
- **Telemetry dashboard** — executive summary, tone profile, key ideas, artifacts generated, and a list of the semantic jumps with their reasons.
- **Zoom for dense maps** — once a conversation has more than 9 topics, a zoom control appears so a crowded graph stays readable.
- **Share back to an LLM** — one click opens ChatGPT or Claude with a compact version of the map pre-loaded, or copies it to your clipboard if it's too big for a URL.

## Installation

**Requirements:** macOS on Apple Silicon (M1 or newer). The current build is `aarch64` only — it will not run on Intel Macs.

1. Download `convoTelemetry_0.2.0_aarch64.dmg` from the [Releases page](https://langscript.gumroad.com/l/convo-telemetry).
2. Open the DMG and drag **convoTelemetry** into Applications.
3. Launch it from Applications or Spotlight.

The app is signed with an Apple Developer ID and notarized, so it opens without a Gatekeeper warning. If macOS ever does complain, right-click the app and choose **Open** once.

**Intel Mac?** There's no Intel build yet. You'd need to build from source (see [Tech Details](#tech-details)) with the `x86_64-apple-darwin` target.

## Usage

The core loop is copy → run in your chat → paste back.

1. Open **New Journey** and click **Copy Prompt to Clipboard**.
2. Paste that prompt into the ChatGPT or Claude conversation you want to map. The AI replies with a JSON payload.
3. Copy the JSON, paste it into the **Parse Telemetry** box, optionally add the conversation's URL, and click **Create Semantic Journey**.
4. The map opens — drag nodes to explore, hover for topic detail, and read the dashboard on the right.

**Updating a map as the conversation grows:**

1. Open the map and click **Update**.
2. Click **Copy Continuation Prompt** and paste it back into the *same* chat. Because the AI already holds the milestone checkpoint, you only describe what's new — no re-pasting the whole journey.
3. Paste the updated JSON it returns into the Update box and apply. A full snapshot is saved to that journey's checkpoint history.

**No conversation handy?** New Journey has an **Inject Sample Telemetry Payload** button that fills the box with realistic sample data so you can try the whole flow without leaving the app.

## What Can You Do With This

![project view ConvoTelemetry](https://github.com/alman-os/convotelemetry-release/blob/main/convoTelemetry_2_UI-projectView_cleaned_brandified.png)

- **See where a long brainstorm actually went** — the jumps and clusters show the path you took, including the tangents you forgot about.
- **Keep a browsable archive of your best AI sessions** — the library is searchable by title, topic, and mood, and every file is plain JSON you can open anywhere.
- **Track how a project conversation evolved** — checkpoint snapshots give you a timeline of each update instead of overwriting history.
- **Hand a conversation's structure to another AI** — send the compact map to ChatGPT or Claude to get observations, follow-up questions, or a continuation.
- **Build a shared output library** — maps land under `~/Documents/AOS/`, the same root other Alman OS apps write to, so a higher-level tool can read them all at once.

## Privacy

Everything stays on your machine. convoTelemetry has no servers, no telemetry of its own, and no API keys. The only network actions are ones you trigger by hand: opening a conversation URL, or opening a ChatGPT/Claude share link in your browser. The analysis itself runs in whatever chat you already use, on your own account.

Your maps are JSON files in `~/Documents/AOS/convoTelemetry/`. Delete the folder and the data is gone.

## Tech Details

- **Stack:** Tauri v2 (Rust shell) + React 19 + Vite + TypeScript + Tailwind CSS.
- **Graph:** a custom force-directed canvas engine — repulsion, link attraction, center gravity, drag, and center-anchored zoom.
- **Storage:** one session folder per journey, a canonical `<slug>.json` plus append-only `checkpoints/`, all under `~/Documents/AOS/convoTelemetry/`.

## Contributing

This repo is source-available so you can inspect how the app works, see exactly how it stores your data, and send fixes. Issues and pull requests are welcome. It is not an open-source license and does not grant permission to ship competing clones — see [License](#license).

## License

This project is source-available under the PolyForm Shield License 1.0.0.

You may read the source, learn from it, and contribute. You may not sell, repackage, white-label, or distribute competing versions of this app without a commercial license.

Commercial licensing: business@alman-os.com

See [LICENSE.md](LICENSE.md).
