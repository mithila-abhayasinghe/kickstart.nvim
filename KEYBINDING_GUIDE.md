# Neovim Keybinding Configuration Guide

## Overview

This guide explains how keybindings are configured in this Neovim setup and provides instructions for modifying them in the future.

## Recent Changes

### Added: `jj` to Exit Insert Mode

**What was changed:** A new keybinding has been added that allows you to exit insert mode by pressing `jj` instead of reaching for the `Esc` key.

**Where it's configured:** `nvim/init.lua`, in the "Basic Keymaps" section (around line 176).

**The code:**
```lua
vim.keymap.set('i', 'jj', '<Esc>', { desc = 'Exit insert mode with jj' })
```

**Why this is useful:** 
- Reduces hand movement since `j` is already on the home row
- Faster workflow, especially for touch typists
- Common pattern in Vim configurations for ergonomics

---

## How to Modify Keybindings

### Understanding the Structure

All keybindings in this config use the `vim.keymap.set()` function with the following structure:

```lua
vim.keymap.set(mode, lhs, rhs, { desc = 'description' })
```

Where:
- **mode**: The Vim mode where the keybinding applies
  - `'n'` = Normal mode (default editing mode)
  - `'i'` = Insert mode (while typing text)
  - `'v'` = Visual mode (while selecting text)
  - `'t'` = Terminal mode (in Neovim's terminal)
  - `'x'` = Visual block mode

- **lhs** (Left Hand Side): The keys you press (the trigger)
  - Examples: `'jj'`, `'<leader>f'`, `'<C-s>'`

- **rhs** (Right Hand Side): What action happens when you press lhs
  - Examples: `'<Esc>'`, `'<cmd>nohlsearch<CR>'`, `vim.diagnostic.setloclist`

- **desc**: A description of what the keybinding does (shown in which-key menu)

### Special Key Syntax

When defining keybindings, special keys use angle bracket notation:

| Syntax | Meaning |
|--------|---------|
| `<Esc>` | Escape key |
| `<CR>` | Enter/Return key |
| `<C-x>` | Ctrl + x |
| `<S-x>` | Shift + x |
| `<M-x>` | Alt/Meta + x |
| `<leader>x` | Space key (configured as leader) + x |
| `<C-\><C-n>` | Ctrl + \ then Ctrl + n |

### Step-by-Step: Changing a Keybinding

#### Example 1: Change `jj` to `jk`

1. Open `nvim/init.lua` in your editor
2. Find the line:
   ```lua
   vim.keymap.set('i', 'jj', '<Esc>', { desc = 'Exit insert mode with jj' })
   ```
3. Replace `'jj'` with `'jk'`:
   ```lua
   vim.keymap.set('i', 'jk', '<Esc>', { desc = 'Exit insert mode with jk' })
   ```
4. Save the file
5. Reload the config by either:
   - Running `:source %` in Neovim (while the init.lua file is open)
   - Restarting Neovim entirely

#### Example 2: Add a New Keybinding

To add a new keybinding, follow these steps:

1. Open `nvim/init.lua`
2. Find the "Basic Keymaps" section (starts around line 169)
3. Add your new keybinding after existing ones:
   ```lua
   -- Save file with Ctrl+s in normal mode
   vim.keymap.set('n', '<C-s>', '<cmd>w<CR>', { desc = 'Save file' })
   ```
4. Save and reload the config as described above

#### Example 3: Change Exit Terminal Mode Keybinding

Current binding: `Esc Esc` (press Escape twice)

To change it to `Ctrl+q`:

1. Find this line (around line 185):
   ```lua
   vim.keymap.set('t', '<Esc><Esc>', '<C-\\><C-n>', { desc = 'Exit terminal mode' })
   ```
2. Replace it with:
   ```lua
   vim.keymap.set('t', '<C-q>', '<C-\\><C-n>', { desc = 'Exit terminal mode' })
   ```
3. Save and reload

### Common Actions (rhs values)

When defining the right-hand side, here are common patterns:

**Execute a command:**
```lua
'<cmd>command_name<CR>'
```
Example: `'<cmd>nohlsearch<CR>'` clears search highlighting

**Call a Lua function:**
```lua
vim.diagnostic.setloclist
```

**Execute Vim keystrokes:**
```lua
'<C-w><C-h>'
```
Example: Navigate to left window

**Execute multiple commands:**
```lua
':command1<CR>:command2<CR>'
```

---

## Finding Existing Keybindings

All keybindings in this config are in `nvim/init.lua` in the "Basic Keymaps" section (lines 169-210 approximately).

To see all currently active keybindings while in Neovim, press `<space>` and wait for the which-key menu to appear. This shows all available keybindings with their descriptions.

---

## Testing Your Changes

After modifying a keybinding:

1. Save the file
2. Run `:source %` in Neovim (make sure you're in the init.lua buffer)
3. Test the new keybinding immediately

If something doesn't work:
- Check for typos in the keybinding definition
- Make sure you're in the correct mode (insert, normal, etc.)
- Run `:checkhealth` to see if there are any configuration errors
- Check the Neovim log: `:messages`

---

## Advanced: Keybinding Conflicts

If a keybinding doesn't work:
- It may conflict with your terminal emulator settings
- Some combinations don't work in all terminals
- Check Neovim documentation: `:help keycodes`

To avoid conflicts, prefer:
- `<leader>` combinations (Space key in this config)
- `<C-...>` (Ctrl combinations)
- Less common key sequences

---

## Resources

- `:help vim.keymap.set()` - Official Neovim documentation for keybindings
- `:help keycodes` - Complete list of special key syntax
- `:help key-notation` - Vim key notation reference
- `:help modes` - Explanation of all Vim modes
