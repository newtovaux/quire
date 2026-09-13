# Quire

Quire is a local-first Markdown notes app that keeps your notes as plain files on disk. It is a single-page browser app with no backend, no account, and no sync service. Your notes live in folders and files you control, so they remain readable in Obsidian, Git, grep, or any plain-text editor.

## Overview

- Open a folder on your machine and Quire treats each subfolder as a library.
- Each Markdown file becomes a note.
- Notes are edited in the browser but saved back to their original `.md` files.
- The app stores only lightweight browser-side state such as the folder handle and UI preferences.

This means Quire works like a fast local notes workspace without locking your data into a proprietary database.

## Project structure

```text
.
├── README.md
├── quire.html
└── LICENSE
```

## Requirements

- A desktop Chromium browser such as Chrome, Edge, Brave, or Opera.
- The app uses the File System Access API to read and write files directly from disk.
- It must be served over HTTP. Opening the app via `file://` is not supported.

## Running Quire

From the project directory, serve it with any local static server:

```bash
cd /path/to/quire
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000/quire.html
```

Click Open notes folder and choose the directory where your notes should live. An empty folder is fine.

> Use the same origin consistently. Permissions are tied to the site origin, so `localhost:8000` and `127.0.0.1:8000` are treated separately.

## Features

- Read, split, and write modes for editing notes
- Markdown rendering for a clean reading view
- Toolbar actions for headings, emphasis, links, lists, quotes, code blocks, tables, and rules
- Keyboard shortcuts for navigation and editing
- Library and note search
- Jump-to-note palette across all libraries
- Safe file operations with verification before delete/replace steps
- Auto-save behavior with explicit save actions
- Local persistence of folder access and last-used note state in IndexedDB

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| Ctrl/Cmd + K | Open note jump palette |
| / | Filter notes in the current library |
| Ctrl/Cmd + N | New note |
| Ctrl/Cmd + S | Save now |
| Ctrl/Cmd + B / I | Bold / italic |
| Ctrl/Cmd + Shift + K | Insert link |
| Tab / Shift + Tab | Indent / outdent |
| Ctrl/Cmd + Z | Undo |
| Ctrl/Cmd + E | Cycle view modes |
| Ctrl + \ | Hide/show libraries and notes pane |
| ? | Show help |

## Markdown support

Quire supports a focused subset of Markdown intended for writing and reading notes efficiently, including:

- headings
- paragraphs
- ordered and unordered lists
- task lists
- tables
- fenced and inline code
- blockquotes
- horizontal rules
- links and bare URLs
- images
- bold, italic, and strikethrough

It is intentionally lightweight rather than a full CommonMark implementation.

## Privacy and data handling

Quire does not upload your notes anywhere. There are no remote requests for analytics, fonts, or CDN assets. The app only stores local browser state needed to remember the notes folder and interface layout.

## Known limitations

- Chromium desktop browsers only
- Library nesting is limited to a single level
- The jump palette indexes the text of a limited number of notes in the background
- Relative image paths are not resolved when the app is served from `localhost`
- Multiple Quire windows opened on the same folder may not see each other's writes immediately

## Contributing

Quire is intentionally a single-file app with no build step. To work on it:

1. Start a local static server.
2. Open `http://localhost:8000/quire.html` in a Chromium browser.
3. Edit the HTML file and reload the page to test changes.

Bug reports and small improvements are welcome.

## License

MIT. See [LICENSE](LICENSE).
