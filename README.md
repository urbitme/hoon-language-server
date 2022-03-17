A language server for Hoon. This is the same server as the [official urbit version](https://github.com/urbit/hoon-language-server) with a fix for global npm installation.

Install with:

```
npm install -g git+https://github.com/urbitme/hoon-language-server
```

This installs `hoon-language-server` which allows your code editor to communicate with an urbit ship running locally on your development machine.

By default it will connect to a fake `~zod` at port 80.  To connect to a different port or ship name, use these options:

```
hoon-language-server -p 8080 -s let -c maltem-ragryx-ravlyn-nopwyx
```

where your ship is a fake `~let` running on port 8080, and `maltem-ragryx-ravlyn-nopwyx` is the passcode used to login to landscape.

## Urbit Setup

You must have a fake ship running and it must be running the hoon language server.  To create and start a fake `~zod`:

```
urbit -F zod -c zod
```

In the urbit dojo, start the language server:

```
|start %language-server
```

If you need the passcode for your ship, enter this in dojo:

```
+code
```

To start the same ship again in the future just run:

```
urbit zod
```

in the same directory you created it in.

## Editor Setup

Your code editor now needs to use `hoon-language-server` as an LSP provider.  There are plugins for common editors that add syntax highlighting, etc. and help connect to the language server.

### VSCode

 * [hoon-vscode](https://github.com/famousj/hoon-vscode)
 * [hoon-assist-vscode](https://github.com/urbit/hoon-assist-vscode)

### Emacs

 * [hoon-mode.el](https://github.com/urbit/hoon-mode.el)

### Vim

 * [hoon.vim](https://github.com/urbit/hoon.vim)

`hoon.vim` does not use the language server itself, but the github page describes how to using `vim-lsp`.

For `coc.nvim` users, add a `languageserver` entry to `~/.config/nvim/coc-settings.json`:

```
{
  "languageserver": {
    "hoon-language-server": {
      "command": "hoon-language-server",
      "args": ["-p", "8080"],
      "filetypes": ["hoon"]
    }
  }
}
```

Using neovim's native LSP support:

```
-- Only `configs` must be required, util is optional if you are using the root resolver functions, which is usually the case.
local configs = require ('lspconfig.configs')
local util = require ('lspconfig.util')

configs['hoon_ls'] = {
  default_config = {
    cmd = { 'hoon-language-server', "-p", "8080" },
    filetypes = { 'hoon' },
    single_file_support = true,
    root_dir = function(fname)
      return util.find_git_ancestor() or vim.loop.os_homedir()
    end,
  },
  docs = {
    description = [[
      https://github.com/urbit/hoon-language-server
    ]],
  },
}
```
