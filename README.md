# xcpc-env

Windows, mingw64, Sublime Text 4, LSP with Clangd, FastOlympicCoding, dbg-macro.

## mingw64

Download: [niXman/mingw-builds-binaries](https://github.com/niXman/mingw-builds-binaries/releases)

> Recommend `x86_64` and `win32-seh-ucrt-rt`.

Remember to add `mingw64\bin` to your system env `Path`.

## Clangd

Download: [clangd/clangd](https://github.com/clangd/clangd)

Remember to add `clangd_xx.x.x\bin` to your system env `Path`.

## Sublime Text 4

### Download

[Build 4192](https://www.sublimetext.com/download), [Hacked by wangzhizhuo](https://blog.csdn.net/wangzhizhuo/article/details/141190550).

> Modify `sublime_text.exe`: `8079 0500 0f94 c2` -> `c641 0501 b200 90`.

### Install Packages

`Ctrl + Shift + P` to call up the Command Palette, and type `Install Package` to install the following packages:

1. `Packages Control`: This can manage all of the installed packages;
2. `ChineseLocalization`(Optional): Set to your mother language;
3. `LSP`: The Language Server Protocol helps you to complete your code, check the syntax, and so on;
4. `LSP-clangd`: According to your code language, select the corresponding server;
5. `LSP-json`: This makes it easier to configure our LSP;
6. `FastOlympicCoding`: You can place the cases on it, and test them;
7. `Terminus`: You can't interact with the program in the default Sublime Text Terminal;
8. `Rose Pine`(Optional): Select the theme you favor.

### Configure

You should select `Preference` Option to configure json. 

#### `Preferences.sublime-settings`

```json
{
  "color_scheme": "Rosé Pine Moon.sublime-color-scheme",
  "ignored_packages":
  [
    "Vintage",
  ],
  "theme": "Rosé Pine Moon.sublime-theme",
  "dark_color_scheme": "Monokai.sublime-color-scheme",
  "light_color_scheme": "Sixteen.sublime-color-scheme",
  "index_files": true,

  "font_face": "CaskaydiaCove NF",
  "font_size": 11,
  "tab_size": 2,
  "translate_tabs_to_spaces": true,
  "expand_tabs_on_save": false,
  "trim_trailing_white_space_on_save": true,
}
```

You may need to choose other `font_face` and `tab_size`.

> You can try some Nerd Fonts [ryanoasis/nerd-fonts](https://www.nerdfonts.com/), which provide with a high number of icons.

#### `LSP.sublime-settings`

```json
{
  // only show red line
  "show_diagnostics_severity_level": 1,
  "show_code_actions": "bulb",
  "lsp_format_on_save": true,
}
```

#### `LSP-clangd.sublime-settings`

```json
// Settings in here override those in "LSP-clangd/LSP-clangd.sublime-settings"
{
  // use gnu, not default msvc
  "initializationOptions": {
    "fallbackFlags": [
      "-target",
      "x86_64-w64-windows-gnu",
      "-std=c++23"
    ],
  }
}
```

#### `FastOlympicCoding.sublime-settings`

```json
{
  "run_settings": [
    {
      "name": "C++",
      "extensions": [
        "cpp"
      ],
      // -std=c++23 -O2 -g -Wall ....
      "compile_cmd": "g++ \"{source_file}\" -std=c++23 -o \"{file_name}\"",
      "run_cmd": "\"{source_file_dir}\\{file_name}.exe\" {args} -debug",
      "lint_compile_cmd": "g++ -std=gnu++23 \"{source_file}\" -I \"{source_file_dir}\""
    },
  ],
  "tests_file_suffix": "__tests"
}
```

#### `Default (Windows).sublime-keymap`

```json
[
  {
    "keys": [
      "ctrl+shift+/"
    ],
    "command": "terminus_open",
    "args": {
      "cmd": "powershell",
      "cwd": "${file_path:${folder}}",
      "panel_name": "My Terminus",
      "show_in_panel": false, // show in tab
    }
  },
]
```

### Compile & Run

Select `Tools` -> `Compile System` -> `Create Compile System`.

```json
{
  "cmd": [
    "g++.exe",
    "-std=c++23",
    "-O2",
    "-Wall",
    "-Wextra",
    "-Wshadow",
    "-Wformat=2",
    "-Wfloat-equal",
    "-Wconversion",
    "-Wlogical-op",
    "-Wshift-overflow=2",
    "-Wduplicated-cond",
    "-Wcast-qual",
    "-Wcast-align",
    "-Wno-unused-parameter",
    "-Wnull-dereference",
    "-D_GLIBCXX_DEBUG",
    "-D_GLIBCXX_DEBUG_PEDANTIC",
    "${file}",
    "-o",
    "${file_base_name}.exe"
  ],
  "working_dir": "$file_path",
  "selector": "source.cpp",
  "variants": [
    // You can run immediately after compiling
    // {
    //   "name": "Run From Outside Terminal",
    //   "shell_cmd": "g++ -std=c++23 -O2 -Wall -Wextra -Wshadow -Wformat=2 -Wfloat-equal -Wconversion -Wlogical-op -Wshift-overflow=2 -Wduplicated-cond -Wcast-qual -Wcast-align -Wno-unused-parameter -Wnull-dereference -D_GLIBCXX_DEBUG -D_GLIBCXX_DEBUG_PEDANTIC \"$file\" -o \"$file_base_name\" && \"${file_path}/${file_base_name}\""
    // }
  ]
}
```

Save as `C++23.sublime-build`. Now you can press `Ctrl + B` to Compile your code, then press `Ctrl + Shift + /` to open a terminal in a tab, which can be arranged to the right side by pressing `Shift + Alt + 2` and dragging the tab.

### Theme

Press `Ctrl + Shift + P`, search with `UI: Customize Theme`, and configure your theme.

```json
// Documentation at https://www.sublimetext.com/docs/color_schemes.html
{
  "variables":
  {
    "pine": "#5abbdc",
    "foam": "#acdfe8",
    "rose": "#faaaa7",
    // "tags": "var(pine)",
    // "functions": "var(foam)"
  },
  "globals":
  {
  },
  "rules":
  [
  ]
}
```

## Format

`.clang-format`:

```txt
BasedOnStyle: LLVM

UseTab: Never

IndentWidth: 2

TabWidth: 4

BreakBeforeBraces: Attach

AllowShortIfStatementsOnASingleLine: true

AllowShortLoopsOnASingleLine: true

IndentCaseLabels: false

ColumnLimit: 0

AccessModifierOffset: -4

NamespaceIndentation: All

FixNamespaceComments: false
```

## dbg-macro

You can use it to print some variables and debug your code.

Download: [sharkdp/dbg-macro](https://github.com/sharkdp/dbg-macro)

> **Attention!** Due to the fact that the Sublime Text 4's Terminal doesn't support ANSI-escaping to show colorful charactors, you should modify some codes in `dbg.h`:
>
> ```diff
> inline bool isColorizedOutputEnabled() {
> #if defined(DBG_MACRO_FORCE_COLOR)
>   return true;
> #elif defined(DBG_MACRO_UNIX)
>   return isatty(fileno(stderr));
> #else
> - return true;
> + return false;
> #endif
> }
> ```
