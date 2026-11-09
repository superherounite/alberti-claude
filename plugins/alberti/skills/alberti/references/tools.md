# Alberti MCP tools

Generated from the app's catalog (137 tools). Every tool also takes optional `document_id` and `page_id` to work on a tab or page other than the active one.

## Alberti's own

| Tool | Kind | What it does |
|---|---|---|
| `alberti_status` | read | Start here. The signed-in Alberti user and server, the active document (pages, current page, selection, top-level screens), whether it is a server file (and its share link), and how complete its design intent brief is. |
| `alberti_get_intent` | read | Read the design intent: the brief (where the design is used, the problem, who uses it and in what situation, scenarios, success criteria, constraints, non-goals), the decisions recorded on this page's screens (chosen / alternative / rejected, with reasons) and the 'why' notes on layers. Read this before designing and design to it. |
| `alberti_set_intent` | edit | Write or update the design intent brief. Only the fields you pass change. `context` sets the size and accessibility rules preflight checks against: hmi (vehicle displays), xr, twin (digital twin / control room), web, mobile, other. Scenarios and success criteria are lists of short sentences. One undo step. |
| `alberti_set_decision` | edit | Record a design decision on a screen (top-level frame): chosen, alternative or rejected, with the reason. Pass status null to clear it. Use it when you explore several directions so the team sees which one won and why. |
| `alberti_set_why` | edit | Note why a layer exists (the reason a button, a field or a warning is there), so developers and QA know. An empty text removes the note. |
| `alberti_make_scenario_screens` | edit | Make one empty screen per brief scenario on the current page, sized for the brief's context and named 'S1 · <scenario>', side by side. Existing S-numbered screens are kept. Returns the new screen IDs to design into. |
| `alberti_preflight` | read | Check the page against the brief's context: touch target and text sizes, contrast, screens the prototype enters but cannot leave, buttons with no interaction, missing start point, scenarios with no flow, default layer names. Run it before handing off. |
| `alberti_get_prototype` | read | Read the page's click prototype: its flows (start screens, linked scenarios) and every interaction (hotspot layer → target screen, trigger, action, transition), including ones imported from Figma. |
| `alberti_add_interaction` | edit | Add a prototype interaction to a layer (the hotspot): on a trigger, do an action. navigate/overlay/swap go to `target_id` (a top-level frame), scroll goes to a layer in the same screen, back and close need no target, url opens `url`. `replace: true` replaces the layer's interactions instead of adding. Smart animate matches layers by name between the two screens. |
| `alberti_set_flow` | edit | Add or update a prototype flow (a named starting point) on the current page. Link it to a brief scenario by passing the scenario's text. Pass remove: true to delete the flow with that name. |
| `alberti_set_motion` | edit | Give a layer an entrance motion that plays in the prototype, the HTML export and the Unity export. Presets: fade-in, slide-up, slide-left, pop, grow, pulse, blink, spin, float, shake, needle. Pass preset null to remove it. |
| `alberti_insert_3d_model` | edit | Insert a 3D model as a 3D layer on the current page (a frame that renders the model; camera and lighting are set in the Design panel). GLB, glTF (single file), OBJ or FBX, from a path on this computer or a URL. With a server file the model is uploaded and converted for engine export. Optional position and size in page coordinates. |
| `alberti_list_server_files` | read | List the Alberti server files the signed-in user can open, most recently edited first, with their workspace, project and share link. |
| `alberti_open_server_file` | edit | Open a server file in a new tab of Alberti (it becomes the active document once it arrives, usually within seconds). Then call list_documents or alberti_status. |
| `alberti_export_engine` | edit | Export the document (or only the given screens) for a game engine on the Alberti server: Unity with UI Toolkit or uGUI, Unreal, or Omniverse. Works on server files. Returns the job report (how faithfully each layer came across) and a download URL for the zip, valid for a limited time. Can take a minute. |

## Editor

