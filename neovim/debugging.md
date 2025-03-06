# DEBUGGING

## Keymaps

```lua
{ "<leader>dd", function() require("dap").continue() end },
{ "<leader>dx", function() require("dap").terminate() end },
{ "<leader>dt", function() require("dapui").toggle() end },
{ "<leader>dc", function() require("dap").run_to_cursor() end },
{ "<leader>db", function() require("dap").toggle_breakpoint() end },
{ "<leader>do", function() require("dap").step_over() end },
{ "<leader>di", function() require("dap").step_into() end },
{ "<leader>dO", function() require("dap").step_out() end },
{ "<leader>dk", function() require("dap").up() end },
{ "<leader>dj", function() require("dap").down() end },
{ "<leader>dh", function() require("dap.ui.widgets").hover() end },
{ "<leader>dp", function() require("dap.ui.widgets").preview() end },
{ "<leader>df", function() require("dap.ui.widgets").centered_float(require('dap.ui.widgets').frames) end },
{ "<leader>ds", function() require("dap.ui.widgets").centered_float(require('dap.ui.widgets').scopes) end },
```

## Configurations

### Node.js: Attach to Process

```lua
{
  type = "pwa-node",
  request = "attach",
  name = "Node.js: Attach to Process",
  processId = require("dap.utils").pick_process,
  cwd = "${workspaceFolder}",
  sourceMaps = true,
}
```

1. Start debugger by pressing: `<leader>dd`
2. Choose `Node.js: Attach to Process`

### Chrome: Launch to Debug URL

```lua
{
  type = "pwa-chrome",
  request = "launch",
  name = "Chrome: Launch to Debug URL",
  url = function()
    local co = coroutine.running()
    return coroutine.create(function()
      vim.ui.input({
        prompt = "Enter URL: ",
        default = "http://localhost:4200",
      }, function(url)
        if url == nil or url == "" then
          return
        else
          coroutine.resume(co, url)
        end
      end)
    end)
  end,
  webRoot = "${workspaceFolder}",
  skipFiles = { "<node_internals>/**" },
  protocol = "inspector",
  sourceMaps = true,
  userDataDir = false,
}
```

1. Start debugger by pressing: `<leader>dd`
2. Choose `Chrome: Launch to Debug URL`

### Java: Attach to Debug

```lua
{
  type = "java",
  request = "attach",
  name = "Java: Attach to Debug",
  hostName = "127.0.0.1",
  port = 5005,
}
```

1. Start debugger by pressing: `<leader>dd`
2. Choose `Java: Attach to Debug`

## Usage

### Jest

1. Add the `test:debug` script to your `package.json`

```json
"test:debug": "node --inspect-brk node_modules/.bin/jest --runInBand"
```

2. Follow [Node.js: Attach to Process](#nodejs-attach-to-process)
