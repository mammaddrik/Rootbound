
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
      - [Introduction to Vim](#introduction-to-vim)
      - [History of Vim](#history-of-vim)
      - [Installation and Version](#installation-and-version-1)
      - [Understanding the Vim Interface](#understanding-the-vim-interface)
      - [Vim Modes](#vim-modes)
      - [Opening and Creating Files](#opening-and-creating-files-1)
      - [Basic Navigation](#basic-navigation)
      - [Editing Text](#editing-text-1)
      - [Copying, Cutting, and Pasting](#copying-cutting-and-pasting-1)
      - [Searching and Replacing Text](#searching-and-replacing-text-1)
      - [Saving and Exiting](#saving-and-exiting-1)
      - [Vim Configuration and Appearance](#vim-configuration-and-appearance)
      - [Vim Keyboard Shortcuts](#vim-keyboard-shortcuts)
      - [Practical Example](#practical-example-1)

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

#### Introduction to Vim

**Vim (Vi Improved)** is a powerful, open-source, terminal-based text editor designed for efficiently creating and modifying text files.

Vim is an enhanced version of the original `vi` editor and provides many additional features, including syntax highlighting, multiple editing modes, macros, plugins, split windows, and advanced text manipulation.

Unlike Nano, Vim uses a **modal editing system**. This means that different keys perform different actions depending on the current mode.

Vim is commonly used by Linux administrators, developers, and system engineers to edit configuration files, write scripts, and manage text directly from the terminal.

**Key Features:**

- Multiple editing modes.
- Advanced text navigation and manipulation.
- Syntax highlighting for programming languages.
- Search and replace functionality.
- Undo and redo operations.
- Split windows and multiple buffers.
- Macros and customizable key mappings.
- Plugin support and extensive customization.

#### History of Vim

Vim was originally developed by **Bram Moolenaar** and first released in 1991 for the Amiga computer.

The name Vim stands for **Vi Improved**, reflecting its goal of extending the functionality of the traditional Unix `vi` editor.

Over time, Vim gained popularity among developers and system administrators because of its powerful editing capabilities, portability, and extensive customization options.

Vim is still widely used in Linux and Unix-like environments and has inspired several other text editors.

#### Installation and Version

Vim is available in the package repositories of most Linux distributions.

**Debian / Ubuntu:**

```bash
sudo apt update
sudo apt install vim
```

**Fedora:**

```bash
sudo dnf install vim
```

**Arch Linux:**

```bash
sudo pacman -S vim
```

To check whether Vim is installed:

```bash
vim --version
```

This command displays the installed Vim version and its available features.

#### Understanding the Vim Interface

When Vim starts, it opens a terminal-based editing environment.

Its interface generally consists of the following areas:

1. **Editing Area:** The main workspace where text is displayed and modified.
2. **Status Line:** Displays information such as the filename, current position, and editing status, depending on the configuration.
3. **Command Line:** Located at the bottom of the screen and used to enter commands, search patterns, and other instructions.
4. **Mode Indicator:** Shows the current editing mode, such as `-- INSERT --`, when applicable.

Unlike Nano, Vim does not display all available keyboard shortcuts on the screen. Users are expected to learn its commands and editing modes.

#### Vim Modes

One of the most important concepts in Vim is its modal editing system.

Each mode provides a different way to interact with text.

**1. Normal Mode**

Normal Mode is the default mode when Vim starts.

It is mainly used for navigating through text, deleting lines, copying content, and executing editing commands.

Examples:

```text
h    Move left
j    Move down
k    Move up
l    Move right
```

Press `Esc` to return to Normal Mode from other editing modes.

**2. Insert Mode**

Insert Mode allows users to type and modify text directly.

To enter Insert Mode, use one of the following commands in Normal Mode:

| Key | Description |
|---|---|
| `i` | Insert text before the cursor. |
| `a` | Insert text after the cursor. |
| `I` | Insert text at the beginning of the line. |
| `A` | Insert text at the end of the line. |
| `o` | Create a new line below the current line and enter Insert Mode. |
| `O` | Create a new line above the current line and enter Insert Mode. |

Press `Esc` to return to Normal Mode.

**3. Visual Mode**

Visual Mode allows users to select portions of text before performing operations such as copying, deleting, or formatting.

| Key | Description |
|---|---|
| `v` | Select text character by character. |
| `V` | Select entire lines. |
| `Ctrl + V` | Select a rectangular block of text. |

After selecting text, you can use commands such as `y` to copy or `d` to delete the selection.

Press `Esc` to leave Visual Mode.

**4. Command-Line Mode**

Command-Line Mode allows users to execute commands by typing them at the bottom of the editor.

Press `:` in Normal Mode to enter this mode.

Examples:

```vim
:w
:q
:wq
```

These commands are used to save files, exit Vim, and save changes before exiting.

Press `Esc` to cancel the current command and return to Normal Mode.

**5. Replace Mode**

Replace Mode allows users to overwrite existing characters instead of inserting new text.

Press:

```text
R
```

in Normal Mode to enter Replace Mode.

Type the replacement text, then press `Esc` to return to Normal Mode.

**Mode Summary:**

| Mode | Purpose | How to Enter |
|---|---|---|
| Normal | Navigation and editing commands. | `Esc` |
| Insert | Enter and modify text. | `i`, `a`, `o`, etc. |
| Visual | Select text. | `v`, `V`, `Ctrl + V` |
| Command-Line | Execute editor commands. | `:` |
| Replace | Overwrite existing text. | `R` |

#### Opening and Creating Files

Vim can open existing files or create new ones directly from the terminal.

**Syntax:**

```bash
vim [OPTIONS] [FILE]
```

**Open an Existing File:**

```bash
vim notes.txt
```

If the file exists, Vim opens it for editing.

**Create a New File:**

```bash
vim newfile.txt
```

If the file does not exist, Vim creates a new editing buffer. The file is written to disk when you save it.

**Open a File Using an Absolute Path:**

```bash
vim /home/user/Documents/notes.txt
```

**Open a File at a Specific Line:**

```bash
vim +10 notes.txt
```

This opens the file with the cursor positioned at line 10.

#### Basic Navigation

Vim provides several keyboard commands for moving through files without using the mouse.

**Character Navigation:**

| Key | Description |
|---|---|
| `h` | Move one character left. |
| `j` | Move one line down. |
| `k` | Move one line up. |
| `l` | Move one character right. |

**Word Navigation:**

| Key | Description |
|---|---|
| `w` | Move to the beginning of the next word. |
| `b` | Move to the beginning of the previous word. |
| `e` | Move to the end of the current or next word. |

**Line Navigation:**

| Key | Description |
|---|---|
| `0` | Move to the beginning of the line. |
| `^` | Move to the first non-blank character of the line. |
| `$` | Move to the end of the line. |
| `gg` | Move to the beginning of the file. |
| `G` | Move to the end of the file. |
| `10G` | Move to line 10. |

**Screen Navigation:**

| Key | Description |
|---|---|
| `Ctrl + F` | Move one page forward. |
| `Ctrl + B` | Move one page backward. |
| `Ctrl + D` | Move half a page down. |
| `Ctrl + U` | Move half a page up. |

#### Editing Text

Vim provides powerful commands for modifying text while in Normal Mode.

**Deleting Text:**

| Command | Description |
|---|---|
| `x` | Delete the character under the cursor. |
| `X` | Delete the character before the cursor. |
| `dd` | Delete the current line. |
| `dw` | Delete from the cursor to the beginning of the next word. |
| `d$` | Delete from the cursor to the end of the line. |
| `D` | Delete from the cursor to the end of the line. |

**Changing Text:**

| Command | Description |
|---|---|
| `r` | Replace the character under the cursor. |
| `R` | Enter Replace Mode. |
| `cw` | Change text from the cursor to the end of the current word. |
| `cc` | Change the entire current line. |
| `C` | Change from the cursor to the end of the line. |
| `s` | Replace the character under the cursor and enter Insert Mode. |

**Undo and Redo:**

| Command | Description |
|---|---|
| `u` | Undo the last change. |
| `Ctrl + R` | Redo the last undone change. |

#### Copying, Cutting, and Pasting

Vim uses the concept of **Yank** to copy text.

**Copy Text:**

| Command | Description |
|---|---|
| `yy` | Copy the current line. |
| `yw` | Copy a word. |
| `y$` | Copy from the cursor to the end of the line. |
| `yG` | Copy from the current line to the end of the file. |

**Cut Text:**

| Command | Description |
|---|---|
| `dd` | Cut the current line. |
| `dw` | Cut text from the cursor to the beginning of the next word. |
| `d$` | Cut from the cursor to the end of the line. |

**Paste Text:**

| Command | Description |
|---|---|
| `p` | Paste after the cursor or below the current line. |
| `P` | Paste before the cursor or above the current line. |

**Note:** Vim stores copied and deleted text in registers. The default unnamed register is used by these basic commands. Access to the operating system clipboard depends on Vim's build and configuration.

#### Searching and Replacing Text

Vim provides built-in commands for searching and replacing text.

**Search Forward:**

```vim
/Linux
```

This searches forward for the word `Linux`.

**Search Backward:**

```vim
?Linux
```

This searches backward for the word `Linux`.

**Navigate Search Results:**

| Key | Description |
|---|---|
| `n` | Move to the next matching result. |
| `N` | Move to the previous matching result. |

**Replace the First Occurrence in the Current Line:**

```vim
:s/old/new
```

**Replace All Occurrences in the Current Line:**

```vim
:s/old/new/g
```

**Replace All Occurrences in the Entire File:**

```vim
:%s/old/new/g
```

**Replace All Occurrences with Confirmation:**

```vim
:%s/old/new/gc
```

The `c` flag asks for confirmation before each replacement.

#### Saving and Exiting

Saving and exiting are performed through Command-Line Mode.

First, press `Esc` to return to Normal Mode.

Then enter one of the following commands:

| Command | Description |
|---|---|
| `:w` | Save the current file. |
| `:q` | Exit Vim if there are no unsaved changes. |
| `:wq` | Save changes and exit. |
| `:x` | Save changes if necessary and exit. |
| `:q!` | Exit without saving changes. |
| `:w filename` | Save the current buffer using a specified filename. |
| `:e filename` | Open another file in the current editing window. |

**Important:** If a file contains unsaved changes, Vim prevents normal exit using `:q` and displays an error message.

Use `:q!` only when you intentionally want to discard unsaved changes.

#### Vim Configuration and Appearance

Vim supports extensive customization through its configuration file.

The main user configuration file is:

```text
~/.vimrc
```

This file contains settings that Vim loads when it starts.

Users can customize features such as:

- Line numbers.
- Syntax highlighting.
- Automatic indentation.
- Tab width.
- Search behavior.
- Display options.
- Keyboard mappings.

**Example Configuration:**

Add the following settings to `~/.vimrc`:

```vim
set number
set relativenumber
syntax on
set autoindent
set tabstop=4
set shiftwidth=4
set expandtab
set hlsearch
set ignorecase
set smartcase
```

**Configuration Explanation:**

| Setting | Description |
|---|---|
| `set number` | Displays absolute line numbers. |
| `set relativenumber` | Displays relative line numbers. |
| `syntax on` | Enables syntax highlighting. |
| `set autoindent` | Automatically preserves indentation. |
| `set tabstop=4` | Sets the displayed width of a tab to four spaces. |
| `set shiftwidth=4` | Sets the indentation width for commands such as `>>` and `<<`. |
| `set expandtab` | Inserts spaces instead of tab characters when using the Tab key. |
| `set hlsearch` | Highlights search matches. |
| `set ignorecase` | Makes searches case-insensitive by default. |
| `set smartcase` | Makes searches case-sensitive when the search pattern contains uppercase letters. |

To open the configuration file:

```bash
vim ~/.vimrc
```

After making changes, save the file and restart Vim or reload the configuration:

```vim
:source ~/.vimrc
```

#### Vim Keyboard Shortcuts

The following tables summarize commonly used Vim commands.

**Modes and Basic Operations**

| Shortcut | Description |
|---|---|
| `Esc` | Return to Normal Mode. |
| `i` | Enter Insert Mode before the cursor. |
| `a` | Enter Insert Mode after the cursor. |
| `o` | Open a new line below the current line. |
| `O` | Open a new line above the current line. |
| `v` | Enter Visual Mode. |
| `V` | Enter Visual Line Mode. |
| `Ctrl + V` | Enter Visual Block Mode. |
| `:` | Enter Command-Line Mode. |
| `R` | Enter Replace Mode. |

**Navigation**

| Shortcut | Description |
|---|---|
| `h` | Move left. |
| `j` | Move down. |
| `k` | Move up. |
| `l` | Move right. |
| `w` | Move to the next word. |
| `b` | Move to the previous word. |
| `e` | Move to the end of a word. |
| `0` | Move to the beginning of the line. |
| `^` | Move to the first non-blank character. |
| `$` | Move to the end of the line. |
| `gg` | Move to the beginning of the file. |
| `G` | Move to the end of the file. |
| `10G` | Move to line 10. |

**Editing**

| Shortcut | Description |
|---|---|
| `x` | Delete the character under the cursor. |
| `dd` | Delete the current line. |
| `dw` | Delete a word. |
| `D` | Delete to the end of the line. |
| `r` | Replace one character. |
| `cw` | Change a word. |
| `cc` | Change the current line. |
| `u` | Undo the last change. |
| `Ctrl + R` | Redo the last undone change. |

**Copying and Pasting**

| Shortcut | Description |
|---|---|
| `yy` | Copy the current line. |
| `yw` | Copy a word. |
| `y$` | Copy to the end of the line. |
| `dd` | Cut the current line. |
| `dw` | Cut a word. |
| `p` | Paste after the cursor or below the line. |
| `P` | Paste before the cursor or above the line. |

**Searching and Replacing**

| Shortcut | Description |
|---|---|
| `/pattern` | Search forward. |
| `?pattern` | Search backward. |
| `n` | Find the next occurrence. |
| `N` | Find the previous occurrence. |
| `:%s/old/new/g` | Replace all occurrences in the file. |
| `:%s/old/new/gc` | Replace all occurrences with confirmation. |

**Saving and Exiting**

| Command | Description |
|---|---|
| `:w` | Save the current file. |
| `:q` | Exit Vim. |
| `:wq` | Save and exit. |
| `:x` | Save if necessary and exit. |
| `:q!` | Exit without saving. |
| `:help` | Open Vim's built-in help system. |

**Important Notes:**

- Most editing commands in these tables are executed in Normal Mode.
- Press `Esc` before entering Normal Mode commands if you are currently inserting text.
- Vim commands are case-sensitive. For example, `p` and `P` perform different operations.
- Some commands can be combined with numbers. For example, `5dd` deletes five lines.
- Press `:help` inside Vim to access its built-in documentation.

#### Practical Example

Let's create and edit a text file using Vim.

**Step 1 — Create a File:**

```bash
vim linux-notes.txt
```

**Step 2 — Enter Insert Mode:**

Press:

```text
i
```

**Step 3 — Enter Some Text:**

```text
Linux is an open-source operating system.

Vim is a powerful terminal-based text editor.

Learning Linux commands is an important step
toward understanding system administration.
```

**Step 4 — Return to Normal Mode:**

Press:

```text
Esc
```

**Step 5 — Navigate Through the Text:**

Use the arrow keys or navigation commands such as:

```text
gg
G
w
b
```

**Step 6 — Save the File:**

Enter:

```vim
:w
```

**Step 7 — Exit Vim:**

Enter:

```vim
:q
```

**Step 8 — Verify the File:**

Back in the terminal, execute:

```bash
cat linux-notes.txt
```

The contents of the file should be displayed in the terminal.

---

**Additional Resources:**

- [Official Vim Website](https://www.vim.org/)
- [Vim Documentation](https://www.vim.org/docs.php)
- [Vim Online Help](https://vimhelp.org/)
- [Vim Tutorial](https://vimdoc.sourceforge.net/htmldoc/usr_toc.html)

---

<p align="right">
  <a href="../Day-01/README.md">← Day 01</a>
  &nbsp; | &nbsp;
  <a href="../Day-03/">Day 03 →</a>
  &nbsp; | &nbsp;
  <a href="#top">Back to top ↑</a>
</p>
