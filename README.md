# SendToTerminal+, an extension for `vscode`

[VSCode extension](https://marketplace.visualstudio.com/items?itemName=ivoh.openfileatcursor) that enables sending text selection to the terminal. Can send multiline statements with defined prefix and/or postfix for the selected text so that language REPL can determine that the content will be multiline.

## Features

Predefined to allow sending multiline statements with `:paste`/`Ctrl+D` to `Scala`'s `console` or `spark-shell`.

```
val s = " some text "
val parts = s.trim()
  .split(" ")
```
will be send in `Scala` file as:
* Prefix:
```
:paste
```
* Selection
```
val s = " some text "
val parts = s.trim()
  .split(" ")
```
* Suffix:
```
Ctrl+D
```


![demo](images/sendToTerminalPlusScalaMultiLine.gif)


If no selection is made then it sends the current line.

## Shortcut
```
Send selection : Alt + Shift + Enter
```



## Extension Settings

Default setting:
```
    "sendtoterminalplus.languages": [
        {
            "langId": "default",
            "delayMode": "default",
            "payloadFormat": "default",
            "linePattern": "{line}",
            "noSelectionPayload": [
                "{currentline}"
            ],
            "oneLineSelectionPayload": [
                "{selection}"
            ],
            "multiLineSelectionPayload": [
                "{selection}"
            ]
        },
        {
            "langId": "scala",
            "delayMode": "default",
            "payloadFormat": "chunk",
            "linePattern": "{line}",
            "noSelectionPayload": [
                "{currentline}"
            ],
            "oneLineSelectionPayload": [
                "{selection}"
            ],
            "multiLineSelectionPayload": [
                ":paste",
                "{selection}",
                "\u0004"
            ]
        },
        {
            "langId": "python",
            "delayMode": "default",
            "payloadFormat": "chunk",
            "linePattern": "{line}",
            "noSelectionPayload": [
                "{currentline}"
            ],
            "oneLineSelectionPayload": [
                "{selection}"
            ],
            "multiLineSelectionPayload": [
                "{selection}",
                ""
            ]
        }
    ]
```

Behaviour can be customized per language basis. The language id `default` is used for all undefined languages. 
* `noSelectionPayload`: defines what text is going to be send if no text is selected. Default value is ["{currentline}"].
* `oneLineSelectionPayload`: defines text sent to the terminal when selected text is only one line. E.g. ["{selection}"] would send one line with the selected text. ["a", "b", "c"] would send three lines with one character per line and not including the selected text at all.
* `multiLineSelectionPayload`: defines text sent to the terminal when selected text is more than one line. E.g. [":paste", "{selection}", "\u0004"] would send `:paste` then selected text and then `Ctrl+D` key press. 
* `linePattern`: defines transformation for each line of selected text. Selected text can be decorated on line basis if necessary.
* `payloadFormat`: defines what is the format sent to terminal. (values are `line`, `chunk` or `all`). Chunk is block of text defined by length (default setting is 1100 characters). This should circumvent limitation of selected text size sent to terminal on some the environments (e.g. Windows 7, etc...)
* `delayMode`: Defines delay period between sending of lines/chunks to terminal. Values are `delayed` or `nodelay`. Delay setting is preconfigured to 1500ms. Default value is "nodelay". Connecting to some apps (e.g. sparkshell) via terminal over network requires to have delays sometimes.

Replacement tags to be used in patterns are:
* `{currentline}` is text on current line.
* `{selection}` is all the selected text.
* `{line}` is the text in line. For use in the `linePattern` only.


## Release Notes

### 1.0.0
* Using `activeTerminal` API to send to currently active terminal. Introduced in [October 2018 (version 1.29)](https://code.visualstudio.com/updates/v1_29#_active-terminal-apis) VSCode update.

### 0.2.0
* Added delay, chunk payload format, line processing, noselection pattern.


### 0.1.0

* Initial release.


-----------------------------------------------------------------------------------------------------------

