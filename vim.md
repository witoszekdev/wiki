# VIM

## Basic motions

### Folds

- `zf` - create fold
- `zd` - delete fold
- `zo` - open fold
- `zc` - close fold
- `zj` - next fold
- `zk` - previous fold
- `zM` - close all folds in file
- `zR` - open all folds in file

### Combining motions

`w` - inside word
`W` - everything that is not white space
`i` - inside
  ex. `viw` - select inside word
`a` - all
`p` - paragraph
  ex. `dap` - delete entire paragraph

### Navigation

- `c` - delete + enter insert mode

Jump list - see all "big movements" inside the file
- `:jump`
- `CTRL+O` - go to previous jump
- `CTRL+I` - go to next jump in history

When viewing list of options:
- `CTRL+N` - go next
- `CTRL+D` - go previous

#### Vertical

- `G` - end of file
- `gg` - start of file
- `$` - end of line
- `zz` center file
- `{ }` - jump between paragraphs
- `[m` / `]m` - move to next opening block, ex. `{}` in JS
- `%` - jump between bracket pairs

#### Horizontal
- `^` - start of line
- `f` - find character
  - *+ SHIFT* - reverse order
- `t` - find up to the character
  - *+ SHIFT* - reverse order
- `;` - move to next occurence
- `,` - move to prev occurence


### Search and replace

When in visual selection press `:` to search in that selection

- `/<search query>` - runs the search query
- `s/<search>/<replace>` - runs search and replace (single line)
- `%s/<search>/<replace>` - search & replace in entire file
- `:nohls` - clears highlighted results

## Quickfix list

Quickfix list has two types of results:
- entries - actual line that matched the search
- file - file that contained the result

- `:copen` - opens quickfix list
- `:cdo` - apply commands on all *entries* in quickfix list
  - Run macro on all results and update file:
    ``:cdo execute "norm @q" | update``
- ``:cfdo`` - apply commands on all files in quickfix list
  - Run search and replace on every file and update:
    `:cfdo s/v2/v3/ | update`

**Reference**
- https://gist.github.com/Integralist/8d01300efcd2006c69e8b9492c0eada8

### Cookbook

Open qfl from buffer contents

```vim
:cexpr join(getline(1,'$'), "\n")
```

## Macros

Macros record every key stroke, and replay them

- `q<letter>` - start recording macro
  - Ex. `ga` - assigns macro to letter `a`
- `@<letter>` - runs assigned macro
  - PROTIP: Macro can be replayed several times, ex. `5@a`

## Window mode

- `CTRL+W` (reffered as `<W>`)  - enter window mode
- `<W> + CTRL+O` - close all other windows except the focused one
- `<W> + CTRL+V` - open new window in horizontal mode

## Registers

In esence they are key -> value store

- `:reg` - view all registers

Types of registers:
- `"` - recent yank
- `0-9` - yank and deletion history
- `<letter>` - arbitrary registers saved by us (incl. macros)
- `%` - path to the current file

To re-run whatever was saved in register we can use `"` + key of register + some motion
Example: `"ap` - will paste everything from register *a*

## Pro tips

- In visual mode: `o` - jump between start and end of selection
- `CTRL+A` - increase number by 1 inside selection (or cursor)
- `D` - remove everything after the cursor
- `CTRL+^` - go to previous file
- `CTRL-C` = `<Esc>`
- `set conceallevel=2` - hide Markdown syntax (like \`)
- `:verbose imap <remap>` - check if a remap is already used in config or by plugin
