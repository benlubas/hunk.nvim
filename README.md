<div align="center">
  <h1>hunk.nvim</h1>
</div>

<div align="center">
  <p>
    A tool for splitting diffs in Neovim
  </p>
</div>

This is a Neovim tool for splitting/editing diffs. It operates over a `left` and `right` directory, producing a diff of
the two which can subsequently be inspected and modified. The `DiffEditor` allows selecting changes by file, hunk or
individual line to produce a new partial diff.

This was primarily built to be used with [jujutsu](https://github.com/martinvonz/jj) as an alternative diff-editor to
it's `:builtin` option, but it's designed generically enough that it can be used for other use cases.

To use it you need to give it two to three directories: a `left`, a `right`, and optionally a `output` directory. These
directories will then be read in and used to produce a set of diffs between the two directories. You will then be
presented with the left and right side of each file and can select the lines from each diff hunk you would like to keep.

When you are happy with your selection you can accept changes and the diff editor will modify the `output` directory (or
the `right` directory if no output is provided) to match your selection.

![preview](assets/preview.png)

## Installation

### Using [folke/lazy.vim](https://github.com/folke/lazy.nvim)

```lua
{
  "julienvincent/hunk.nvim",
  cmd = { "DiffEditor" },
  config = function()
    require("hunk").setup()
  end,
}
```

### Dependencies

+ [nui.nvim](https://github.com/MunifTanjim/nui.nvim)
+ [nvim-web-devicons](https://github.com/nvim-tree/nvim-web-devicons) (optional)
+ [mini.icons](https://github.com/echasnovski/mini.icons) (optional)

If you want file type icons in the file tree then you should have one of either `mini.icons` or `nvim-web-devicons`
installed. Otherwise, neither are required.

## Configuration

```lua
local hunk = require("hunk")
hunk.setup({
  keys = {
    global = {
      quit = { "q" },
      accept = { "<leader><Cr>" },
      focus_tree = { "<leader>e" },
    },

    tree = {
      expand_node = { "l", "<Right>" },
      collapse_node = { "h", "<Left>" },

      open_file = { "<Cr>" },

      toggle_file = { "a" },
    },

    diff = {
      toggle_line = { "a" },
      toggle_hunk = { "A" },
    },
  },

  ui = {
    tree = {
      -- Mode can either be `nested` or `flat`
      mode = "nested",
      width = 35,
    },
    --- Can be either `vertical` or `horizontal`
    layout = "vertical",
  },

  icons = {
    selected = "󰡖",
    deselected = "",
    partially_selected = "󰛲",

    folder_open = "",
    folder_closed = "",
  },
})
```

## Using with Jujutsu

[Jujutsu](https://github.com/martinvonz/jj) is an alternative VCS that has a focus on working with individual commits
and their diffs.

A lot of commands in jujutsu allow you to select parts of a diff. The tool used to select the diff can be configured via
their `ui.diff-editor` config option. To use `hunk.nvim` add the following to your jujutsu `config.toml`:

```toml
[ui]
diff-editor = ["nvim", "-c", "DiffEditor $left $right $output"]
```

You can find more info on this config in [the jujutsu docs](https://martinvonz.github.io/jj/latest/config/#editing-diffs).
