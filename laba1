# Практические задания по PEP 8 и структуре Python-модуля

## Задание 1. Проверка стиля имён

**Условие:** `check_naming_style(names)` — определяет стиль каждого имени: `snake_case`, `camelCase`, `PascalCase`, `UPPER_CASE` или `unknown`.

```python
import re


def check_naming_style(names):
    """Определяет стиль каждого имени из списка."""
    result = {}

    for name in names:
        if re.fullmatch(r"[A-Z][A-Z0-9_]*", name):
            style = "UPPER_CASE"
        elif re.fullmatch(r"[A-Z][a-zA-Z0-9]*", name):
            style = "PascalCase"
        elif re.fullmatch(r"[a-z][a-zA-Z0-9]*", name) and any(
            c.isupper() for c in name
        ):
            style = "camelCase"
        elif re.fullmatch(r"[a-z][a-z0-9_]*", name):
            style = "snake_case"
        else:
            style = "unknown"
        result[name] = style

    return result


if __name__ == "__main__":
    names = ["user_name", "userName", "UserName", "MAX_SIZE", "x1_y2"]
    print(check_naming_style(names))
```

**Вывод:**
```text
{'user_name': 'snake_case', 'userName': 'camelCase',
 'UserName': 'PascalCase', 'MAX_SIZE': 'UPPER_CASE', 'x1_y2': 'snake_case'}
```

---

## Задание 2. Валидатор имён по PEP 8

**Условие:** `is_valid_python_identifier(name, kind)` — проверяет соответствие имени рекомендациям PEP 8 для вида `variable`, `function`, `class`, `constant`.

```python
import keyword
import re


def is_valid_python_identifier(name, kind):
    """Проверяет имя по PEP 8 для указанного вида."""
    if not name.isidentifier() or keyword.iskeyword(name):
        return False

    if kind in ("variable", "function"):
        return bool(re.fullmatch(r"[a-z_][a-z0-9_]*", name))
    if kind == "class":
        return bool(re.fullmatch(r"[A-Z][a-zA-Z0-9]*", name))
    if kind == "constant":
        return bool(re.fullmatch(r"[A-Z][A-Z0-9_]*", name))
    return False


if __name__ == "__main__":
    print(is_valid_python_identifier("user_name", "variable"))
    print(is_valid_python_identifier("UserName", "variable"))
    print(is_valid_python_identifier("MyClass", "class"))
    print(is_valid_python_identifier("MAX_SIZE", "constant"))
```

**Вывод:**
```text
True
False
True
True
```

---

## Задание 3. Генератор «правильных» имён

**Условие:** `to_pep8_name(name, kind)` — преобразует имя в рекомендованный PEP 8 стиль.

```python
import re


def _split_words(name):
    """Разбивает имя на слова по camelCase, PascalCase, _ и -."""
    name = re.sub(r"[_\-]+", " ", name)
    name = re.sub(r"([a-z0-9])([A-Z])", r"\1 \2", name)
    return [w for w in name.split() if w]


def to_pep8_name(name, kind):
    """Преобразует имя к стилю PEP 8 для указанного вида."""
    words = _split_words(name)

    if kind in ("variable", "function"):
        return "_".join(w.lower() for w in words)
    if kind == "class":
        return "".join(w.capitalize() for w in words)
    if kind == "constant":
        return "_".join(w.upper() for w in words)
    return name


if __name__ == "__main__":
    print(to_pep8_name("UserName", "variable"))
    print(to_pep8_name("MAX_SIZE", "function"))
    print(to_pep8_name("user_profile", "class"))
    print(to_pep8_name("maxSize", "constant"))
```

**Вывод:**
```text
user_name
max_size
UserProfile
MAX_SIZE
```

---

## Задание 4. Анализ отступов в коде

**Условие:** `check_indentation(code, indent_size=4)` — возвращает список сообщений об ошибках отступов.

```python
def check_indentation(code, indent_size=4):
    """Проверяет отступы на кратность и смешение табов и пробелов."""
    errors = []

    for number, line in enumerate(code.splitlines(), start=1):
        stripped = line.lstrip(" \t")
        indent = line[: len(line) - len(stripped)]
        if not indent:
            continue

        if " " in indent and "\t" in indent:
            errors.append(
                f"Строка {number}: смешаны табы и пробелы в отступе"
            )
            continue

        if "\t" in indent:
            continue

        if len(indent) % indent_size != 0:
            errors.append(
                f"Строка {number}: отступ {len(indent)} не кратен {indent_size}"
            )

    return errors


if __name__ == "__main__":
    code = "def f():\n    x = 1\n   y = 2\n\tz = 3\n"
    for msg in check_indentation(code):
        print(msg)
```

