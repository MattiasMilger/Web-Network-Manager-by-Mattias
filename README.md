# Web Network Manager by Mattias

A browser-based inventory for the hosts and servers in a home or lab network, built with vanilla HTML, CSS, and JavaScript. Document each device, track recurring tasks such as updates and reboots, and jump straight to its web interfaces. Runs entirely client-side - no server or backend required. Designed for GitHub Pages.

## Try It Out

The live version is available at: **https://mattiasmilger.github.io/Web-Network-Manager-by-Mattias/**

### Run Locally

Open `index.html` in a modern browser. No build tools or dependencies required.

## Features

- **Hosts** - Add and edit hosts with a name, symbol, role, group, hardware, OS / platform, OS language and tags. Everything is free text; "Used before" buttons under each field let you reuse earlier values with one click.
- **Symbols** - Pick one of about 60 emoji from the palette or type your own.
- **Addresses & networks** - Give a host one address row per network (for example home LAN and VPN). Each row can hold an IP address, a DNS name, or both. When two or more networks exist, a **network filter** appears in the header (like the role filter): pick a network to show only hosts that have an address on it, and all links use that network's addresses.
- **Endpoints** - Define each web interface or service with a function (the button text), a scheme, a port and an optional path. Standard ports are filled in per scheme (http 80, https 443, ssh 22, rdp 3389, vnc 5900, smb 445, ftp 21) and can be changed. Scheme **url** opens any full custom URL.
- **DNS and IP fallback** - When the active network has both a DNS name and an IP, each endpoint gets a main button using the DNS name plus a **via IP** button for when DNS is down.
- **SSH** - SSH endpoints copy a ready command such as `ssh -p 2222 user@host`. **Launcher .bat** downloads a small Windows file that opens a terminal and connects.
- **Schedules** - Recurring tasks per host, such as updates, reboots or backups. Each schedule has a name, a type (manual or automatic), a repeat (every N days, or on specific days of the month such as the 1st and 2nd), a start date and a time window / note. Manual schedules turn orange when due within 7 days and red when overdue; **Mark ... done** records the date and counts the next due date from today. Automatic schedules just show your note. The app never runs anything itself.
- **Network map** - A separate **Map** view draws the connections between hosts as simple lines with arrows and labels, for example a router pointing to the DNS servers it uses. Connections are added inside the map and hosts are arranged automatically.
- **Log** - Record reboots, shutdowns, incidents, updates and notes with a date and time, per host.
- **Notes & commands** - Keep as many titled entries per host as you like, for anything you want to store and copy: a Docker compose YAML, useful commands, location, serial number or warranty. Every entry is a separate collapsible section on the host card with a **Copy** button, so only the ones you open are shown. Reorder entries in the Edit form by dragging the ⠿ handle (mouse, touch or pen), or with the arrow keys.
- **Groups** - Group hosts, collapse and expand groups, and rename them.
- **Drag and drop ordering** - Drag a host to reorder it or move it to another group, and drag a group header to reorder groups. On touch screens, press and hold first. **Reset order** sorts everything A-Z.
- **Search & filters** - Live search with highlighting, a role filter, a network filter and clickable tag filters.
- **Safety nets** - Warning when an address is already used on the same network, and a warning before a form with unsaved changes is closed by accident.
- **Data Config** - Export a JSON backup, import it again, or export a readable Markdown inventory. Reset everything with a typed `RESET` confirmation.
- **Keyboard shortcuts** - `N` adds a host, `/` focuses search, `Ctrl+Enter` saves, `Esc` closes dialogs.
- **Dark / Light Theme** - Toggle with the ◐ button (dark by default, choice is remembered).
- **Help & Info** - Built-in help dialog describing every feature.
- **Privacy** - Everything stays in your browser. Nothing is uploaded and no host is contacted.

## Project Structure
Web Network Manager by Mattias/
├── index.html              # Structure, styling, and application logic (single file)
└── README.md               # This file

### Module Responsibilities

| Module | Purpose |
|---|---|
| `index.html` | UI layout, CSS design tokens and theming, host form, endpoint and link building, update tracking, log, drag-and-drop ordering, search and filters, message banners, Help & Info, JSON / Markdown export and import |

## How It Works

1. Click **+ Add Host**, enter a name and at least one IP address or DNS name.
2. Add endpoints for the host's web interfaces and services. Pick a scheme and the standard port is filled in.
3. Optionally group the host, add tags, enable update tracking and save commands or notes.
4. Click a service button on the host card to open it. Use **via IP** if DNS is unavailable.
5. Pick a network in the **network filter** (for example when you are connected over VPN) to show only hosts on that network and use its addresses.
6. Click **Mark ... done** on a manual schedule (updates, reboots, backups) after you have done the task, and use **Log** to record reboots and incidents.
7. Back up regularly from **Data Config**.

## Technical Notes

- **No external dependencies** - pure vanilla HTML, CSS, and JavaScript.
- **Client-side only** - No data is transmitted to any server. Hosts are never pinged or probed.
- **Storage** - Data is saved in `localStorage` under the keys `netmanager.hosts` (hosts), `netmanager.layout` (group order, collapsed groups, active network), `netmanager.links` (map connections), `netmanager.schema` (data format version), `netmanager.backup` (copy of the previous data, written once when an upgrade changes the stored format) and `netmanager.theme`. It is stored **unencrypted** and `localStorage` is shared by every app on the same origin, so do not store passwords. Use a password manager and note only where the password lives.
- **Data model** - Each host stores its addresses per network, endpoints, tags, schedules, log entries, and a list of titled notes and commands (up to 40 entries, 20,000 characters per entry). All loaded and imported data is validated and normalised before use.
- **Compatibility** - Every save records a schema version. When a newer release changes the stored format, the old data is upgraded automatically and the previous copy is kept in `netmanager.backup`. JSON backups from older releases still import. If saved data comes from a *newer* release than the page you opened, the app shows it read-only and refuses to save, so nothing is overwritten.
- **Input handling** - Imported data is sanitised: addresses, ports and SSH usernames are validated before they are used to build links or the `.bat` launcher.
- **Markdown export** - Includes notes and commands (under their titles, as code blocks) and logs.
- **Pointer events** - Drag and drop supports mouse, touch (long-press) and pen input.
- **Design** - Follows the shared design guidelines: dark-first with a light alternate theme, flat panels, floating message banners, no tooltips and no native browser dialogs.

### Changing the data format

1. Bump `SCHEMA` in `index.html`.
2. Add `MIGRATIONS[<previous version>]`, a function that turns the stored data of the previous version into the new shape.
3. Keep `sanitize()` tolerant of missing fields, since it runs on all loaded and imported data.
4. Test with data saved by the previous release.

## Browser Support

Works in all modern browsers (Chrome, Firefox, Edge, Safari).

## Credits

**Developer**: Mattias Milger
**Email**: mattias.r.milger@gmail.com
**GitHub**: [MattiasMilger](https://github.com/MattiasMilger)

## More Projects

Check out more of my work at [mattiasmilger.github.io](https://mattiasmilger.github.io/)
