# VSCode settings

## Extension list

1. Markdown All in One
1. Markdown Preview Enhanced
1. markdownlint
1. PlantUML
1. Spell Right

## Settings.json

```json
{
    "git.confirmSync": false,
    "git.enableSmartCommit": true,
    "git.autofetch": true,
    "workbench.editorAssociations": {
        "*.sql": "default"
    },
    "workbench.editor.enablePreview": false,
    "git.ignoreRebaseWarning": true,
    "markdown-preview-enhanced.previewTheme": "vue.css",
    "explorer.confirmDragAndDrop": false,
    "markdown.extension.orderedList.marker": "one",
    "editor.tabSize": 3,
    "editor.defaultFormatter": "DavidAnson.vscode-markdownlint",
    "markdown.extension.tableFormatter.normalizeIndentation": true,
    "markdown.extension.tableFormatter.enabled": false,
    "markdownlint.config": {
        "MD013": false,
        "MD024": false,
    },
    "markdown.extension.toc.levels": "2..3",
    "markdown.extension.toc.orderedList": true,
    "explorer.confirmDelete": false,
    "diffEditor.renderSideBySide": false
}
```