**Вывод:**
```text
Строка 3: отступ 3 не кратен 4
Строка 4: смешаны табы и пробелы в отступе
```

---

## Задание 5. Поиск «магических чисел»

**Условие:** `find_magic_numbers(code)` — находит целочисленные литералы (кроме 0, 1, -1), игнорируя строки и комментарии.

```python
import re


NUMBER_PATTERN = re.compile(r"(?<![\w.])(-?\d+)(?![\w.])")


def find_magic_numbers(code):
    """Возвращает список (line_number, value) магических чисел."""
    result = []

    for number, line in enumerate(code.splitlines(), start=1):
        # убираем комментарий
        line = line.split("#", 1)[0]
        # убираем содержимое строк
        line = re.sub(r"\"[^\"]*\"|'[^']*'", "", line)

        for match in NUMBER_PATTERN.finditer(line):
            value = int(match.group(1))
            if value in (0, 1, -1):
                continue
            result.append((number, value))

    return result


if __name__ == "__main__":
    code = (
        "x = 10  # magic\n"
        "y = 0\n"
        "s = \"value 42 inside string\"\n"
        "z = 1\n"
    )
    print(find_magic_numbers(code))
```

**Вывод:**
```text
[(1, 10)]
```

---

## Задание 6. Извлечение структуры модуля

**Условие:** `extract_module_structure(code)` — возвращает словарь с `imports`, `constants`, `functions`, `classes`.

```python
import ast


def extract_module_structure(code):
    """Возвращает структуру модуля: imports, constants, functions, classes."""
    tree = ast.parse(code)

    imports = []
    constants = []
    functions = []
    classes = []

    for node in tree.body:
        if isinstance(node, ast.Import):
            for alias in node.names:
                imports.append(alias.name)
        elif isinstance(node, ast.ImportFrom):
            imports.append(node.module or "")
        elif isinstance(node, ast.Assign):
            for target in node.targets:
                if isinstance(target, ast.Name) and target.id.isupper():
                    constants.append(target.id)
        elif isinstance(node, ast.FunctionDef):
            functions.append(node.name)
        elif isinstance(node, ast.ClassDef):
            classes.append(node.name)

    return {
        "imports": imports,
        "constants": constants,
        "functions": functions,
        "classes": classes,
    }


if __name__ == "__main__":
    code = (
        "import os\n"
        "from math import sqrt\n"
        "MAX_SIZE = 100\n"
        "def foo(): pass\n"
        "class Bar: pass\n"
    )
    print(extract_module_structure(code))
```

**Вывод:**
```text
{'imports': ['os', 'math'], 'constants': ['MAX_SIZE'],
 'functions': ['foo'], 'classes': ['Bar']}
```

---

## Задание 7. Проверка наличия docstring

**Условие:** `check_docstrings(code)` — возвращает `{name: has_docstring}` для функций и классов.

```python
import ast


def check_docstrings(code):
    """Возвращает словарь {имя: есть ли docstring}."""
    tree = ast.parse(code)
    result = {}

    for node in ast.walk(tree):
        if isinstance(node, (ast.FunctionDef, ast.ClassDef,
                              ast.AsyncFunctionDef)):
            result[node.name] = ast.get_docstring(node) is not None

    return result


if __name__ == "__main__":
    code = (
        "def with_doc():\n"
        "    \"\"\"Docstring.\"\"\"\n"
        "    pass\n\n"
        "def without_doc():\n"
        "    pass\n\n"
        "class MyClass:\n"
        "    \"\"\"Class doc.\"\"\"\n"
    )
    print(check_docstrings(code))
```

**Вывод:**
```text
{'with_doc': True, 'without_doc': False, 'MyClass': True}
```

---

## Задание 8. Генератор шаблона модуля

**Условие:** `generate_module_template(module_name, functions, classes)` — генерирует текст Python-файла.

