<h1 style="text-align:center;font-size:40px;">MD</h1>

<h1 style="text-align:center;font-size:40px;margin-top: 350px">Titulo</h1>
<h2 style="text-align:center;font-size:30px;">Nombre</h2>

```html
<h1 style="text-align:center;font-size:40px;margin-top: 350px">Titulo</h1>
<h2 style="text-align:center;font-size:30px;">Nombre</h2>

<!-- Salto de pagina -->
<div style="page-break-after: always;"></div>
```


<div align='center' ><img src='https://i.imgur.com/uujWGjb.png'/></div>

```html
<div align='center'><img src='X'/></div>
<div align='center' style='height:100%; width=100%'><img src='X'/></div>
<!-- Solo con HEIGHT ya redimensiona -->
<div align='center'>
    <img src='X'/>
</div>
```


## VSSCODE

```html
<!-- settings -->
"pasteImage.insertPattern": "<div align='center'><img src='./.assets/$imageFileName}'/></div>",

<!-- markdown-pdf -->
<!-- footer -->
<div style="font-size: 9px; margin: 0 auto;"> <span class='pageNumber'></span> / <span class='totalPages'></span></div>

<!-- header -->
<div style="font-size: 9px; margin-left: 1cm;"> <span>Nombre</span></div> <div style="font-size: 9px; margin-left: auto; margin-right: 1cm; "> <span class='date'></span></div>
```

### settings.json

