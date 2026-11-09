# Alberti for Claude Code

[한국어](#한국어) · [English](#english)

Let Claude Code read and edit the design open in [Alberti](https://alberti.design): layers, components, variables, design intent, the click prototype, layer motion, 3D models, and export to Unity, Unreal and Omniverse.

## English

### What you need

- An Alberti account (sign in with Google at https://alberti.design) and Alberti open: in the browser, or the desktop app (https://alberti.design/download). Claude works in the window you have open.
- [Claude Code](https://claude.com/claude-code). For the Claude app (claude.ai, desktop, mobile) you need no plugin: add the connector below.

### In the Claude app

Settings → Connectors → **Add custom connector**: name `Alberti`, URL `https://alberti.design/mcp`. Sign in with your Alberti account and allow access.

### Install

In Claude Code:

```
/plugin marketplace add superherounite/alberti-claude
/plugin install alberti@alberti
```

Restart Claude Code if it asks, run `/mcp` and sign in to Alberti once. Then open a design in Alberti and try:

```
Use Alberti: check what's open, read the design intent, and make the first screen for scenario 1.
```

Claude asks before using each Alberti tool. Choose **"Yes, and don't ask again"** to allow the Alberti tools for good.

The plugin adds the `alberti` MCP server and a skill that teaches Claude how Alberti works (intent first, look before changing, name layers for engine export, prototype, preflight, export).

### Without the plugin

```
claude mcp add --scope user --transport http alberti https://alberti.design/mcp
```

Other MCP clients (Cursor, Windsurf, …) can use the same URL as a Streamable HTTP server with OAuth.

Working offline or on a self-hosted Alberti? The desktop app also serves MCP clients on the same computer at `http://127.0.0.1:7621/mcp` (no sign-in).

### Good to know

- Every edit is one undo step in Alberti (⌘Z / Ctrl+Z) and shows on the canvas as Claude. If the design is a server file, edits save and reach your collaborators live.
- Claude acts as you and only reaches what you can open. See and disconnect connected apps, or turn Claude off, in **Alberti → Settings → Server & team → Claude**.

## 한국어

### 필요한 것

- Alberti 계정(https://alberti.design 에서 구글로 로그인)과 열려 있는 Alberti 창. 브라우저든 설치형 앱(https://alberti.design/download)이든 됩니다. Claude는 열어 둔 창에서 작업합니다.
- [Claude Code](https://claude.com/claude-code). Claude 앱(claude.ai, 데스크톱, 모바일)은 플러그인 없이 아래 커넥터만 추가하면 됩니다.

### Claude 앱에서

설정 → 커넥터 → **사용자 지정 커넥터 추가**: 이름 `Alberti`, URL `https://alberti.design/mcp`. Alberti 계정으로 로그인하고 허용을 누르세요.

### 설치

Claude Code에서:

```
/plugin marketplace add superherounite/alberti-claude
/plugin install alberti@alberti
```

다시 시작하라고 하면 Claude Code를 다시 켜고, `/mcp`에서 Alberti에 한 번 로그인하세요. Alberti에서 디자인을 하나 연 뒤 이렇게 시작해 보세요.

```
Alberti에서 지금 열린 파일을 확인하고, 디자인 의도를 읽은 다음 시나리오 1의 첫 화면을 만들어 줘.
```

Claude는 Alberti 도구를 쓸 때마다 허락을 묻습니다. **"Yes, and don't ask again"**을 고르면 다음부터 묻지 않습니다.

플러그인은 `alberti` MCP 서버와 함께, Claude에게 Alberti 작업 방식(의도부터, 고치기 전에 보기, 엔진 익스포트를 위한 레이어 이름, 프로토타입, 프리플라이트, 익스포트)을 알려 주는 스킬을 설치합니다.

### 플러그인 없이

```
claude mcp add --scope user --transport http alberti https://alberti.design/mcp
```

Cursor, Windsurf 같은 다른 MCP 클라이언트도 같은 주소를 OAuth가 있는 Streamable HTTP 서버로 쓰면 됩니다.

오프라인이거나 자체 서버에 설치한 Alberti라면, 설치형 앱이 같은 컴퓨터의 MCP 클라이언트를 위해 `http://127.0.0.1:7621/mcp`(로그인 없음)에서도 대기합니다.

### 알아 두실 것

- Claude가 고친 내용은 Alberti에서 한 단계씩 실행 취소(⌘Z / Ctrl+Z)할 수 있고, 캔버스에 Claude로 표시됩니다. 서버 파일이면 바로 저장되고 함께 작업하는 사람에게도 실시간으로 보입니다.
- Claude는 내 계정으로, 내가 열 수 있는 파일에만 닿습니다. 연결된 앱 보기·끊기, Claude 끄기는 **Alberti → 설정 → Server & team → Claude**에서 합니다.

---

MIT License · © 2026 Super Hero Unite, Inc. · Alberti is a product of Super Hero Unite, Inc.
