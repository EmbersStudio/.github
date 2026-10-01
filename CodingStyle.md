# 通用编码规范（CodingStyle.md）

## 1. 通用原则（适用于所有语言）

- **字符编码**：**UTF‑8 without BOM**（所有文本文件）
- **换行符**：**CRLF**（Windows 风格）—— 仓库存储 `LF`，检出自动转换为 `CRLF`（由 `.gitattributes` 控制）
- **文件末尾**：保留一个空行（`insert_final_newline = true`）
- **缩进**：统一使用 **4 个空格**，**禁用 Tab**（但 YAML 文件可例外使用 2 个空格）
- **行尾空白**：自动修剪（`trim_trailing_whitespace = true`），Markdown 例外
- **最大行长度**：建议 **不超过 120 字符**（Markdown 的段落不强制折行）

---

## 2. 各语言详细规范

### 2.1 C++（.cpp / .h / .hpp / .cc / .c）

- **缩进**：4 空格
- **括号风格**：左括号不换行（K&R），但 `else` / `catch` 单独成行
- **访问修饰符**：`public:` / `protected:` / `private:` 顶格（列 0）
- **指针/引用**：靠左类型（`QWidget* parent`、`const std::string& s`）
- **命名规则**
    - 类名：`PascalCase`（如 `MainWindow`）
    - 变量/函数：`snake_case`（如 `user_name`、`get_data()`）
    - 常量/宏：`UPPER_CASE`（如 `MAX_BUFFER_SIZE`）
    - Qt 虚函数重载（如 `paintEvent`）保留原始风格
- **头文件组织**
    - 使用 `#pragma once`
    - 包含顺序：自定义头 → 第三方库（如 Qt）→ 标准库，每组内按字母序（**大小写敏感**）
    - 类声明顺序：`public` → `protected` → `private` → `signals` → `private slots`
        - 每个分区内：构造函数/析构函数 → 成员函数 → 成员变量（常量优先）
    - 模板实现放入 `类名_impl.h`（同目录）
    - 静态工具函数独立为 `*_util.h/cpp`，不放入类内
- **注释**
    - 函数注释使用 Doxygen（`/// @brief ...`）或行前 `//`
    - 行尾注释与代码间隔一个 **Tab**，`//` 后跟一个空格
- **日志**：使用 `LOG_MODULE` 宏，分级（ERROR / WARN / INFO / DEBUG），避免高频输出
- **格式化**：由 `clang-format` 自动处理（规则见 `.clang-format`）

### 2.2 Python（.py）

- **缩进**：4 空格（严格遵循 PEP 8）
- **命名**
    - 模块文件名：`PascalCase`（如 `FileUtils.py`）
    - 内部函数/变量：`snake_case`
    - 类名：`PascalCase`
- **导入顺序**（每组内按字母序，**大小写敏感**）
    1. 自定义模块（相对导入或项目内）
    2. 第三方库（如 PyQt5、numpy）
    3. 标准库（如 os、sys）
- **文档字符串**：函数/类使用三引号 docstring（推荐 Google 风格或 NumPy 风格）
- **行尾注释**：与代码间隔一个 Tab，`#` 后跟一个空格
- **格式化**：建议使用 `black` + `isort`（但本规范未强制，可后续补充）

### 2.3 TypeScript / JavaScript（.ts / .tsx / .js / .jsx）

- **缩进**：4 空格（Prettier 统一）
- **命名**
    - 变量/函数：`camelCase`
    - 类/接口/类型：`PascalCase`
    - 常量：`UPPER_CASE` 或 `camelCase`（项目统一）
- **分号**：必须使用（`semi: true`）
- **引号**：字符串使用单引号（`singleQuote: true`）
- **括号**：箭头函数参数始终加括号（`arrowParens: always`）
- **导入顺序**：外部库 → 内部模块 → 相对路径，组内按字母序
- **格式化**：由 Prettier 自动处理（配置见 `.prettierrc.json`）

### 2.4 Markdown（.md）

