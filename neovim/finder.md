# Finder

## Keymaps

```lua
-- Buffers & Files
{ "<leader>ff", function() require('fzf-lua').files() end },
{ "<leader>fh", function() require("fzf-lua").highlights() end },
{ "<leader>fq", function() require("fzf-lua").quickfix() end },

-- Search
{ "<leader>fg", function() require('fzf-lua').live_grep() end },
{ "<leader>f?", function() require('fzf-lua').lgrep_curbuf() end },

-- Git
{ "<leader>fb", function() require("fzf-lua").git_branches() end },

-- LSP
{ "gr", function() require("fzf-lua").lsp_references() end },
{ "gd", function() require("fzf-lua").lsp_definitions() end },
{ "gD", function() vim.diagnostic.enable(not vim.diagnostic.is_enabled()) end },
{ "g=", function() require('fzf-lua').lsp_code_actions() end },
{ "<leader>f@", function() require("fzf-lua").lsp_document_symbols() end },
{ "<leader>f#", function() require("fzf-lua").lsp_live_workspace_symbols() end },
{ "<leader>fd", function() require("fzf-lua").diagnostics_document() end, },

-- Misc
{ "<leader>fp", function() require("fzf-lua").commands() end, },
{ "<leader>f:", function() require("fzf-lua").command_history() end, },
{ "<leader>fk", function() require("fzf-lua").keymaps() end, },
{ "<leader>fr", function() require("fzf-lua").registers() end, },
```
