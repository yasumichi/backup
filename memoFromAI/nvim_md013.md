# nvim md013 警告抑制

Neovimにおいて、markdownlint（特にnvim-lintやLazyVim経由）のMD013（行の長さ制限）警告を抑制する方法はいくつかあります。プロジェクト全体で無効にするか、ファイル単位で無効にするかで方法が異なります。

最も推奨されるのは、プロジェクトルートに設定ファイルを作成して無効化する方法です。

- [LazyExtras markdown; How to configure markdownlint? · LazyVim/LazyVim · Discussion #2268](https://github.com/LazyVim/LazyVim/discussions/2268)
- [How to disable warning: Line Length [Expected: 80; Actual: *] · LazyVim/LazyVim · Discussion #2367](https://www.google.com/url?sa=i&source=web&rct=j&url=https://github.com/LazyVim/LazyVim/discussions/2367&ved=2ahUKEwii0pjKjfySAxX7rVYBHaIPNsoQqYcPegQIGhAG&opi=89978449&cd&psig=AOvVaw0MVdqAgw7Spmv67N4-Gxjv&ust=1772365350275000)
- [layout - How to handle wide markdown tables and line-length checks in pre-commit? - Stack Overflow](https://stackoverflow.com/questions/74319856/how-to-handle-wide-markdown-tables-and-line-length-checks-in-pre-commit)
- [UNPKG](https://app.unpkg.com/markdownlint@0.23.1/files/README.md)

1. プロジェクト全体でMD013を無効にする

プロジェクトのルートディレクトリ（.gitがある場所など）に、.markdownlint.jsonまたは.markdownlint.yamlファイルを作成します

.markdownlint.json の場合:

```json
{
  "MD013": false
}
```

これで、すべてのMarkdownファイルでこの警告が表示されなくなります。

1. 行の長さの上限を増やす（無効化せずに警告を減らす）

完全に無効にしたくない場合は、制限を長く設定（例：120文字や200文字など）することもできます。

.markdownlint.json の場合:

```json
{
  "MD013": {
    "line_length": 120"code_block_line_length": 160
  }
}
```

1. ファイルの先頭でMD013を無効にする

特定のファイル内だけで無効にしたい場合、ファイルの冒頭に以下のコメントを追加します。

```markdown
<!-- markdownlint-disable MD013 -->
```

1. nvim-lint（LazyVimなど）のプラグイン設定で無効にする

LazyVimやnvim-lintのプラグイン設定で直接引数を渡して無効化します（~/.config/nvim/lua/plugins/lint.luaなど）。

```lua
return {
  {
    "mfussenegger/nvim-lint", opts = {
      linters = {
        markdownlint = {
          args = { "--disable""MD013""--" }}}}}}
```

※LazyVimのdiscussionsで推奨されている方法。

1. 一時的に特定の行だけ無効にする

長いURLやテーブルの行だけを一時的に無効にしたい場合、以下のコメントをその行の下に追加します。

```markdown
これは長すぎる行です <!-- markdownlint-disable-line MD013 -->
```

補足: 何が警告を出しているか？

nvim-lintを使用している場合、上記の設定で解決します。null-ls (none-ls) を使用している場合も同様の設定（extra_args）が有効です。
