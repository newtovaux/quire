# Quire

Markdown notes kept as files you own.

Quire is a single HTML file. Point it at a folder on your disk and it becomes a notes app: subfolders are libraries, `.md` files are notes. There is no database, no account, no sync service and no build step. Everything it writes is a plain Markdown file that git, Obsidian, `grep` and your backups can all read.

```
notes/                 <- the folder you choose
├── Work/              <- a library
│   ├── Weekly review.md
│   └── Team meeting notes.md
├── Reading/
│   └── Why we sleep.md
└── Ideas/
    └── Weekend project.md
```

## Requirements

- A Chromium browser on the desktop: Chrome, Edge, Brave or Opera. Quire uses the [File System Access API](https://developer.mozilla.org/en-US/docs/Web/API/File_System_Access_API) to read and write your files, which Firefox and Safari do not implement. The app says so plainly rather than half working.
- Serving over `http`. Opening the file directly with `file://` gives the page an opaque origin, and the folder picker refuses to run there.

## Running it

Clone the repository, then serve the directory with whatever you have to hand:

```bash
php -S localhost:8000          # PHP
python3 -m http.server 8000    # Python
npx serve .                    # Node
```

Open <http://localhost:8000/quire-notes.html>, click **Open notes folder** and choose where your notes should live. An empty folder is fine, and Quire will never touch anything outside it.

Stick to one address. Permissions are tied to the origin, so notes opened from `localhost:8000` are remembered separately from `127.0.0.1:8000`.

## What it does

**Files, not records.** Libraries are folders and notes are files. Rename a file outside the app and Quire picks it up on the next refresh. Delete the app and your notes are still there.

**Saves itself.** Notes save a moment after you stop typing, and again when you switch note, hide the tab or press Ctrl+S. The status bar shows whether the current note is saved, saving, or has unsaved changes.

**Reads and writes side by side.** Read, Split and Write modes, with the panes scroll-linked in Split. Prose is set in a serif for reading, the editor is monospaced.

**A writing toolbar.** Headings, bold, italic, strikethrough, inline code, links, bulleted, numbered and task lists, quotes, code blocks, tables and rules. The buttons toggle rather than insert blindly, so clicking H2 on a heading that is already H2 removes it, and clicking bold inside `**bold**` unwraps it. Everything goes through the browser's own undo stack.

**Live checkboxes.** Tick a task in the reading pane and `- [ ]` becomes `- [x]` in the file, with your caret and scroll position left where they were.

**Jump anywhere.** Ctrl+K opens a palette that fuzzy-matches note titles across every library, so `wr` finds "Weekly review". If nothing matches by title it searches the text of your notes and shows the line it found. The library search box is separate and stays scoped to the list you are looking at.

**Moves that cannot eat your work.** The File System Access API has no move operation, so moving, copying and renaming write the new file, read it back, compare it against the original, and only then delete anything. Moves offer an Undo in the status bar. Drag a note onto a library to move it, hold Alt to copy, or use the Move button.

**Remembers where you were.** The folder handle lives in IndexedDB, along with the last note you had open and whether you had the rails collapsed. Browsers ask permission once per session before handing a folder back, so a remembered folder shows a one click **Reopen** rather than opening on its own.

Press `?` in the app for the same list with every key on it.

## Keyboard shortcuts

| Key | What it does |
| --- | --- |
| `Ctrl+K` | Jump to any note in any library, by title or by text |
| `/` | Filter the notes in the current library |
| `Ctrl+N` | New note |
| `Ctrl+S` | Save now |
| `Ctrl+B` / `Ctrl+I` | Bold, italic |
| `Ctrl+Shift+K` | Link |
| `Tab` / `Shift+Tab` | Indent, outdent |
| `Ctrl+Z` | Undo, including toolbar changes and ticked boxes |
| `Ctrl+E` | Cycle Read, Split and Write |
| `Ctrl+\` | Hide or show the libraries and notes lists |
| `?` | Help |

Mac users get the same with ⌘, and the help panel prints it that way.

## Markdown supported

Headings, paragraphs, nested ordered and unordered lists, task lists, tables, fenced and inline code, blockquotes, horizontal rules, links, bare URLs, images, bold, italic and strikethrough. The renderer is about a hundred lines and deliberately small, so it is CommonMark-ish rather than CommonMark. Footnotes, definition lists and inline HTML are not supported.

## Privacy

Nothing is uploaded, because there is nowhere to upload it to. There are no network requests at all: no fonts, no CDN, no analytics. The only thing stored in the browser is the folder handle and a little layout state in IndexedDB, and the **Forget folder** button clears it.

## Known limits

- Chromium only, for the reasons above.
- Libraries are one level deep. Folders nested inside a library are ignored.
- The jump palette reads the text of up to 800 notes in the background. Beyond that it still matches on titles.
- Relative image paths in a note do not resolve, because the page is served from `localhost` rather than from your notes folder.
- Two copies of Quire open on the same folder will not notice each other's writes. Use Refresh.

## Contributing

It is one file with no dependencies and no build. Open `quire-notes.html`, edit it, reload. The script is divided into commented sections: names and paths, remembering the folder, libraries, notes, moving notes, help, jump palette, saving, rendering, Markdown, editor behaviour, and the chrome around it.

Bug reports are welcome, particularly anything that touches the write, verify, delete paths.

## Licence

MIT. See [LICENCE](LICENCE).
