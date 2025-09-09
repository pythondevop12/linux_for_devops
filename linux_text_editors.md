# 📝 Text Editors in Linux: nano, vim, gedit

## 1. Nano
Nano is a **beginner-friendly text editor** commonly available on most Linux distributions.

### Features:
- Simple and easy to use.
- Command shortcuts displayed at the bottom.
- Supports basic operations like search, replace, cut, copy, paste.

### Common Commands:
- `nano filename` → Open or create a file.
- `Ctrl + O` → Save changes.
- `Ctrl + X` → Exit nano.
- `Ctrl + W` → Search text.

---

## 2. Vim
Vim (**Vi Improved**) is a **powerful and advanced text editor** used widely by developers and system administrators.

### Features:
- Modal editor (Normal, Insert, Visual, Command modes).
- Highly customizable with plugins and configurations.
- Efficient for editing large files and programming.

### Modes in Vim:
- **Normal Mode** → Default mode (navigation and commands).
- **Insert Mode** → For writing text (`i` to enter).
- **Visual Mode** → For selecting text.
- **Command Mode** → For executing commands (`:` prefix).

### Common Commands:
- `vim filename` → Open a file.
- `i` → Enter insert mode.
- `:w` → Save file.
- `:q` → Quit.
- `:wq` → Save and quit.
- `dd` → Delete a line.
- `yy` → Copy a line.
- `p` → Paste.

---

## 3. Gedit
Gedit is a **GUI-based text editor** (default for GNOME desktop environment).

### Features:
- User-friendly graphical interface.
- Supports syntax highlighting for many languages.
- Tabs for opening multiple files.
- Plugins available for extra functionality.

### Usage:
- Open via terminal: `gedit filename`
- Supports drag-and-drop file opening.
- Useful for editing scripts, configuration files, and programming.

---

## 🔑 Summary
| Editor | Type | Difficulty | Best For |
|--------|------|------------|----------|
| **Nano** | CLI | Easy | Beginners, quick edits |
| **Vim** | CLI | Hard (steep learning curve) | Advanced users, programmers, sysadmins |
| **Gedit** | GUI | Easy | Desktop users, programming with GUI support |
