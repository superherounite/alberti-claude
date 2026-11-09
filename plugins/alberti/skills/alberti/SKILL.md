---
name: alberti
description: Design in Alberti through its MCP server. Use when the user asks to look at, create, change, review, prototype or export a design in Alberti (alberti.design), mentions an Alberti file, screen or frame, or wants a UI design turned into Unity, Unreal or Omniverse assets.
---

# Working in Alberti

Alberti is a design editor (Figma-compatible documents) with 3D layers, click prototypes, layer motion and export to game engines. The `alberti` MCP server in this plugin talks to the **Alberti desktop app running on this computer**: every tool reads or edits the design open in that app, live, and every edit is one undo step the user can reverse with ⌘Z / Ctrl+Z. If the file is a server file, edits save and reach collaborators in real time.

## 1. Connect first

Call `alberti_status` before anything else. It returns the signed-in user, the open document, its pages and screens, the selection and whether there is a design intent brief.

If the call fails or the tools are missing, tell the user what to do and stop. Do not try to start the app yourself.

| Symptom | What the user does |
|---|---|
| Connection refused, no `alberti` tools | Install and open the Alberti desktop app (https://alberti.design/download). It listens on `http://127.0.0.1:7621/mcp` while it runs. |
| "Claude Code is turned off in Alberti" | Alberti → Settings → Server & team → Claude Code → turn on. |
| "No design is open in Alberti" | Open or create a design in the app. |
| Export says "not a server file" | Sign in to Alberti; new designs become server files on their first change. |

The web version of Alberti cannot be reached from Claude Code; the desktop app is required.

## 2. Design to the intent

Alberti keeps the *why* of a design in the file:

- **Brief** (`alberti_get_intent`): where the design is used (`context`), the problem, who uses it and in what situation, scenarios, success criteria, constraints and non-goals. Read it before designing and design to it. If there is none and the task is more than a small fix, ask the user for the essentials or draft one with `alberti_set_intent` and show it to them.
- **Context sets the rules** preflight checks against:

  | context | screen | smallest target | smallest text |
  |---|---|---|---|
  | `hmi` (vehicle display) | 1920×720 | 80 px | 20 px |
  | `xr` | 2048×1024 | 64 px | 20 px |
  | `twin` (digital twin, control room) | 1920×1080 | 32 px | 12 px |
  | `web` | 1440×900 | 24 px | 12 px |
  | `mobile` | 390×844 | 44 px | 12 px |
  | `other` | 1440×900 | 44 px | 12 px |

- **Decisions** (`alberti_set_decision`): when you explore alternatives, put each in its own top-level frame, then record which was chosen, which are alternatives and which were rejected, each with a reason.
- **Why** (`alberti_set_why`): note why a non-obvious layer exists (a warning, a disabled state, a field the regulation requires), so developers and QA know.
- `alberti_make_scenario_screens` makes one empty, correctly sized screen per scenario to design into.

## 3. Look, then change

- Orient with `get_page_tree` (pass `depth` and `root_id` on big files), `find_nodes` / `query_nodes`, `get_node`, `describe`.
- See what you made with `export_image` on the screen you changed. Check it visually before saying you are done.
- Reuse what the file has: `get_components`, `list_libraries`, `insert_library_component`, `list_variables` and `bind_variable` instead of hard-coded colours and sizes. `design_to_tokens` summarises the file's tokens.
- Build larger pieces with `render` (Design JSX, see `get_jsx` on an existing layer for the dialect) and adjust with `update_node`, `set_layout` (auto layout), `set_text`, `set_fill` and friends. `batch_update` changes many layers in one step.
- Top-level frames on a page are **screens**. Keep them as frames, not groups.
- **Name layers for people and engines.** Engine export turns layer names into GameObject and element names; "Frame 12" and "Rectangle 3" are not acceptable in a finished screen. `rename_node` fixes them; preflight flags them.
- Run `lint` for low contrast, small targets and unbound colours.

## 4. Prototype and motion

- `alberti_get_prototype` lists flows and interactions, including those imported from Figma.
- `alberti_add_interaction` wires a hotspot layer to a screen: `trigger` (`click`, `hover`, `press`, `drag`, `timeout`, `key`, `mouse-enter`, `mouse-leave`), `action` (navigate, overlay, swap, scroll, back, close, url) and a `transition` (`instant`, `dissolve`, `smart`, `move-in`, `move-out`, `push`, `slide-in`, `slide-out`). Smart animate (`smart`) matches layers by name between screens, so name the moving layers the same on both.
- `alberti_set_flow` names a starting point and links it to a brief scenario.
- `alberti_set_motion` gives a layer an entrance motion (`fade-in`, `slide-up`, `slide-left`, `pop`, `grow`, `pulse`, `blink`, `spin`, `float`, `shake`, `needle`). It plays in the prototype, the HTML export and Unity.
- Run `alberti_preflight` before handing off: it lists targets and text below the context's minimum, low contrast, dead-end screens, buttons without interactions, a missing start point and scenarios without a flow.

## 5. 3D and engine export

- `alberti_insert_3d_model` places a GLB, glTF, OBJ or FBX from a path on this computer (or a URL) as a 3D layer. The user sets camera and lighting in the Design panel.
- `alberti_export_engine` exports the document, or only `node_ids`, on the Alberti server: `unity` with `ui_system` `ui-toolkit` (default) or `ugui` (world-space, XR and legacy projects), `unreal`, or `omniverse`. It returns a fidelity `report` and a time-limited `download_url` for the zip. Download it with your own tools into the user's project if they ask, and summarise anything the report says did not come across.

## 6. Files

`alberti_list_server_files` lists the server files the user can open, with share links; `alberti_open_server_file` opens one in a new tab. `list_documents` lists the open tabs; pass `document_id` (and `page_id`) to any tool to work on a tab other than the active one.

## Etiquette

- Say what you are about to change on the user's canvas before larger edits; they may be watching.
- Prefer a few meaningful tool calls over hundreds of tiny ones; use `batch_update` and `render`.
- Never delete screens or layers the user did not ask you to remove. Undo is per call, so a long series of edits is many undo steps.

See `references/tools.md` for the full tool list.