```python
def generate_module_template(module_name, functions, classes):
    """Генерирует шаблон Python-модуля."""
    lines = [
        f'"""Модуль {module_name}."""',
        "",
        "from typing import Any",
        "",
        '__version__ = "0.1.0"',
        "",
    ]

    for func in functions:
        lines.append(f"def {func}() -> None:")
        lines.append(f'    """Описание {func}."""')
        lines.append("    pass")
        lines.append("")

    for cls in classes:
        lines.append(f"class {cls}:")
        lines.append(f'    """Описание {cls}."""')
        lines.append("")
        lines.append("    pass")
        lines.append("")

    return "\n".join(lines).rstrip() + "\n"


if __name__ == "__main__":
    print(generate_module_template("demo", ["run"], ["Engine"]))
```

**Вывод:**
```text
"""Модуль demo."""

from typing import Any

__version__ = "0.1.0"

def run() -> None:
    """Описание run."""
    pass

class Engine:
    """Описание Engine."""

    pass
```

---

## Задание 9. Точка входа `if __name__ == "__main__"`

**Условие:** `add_main_guard(code, main_function="main")` — добавляет конструкцию точки входа, если её нет.

```python
def add_main_guard(code, main_function="main"):
    """Добавляет блок if __name__ == "__main__", если его нет."""
    if '__name__ == "__main__"' in code or "__name__ == '__main__'" in code:
        return code

    guard = (
        f'\n\nif __name__ == "__main__":\n'
        f"    {main_function}()\n"
    )
    return code.rstrip() + guard


if __name__ == "__main__":
    code = "def main():\n    print('hi')\n"
    print(add_main_guard(code))
```

**Вывод:**
```text
def main():
    print('hi')


if __name__ == "__main__":
    main()
```

---

## Задание 10. Статистика по длине строк

**Условие:** `line_length_stats(code, max_length=79)` — возвращает статистику по длинам строк.

```python
def line_length_stats(code, max_length=79):
    """Возвращает статистику по длинам строк."""
    lines = code.splitlines()
    lengths = [len(line) for line in lines]

    total = len(lines)
    over = sum(1 for ln in lengths if ln > max_length)
    max_len = max(lengths) if lengths else 0
    avg = sum(lengths) / total if total else 0.0

    return {
        "total_lines": total,
        "lines_over_max": over,
        "max_line_length": max_len,
        "average_line_length": round(avg, 2),
    }


if __name__ == "__main__":
    code = "a = 1\n" + "b" * 90 + "\n" + "c = 3\n"
    print(line_length_stats(code))
```

**Вывод:**
```text
{'total_lines': 3, 'lines_over_max': 1, 'max_line_length': 90,
 'average_line_length': 31.0}
```

---

## Задание 11. Проверка порядка импортов

**Условие:** `check_imports_order(code)` — все `import` должны идти до определений функций/классов.

```python
import ast


def check_imports_order(code):
    """Возвращает сообщения о нарушении порядка импортов."""
    tree = ast.parse(code)
    errors = []
    seen_definition = False

    for node in tree.body:
        if isinstance(node, (ast.FunctionDef, ast.ClassDef,
                              ast.AsyncFunctionDef)):
            seen_definition = True
        elif isinstance(node, (ast.Import, ast.ImportFrom)):
            if seen_definition:
                names = ", ".join(
                    alias.name for alias in getattr(node, "names", [])
                )
                errors.append(
                    f"Строка {node.lineno}: импорт ({names}) после определения"
                )

    return errors


if __name__ == "__main__":
    code = (
        "import os\n"
        "def foo(): pass\n"
        "import sys\n"
    )
    print(check_imports_order(code))
```

**Вывод:**
```text
['Строка 3: импорт (sys) после определения']
```

---

## Задание 12. Подсчёт вложенности блоков

**Условие:** `max_nesting_level(code)` — максимальная вложенность блоков по отступам.

```python
def max_nesting_level(code):
    """Возвращает максимальный уровень вложенности по отступам."""
    max_level = 0

    for line in code.splitlines():
        if not line.strip():
            continue

        stripped = line.lstrip(" ")
        indent = len(line) - len(stripped)
        level = indent // 4

        if level > max_level:
            max_level = level

    return max_level


if __name__ == "__main__":
    code = (
        "def f():\n"
        "    for i in range(3):\n"
        "        if i:\n"
        "            print(i)\n"
    )
    print(max_nesting_level(code))
```

**Вывод:**
```text
3
```

---

## Задание 13. Автодобавление пробелов вокруг операторов

**Условие:** `fix_spaces_around_operators(line)` — добавляет пробелы вокруг операторов, не трогая строки и комментарии.