| Tool | Kind | What it does |
|---|---|---|
| `list_documents` | read | List the documents (tabs) open in Alberti with their IDs, names, pages and current page. |
| `get_selection` | read | Get the selected nodes: the place to start when the user points at layers. Returns each selected node with its direct children by default; children past the depth are counted as childCount. Use get_node or describe for deeper branches. |
| `get_node` | read | Get detailed properties of a node by ID. Use depth to limit child recursion (0 = node only, 1 = direct children, etc). Default: unlimited. |
| `find_nodes` | read | Find nodes by name pattern and/or type. |
| `get_jsx` | read | Get JSX representation of a node and its children. Compact round-trip format — same syntax as the render tool. |
| `render` | edit | Render JSX to design nodes. Supports inline SVG paths, including open stroked paths: <svg viewBox="0 0 24 24" size={24}><path d="M2 12 L22 12" stroke="#000" fill="none" /></svg>. Use replace_id to replace a placeholder while preserving its position. |
| `update_node` | edit | Update properties of an existing node: position, size, opacity, corner radius, visibility, text, font. |
| `set_layout` | edit | Set auto-layout (flexbox) on a frame. Direction, alignment, spacing, padding. |
| `set_layout_child` | edit | Configure auto-layout child: sizing (FIXED/HUG/FILL), grow, alignment, absolute positioning. |
| `set_radius` | edit | Set corner radius. Use individual corners for independent values. |
| `set_fill` | edit | Set fill on a node. Solid: color="#ff0000". Linear gradient: gradient="top-bottom" or "left-right" with color (start) and color_end (end). |
| `set_stroke` | edit | Set the stroke (border) of a node. |
| `set_text` | edit | Set text content of a text node. |
| `set_text_properties` | edit | Set text layout properties: alignment, auto-resize, text case, decoration, truncation. |
| `delete_node` | edit | Delete a node by ID. |
| `reparent_node` | edit | Move a node into a different parent. |
| `node_resize` | edit | Resize a node. |
| `batch_update` | edit | Execute multiple modifications in one call. Each operation is {id, props} where props can include: spacing, padding, padding_horizontal, padding_vertical, counter_align, sizing_horizontal, sizing_vertical, grow, name, visible, corner_radius, auto_resize (for text), direction. Runs all updates with one layout recompute. |
| `stock_photo` | edit | Search stock photos and apply to leaf image placeholders or closed area geometry. Pass a JSON array; each item is {id, query, index?, orientation?}. Containers with content, text, lines, and structural nodes are rejected. |
| `describe` | read | Semantic description of one or more nodes. Pass `id` for a single node, or `ids` for multiple nodes in one call. Omit depth for auto — adapts to subtree size (small block → deeper, large page → shallower). |
| `diff_create` | read | Patch that turns one node tree into another, as JSX attribute changes per node plus moved, added, and removed children. Children are matched by name. Apply it to the first tree with diff_apply. |
| `diff_jsx` | read | Structural diff between two nodes in JSX format. Shows added/removed children, changed props. |
| `diff_visual` | read | Pixel diff between two rendered nodes. Returns a PNG with changed pixels in red over a faded copy of the source, the changed pixel ratio, and the changed region in source-node coordinates. Use it to confirm an edit only touched the intended area. |
| `calc` | read | Arithmetic calculator. ALWAYS use instead of mental math. Pass one expression or a JSON array of expressions — all evaluated in one call. Supports: + - * / % ** ( ) min max floor ceil round abs sqrt pow. Examples: "844 - 56 - 96 - 82", '["1440 * 8 / 12", "(952 - 16) / 2", "floor(390 * 0.6)"]' |
| `viewport_zoom_to_fit` | read | Zoom viewport to fit specified nodes. |
| `get_page_tree` | read | Get the node tree of the current page. Returns lightweight hierarchy: id, type, name, size, and text for TEXT nodes. Use depth, root_id, or node_types to keep large pages small. Use get_node for full properties of a specific node. |
| `get_current_page` | read | Get the current page name and ID. |
| `list_pages` | read | List all pages in the document. |
| `select_nodes` | read | Select one or more nodes by ID. |
| `query_nodes` | read | Query nodes using XPath selectors. Returns id, name, and type, plus text for TEXT nodes. Node types are element names (FRAME, TEXT, RECTANGLE, ELLIPSE, etc.). Attributes: id, name, width, height, x, y, visible, opacity, cornerRadius, fontSize, fontFamily, fontWeight, layoutMode, itemSpacing, paddingTop/Right/Bottom/Left, strokeWeight, rotation, locked, blendMode, text, lineHeight, letterSpacing. Examples: //FRAME — all frames //FRAME[@width < 300] — frames narrower than 300px //COMPONENT[starts-with(@name, 'Button')] — components starting with "Button" //SECTION/FRAME — direct frame children of sections //SECTION//TEXT — all text nodes inside sections //*[@cornerRadius > 0] — any node with corner radius //TEXT[contains(@text, 'Hello')] — text nodes containing "Hello" //*[@id='1:2']//TEXT[not(ancestor-or-self::*[@visible='false'])] — visible text inside a frame, in layer order |
| `get_components` | read | List reusable components from the document and enabled component libraries, optionally filtered by name. |
| `list_libraries` | read | List available published component libraries. |
| `insert_library_component` | edit | Insert a reusable component from an enabled library by stable library and asset identity. |
| `switch_page` | read | Switch to a different page by name or ID. |
| `page_bounds` | read | Get bounding box of all objects on the current page. |
| `get_comments` | read | Read the comments people left on the canvas, oldest first: each thread with its text, author, page, the layer it is pinned to, its position, replies, and whether it is resolved. Open threads only unless includeResolved is set. Use it to find review feedback to act on, then reply_to_comment and resolve_comment. |
| `add_comment` | edit | Leave a comment on the canvas, as a person does with the Comment tool: pinned on a layer by ID, which it then follows, or at x, y on a page. Text may use Markdown: **bold**, *italic*, ~~strikethrough~~, [links](https://…), and - or 1. lists. |
| `reply_to_comment` | edit | Answer a comment thread by ID from get_comments, for example to say what you changed. A reply reopens a resolved thread. Text may use Markdown, as in add_comment. |
| `resolve_comment` | edit | Mark a comment thread by ID as resolved once its feedback is addressed, which hides it from the canvas and the open list; pass resolved: false to reopen it. |
| `get_font_status` | read | Report whether fonts used on the current page are faithfully available. Returns requested faces, their loaded source, active substitutions, why an installed face could not be loaded, and affected nodes. |
| `list_fonts` | read | List fonts used in the current page. |
| `list_available_fonts` | read | List font families the host can render (system fonts on desktop plus any bundled fonts). Use this to discover what fonts are available to set on a text node — distinct from list_fonts which only reports families currently used in the page. |
| `create_shape` | edit | Create a shape on the canvas. Use FRAME for containers/cards, RECTANGLE for solid blocks, ELLIPSE for circles, TEXT for labels, LINE for rules and dividers, STAR for starbursts and badges, POLYGON for triangles and regular polygons, and SECTION for page sections. Use create_vector with an SVG path for arbitrary shapes. |
| `search_icons` | read | Search Iconify for icons by keyword. Accepts multiple queries — all searched in parallel. Returns results keyed by query. |
| `insert_icon` | edit | Insert one or more vector icons onto the canvas. Pass a single name or multiple names to batch-insert into the same parent. If already cached by fetch_icons — instant, no network request. |
| `fetch_icons` | read | Pre-fetch icons from Iconify into cache. Batches by prefix (one HTTP request per set). Call this once with all needed icons, then use insert_icon to place them instantly. Popular sets: lucide (outline), mdi (filled), heroicons, tabler, solar, mingcute, ri (remix). |
| `combine_as_variants` | edit | Combine components sharing a parent into a component set (variant set). Components named as Figma names variants, "State=On, Size=Large", get those variant properties; names like "Button/Primary" derive a Variant property from the segments after the first slash. |
| `create_component` | edit | Convert a frame/group into a component. |
| `create_instance` | edit | Create an instance of a component. |
| `expose_instance_swap` | edit | Expose nested instances as an instance-swap slot on their component. |
| `create_slot` | edit | Make a frame inside a main component a slot, as Create slot does: instances can then hold their own content in it, and a behaviour can bind it as a part. Returns the slot name. |
| `get_behaviour` | read | Read the behaviour of a main component, component set, or variant by property and slot names, with the required values and parts still missing. Without an ID, list every behaviour kind with the values and parts it binds. |
| `set_behaviour` | edit | Make a main component or component set behave as a Reka UI control in preview and code, naming its own properties and slots: e.g. { kind: "switch", values: { value: "State" }, parts: { thumb: "Thumb" }, states: "Interaction" }. Variant on/off values are guessed from names like On and Off unless given as { property, on, off }. Pass null to remove it. Returns the behaviour and the required values and parts still missing; get_behaviour lists every kind with its values and parts. |
| `create_page` | edit | Create a new page. |
| `create_vector` | edit | Create a vector node from SVG path data or a VectorNetwork. |
| `create_slice` | edit | Create a slice (export region) on the canvas. |
| `import_svg` | edit | Import raw SVG markup onto the canvas as editable layers, as pasting it into Figma does: a frame (a group when the SVG has no width, height, or viewBox) with a group per <g>, a vector per shape, and a text layer per <text>, named by their ids, and a mask group around clipped content. Supports common SVG shapes, text with <tspan> styles, inherited presentation attributes, opacity, transforms, gradients, clip paths, and internal <use> references. |
| `set_effects` | edit | Set effects on a node (drop shadow, inner shadow, blur). Pass an array or a single effect. |
| `set_opacity` | edit | Set opacity of a node (0-1). |
| `set_font` | edit | Set font properties of a text node. |
| `set_visible` | edit | Set visibility of a node. |
| `set_constraints` | edit | Set resize constraints for a node within its parent. |
| `set_rotation` | edit | Set rotation angle of a node in degrees. |
| `set_minmax` | edit | Set min/max width and height constraints on a node. |
| `set_font_range` | edit | Set font properties for a text range. |
| `set_text_resize` | edit | Set text auto-resize mode. |
| `set_blend` | edit | Set blend mode of a node. |
| `set_locked` | edit | Set locked state of a node. |
| `set_stroke_align` | edit | Set stroke alignment of a node. |
| `set_image_fill` | edit | Set an image fill on a node from base64-encoded image data. |
| `clone_node` | edit | Clone (duplicate) a node. |
| `node_move` | edit | Move a node to new coordinates. |
| `rename_node` | edit | Rename a node in the layers panel. |
| `group_nodes` | edit | Group selected nodes. |
| `ungroup_node` | edit | Ungroup a group node. |
| `flatten_nodes` | edit | Flatten nodes into a single vector. |
| `node_to_component` | edit | Convert one or more frames/groups into components. |
| `node_bounds` | read | Get absolute bounding box of a node. |
| `node_ancestors` | read | Get the ancestor chain from a node to the page root. |
| `node_children` | read | Get direct children of a node. |
| `node_tree` | read | Get a node tree with types and hierarchy. |
| `node_bindings` | read | Get variable bindings for a node. |
| `node_replace_with` | edit | Replace a node with JSX content. |
| `arrange` | edit | Arrange top-level nodes on the canvas in a grid, row, or column layout. Useful after batch creation to tidy up overlapping frames. |
| `list_variables` | read | List all design variables (colors, numbers, strings, booleans). |
| `list_collections` | read | List all variable collections. |
| `get_variable` | read | Get a variable by ID. |
| `find_variables` | read | Find variables by name pattern. |
| `create_variable` | edit | Create a new variable in a collection. |
| `set_variable` | edit | Set the value of a variable for a specific mode. |
| `delete_variable` | edit | Delete a variable. |
| `bind_variable` | edit | Bind a variable to a node property. For fills/strokes color bindings use indexed format like "fills/0/color". |
| `unbind_variable` | edit | Remove a variable binding from a node property. |
| `get_collection` | read | Get a variable collection by ID. |
| `create_collection` | edit | Create a new variable collection. |
| `delete_collection` | edit | Delete a variable collection and all its variables. |
| `boolean_union` | edit | Union (combine) multiple nodes. |
| `boolean_subtract` | edit | Subtract the second node from the first. |
| `boolean_intersect` | edit | Intersect multiple nodes. |
| `boolean_exclude` | edit | Exclude (XOR) multiple nodes. |
| `path_get` | read | Get vector path data of a node. |
| `path_set` | edit | Set vector path data on a node. Provide a VectorNetwork JSON. |
| `path_scale` | edit | Scale vector path from center. |
| `path_flip` | edit | Flip vector path horizontally or vertically. |
| `path_move` | edit | Move all path points by an offset. |
| `viewport_get` | read | Get current viewport position and zoom level. |
| `viewport_set` | read | Set viewport position and zoom. |
| `export_svg` | read | Export nodes as SVG markup. Returns the SVG string. |
| `export_pdf` | read | Export nodes as a vector PDF document. Text remains selectable, paths stay sharp at any zoom. Returns base64-encoded PDF data. |
| `export_image` | read | Export nodes as a raster image (PNG, JPG, or WEBP). Returns base64-encoded image data. Use to visually verify designs. |
| `analyze_colors` | read | Analyze color palette usage across the current page. Shows frequency, variable bindings, and optionally clusters similar colors. |
| `analyze_typography` | read | Analyze typography usage across the current page. Shows font families, sizes, weights, and their frequencies. |
| `lint` | read | Check the design for accessibility and consistency issues: low text contrast, small touch targets and text, off-scale spacing and radius, colors that match a variable but are not bound, and structure hygiene. Findings are sorted by severity. A `fix` is safe to apply with lint_fix; `suggestions` change values and need judgment. |
| `lint_fix` | edit | Apply lint fixes: bind colors to the variable they match and round geometry to whole pixels. With suggestions: true, also apply each finding’s first suggestion (snap radius and spacing to the scale, raise text to the minimum size, convert groups to frames, delete hidden layers). Returns what was applied and what remains. |
| `analyze_spacing` | read | Analyze spacing values (gap, padding) across the current page. Checks grid compliance. |
| `analyze_clusters` | read | Find repeated design patterns (potential components). Groups nodes by structural signature — type, size, and child structure. |
| `analyze_overlaps` | read | Detect visual overlaps and layout overflows across the current page. Useful for finding content that covers footers, text that bleeds outside frames, and accidental sibling overlaps. |
| `diff_show` | read | Preview setting JSX attributes on a node, without changing it. Returns the patch, which diff_apply applies. |
| `diff_apply` | edit | Apply a patch from diff_create, diff_show, or diff_changes. Each node must still have the patch's old values unless force is set, and nothing changes unless every hunk applies. Use dryRun to check first. |
| `design_to_tokens` | read | Write the document variables as a stylesheet of CSS custom properties: defaults in :root, every other mode under its condition (a selector such as [data-theme="dark"] or an @media query), aliases as var() references. The tailwind format puts tokens with a Tailwind v4 namespace (--color-*, --spacing-*, --radius-*, --text-*, …) in @theme and adds a @custom-variant per mode. The dtcg format returns W3C design token files instead: one per collection mode, text and effect styles as typography and shadow tokens, and a resolver document; collection and type filters do not apply to it. Issues lists tokens or modes that could not be written. |
| `import_design_tokens` | edit | Import W3C design token (DTCG) files into the document as variables, and typography and shadow tokens as text and effect styles. Pass each file with its path: a file per mode grouped by folder (as Figma exports modes), a .resolver.json with the files it refers to, or a Tokens Studio file with $themes. Collections and modes with the same names as existing ones are updated, variables are matched by name, and nothing is deleted. Returns what was added, updated, and skipped and why. |
| `design_to_component_map` | read | Analyze the document and return a structured component decomposition: components (with variants, props, instance counts), screens, and a dependency overview. |
