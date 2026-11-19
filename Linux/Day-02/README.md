
<div id="top"></div>

<p align="center">
  <a href="https://github.com/mammaddrik/RootBound">
    <img src="https://i.postimg.cc/mrDNbCQ8/Root-Bound-Day-02.png" alt="RootBound-Day-02">
  </a>
</p>

# Day 02 — Text Editors

## Table of Contents

- [Day 02 — Text Editors](#day-02--text-editors)
  - [Table of Contents](#table-of-contents)
  - [Text Editors](#text-editors)
    - [Nano](#nano)
      - [Introduction to Nano](#introduction-to-nano)
      - [History of Nano](#history-of-nano)
      - [Installation and Version](#installation-and-version)
      - [Understanding the Nano Interface](#understanding-the-nano-interface)
      - [Opening and Creating Files](#opening-and-creating-files)
      - [Editing Text](#editing-text)
      - [Saving and Exiting](#saving-and-exiting)
      - [Searching and Replacing Text](#searching-and-replacing-text)
      - [Copying, Cutting, and Pasting](#copying-cutting-and-pasting)
      - [Nano Configuration and Appearance](#nano-configuration-and-appearance)
      - [Nano Keyboard Shortcuts](#nano-keyboard-shortcuts)
      - [Practical Example](#practical-example)
    - [Vim](#vim)

---

## Text Editors

### Nano

#### Introduction to Nano

**GNU Nano** is a lightweight, terminal-based text editor designed to make editing text files simple and accessible. It is commonly used in Linux systems for creating, viewing, and modifying configuration files, scripts, and documents.

Unlike more advanced editors such as Vim, Nano does not require users to learn different editing modes. Users can start typing immediately after opening a file, making it particularly suitable for beginners.

Nano provides several useful features, including:

- Creating and editing text files.
- Searching and replacing text.
- Copying, cutting, and pasting text.
- Undoing and redoing changes.
- Syntax highlighting.
- Automatic indentation.
- Line numbering.
- Configurable keyboard shortcuts and appearance.

#### History of Nano

Nano was originally developed as a free replacement for the Pico text editor, which was part of the Pine email software developed at the University of Washington.

The project began in late 1999 under the name **TIP (TIP Isn't Pico)**. On January 10, 2000, it was renamed Nano because another Unix utility already used the name `tip`.

In February 2001, Nano became an official GNU project, and its first production release was published on March 22, 2001.

Since then, Nano has continued to evolve while maintaining its original goal of providing a simple and user-friendly terminal text editor.

#### Installation and Version

Nano is preinstalled on many Linux distributions. However, if it is not available, it can be installed using the distribution's package manager.

**Debian / Ubuntu:**

```bash
sudo apt update
sudo apt install nano
```

**Fedora:**

```bash
sudo dnf install nano
```

**Arch Linux:**

```bash
sudo pacman -S nano
```

To check whether Nano is installed:

```bash
nano --version
```

This command displays the installed Nano version and build information.

#### Understanding the Nano Interface

When Nano starts, it displays a simple text-editing interface inside the terminal.

The default interface consists of four main areas:

1. **Title Bar:** Displays the Nano version, current filename, and modification status.
2. **Editing Area:** The main workspace where users enter and modify text.
3. **Status Bar:** Displays messages, prompts, search results, and other information.
4. **Shortcut Bar:** Displays commonly used keyboard shortcuts at the bottom of the screen.

The shortcut bar uses special symbols to represent key combinations.

For example:

- `^` represents the `Ctrl` key.
- `M-` represents the `Meta` key, usually accessed with `Alt`.

Therefore:

- `^X` means `Ctrl + X`.
- `M-U` means `Alt + U`.

**Important:** The exact appearance of Nano may vary depending on its version, terminal settings, and configuration.

#### Opening and Creating Files

Nano can open an existing file or create a new one directly from the terminal.

**Syntax:**

```bash
nano [OPTIONS] [FILE]
```

**Example — Open an Existing File:**

```bash
nano notes.txt
```

If `notes.txt` exists, Nano opens it for editing.

**Example — Create a New File:**

```bash
nano newfile.txt
```

If the specified file does not exist, Nano opens a new editing buffer. The file is created when the changes are saved.

**Open a File Using an Absolute Path:**

```bash
nano /home/user/Documents/notes.txt
```

**Open a File at a Specific Line:**

```bash
nano +10 notes.txt
```

This opens `notes.txt` with the cursor positioned at line 10.

#### Editing Text

One of Nano's main advantages is its modeless editing behavior.

Unlike Vim, Nano does not require users to switch between different editing modes to enter or modify text.

After opening a file, users can immediately:

- Type new text.
- Delete characters using `Backspace` or `Delete`.
- Move the cursor using the arrow keys.
- Create new lines by pressing `Enter`.
- Select, copy, cut, and paste text using keyboard shortcuts.

All changes are initially made in the editing buffer. They are not permanently written to the file until the user saves them.

#### Saving and Exiting

Saving and exiting are among the most important operations in Nano.

**Save a File:**

Press:

```text
Ctrl + O
```

Nano displays a filename prompt at the bottom of the screen.

Press `Enter` to confirm the filename and save the changes.

**Exit Nano:**

Press:

```text
Ctrl + X
```

If there are unsaved changes, Nano asks whether to save them before exiting.

- Press `Y` to save changes.
- Press `N` to discard changes.
- Press `Ctrl + C` to cancel the exit operation and return to editing.

#### Searching and Replacing Text

Nano provides built-in search and replacement functionality.

**Search for Text:**

Press:

```text
Ctrl + F
```

Enter the desired search term and press `Enter`.

Nano searches forward from the current cursor position.

**Search Backward:**

Press:

```text
Ctrl + B
```

Enter the search term to search backward.

**Find the Next Occurrence:**

Press:

```text
Alt + F
```

**Find the Previous Occurrence:**

Press:

```text
Alt + B
```

**Replace Text:**

Press:

```text
Ctrl + \
```

Enter the text you want to find, then enter its replacement.

Nano allows you to review each match and decide whether to replace it.

**Note:** Search shortcuts differ between Nano versions. The shortcuts above follow the default bindings introduced in Nano 8.0 and later.

#### Copying, Cutting, and Pasting

Nano provides a built-in cut buffer that allows users to move or duplicate text.

**Cut a Line:**

```text
Ctrl + K
```

Cuts the current line or selected text.

**Paste Text:**

```text
Ctrl + U
```

Inserts the contents of Nano's cut buffer at the cursor position.

**Select Text:**

```text
Ctrl + 6
```

Sets or clears the selection mark. Use the arrow keys to extend the selection.

**Copy Selected Text:**

```text
Alt + 6
```

Copies the selected text into Nano's cut buffer without removing it from its original location.

#### Nano Configuration and Appearance

Nano supports customization through configuration files, allowing users to adjust its behavior and appearance.

The main user configuration file is:

```text
~/.nanorc
```

System-wide configuration is commonly stored in:

```text
/etc/nanorc
```

Users can configure options such as:

- Syntax highlighting.
- Line numbers.
- Automatic indentation.
- Tab width.
- Mouse support.
- Cursor position display.
- Backup file creation.

**Example Configuration:**

Add the following settings to `~/.nanorc`:

```bash
set linenumbers
set autoindent
set tabsize 4
set mouse
set constantshow
```

These settings enable line numbers, automatic indentation, four-character tab width, mouse support, and a continuously displayed cursor position.

**Syntax Highlighting:**

Nano can highlight programming language syntax using syntax definition files.

Depending on the Linux distribution, syntax definitions may be installed under directories such as `/usr/share/nano/`.

The available syntax definitions and colors depend on the installed Nano version and configuration.

#### Nano Keyboard Shortcuts

Keyboard shortcuts are one of the most important parts of working efficiently with Nano.

The following table summarizes commonly used default shortcuts.

**File Operations**

| Shortcut | Description |
|---|---|
| `Ctrl + O` | Save the current file. |
| `Ctrl + X` | Exit Nano. |
| `Ctrl + R` | Insert another file into the current buffer. |
| `Ctrl + S` | Save the current file on supported terminals and configurations. |

**Text Editing**

| Shortcut | Description |
|---|---|
| `Ctrl + K` | Cut the current line or selected text. |
| `Ctrl + U` | Paste text from the cut buffer. |
| `Ctrl + 6` | Set or clear the selection mark. |
| `Alt + 6` | Copy selected text. |
| `Backspace` | Delete the character before the cursor. |
| `Delete` | Delete the character under the cursor. |
| `Alt + U` | Undo the last action. |
| `Alt + E` | Redo the last undone action. |

**Navigation**

| Shortcut | Description |
|---|---|
| `Ctrl + A` | Move to the beginning of the current line. |
| `Ctrl + E` | Move to the end of the current line. |
| `Ctrl + Y` | Move one page up. |
| `Ctrl + V` | Move one page down. |
| `Ctrl + _` | Go to a specific line and column. |
| `Ctrl + C` | Display the current cursor position. |
| `Alt + \` | Move to the beginning of the file. |
| `Alt + /` | Move to the end of the file. |

**Search and Replace**

| Shortcut | Description |
|---|---|
| `Ctrl + F` | Search forward. |
| `Ctrl + B` | Search backward. |
| `Alt + F` | Find the next occurrence forward. |
| `Alt + B` | Find the previous occurrence backward. |
| `Ctrl + \` | Search and replace text. |

**Help and Other Operations**

| Shortcut | Description |
|---|---|
| `Ctrl + G` | Open the built-in help menu. |
| `Ctrl + T` | Open the tool menu when available, such as spell checking. |
| `Ctrl + L` | Refresh the terminal display. |

**Important Notes:**

- `Ctrl` shortcuts are represented by `^` in Nano.
- `Alt` shortcuts are represented by `M-`.
- Some shortcuts may behave differently depending on the Nano version, terminal, or configuration.
- If an `Alt` shortcut does not work, pressing `Esc` followed by the corresponding key may work as an alternative.
- Press `Ctrl + G` inside Nano to view the built-in help and check the available shortcuts.

#### Practical Example

Let's create a simple text file using Nano and practice some of the commands introduced above.

**Step 1 — Create a File:**

```bash
nano linux-notes.txt
```

**Step 2 — Enter Some Text:**

```text
Linux is an open-source operating system.

Nano is a simple terminal-based text editor.

Learning Linux commands is an important step
toward understanding system administration.
```

**Step 3 — Save the File:**

Press `Ctrl + O`, then press `Enter` to confirm the filename.

**Step 4 — Search for a Word:**

Press `Ctrl + F`, enter `Linux`, and press `Enter`.

**Step 5 — Exit Nano:**

Press `Ctrl + X`.

**Step 6 — Verify the File:**

Back in the terminal, execute:

```bash
cat linux-notes.txt
```

The contents of the file should be displayed in the terminal.

---

**Additional Resources:**

- [GNU Nano Official Website](https://nano-editor.org/)
- [GNU Nano Manual](https://www.nano-editor.org/dist/latest/nano.html)
- [GNU Nano FAQ](https://nano-editor.org/faq.html)
- [GNU Nano Keyboard Shortcuts](https://nano-editor.org/cheatsheet.html)

### Vim

---

<p align="right">
  <a href="../Day-01/README.md">← Day 01</a>
  &nbsp; | &nbsp;
  <a href="../Day-03/">Day 03 →</a>
  &nbsp; | &nbsp;
  <a href="#top">Back to top ↑</a>
</p>