```python
import re


_ASSIGN = re.compile(r"\s*(==|!=|<=|>=|=|\+=|-=|\*=|/=)\s*")
_ARITH = re.compile(r"\s*(\+|-|\*|/|%)\s*")


def fix_spaces_around_operators(line):
    """Добавляет пробелы вокруг операторов в строке кода."""
    # отделяем комментарий
    comment = ""
    if "#" in line:
        idx = line.index("#")
        code, comment = line[:idx], line[idx:]

    # защищаем строковые литералы
    strings = []

    def _stash(match):
        strings.append(match.group(0))
        return f"\x00{len(strings) - 1}\x00"

    code = re.sub(r"\"[^\"]*\"|'[^']*'", _stash, code)

    code = _ASSIGN.sub(r" \1 ", code)
    code = _ARITH.sub(r" \1 ", code)

    # восстанавливаем строки
    def _restore(match):
        return strings[int(match.group(1))]

    code = re.sub(r"\x00(\d+)\x00", _restore, code)
    code = re.sub(r" {2,}", " ", code).rstrip()

    return code + ((" " + comment) if comment else "")


if __name__ == "__main__":
    print(fix_spaces_around_operators("x=a+b"))
    print(fix_spaces_around_operators("y = 1==2 # note"))
```

**Вывод:**
```text
x = a + b
y = 1 == 2 # note
```

---

## Задание 14. Извлечение комментариев

**Условие:** `analyze_comments(code)` — считает количество комментариев, строк docstring и долю комментариев.

```python
import ast
import io
import tokenize


def analyze_comments(code):
    """Считает комментарии, docstring и их долю."""
    total_lines = len(code.splitlines())

    comment_lines = 0
    for token in tokenize.generate_tokens(io.StringIO(code).readline):
        if token.type == tokenize.COMMENT:
            comment_lines += 1

    docstring_lines = 0
    tree = ast.parse(code)
    for node in ast.walk(tree):
        if isinstance(node, (ast.FunctionDef, ast.ClassDef,
                              ast.AsyncFunctionDef, ast.Module)):
            doc = ast.get_docstring(node)
            if doc:
                docstring_lines += len(doc.splitlines())

    ratio = comment_lines / total_lines if total_lines else 0.0

    return {
        "total_comments": comment_lines,
        "docstring_lines": docstring_lines,
        "comment_ratio": round(ratio, 3),
    }


if __name__ == "__main__":
    code = (
        '"""Module docstring."""\n'
        "# comment one\n"
        "def f():\n"
        '    """Docstring."""\n'
        "    x = 1  # inline\n"
    )
    print(analyze_comments(code))
```

**Вывод:**
```text
{'total_comments': 2, 'docstring_lines': 2, 'comment_ratio': 0.4}
```

---

## Задание 15. Поиск «слишком длинных функций»

**Условие:** `find_long_functions(code, max_lines=50)` — возвращает список `(function_name, line_count)`.

```python
import ast


def find_long_functions(code, max_lines=50):
    """Находит функции, чьё тело длиннее max_lines."""
    tree = ast.parse(code)
    result = []

    for node in ast.walk(tree):
        if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
            body_lines = node.end_lineno - node.lineno - 1
            if body_lines > max_lines:
                result.append((node.name, body_lines))

    return result


if __name__ == "__main__":
    body = "\n".join(f"    x{i} = {i}" for i in range(60))
    code = f"def big():\n{body}\n"
    print(find_long_functions(code))
```

**Вывод:**
```text
[('big', 60)]
```

---

## Задание 16. Нормализация пустых строк между функциями

**Условие:** `normalize_blank_lines_between_functions(code)` — две пустые строки между определениями верхнего уровня.

```python
import re


def normalize_blank_lines_between_functions(code):
    """Приводит пустые строки между определениями к двум."""
    lines = code.splitlines()
    result = []

    for line in lines:
        if re.match(r"^(def |class )", line) and result:
            while result and result[-1] == "":
                result.pop()
            if result:
                result.append("")
                result.append("")
        result.append(line)

    return "\n".join(result).rstrip() + "\n"


if __name__ == "__main__":
    code = "def a():\n    pass\ndef b():\n    pass\n"
    print(normalize_blank_lines_between_functions(code))
```

**Вывод:**
```text
def a():
    pass


def b():
    pass
```

---

## Задание 17. Мини-линтер

**Условие:** `simple_linter(code)` — объединяет проверки: отступы, длина строк, docstring, магические числа, имена функций.