- **缩进**：列表项等使用 4 空格（与通用缩进一致）
- **行宽**：段落不强制折行（`proseWrap: never`），但代码块内的行宽建议不超过 120
- **标题层级**：使用 `#` 风格，末尾不留空格
- **列表**：使用 `-` 或 `*`，子项缩进 4 空格
- **代码块**：指定语言（如 ```cpp）
- **格式化**：由 Prettier 处理

### 2.5 JSON / YAML / CMake

- **JSON**
    - 缩进 4 空格
    - 键名双引号，末尾逗号（`trailingComma: all`）
    - 由 Prettier 格式化
- **YAML**
    - 缩进 **2 空格**（与常见 CI/CD 配置保持一致）
    - 由 Prettier 覆盖规则格式化
- **CMake（CMakeLists.txt / .cmake）**
    - 缩进 4 空格
    - 命令大写（如 `ADD_EXECUTABLE`），参数小写，保持可读性
    - 行尾注释规则同通用

### 2.6 Godot / GDScript（.gd / .tscn / .tres / .godot / .import）

#### 2.6.1 通用

- **缩进**：4 空格，禁用 Tab
- **换行符**：CRLF
- **文件末尾**：保留一个空行
- **最大行长度**：120 字符
- **字符编码**：UTF-8 without BOM
- **Godot 版本**：锁 Godot 4.x stable，不中途升级
- **渲染**：2D，优先 Compatibility，便于 Android

#### 2.6.2 命名规则

| 类型     | 规则              | 示例                                   |
| -------- | ----------------- | -------------------------------------- |
| 脚本文件 | `snake_case.gd`   | `player.gd`、`rule_engine.gd`          |
| 类名     | `PascalCase`      | `class_name RuleEngine`                |
| 节点名   | `PascalCase`      | `Player`、`GoldKing`                   |
| 场景文件 | `PascalCase.tscn` | `Main.tscn`、`Player.tscn`             |
| 资源文件 | `snake_case.tres` | `theme_dark.tres`                      |
| 变量     | `snake_case`      | `hp`、`attack_cd`                      |
| 函数     | `snake_case`      | `take_damage()`                        |
| 常量     | `UPPER_CASE`      | `MAX_CHAIN_DEPTH`                      |
| 信号     | `snake_case`      | `chain_changed`                        |
| 私有成员 | 前缀 `_`          | `_queue`、`_listeners`                 |
| 事件常量 | `UPPER_CASE`      | `ON_KILL`、`ON_CRIT`、`ON_PICKUP_GOLD` |

#### 2.6.3 类型注解

- 尽量使用显式类型
- 能推断时可用 `:=`
- 函数参数与返回值尽量写类型

```gdscript
func _on_event(event_name: String, payload: Dictionary) -> void:
    pass

var chain_count: int = 0
var equipped_rules: Array[Dictionary] = []
```

#### 2.6.4 节点引用

- 使用 `@onready`
- 节点路径尽量短，复杂节点在 `_ready()` 中缓存

```gdscript
@onready var player: CharacterBody2D = $Player
@onready var enemies: Node2D = $Enemies
```

#### 2.6.5 事件总线

- 事件常量统一放在 `EventBus`
- 订阅与取消订阅成对出现
- 场景对象在 `_exit_tree()` 中断开连接

```gdscript
func _ready() -> void:
    EventBus.subscribe(EventBus.ON_CRIT, _on_crit)

func _exit_tree() -> void:
    EventBus.unsubscribe(EventBus.ON_CRIT, _on_crit)
```

#### 2.6.6 数据驱动

- 规则牌、数值、叙事碎片使用 JSON
- JSON 字段名统一小写下划线
- 加载失败必须打印错误，不静默跳过
- 数值集中在 `data/balance.json`，不硬编码在脚本

#### 2.6.7 实体与对象池

- 高频创建销毁的实体（敌人、金币、子弹、爆炸、伤害数字）使用对象池
- 实体数量按规划设定上限（敌人 30、金币 100、子弹 50）
- 死亡后回收到池，不直接 `queue_free()`（对象池实体）

#### 2.6.8 场景与资源文件

- `.tscn`、`.tres`、`.godot` 是 Godot 工程文件，**不经过任何外部格式化器**
- 用 Godot 编辑器修改，不要手动编辑或用 Prettier 处理
- `.import` 文件通常要提交，不要忽略

#### 2.6.9 目录结构

```text
project.godot
README.md
docs/
core/
  event_bus/
  combat/
  entities/
  rules/
  rooms/
  meta/
data/
  rules/
  balance.json
  narrative/
ui/
  hud/
  card_pick/
  result/
  codex/
  pause/
  tutorial/
  common/
art/
  placeholder/
  rules/
  enemies/
  items/
  scene/
  ui/
  vfx/
  fonts/
audio/
  sfx/
  bgm/
