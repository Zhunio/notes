# Testing

## Keymaps

```lua
{ "<leader>tt", function() require("neotest").run.run() end },
{ "<leader>tl", function() require("neotest").run.run_last() end },
{ "<leader>tf", function() require("neotest").run.run(vim.fn.expand("%")) end },
{ "<leader>ta", function() require("neotest").run.run({suite = true}) end },
{ "<leader>ts", function() require("neotest").run.stop() end },
{ "<leader>te", function() require("neotest").summary.toggle() end },
{ "<leader>to", function() require("neotest").output.open({ enter = true }) end },
{ "<leader>tp", function() require("neotest").output_panel.toggle() end },
```