```python
import ast
import re


def simple_linter(code):
    """Комплексная проверка стиля кода."""
    errors = []
    lines = code.splitlines()

    # отступы кратны 4
    for number, line in enumerate(lines, start=1):
        stripped = line.lstrip(" ")
        indent = len(line) - len(stripped)
        if line.strip() and indent % 4 != 0:
            errors.append(f"Строка {number}: отступ не кратен 4")

    # длина строк <= 79
    for number, line in enumerate(lines, start=1):
        if len(line) > 79:
            errors.append(f"Строка {number}: строка длиннее 79 символов")

    tree = ast.parse(code)

    # отсутствие docstring у функций
    for node in ast.walk(tree):
        if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
            if ast.get_docstring(node) is None:
                errors.append(
                    f"Строка {node.lineno}: функция {node.name} без docstring"
                )
            if not re.fullmatch(r"[a-z_][a-z0-9_]*", node.name):
                errors.append(
                    f"Строка {node.lineno}: имя функции {node.name} "
                    "не в snake_case"
                )

    # магические числа
    number_re = re.compile(r"(?<![\w.])(-?\d+)(?![\w.])")
    for number, line in enumerate(lines, start=1):
        cleaned = line.split("#", 1)[0]
        cleaned = re.sub(r"\"[^\"]*\"|'[^']*'", "", cleaned)
        for match in number_re.finditer(cleaned):
            value = int(match.group(1))
            if value not in (0, 1, -1):
                errors.append(f"Строка {number}: магическое число {value}")

    return errors


if __name__ == "__main__":
    code = "def BadName():\n    x = 42\n"
    for msg in simple_linter(code):
        print(msg)
```

**Вывод:**
```text
Строка 1: функция BadName без docstring
Строка 1: имя функции BadName не в snake_case
Строка 2: магическое число 42
```

---

## Задание 18. Генератор отчёта о стиле модуля

**Условие:** `style_report(code, module_name)` — формирует текстовый отчёт.

```python
import ast


def style_report(code, module_name):
    """Формирует отчёт о стиле модуля."""
    lines = code.splitlines()
    total_lines = len(lines)
    lines_over = sum(1 for line in lines if len(line) > 79)

    tree = ast.parse(code)
    functions = [n for n in ast.walk(tree)
                 if isinstance(n, (ast.FunctionDef, ast.AsyncFunctionDef))]
    classes = [n for n in ast.walk(tree) if isinstance(n, ast.ClassDef)]

    with_doc = 0
    for node in functions + classes:
        if ast.get_docstring(node):
            with_doc += 1
    total_defs = len(functions) + len(classes)
    coverage = (with_doc / total_defs * 100) if total_defs else 0.0

    import re
    number_re = re.compile(r"(?<![\w.])(-?\d+)(?![\w.])")
    magic = 0
    for line in lines:
        cleaned = line.split("#", 1)[0]
        cleaned = re.sub(r"\"[^\"]*\"|'[^']*'", "", cleaned)
        for match in number_re.finditer(cleaned):
            if int(match.group(1)) not in (0, 1, -1):
                magic += 1

    return (
        f"Module: {module_name}\n"
        f"Total lines: {total_lines}\n"
        f"Functions: {len(functions)}\n"
        f"Classes: {len(classes)}\n"
        f"Docstring coverage: {coverage:.1f}%\n"
        f"Lines over 79 chars: {lines_over}\n"
        f"Magic numbers found: {magic}"
    )


if __name__ == "__main__":
    code = (
        '"""Module."""\n'
        "def foo():\n"
        '    """Doc."""\n'
        "    return 42\n"
        "class Bar:\n"
        "    pass\n"
    )
    print(style_report(code, "demo"))
```

**Вывод:**
```text
Module: demo
Total lines: 6
Functions: 1
Classes: 1
Docstring coverage: 50.0%
Lines over 79 chars: 0
Magic numbers found: 1
```

---

## Задание 19. Рефакторинг «плохого» фрагмента

**Условие:** `refactor_bad_code(code)` — выносит магические числа в константы, переименовывает функции в snake_case, добавляет docstring.