tests/
```

#### 2.6.10 资源命名

```text
规则牌图标：rule_<id>_icon.png
敌人：enemy_<type>_<state>.png
Boss：boss_gold_king_<state>.png
UI：ui_<area>_<element>_<state>.png
特效：vfx_<type>_<index>.png
音效：sfx_<type>_<index>.ogg
```

示例：

```text
art/rules/rule_crit_bounty_icon.png
art/enemies/enemy_slime_idle.png
art/items/gold_yellow_8.png
ui/hud/chain_label.png
audio/sfx/sfx_chain_01.ogg
```

#### 2.6.11 提交规范

- `feat: 玩家自动攻击`
- `fix: 敌人接触伤害重复触发`
- `docs: 补充事件总线说明`
- `art: 添加规则牌图标`
- `data: 调整 balance.json`
- `chore: 更新 .gitignore`

要求：

- 每日至少 1 次提交到 `dev`
- 关键节点合并到 `main`
- 小步提交，不混入无关改动
- 构建产物、本地存档、签名文件不提交

#### 2.6.12 不提交内容

```text
.godot/
export/
*.tmp
*.translation
*.apk
*.aab
*.exe
*.pck
*.zip
*.jks
*.keystore
local.properties
save.json
*.log
.DS_Store
Thumbs.db
```

`.import` 文件通常要提交。 `project.godot`、`export_presets.cfg` 要提交。

---

## 3. 注释与文档规范（通用）

- **行首注释**：放在代码块上方，与代码块缩进一致，`//`、`#` 或 `##` 后跟一个空格
- **行尾注释**：与代码间隔一个 **Tab**，注释符后跟一个空格
- **文档注释**：对于公共 API（C++ 的类/函数、Python 的模块/函数、GDScript 的公共方法），建议使用结构化注释（Doxygen / docstring）描述功能、参数、返回值和注意事项

---

## 4. 日志记录原则（通用）

- **ERROR**：严重影响功能的错误
- **WARN**：可恢复的异常
- **INFO**：重要状态变化（初始化、用户操作等）
- **DEBUG**：详细调试信息（Release 默认可禁用）
- 避免在热点循环或高频回调中输出日志，可采样或条件输出
- GDScript 中事件日志格式：`[EventBus] ON_CRIT {"damage":20,"target":"Enemy_1"}`

---

## 5. 格式化工具总表

| 文件类型 | 格式化工具 | 配置文件 | VSCode 插件 |
| --- | --- | --- | --- |
| GDScript (.gd) | gdformat | `.gdlintrc` / `.gdformatrc` | EddieDover.gdscript-formatter-linter |
| JSON | Prettier | `.prettierrc.json` | esbenp.prettier-vscode |
| Markdown | Prettier | `.prettierrc.json` | esbenp.prettier-vscode |
| YAML | Prettier | `.prettierrc.json` | esbenp.prettier-vscode |
| TypeScript / JavaScript | Prettier | `.prettierrc.json` | esbenp.prettier-vscode |
| C++ | clang-format | `.clang-format` | ms-vscode.cpptools |
| Godot 场景 / 资源 (.tscn / .tres) | 不格式化 | — | — |

---

## 6. GDScript 工具链

### 6.1 安装 gdtoolkit

```bash
# 方式一：pip
python -m pip install gdtoolkit

# 方式二：pipx（推荐，隔离环境）
pipx install "gdtoolkit==4.*"
```

安装后确认：

```bash
gdformat --version
gdlint --version
```

### 6.2 VSCode 集成

安装插件 `EddieDover.gdscript-formatter-linter`。保存 `.gd` 文件时自动调用 `gdformat` 格式化，并调用 `gdlint` 检查代码规范。

### 6.3 命令行使用

```bash
# 格式化单个文件
gdformat path/to/script.gd

# 格式化整个目录
gdformat core/ ui/ data/

# 仅检查格式，不修改
gdformat --check core/

# 代码检查
gdlint core/ ui/ data/
```

### 6.4 行宽配置

`.gdlintrc` 中设置：

```yaml
max-line-length: 120
```

与 Prettier 的 `printWidth: 120` 保持一致。

### 6.5 注意事项

- gdformat 会重写文件，建议先提交或暂存当前改动再格式化
- 首次格式化整个项目后，单独提交一次“格式化”提交，避免与逻辑改动混在一起
- `.tscn`、`.tres`、`.godot` 不经过任何外部格式化器

---

## 7. 配置文件清单（仓库根目录）

以下所有配置文件应放置于项目根目录，并提交至版本库。

### 7.1 `.editorconfig`

统一编辑器的基本行为，支持多种 IDE。

```ini
root = true

[*]
charset = utf-8
end_of_line = crlf
indent_style = space
indent_size = 4
trim_trailing_whitespace = true
insert_final_newline = true

[*.{yml,yaml}]
indent_size = 2

[*.md]
trim_trailing_whitespace = false

[*.{gd,tscn,tres,godot,import,cfg,gdshader}]
charset = utf-8

[*.{cpp,h,hpp,c,cc}]
charset = utf-8

[CMakeLists.txt]
charset = utf-8

[*.cmake]
charset = utf-8
```

