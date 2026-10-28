[README.md](https://github.com/user-attachments/files/34093362/README.md)
# yipcad

Copyright © 2026. All rights reserved except for the permissions below.

A standalone browser editor for building bhop and surf courses, decorating maps, and testing movement locally. Written in native JavaScript, HTML, CSS and WebGL, with a local server using Node.js built-in modules. No npm dependencies or Three.js are required.

**Status: working local editor and approximate movement preview. A Windows launcher and limited CS:Source export/compilation prototype are available; online compilation is not implemented.** Saving a `.bhop.json` project works now; this is an editable project file, not a playable Counter-Strike map.

## Download and open the app

**Developed by sornin.**

The Windows app is supplied inside **bhop-builder-desktop.zip**, available from this repository or its release downloads. The ZIP contains the app and its supporting files.

1. Download **bhop-builder-desktop.zip**.
2. Right-click the downloaded ZIP and choose **Extract All**.
3. Open the extracted folder and double-click **BhopBuilder.exe**.
4. Click **Open map editor** in the launcher.

Keep all extracted files and folders together. Do not run the EXE from inside the ZIP or move it away from its supporting files. Microsoft Edge and Node.js must be installed; they are not bundled in the ZIP. CS:Source BSP compilation also requires the matching game and compiler tools. This is a prototype, and actual BSP compilation has not yet been verified in-game.

## Planned hosting

Website-based compilation is planned around a self-hosted build worker to avoid the ongoing cost of a dedicated cloud compilation server.

The intended experience is to build a map in the browser, choose a supported game, click Compile, and download the finished map without installing map-making tools yourself. A separate build worker would run the required tools in the background. This service does not exist yet, and there is no announced release date.

Counter-Strike: Source is the first desktop compilation target. CS2 requires a separate Source 2 pipeline. Longer-term plans include investigating the other Counter-Strike versions and three standalone browser-game targets. The browser-game targets are to be confirmed. Those additional targets are ambitions, not claims of current compatibility. Each needs its own conversion, testing and toolchain work.

The website is planned to run on Cloudflare, with compilation handled by a separate local build computer.

## What works today

| Feature | Status |
| --- | --- |
| 3D building, move/rotate/resize and numeric properties | Available |
| Selection boxes, group copying, undo/redo | Available |
| Alignment, even gaps, row/grid copies, wall/bridge drawing | Available |
| Editable straight and curved surf ramps | Available |
| Graffiti painting on assets and ramps | Available |
| Skybox, grid/ground toggles and procedural materials | Available |
| 117 building-piece and scenery types | Available |
| Browser drafts and local `.bhop.json` save/open | Available |
| Local first-person bhop/surf movement preview | Available; approximate physics |
| CS:Source desktop export | Prototype: simple convex brushes; requires installed tools; not yet verified in-game |
| Online compilation, accounts and build queue | Planned; not implemented |
| Browser-game integrations | Planned; formats and target games to be confirmed |
| Multiplayer, bhop timer, checkpoints and SSJ route gates | Not implemented |



## Optional: run it locally

Local setup is only needed if you prefer to run your own copy or work on the code. If you are using the hosted Pages editor, skip this section.

Use a recent Node.js installation. Development checks have run with Node 24. A desktop browser with WebGL is required for the 3D view.

1. Download and extract this repository, or clone it with Git.
2. Open a terminal in the folder containing `server.mjs`.
3. Run:

```sh
node server.mjs
```

4. Open `http://127.0.0.1:4173/`.
5. Keep the terminal running while using the editor. Press Ctrl+C to stop the server.

On Windows, `Start-Editor.cmd` is an alternative launcher. It looks for an existing Node runtime; it does not install software. There is no `npm install` step.

To open the large example course, visit:

```text
http://127.0.0.1:4173/?course=terminal-five
```

For another port in PowerShell:

```powershell
$env:BHOP_PORT = '4174'
node server.mjs
```

The supplied server binds only to `127.0.0.1`. It is a local preview server, not the future public compilation backend.

## Controls and workflow

| Action | Control |
| --- | --- |
| Place an asset | Choose it from the library, then click the grid |
| Finish placement | Escape |
| Orbit / pan / zoom | Right-drag / middle-drag / mouse wheel |
| Select / move / rotate / resize | Q / W / E / R |
| Shape a selected surf ramp | T |
| Paint a selected asset | B |
| Frame selection | F |
| Undo / redo | Ctrl+Z / Ctrl+Shift+Z |
| Copy / paste / duplicate | Ctrl+C / Ctrl+V / Ctrl+D |
| Select all visible pieces | Ctrl+A |
| Delete selected pieces | Delete |
| Move by a grid step | Arrow keys; Alt+Arrow for vertical movement |
| Save project | Ctrl+S or Save |

Drag a selection box in Select mode. Shift-click or Shift-drag extends the selection. Copy/paste uses an internal editor clipboard, which resets when the page reloads. Group copies retain relative placement; numeric rotation and resize editing are for a single selected piece.

Use the Properties panel for precise dimensions and transforms. Press Enter or leave a field to apply it. Build tools provide alignment, even gaps, ground placement, repeated arrays and two-click walls/bridges. Grid and Ground are independent editing helpers; they are not exported objects or playtest collision surfaces.

### Surf ramps and paint

Surf ramps support width, length, slope, bend, thickness and high-edge side. The bend is a circular arc, limited to avoid folding the geometry into itself; this is not a general-purpose Blender-style vertex editor.

Select one asset, press B and drag to paint. Brush colour and size are configurable. Paint follows the object when moved, resized or reshaped and is retained in project saves. Ctrl+Z undoes a stroke. Painting is decorative and does not alter collision.

### Asset library

The library includes basic structures, industrial course pieces, rocks and trees, street furniture, graveyard props, Viking/Egyptian/Moon/Space themes, a convenience-store facade and furnishings, a furnished Egyptian temple, and a 20-piece railway kit.

Railway assets include a commuter train, locomotive, passenger carriage, freight wagon, track sections, platforms, canopy, footbridge, station entrance, shelter, signals, ticket machine, signs, bench and clock. These are stationary scenery/building assets.

Use category filters and search, or Preview assets to inspect pieces before placing them. Jump pads, boost ramps and checkpoint gates are visual assets: they do not yet activate gameplay effects. Pulsing edge materials are surface effects, not lights illuminating the environment.

### Terminal Five sample map

The bundled course contains 533 objects spanning all 117 asset types, including 32 railway vehicles. Five routes share common concourses: Local, Yard, Express, Skyline and Nightline. Nominal gaps increase from approximately 93 to 563 units.

SSJ means Speed of Sixth Jump. Route gaps are provisional design values, not verified SSJ thresholds. Movement must be tested in the destination game before describing a route as playable at a particular speed.

Most surrounding scenery has collision disabled. Enable an object's Collision setting if needed. The example course has its own browser draft, separate from the ordinary editor. Your personal changes in browser storage are not automatically part of this repository or the bundled sample.

## Local playtesting

Click Playtest, set the feet position and facing, or use Pick spawn on map. Start from a clear, solid surface with headroom. The saved spawn is shown by a green marker.

WASD moves, Space automatically jumps on landing, R restarts and Escape pauses. Click to play requests mouse capture. If capture is unavailable, Play with drag-look allows left-drag camera movement. Back to editor restores the editor view.

The movement model includes air acceleration, gravity, momentum-preserving autojump and sliding against steep surfaces. Collision uses actual visible, collision-enabled mesh triangles, including curved ramps. It is an approximation, not a verified recreation of CS2, CS:GO, CS:Source or other games' physics.

There is no automatic stair stepping, crouching, timer, checkpoint system, boost-pad logic, multiplayer or SSJ gating. The editor's ground/grid helpers have no collision. Falling sufficiently below the lowest solid asset resets the player.

## Saving and project format

Save downloads an editable `.bhop.json` file. Open validates and reads a chosen file locally. Browser autosave is convenient, but clearing browser data can remove drafts; save downloaded backups regularly. Drafts are tied to the browser origin, so localhost, 127.0.0.1 and a future public domain have separate storage.

Projects use `format: "bhop-builder"`, version 2. Version 1 projects are also accepted. Objects record identifiers, type, name, transforms, colour, material, visibility and collision. Surf shape, painting and playtest configuration are preserved where present.

Coordinates use a Source-style scale with X/Y horizontal and Z up. Positions are object centres, sizes are full dimensions, and rotations are degrees. Using these units does not itself establish engine compatibility.

Limits include 8 MB per project, 2,000 objects and bounded paint data. Imports reject unexpected fields, unsafe prototype properties, duplicate IDs and invalid numbers. Complex valid maps can still consume substantial memory or slow the browser.

## Export and compilation roadmap

The planned compiler is an application/service around the game tools, not an independent replacement for Valve's compilers.

1. Implement and verify a minimal CS2 export: room, platforms, a surf ramp, collision, materials and player spawn.
2. Compile it automatically on a local build worker and test it in CS2.
3. Expand export coverage to complex meshes, props, paint and environment settings, reporting unsupported features clearly.
4. Add an authenticated job API, queue, progress/error reporting and temporary downloads.
5. Connect the website to a restricted local build worker. The worker would retrieve jobs through outbound connections; the public website should not expose a personal PC's file system or command shell.
6. Add further game targets after their pipelines pass real in-game tests.

Intended architecture:

```text
Browser editor on Cloudflare
          |
          v
Build API and queue
          |
          v
Isolated build worker on a self-hosted machine
          |
          v
Temporary finished-map storage -> browser download
```

A local worker would only compile while that PC is available. Queue limits and availability would be visible to users. Public jobs require isolation, fixed compiler arguments, input validation, resource limits, authentication/rate limits and cleanup. These are future implementation requirements, not protections already supplied by a working service.

Export conversion and native compilation are separate steps. Exporting supported source files can potentially happen in the browser; generating playable native game maps needs the appropriate build tools. Website visitors would not need those tools installed when using the planned hosted worker. Playing a map still requires access to its target game.

### Intended game scope

| Target | Engine family | Roadmap |
| --- | --- | --- |
| Counter-Strike 2 | Source 2 | First export/compilation target |
| Counter-Strike: Global Offensive | Source 1, game-specific branch | Later investigation |
| Counter-Strike: Source | Source 1 | Later investigation |
| Counter-Strike 1.6 and Condition Zero | GoldSrc | Separate possible expansion, not Source support |
| Browser-game target 1 | Name and map format to be confirmed | Planned integration; no compatible export yet |
| Two additional standalone browser-game targets | Names to be confirmed | Separate adapters or games; not implemented |

The long-term ambition is broad Counter-Strike support. This table does not promise every historical version, console release, spin-off or engine branch. CS2 is not compatible with Source 1 output, and GoldSrc needs another pipeline. Browser games also need their own supported map formats and gameplay behaviour. No private game source code or format documentation is assumed available.

Engine references: [Source](https://developer.valvesoftware.com/wiki/Source), [Source 2](https://developer.valvesoftware.com/wiki/Source_2), [GoldSrc](https://developer.valvesoftware.com/w/index.php?redirect=no&title=GoldSrc).

## Why local build hosting first? Google Cloud cost examples

**These are illustrative infrastructure budgets, not a quote for a finished product.** Pricing references were checked on 21 October 2026. All amounts are USD, before tax. Australian billing/exchange rates and region selection can change the total. Development labour, buying a PC, game/tool entitlements and ongoing maintenance are not included.

There is no fixed price for “all Counter-Strike games plus three browser games.” Cost depends on build hours, map complexity, concurrent users, stored assets, downloads and whether the browser games need live multiplayer servers. Supporting another format does not automatically require another always-on machine: compatible jobs can share a queue and worker.

### A transparent example build-machine calculation

Google's published accelerator pricing table lists a `g2-standard-8` example with one L4 GPU, 8 vCPUs and 32 GiB RAM at approximately **$0.853624312/hour** in its displayed default pricing selection. This includes the machine's GPU, CPU and RAM; do not add another GPU charge. Check the selected region in the calculator before ordering. [Google accelerator-optimized pricing](https://cloud.google.com/products/compute/pricing/accelerator-optimized)

Google lists Windows Server licensing for most VM types at **$0.046 per visible vCPU-hour**. For an 8-vCPU example this adds $0.368/hour. [Google VM and Windows pricing](https://cloud.google.com/products/compute/pricing/general-purpose)

```text
Illustrative GPU build worker:
$0.853624312 machine + (8 x $0.046 Windows) = $1.221624312/hour
50 powered-on hours  = about $61
200 powered-on hours = about $244
730 powered-on hours = about $892
```

This is a price model, not a tested CS2 hosting configuration. Windows image support, GPU/graphics-driver licensing, ray-tracing compatibility, tool availability, unattended operation and region capacity must be verified. Any additional graphics licensing is not included above. A different compatible machine may cost more or less. Powered-on time includes idle time, startup, tool updates, failed builds and retries.

### Example budgets for the broader service

The following scenarios assume one shared GPU build pool and a separate CPU build pool for older-engine jobs. The **$0.50/hour CPU worker rate is a planning assumption**, including its assumed OS cost, not a verified Google SKU price. The three browser games are assumed to run gameplay in visitors' browsers, with no always-on multiplayer simulation servers.

| Scenario | GPU worker hours x $1.2216 | CPU worker hours x assumed $0.50 | Other-services allowance | Approximate total/month |
| --- | ---: | ---: | ---: | ---: |
| Small capped pilot | 50 = $61 | 50 = $25 | $50 | **$136** |
| Moderate queued usage | 200 = $244 | 200 = $100 | $100 | **$444** |
| One GPU + one CPU worker always on | 730 = $892 | 730 = $365 | $150 | **$1,407** |
| Two GPU + two CPU workers always on | 1,460 = $1,784 | 1,460 = $730 | $350 | **$2,864** |

Other-services allowances are estimates for persistent disks, API/queue/database, static game delivery, storage, backups, logs and download traffic. They are not quoted bundles or spending caps. If all components were hosted on Google, actual charges for these services would need to be added from the calculator. The intended Cloudflare frontend would change that part of the calculation.

For scale, Google's displayed Iowa Standard object-storage rate is about **$0.02/GiB-month**, or roughly $10/month for 500 GiB of objects before requests and transfer. That is object storage, not a VM's attached disk. Downloads and operations are separately priced and can dominate a popular service. [Google Cloud Storage pricing](https://cloud.google.com/storage/pricing)

These examples do not establish how many maps can be compiled per month. We have not benchmarked a working exporter/compiler yet. A hypothetical 10-minute build on the example GPU worker uses about $0.20 of compute; a 60-minute build uses about $1.22, excluding other costs. Neither duration is a measured map-build time.

If any of the three browser games require multiplayer servers, persistent worlds, accounts or significant backend processing, their costs must be estimated separately once their names, design and expected player counts are known. They are not silently included as unlimited services in the table.

A $10 monthly website budget may be enough for a low-traffic static frontend, but it is not a realistic budget for an always-on rented native compilation service. Local PC hosting avoids renting the worker, although electricity, internet, storage, hardware wear and website services still cost money.

Use the [Google Cloud Pricing Calculator](https://cloud.google.com/products/calculator) with the actual region, OS, machine, powered-on hours, disk size, download traffic and backend design. Budgets/alerts should be paired with application-enforced job limits; an estimate is not a bill cap.

## Security, privacy and limitations

The current editor has no external analytics or uploads in its browser code. Project imports are read locally, and drafts are stored in browser local storage. The local server serves only `dist`, checks Host headers, restricts resolved paths, accepts GET/HEAD and provides a restrictive Content Security Policy. The launcher does not download software.

The online compilation service is still in development planning and is not available in this version.

Save backups before major edits. Very detailed maps can exhaust browser/GPU resources. The current editor does not guarantee compatibility with any external game's map loader or movement settings.

## Development

Run the automated tests with:

```sh
node --test tests/*.test.mjs
```

The suite covers project validation, geometry, surf editing, paint limits, history, scene updates, collision/physics and local-server restrictions.

See [the detailed editor reference](docs/editor-reference.md) for implementation and asset notes.

## Credits

sornin — Developer.

## Ownership and contributions

This is an independent project, not an official Valve, Counter-Strike or 7-Eleven product. Referenced product names and branding belong to their respective owners. The repository does not include Valve's game binaries or compilation tools.

This project is source available. You may use, study, copy, modify and share this software free of charge, provided you retain this copyright notice and these terms. You may not sell the software or modified versions of it without written permission from the copyright holder. These permissions apply only to material the copyright holder has the right to license; third-party material remains subject to its own terms.

Bug reports are useful: include reproducible steps, browser/version, the relevant error and a minimal non-private example project where possible. Do not post account credentials, private map drafts or cloud keys in issues.