```json
{
    "terminal.integrated.fontFamily": "'hack nerd font mono', consolas, monospace, haks, console, montserrat, 'Droid Sans Mono', 'monospace', 'Droid Sans Fallback'",
	"terminal.integrated.gpuAcceleration": "on",
    "workbench.editorAssociations": {
        "*.ipynb": "jupyter-notebook"
    },
    "explorer.confirmDelete": false,
    "editor.renderWhitespace": "none",
    "vsicons.dontShowNewVersionMessage": true,
    "oracledevtools.bookmarkFileFolder": "/home/ASIR/blodeiro/Oracle/oracle.oracledevtools",
    "oracledevtools.connectionConfiguration.configFilesFolder": "/home/ASIR/blodeiro/Oracle/network/admin",
    "oracledevtools.connectionConfiguration.walletFileFolder": "/home/ASIR/blodeiro/Oracle/network/admin",
    "oracledevtools.connections": [],
    "mssql.connections": [
        {
            "server": "{{put-server-name-here}}",
            "database": "{{put-database-name-here}}",
            "user": "{{put-username-here}}",
            "password": ""
        }
    ],
    "glassit.alpha": 195,
    "vscodeGoogleTranslate.preferredLanguage": "Spanish",
    "explorer.confirmDragAndDrop": false,
    "notebook.cellToolbarLocation": {
        "default": "right",
        "jupyter-notebook": "left"
    },
    "workbench.editor.untitled.hint": "hidden",
    "kite.showWelcomeNotificationOnStartup": false,
    "tabnine.experimentalAutoImports": true,
    "workbench.colorTheme": "monokai-dark-pro-colorful",
    "editor.fontFamily": "'JetBrains Mono','Victor Mono', monospace",
    "editor.tokenColorCustomizations": {    /* darrr */
        "textMateRules": [

            {
                "name": "all",
                "scope": [
                    "all",
                    "comment",
                    "entity.name.type.class",
                    "keyword",
                    "storage.modifier",
                    "storage.type",
                    "support.class.builtin",
                    "keyword.control",
                    "constant.language",
                    "entity.other.attribute-name",
                    "entity.name.method",
                    "entity.name.tag",
                    "string.quoted",
                ],
                "settings": {
                    "fontStyle": "italic"
                }
            },
            {
                "name": "constant.numeric",
                "scope": [
                    "constant"
                ],
                "settings": {
                    "foreground": "#15ff00",
                    "fontStyle": "bold"
                }
            },
            {
                "name": "comment", 
                "scope": [
                    "comment",
                    "punctuation.definition.comment"
                ],
                "settings": {
                    "fontStyle": "italic"
                }
            },
            {
                "name": "envKeys",
                "scope": "string.quoted.double.env,source.env,constant.numeric.env",
                "settings": {
                    "foreground": "#19354900",
                }
            }
        ]
    },
    "editor.fontLigatures": true,
    "editor.letterSpacing": 0,
    "editor.tabSize": 4,
    "database-client.recordSelectSQL": false,
    "database-client.showTrigger": true,
    "editor.minimap.maxColumn": 100,
    "editor.minimap.renderCharacters": false,
    "terminal.integrated.defaultProfile.linux": "zsh",
    "editor.cursorSmoothCaretAnimation": "on",
    "editor.smoothScrolling": true,
    "editor.cursorBlinking": "expand",
    "editor.cursorStyle": "line",
    "editor.renderWhitespace": "selection",
    "breadcrumbs.enabled": true,
    "git.enableSmartCommit": true,
	"git.confirmSync": false,
    "google.drive.alertMissingCredentials": false,
    "markdown-pdf.headerTemplate": "<div style=\"font-size: 9px; margin-left: 1cm;\"> <span>Name</span></div> <div style=\"font-size: 9px; margin-left: auto; margin-right: 1cm; \"> <span class='date'></span></div>",
    "[markdown]": {
        "editor.defaultFormatter": "yzhang.markdown-all-in-one"
    },
    "MarkdownPaste.path": "${fileDirname}/.assets",
    "outline-map.color": {
    
    },
    "outline-map.defaultMaxDepth": 3,
    "outline-map.maxDepth": 5,
    "cSpell.language": "es-ES,es",
    "markdown-preview-enhanced.imageFolderPath": "/.assets",
    "MarkdownPaste.lang_rules": [

        {
            "asciidoc": [
                {
                    "regex": "^(?:https?://)?(?:(?:(?:www\\.?)?youtube\\.com(?:/(?:(?:watch\\?.*?v=([^&\\s]+).*)|))?))",
                    "options": "g",
                    "replace": "image::https://img.youtube.com/vi/$1/0.jpg[link=\"https://www.youtube.com/watch?v=$1\"]"
                },
                {
                    "regex": "^(https?://.*)",
                    "options": "ig",
                    "replace": "image::$1[linktext,300]"
                },
                {
                    "regex": "(.*/media/)(.*)",
                    "options": "",
                    "replace": "image::$2[linktext,300]"
                }
            ]
        }
    ],
    "MarkdownPaste.rules": [

        {
            "regex": "^(?:https?://)?(?:(?:(?:www\\.?)?youtube\\.com(?:/(?:(?:watch\\?.*?v=([^&\\s]+).*)|))?))",
            "options": "g",
            "replace": "[![](https://img.youtube.com/vi/$1/0.jpg)](https://www.youtube.com/watch?v=$1)"
        },
        {
            "regex": "^(https?://.*)",
            "options": "ig",
            "replace": "[]($1)"
        }
    ],
    "MarkdownPaste.silence": true,
    "ShortcutMenuBar.paste": true,
    "pastePicture.markdownFormat": "html",
    "pasteImage.basePath": "${currentFileDir}/.assets",
    "pasteImage.path": "${currentFileDir}/.assets",
    "pasteImage.insertPattern": "<div align='center'><img src='./.assets/${imageFileName}'/></div>",
    "launch": {
    
        "configurations": [],
        "compounds": []
    },
    "pasteImage.defaultName": "YMMDDHHmmss",
    "json.schemas": [

    
    ],
    "workbench.iconTheme": "hypernym-icons",
    "editor.fontVariations": false,
    "git.autofetch": true,
    "extensions.ignoreRecommendations": true,
    "cSpell.languageSettings": [
    

    
    ],
    "oracledevtools.download.otherFolder": "/home/b4391co/downloads",
    "files.autoSave": "afterDelay",
    "vscode-blur-linux.opacity": 80,
    "codesnap.backgroundColor": "#ffffff",
    "codesnap.shutterAction": "copy",
 
```