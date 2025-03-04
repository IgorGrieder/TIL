# Lint Tip

Because I do use neovim's `Obsidian Plugin` sometimes markdown linters can cause
some trouble. One way of configuring local related linter config is by creating
a `.markdownlint.json` file and adding the warning code and set it to false.

## Example

MD lint code for maximum 80 chrs per line.

```terminal
{
  "MD013": false
}
```
