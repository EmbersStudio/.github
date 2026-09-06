# 通用编码规范（CodingStyle.md）

## 1. 通用原则（适用于所有语言）

- **字符编码**：**UTF‑8 without BOM**（所有文本文件）
- **换行符**：**CRLF**（Windows 风格）—— 仓库存储 `LF`，检出自动转换为 `CRLF`（由 `.gitattributes` 控制）
- **文件末尾**：保留一个空行（`insert_final_newline = true`）
- **缩进**：统一使用 **4 个空格**，**禁用 Tab**（但 YAML 文件可例外使用 2 个空格）
- **行尾空白**：自动修剪（`trim_trailing_whitespace = true`）
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
- **格式化**：由 `clang-format` 自动处理（规则见配置文件）

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
- **格式化**：由 Prettier 自动处理（配置见后）

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

---

## 3. 注释与文档规范（通用）

- **行首注释**：放在代码块上方，与代码块缩进一致，`//` 或 `#` 后跟一个空格
- **行尾注释**：与代码间隔一个 **Tab**，注释符后跟一个空格
- **文档注释**：对于公共 API（C++ 的类/函数、Python 的模块/函数），建议使用结构化注释（Doxygen / docstring）描述功能、参数、返回值和注意事项。

---

## 4. 日志记录原则（C++/Python 通用）

- **ERROR**：严重影响功能的错误
- **WARN**：可恢复的异常
- **INFO**：重要状态变化（初始化、用户操作等）
- **DEBUG**：详细调试信息（Release 默认可禁用）
- 避免在热点循环或高频回调中输出日志，可采样或条件输出。

---

# 5. 配置文件清单（仓库根目录）

以下所有配置文件应放置于项目根目录，并提交至版本库。

### 5.1 `.editorconfig`

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

[*.{cpp,h,hpp,c,cc}]
charset = utf-8

[*.md]
charset = utf-8

[CMakeLists.txt]
charset = utf-8

[*.cmake]
charset = utf-8
```

### 5.2 `.gitattributes`

控制行尾转换和 Git 差异行为。

```gitattributes
# 统一行尾：仓库存储 LF，检出 CRLF
* text=auto eol=crlf

# 显式声明文本文件编码（便于 diff）
*.cpp text charset=utf-8
*.h   text charset=utf-8
*.hpp text charset=utf-8
*.md  text charset=utf-8
*.txt text charset=utf-8
CMakeLists.txt text charset=utf-8
*.cmake text charset=utf-8

# 二进制文件（默认已处理，可显式声明）
*.jpg binary
*.png binary
*.gif binary
*.pdf binary
*.docx binary
```

### 5.3 `.clang-format`（C++ 专用，采用详细版本）

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

### 5.4 `.prettierrc.json`（适用于 JSON、Markdown、YAML、TypeScript 等）

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
    }
  ]
}
```

### 5.5 VS Code 项目工作区设置（`.vscode/settings.json`）

确保编辑器自动使用正确的格式化工具。

```json
{
  // 通用编辑器设置
  "editor.formatOnSave": true,
  "editor.tabSize": 4,
  "editor.insertSpaces": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",

  // Prettier 配置
  "prettier.requireConfig": true,
  "prettier.configPath": ".prettierrc.json",

  // C/C++ 使用 clang-format（通过 ms-vscode.cpptools）
  "C_Cpp.formatting": "clangFormat",
  "C_Cpp.clang_format_style": "file",
  "C_Cpp.clang_format_fallbackStyle": "{ BasedOnStyle: LLVM, IndentWidth: 4, UseTab: Never }",

  // 语言特定格式化器
  "[cpp]": {
    "editor.defaultFormatter": "ms-vscode.cpptools"
  },
  "[h]": {
    "editor.defaultFormatter": "ms-vscode.cpptools"
  },
  "[hpp]": {
    "editor.defaultFormatter": "ms-vscode.cpptools"
  },
  "[json]": {
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
  }
}
```

---

## 6. 附加建议

- **TypeScript 项目**：可搭配 ESLint（如 `@typescript-eslint`），并配置 `.eslintrc.js` 与 Prettier 协同工作（使用 `eslint-config-prettier`）。
- **Python 项目**：推荐使用 `black` + `isort`，并可在 `.vscode/settings.json` 中配置 `"[python]": { "editor.defaultFormatter": "ms-python.python" }` 并启用 `"python.formatting.provider": "black"`。
- **CI/CD 检查**：可在 GitHub Actions 或 GitLab CI 中运行 `clang-format --dry-run` 和 `prettier --check` 以保持一致性。