### 7.2 `.gitattributes`

控制行尾转换和 Git 差异行为。

```gitattributes
# 统一行尾：仓库存储 LF，检出 CRLF
* text=auto eol=crlf

# Godot 文本文件
*.gd        text charset=utf-8
*.tscn      text charset=utf-8
*.tres      text charset=utf-8
*.godot     text charset=utf-8
*.import    text charset=utf-8
*.cfg       text charset=utf-8
*.json      text charset=utf-8
*.md        text charset=utf-8
*.txt       text charset=utf-8
*.csv       text charset=utf-8
*.gdshader  text charset=utf-8

# C++ / CMake
*.cpp text charset=utf-8
*.h   text charset=utf-8
*.hpp text charset=utf-8
*.cc  text charset=utf-8
CMakeLists.txt text charset=utf-8
*.cmake text charset=utf-8

# 二进制文件
*.png     binary
*.jpg     binary
*.jpeg    binary
*.gif     binary
*.webp    binary
*.ogg     binary
*.wav     binary
*.mp3     binary
*.ttf     binary
*.otf     binary
*.mp4     binary
*.apk     binary
*.aab     binary
*.exe     binary
*.pck     binary
*.zip     binary
*.jks     binary
*.keystore binary

# SVG 是文本
*.svg text charset=utf-8
```

### 7.3 `.clang-format`（C++ 专用）

```yaml
Language: Cpp
BasedOnStyle: LLVM
Standard: c++20

IndentWidth: 4
TabWidth: 4
UseTab: Never
ContinuationIndentWidth: 4
AlignAfterOpenBracket: DontAlign

BreakBeforeBraces: Custom
BraceWrapping:
    AfterCaseLabel: false
    AfterClass: false
    AfterControlStatement: Never
    AfterEnum: false
    AfterFunction: false
    AfterNamespace: false
    AfterStruct: false
    AfterUnion: false
    AfterExternBlock: false
    BeforeCatch: true
    BeforeElse: true
    BeforeLambdaBody: false
    BeforeWhile: false
    IndentBraces: false
    SplitEmptyFunction: false
    SplitEmptyRecord: false
    SplitEmptyNamespace: false

AccessModifierOffset: -4
NamespaceIndentation: All

DerivePointerAlignment: false
PointerAlignment: Left
ReferenceAlignment: Left

SpaceAfterTemplateKeyword: false
SpaceBeforeParens: ControlStatements

BreakConstructorInitializers: BeforeComma
ConstructorInitializerIndentWidth: 4

# 列宽不限（保留现有长行，如 LOG 宏）
ColumnLimit: 0

AllowShortCaseLabelsOnASingleLine: true
AllowShortIfStatementsOnASingleLine: WithoutElse

SortIncludes: CaseSensitive
IncludeBlocks: Preserve

IndentCaseLabels: false
IndentPPDirectives: None

MaxEmptyLinesToKeep: 2
```

### 7.4 `.prettierrc.json`

适用于 JSON、Markdown、YAML、TypeScript 等。

```json
{
    "tabWidth": 4,
    "useTabs": false,
    "printWidth": 120,
    "endOfLine": "crlf",
    "proseWrap": "never",
    "singleQuote": true,
    "semi": true,
    "arrowParens": "always",
    "trailingComma": "all",
    "bracketSpacing": true,
    "jsxSingleQuote": false,
    "overrides": [
        {
            "files": "*.yml",
            "options": {
                "tabWidth": 2
            }
        },
        {
            "files": "*.yaml",
            "options": {
                "tabWidth": 2
            }
        }
    ]
}
```

### 7.5 `.gdlintrc`

GDScript 代码检查规则。

```yaml
class-definitions-order:
    - tools
    - classnames
    - extends
    - docstrings
    - signals
    - enums
    - consts
    - exports
    - pub_vars
    - prv_vars
    - onready_vars
    - others
    - funcs

max-file-lines: 2000
max-line-length: 120
max-public-methods: 30
function-arguments-number: 8
max-nested-blocks: 4
max-returns: 4
max-branches: 12
max-locals: 15
tab-characters: 4
```

### 7.6 `.gdformatrc`（可选）

如果 gdformat 插件支持读取该文件，可加入；若不支持可删除。

```yaml
line-length: 120
```

### 7.7 `.vscode/settings.json.default`

