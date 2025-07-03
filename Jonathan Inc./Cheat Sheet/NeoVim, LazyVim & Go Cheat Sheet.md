	 [## 🚀 Basic Vim Navigation

### Movement

- `h j k l` - Left, down, up, right
- `w` - Jump to next word
- `b` - Jump to previous word
- `0` - Beginning of line
- `$` - End of line
- `gg` - Go to top of file
- `G` - Go to bottom of file
- `Ctrl + u` - Page up
- `Ctrl + d` - Page down

### Editing

- `i` - Insert mode (before cursor)
- `a` - Insert mode (after cursor)
- `o` - New line below and insert
- `O` - New line above and insert
- `Esc` - Return to normal mode
- `x` - Delete character
- `dd` - Delete line
- `yy` - Copy line
- `p` - Paste
- `u` - Undo
- `Ctrl + r` - Redo

## 📁 File Management (LazyVim)

### File Operations

- `Space + f + f` - Find files (fuzzy search)
- `Space + f + r` - Recent files
- `Space + f + g` - Find files by grep/content
- `Space + e` - Toggle file explorer
- `Space + w` - Save file
- `Space + q` - Quit
- `Space + Tab` - Switch to last buffer

### File Explorer (when open)

- `Enter` - Open file/folder
- `a` - Create new file
- `d` - Delete file
- `r` - Rename file
- `q` - Close explorer

## 🔍 Search & Navigation

### Finding & Searching

- `/` - Search forward
- `?` - Search backward
- `n` - Next search result
- `N` - Previous search result
- `*` - Search for word under cursor
- `Space + /` - Search in current buffer
- `Space + s + g` - Live grep (search in all files)
- `Space + s + w` - Search current word in project

### Buffer Navigation

- `Space + b + b` - List buffers
- `Space + b + d` - Delete current buffer
- `[b` - Previous buffer
- `]b` - Next buffer

## 🐹 Go-Specific Features

### Language Server (LSP)

- `g + d` - Go to definition
- `g + D` - Go to declaration
- `g + r` - Go to references
- `g + i` - Go to implementation
- `K` - Show documentation/hover info
- `Space + c + a` - Code actions (add imports, extract function, etc.)
- `Space + c + r` - Rename symbol
- `Space + c + f` - Format file (gofmt)

### Go Development

- `Space + c + a` → "Add import" - Auto-add missing imports
- `Space + c + a` → "Remove unused imports" - Clean up imports
- `Space + c + a` → "Generate tests" - Create test functions
- `Space + c + a` → "Fill struct" - Auto-complete struct fields

### Testing & Debugging

- `Space + t + t` - Run tests in current file
- `Space + t + f` - Run all tests
- `Space + d + b` - Toggle breakpoint
- `Space + d + c` - Continue debugging
- `Space + d + s` - Step into
- `Space + d + o` - Step over

## 💻 General Coding

### Code Editing

- `Space + c + f` - Format file
- `Space + c + l` - Toggle line numbers
- `Space + c + d` - Show diagnostics
- `]d` - Next diagnostic/error
- `[d` - Previous diagnostic/error
- `Space + x + x` - Trouble (error list)

### Comments

- `g + c + c` - Toggle comment on current line
- `g + c` (visual mode) - Toggle comment on selection
- `g + c + o` - Add comment below
- `g + c + O` - Add comment above

### Multi-cursor & Selection

- `Ctrl + n` - Select next occurrence of word
- `Ctrl + x` - Skip current, select next
- `Alt + n` - Select all occurrences

## 🔧 Window & Tab Management

### Windows/Splits

- `Space + w + v` - Vertical split
- `Space + w + s` - Horizontal split
- `Space + w + h/j/k/l` - Navigate between windows
- `Space + w + q` - Close window
- `Space + w + =` - Equalize window sizes

### Tabs

- `Space + Tab + Tab` - New tab
- `Space + Tab + ]` - Next tab
- `Space + Tab + [` - Previous tab
- `Space + Tab + d` - Close tab

## 🎨 Appearance & UI

### Colorschemes

- `Space + u + C` - Change colorscheme
- `:colorscheme [name]` - Set specific colorscheme

### UI Toggles

- `Space + u + l` - Toggle line numbers
- `Space + u + w` - Toggle word wrap
- `Space + u + s` - Toggle spell check
- `Space + u + h` - Toggle search highlight

## 🔌 Plugin Management

### LazyVim

- `:Lazy` - Open plugin manager
- `:LazyExtras` - Browse and enable language extensions
- `:LazyHealth` - Check plugin health
- `I` (in Lazy) - Install plugins
- `U` (in Lazy) - Update plugins
- `X` (in Lazy) - Clean unused plugins

### Mason (LSP/Tools)

- `:Mason` - Open tool manager
- `i` - Install tool
- `u` - Update tool
- `X` - Uninstall tool

## 🐛 Debugging & Diagnostics

### Health Checks

- `:checkhealth` - Check Neovim health
- `:checkhealth lazy` - Check LazyVim
- `:checkhealth vim.lsp` - Check LSP status

### Logs & Info

- `:messages` - Show recent messages
- `:LspInfo` - LSP status
- `:GoInstallBinaries` - Install Go tools (if available)

## 💡 Pro Tips

### Efficiency Boosters

1. **Learn to stay in Normal mode** - Don't live in Insert mode
2. **Use `f + [character]`** - Jump to character on current line
3. **Use `.`** - Repeat last action
4. **Use `ci"` or `ca"`** - Change inside/around quotes
5. **Use `Space + Space`** - Quick file switcher
6. **Use `%`** - Jump between matching brackets

### Go Best Practices

1. **Format on save** - LazyVim does this automatically
2. **Use code actions** - `Space + c + a` for auto-imports and refactoring
3. **Hover for docs** - `K` to see function documentation
4. **Navigate by definition** - `g + d` instead of scrolling

### Essential Workflow

1. `Space + f + f` → Open file
2. `g + d` → Navigate to definitions
3. `Space + c + a` → Fix imports/run code actions
4. `Space + /` → Search in file
5. `Space + s + g` → Search in project
6. `:w` → Save frequently

---

_Remember: LazyVim is built on top of Neovim, so all standard Vim commands work. Start with the basics and gradually add more advanced features to your workflow._