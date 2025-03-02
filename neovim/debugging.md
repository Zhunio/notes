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

## Usage

### Jest

1. Add the `test:debug` script to your `package.json`

```json
"test:debug": "node --inspect-brk node_modules/.bin/jest --runInBand"
```

2. Follow [Node.js: Attach to Process](#nodejs-attach-to-process)