团队共享编辑器配置模板。个人使用时复制为 `.vscode/settings.json`，并填入自己的 Godot 路径。

```json
{
    "godotTools.editorPath.godot4": "<请替换为你的 Godot 4 tools 版可执行文件路径>",

    "editor.formatOnSave": true,
    "editor.tabSize": 4,
    "editor.insertSpaces": true,
    "editor.defaultFormatter": "esbenp.prettier-vscode",

    "prettier.requireConfig": true,
    "prettier.configPath": ".prettierrc.json",

    "C_Cpp.formatting": "clangFormat",
    "C_Cpp.clang_format_style": "file",
    "C_Cpp.clang_format_fallbackStyle": "{ BasedOnStyle: LLVM, IndentWidth: 4, UseTab: Never }",

    "[gdscript]": {
        "editor.defaultFormatter": "EddieDover.gdscript-formatter-linter",
        "editor.formatOnSave": true
    },
    "[json]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[jsonc]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[markdown]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[yaml]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[typescript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[typescriptreact]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    },
    "[cpp]": {
        "editor.defaultFormatter": "ms-vscode.cpptools"
    },
    "[h]": {
        "editor.defaultFormatter": "ms-vscode.cpptools"
    },
    "[hpp]": {
        "editor.defaultFormatter": "ms-vscode.cpptools"
    },
    "[tscn]": {
        "editor.formatOnSave": false
    },
    "[tres]": {
        "editor.formatOnSave": false
    },
    "[godot]": {
        "editor.formatOnSave": false
    },

    "gdscript-formatter-linter.lintOnSave": true,
    "gdscript-formatter-linter.formatOnSave": true,

    "files.eol": "\r\n",
    "files.insertFinalNewline": true,
    "files.trimTrailingWhitespace": true
}
```

**使用说明：**

1. 克隆仓库后，复制模板：

    ```bash
    cp .vscode/settings.json.default .vscode/settings.json
    ```

    Windows PowerShell：

    ```powershell
    Copy-Item .vscode\settings.json.default .vscode\settings.json
    ```

2. 打开 `.vscode/settings.json`，把第一行的占位符替换为你本地的 Godot 4 编辑器路径。
    - 必须指向 **tools 版**，不能是 server 或 headless 版。
    - Windows 示例：`"C:/Program Files/Godot/godot.windows.opt.tools.64.exe"`
    - macOS 示例：`"/Applications/Godot.app/Contents/MacOS/Godot"`
    - Linux 示例：`"/usr/local/bin/godot4"`

3. 重启 VS Code，Godot Tools 插件即可正常识别项目。

4. `.vscode/settings.json` 已被 `.gitignore` 忽略，不会提交。

### 7.8 `.vscode/extensions.json`

推荐插件列表。

```json
{
    "recommendations": [
        "geequlim.godot-tools",
        "EddieDover.gdscript-formatter-linter",
        "esbenp.prettier-vscode",
        "EditorConfig.EditorConfig",
        "ms-vscode.cpptools"
    ]
}
```

如果团队不写 C++，可以移除 `ms-vscode.cpptools`。

---

## 8. 附加建议

- **TypeScript 项目**：可搭配 ESLint（如 `@typescript-eslint`），并配置 `.eslintrc.js` 与 Prettier 协同工作（使用 `eslint-config-prettier`）
- **Python 项目**：推荐使用 `black` + `isort`，并可在 `.vscode/settings.json` 中配置 `"[python]": { "editor.defaultFormatter": "ms-python.python" }` 并启用 `"python.formatting.provider": "black"`
- **Godot 项目**：推荐使用 `gdformat` + `gdlint`，通过 `EddieDover.gdscript-formatter-linter` 集成到 VSCode
- **CI/CD 检查**：可在 GitHub Actions 或 GitLab CI 中运行 `clang-format --dry-run`、`prettier --check` 和 `gdlint` 以保持一致性

---

## 9. 检查清单

提交代码前，确认以下各项：

- [ ] 文件编码为 UTF-8 without BOM
- [ ] 文件末尾有空行
- [ ] 行尾为 CRLF
- [ ] 没有引入 Tab 缩进
- [ ] 行宽不超过 120 字符（Markdown 段落除外）
- [ ] `.gd` 文件 gdformat 格式化通过
- [ ] `.gd` 文件 gdlint 无报错
- [ ] JSON 文件 Prettier 格式化通过
- [ ] Markdown 文件 Prettier 格式化通过
- [ ] `.tscn` / `.tres` / `.godot` 没有被格式化工具改动
- [ ] 没有提交构建产物、本地存档、签名文件
- [ ] 提交信息符合规范

```


```