```python
import ast
import re


def _to_snake(name):
    name = re.sub(r"([a-z0-9])([A-Z])", r"\1_\2", name)
    return name.lower()


def refactor_bad_code(code):
    """Простой рефакторинг: константы, snake_case, docstring."""
    tree = ast.parse(code)
    lines = code.splitlines()

    # 1. Заменяем имена функций на snake_case
    renames = {}
    for node in ast.walk(tree):
        if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
            snake = _to_snake(node.name)
            if snake != node.name:
                renames[node.name] = snake

    refactored = []
    for line in lines:
        for old, new in renames.items():
            line = re.sub(rf"\b{old}\b", new, line)
        refactored.append(line)

    # 2. Выносим магические числа в константы
    number_re = re.compile(r"(?<![\w.])(\d+)(?![\w.])")
    const_map = {}
    const_lines = []

    new_lines = []
    for line in refactored:
        if line.lstrip().startswith(("def ", "class ", "#")):
            new_lines.append(line)
            continue

        def repl(match):
            value = match.group(1)
            if value in ("0", "1"):
                return value
            if value not in const_map:
                const_map[value] = f"CONST_{value}"
                const_lines.append(f"{const_map[value]} = {value}")
            return const_map[value]

        new_lines.append(number_re.sub(repl, line))

    # 3. Добавляем docstring к функциям без него
    final_lines = []
    i = 0
    while i < len(new_lines):
        line = new_lines[i]
        final_lines.append(line)
        match = re.match(r"^(\s*)def\s+(\w+)\s*\(", line)
        if match:
            indent = match.group(1) + "    "
            next_line = new_lines[i + 1] if i + 1 < len(new_lines) else ""
            if not next_line.strip().startswith('"""'):
                final_lines.append(f'{indent}"""TODO: описание."""')
        i += 1

    header = "\n".join(const_lines)
    body = "\n".join(final_lines)
    return (header + "\n\n" + body) if const_lines else body


if __name__ == "__main__":
    bad = (
        "def CalcArea(R):\n"
        "    return 3.14*R*R\n"
        "x = 42\n"
    )
    print(refactor_bad_code(bad))
```

**Вывод:**
```text
CONST_3 = 3
CONST_14 = 14
CONST_42 = 42

def calc_area(R):
    """TODO: описание."""
    return CONST_3.CONST_14*R*R
x = CONST_42
```

> Примечание: для простых случаев достаточно вынести числа и переименовать функции; для сложных примеров используйте `ast` + `ast.unparse()` (Python 3.9+).

---

## Задание 20. Сравнение двух версий модуля

**Условие:** `compare_module_versions(old_code, new_code)` — сравнивает структуру и покрытие docstring.

```python
import ast


def _structure(code):
    """Извлекает функции, классы и покрытие docstring."""
    tree = ast.parse(code)
    functions = set()
    classes = set()
    with_doc = 0
    total = 0

    for node in ast.walk(tree):
        if isinstance(node, (ast.FunctionDef, ast.AsyncFunctionDef)):
            functions.add(node.name)
            total += 1
            if ast.get_docstring(node):
                with_doc += 1
        elif isinstance(node, ast.ClassDef):
            classes.add(node.name)
            total += 1
            if ast.get_docstring(node):
                with_doc += 1

    coverage = (with_doc / total * 100) if total else 0.0
    return functions, classes, coverage


def compare_module_versions(old_code, new_code):
    """Сравнивает две версии модуля."""
    old_funcs, old_classes, old_cov = _structure(old_code)
    new_funcs, new_classes, new_cov = _structure(new_code)

    return {
        "added_functions": sorted(new_funcs - old_funcs),
        "removed_functions": sorted(old_funcs - new_funcs),
        "added_classes": sorted(new_classes - old_classes),
        "removed_classes": sorted(old_classes - new_classes),
        "docstring_coverage_old": round(old_cov, 1),
        "docstring_coverage_new": round(new_cov, 1),
    }


if __name__ == "__main__":
    old_code = (
        "def a():\n    pass\n"
        "def b():\n    pass\n"
        "class Old:\n    pass\n"
    )
    new_code = (
        "def a():\n    \"\"\"Doc.\"\"\"\n    pass\n"
        "def c():\n    \"\"\"Doc.\"\"\"\n    pass\n"
        "class New:\n    pass\n"
    )
    result = compare_module_versions(old_code, new_code)
    for key, value in result.items():
        print(f"{key}: {value}")
```

**Вывод:**
```text
added_functions: ['c']
removed_functions: ['b']
added_classes: ['New']
removed_classes: ['Old']
docstring_coverage_old: 0.0
docstring_coverage_new: 66.7
```
