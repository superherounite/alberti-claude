# Alberti for Claude Code

[한국어](#한국어) · [English](#english)

Let Claude Code read and edit the design open in [Alberti](https://alberti.design): layers, components, variables, design intent, the click prototype, layer motion, 3D models, and export to Unity, Unreal and Omniverse.

## English

### What you need

- The **Alberti desktop app** (macOS; Windows soon): https://alberti.design/download. It runs Alberti's MCP server on this computer at `http://127.0.0.1:7621/mcp`. Nothing else to install.
- [Claude Code](https://claude.com/claude-code).

### Install

In Claude Code:

```
/plugin marketplace add superherounite/alberti-claude
/plugin install alberti@alberti
```

Restart Claude Code if it asks. Then open a design in Alberti and try:

```
Use Alberti: check what's open, read the design intent, and make the first screen for scenario 1.
```

Claude asks before using each Alberti tool. Choose **"Yes, and don't ask again"** to allow the Alberti tools for good.

The plugin adds the `alberti` MCP server and a skill that teaches Claude how Alberti works (intent first, look before changing, name layers for engine export, prototype, preflight, export).

### Without the plugin

```
claude mcp add --scope user --transport http alberti http://127.0.0.1:7621/mcp
```

Other MCP clients (Cursor, Windsurf, …) can use the same URL as a Streamable HTTP server.

### Good to know

- Every edit is one undo step in Alberti (⌘Z / Ctrl+Z) and shows on the canvas as Claude. If the design is a server file, edits save and reach your collaborators live.
- Only programs on this computer can connect; web pages are refused. Turn the connection off in **Alberti → Settings → Server & team → Claude Code**.
- The web version of Alberti cannot be reached from Claude Code.

## 한국어

### 필요한 것

- **Alberti 설치형 앱**(macOS, Windows는 곧 공개): https://alberti.design/download. 앱이 켜져 있는 동안 이 컴퓨터의 `http://127.0.0.1:7621/mcp`에서 Alberti MCP 서버가 동작합니다. 따로 설치할 것은 없습니다.
- [Claude Code](https://claude.com/claude-code).

### 설치

Claude Code에서:

```
/plugin marketplace add superherounite/alberti-claude
/plugin install alberti@alberti
```

다시 시작하라고 하면 Claude Code를 다시 켜 주세요. Alberti에서 디자인을 하나 연 뒤 이렇게 시작해 보세요.

```
Alberti에서 지금 열린 파일을 확인하고, 디자인 의도를 읽은 다음 시나리오 1의 첫 화면을 만들어 줘.
```

Claude는 Alberti 도구를 쓸 때마다 허락을 묻습니다. **"Yes, and don't ask again"**을 고르면 다음부터 묻지 않습니다.

플러그인은 `alberti` MCP 서버와 함께, Claude에게 Alberti 작업 방식(의도부터, 고치기 전에 보기, 엔진 익스포트를 위한 레이어 이름, 프로토타입, 프리플라이트, 익스포트)을 알려 주는 스킬을 설치합니다.

### 플러그인 없이

```
claude mcp add --scope user --transport http alberti http://127.0.0.1:7621/mcp
```

Cursor, Windsurf 같은 다른 MCP 클라이언트도 같은 주소를 Streamable HTTP 서버로 쓰면 됩니다.

### 알아 두실 것

- Claude가 고친 내용은 Alberti에서 한 단계씩 실행 취소(⌘Z / Ctrl+Z)할 수 있고, 캔버스에 Claude로 표시됩니다. 서버 파일이면 바로 저장되고 함께 작업하는 사람에게도 실시간으로 보입니다.
- 이 컴퓨터의 프로그램만 연결할 수 있고, 웹 페이지는 막혀 있습니다. 연결을 끄려면 **Alberti → 설정 → Server & team → Claude Code**에서 끄면 됩니다.
- 웹 버전 Alberti는 Claude Code와 연결되지 않습니다.

---

MIT License · © 2026 Super Hero Unite, Inc. · Alberti is a product of Super Hero Unite, Inc.
