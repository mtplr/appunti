Some useful shortcuts.

# Notes

* `SB` = spacebar
* `BS` = backspace
* `^` = Ctrl
* `⌥`= Alt

# MacOS

Backtick: `⌥ 9`

Tilde: `⌥ 5`

**DEL = `Fn BS`**

Spotlight: `⌘ SB`

Screenshot: `⌘ ⇧ 4` (`5` full, `SB` window)

Open a file: `⌘ O`

New file: `⌘ ⌥ N` (see [the script here](https://apple.stackexchange.com/questions/129699/create-a-new-txt-file-in-finder-keyboard-shortcut))

New folder: `⌘ ⇧ N`

Inspect/change extension: `⌘ I`

Redo: `⌘ ⇧ Z`

Copy path: RightClick on the file + `⌥`

Cut:`⌘ C` Paste:`⌘ ⇧ V`

Go to end (begin) of a line: `⌘ →`

Preferences: `⌘,`

In Preview, to switch from single-double page etc: `⌘1, ⌘2 ...`

Close session: `^⌘Q`

# VSCode

Run without debugging: `^ ⌥`

Comment/uncomment block: `⌘ ⇧ 7`

Delete a line: `⌘ ⇧ K`

Copy a line up (down): `⌥ ⇧ ↑`

**Bookmarks** shortcut: `⌥⌘K` insert; `⌥⌘L` avanti

Search & Replace: `⌘ ⌥ F`

Insert multiple cursors: `⌥`+ click

Go to line: `^G`

## Snippet keys

### Latex

Shortcuts:

`$...$` cmd+shift+1

`\ce{...}` cmd+shift+2

Example for `\ce{}` in `keybindings.json`:

```json
    {
        "key": "cmd+shift+1",
        "command": "editor.action.insertSnippet",
        "when": "editorTextFocus",
        "args": {
            "snippet": "\\ce{$TM_SELECTED_TEXT}"            
        }
    },
```

Snippet:

Fraction `frac`, Figure `fig`, Equation `eq`, Table1 `t1`, itemize `itemize`, `bold`, `italic`

### Python

Quick plot with matplotlib `quick-plot`
