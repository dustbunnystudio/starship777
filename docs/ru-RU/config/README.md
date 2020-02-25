# Конфигурация

::: tip Подсказка

🔥 Конфигурация все еще находится в стадии разработки. Множество новых опций будут доступны в будущих версиях.

:::

Чтобы начать конфигурацию Starship, создайте следующий файл: `~/.config/starship.toml`.

```sh
$ mkdir -p ~/.config && touch ~/.config/starship.toml
```

Вся конфигурация Starship выполняется в этом файле [TOML](https://github.com/toml-lang/toml):

```toml
# Не добавлять пустую строку в начале ввода
add_newline = false

# Поменять символ "❯" на символ "➜"
[character]      # Имя настраемого модуля - "character"
symbol = "➜"      # Сегменту "symbol" присваеваем значение "➜"

# Отключить модуль пакетов, полностью скрывая его из терминала
[package]
disabled = true
```

Вы можете изменить расположение файла `starship.toml` переменной окружения `STARSHIP_CONFIG`:
```sh
export STARSHIP_CONFIG=~/.starship
```

### Терминология

**Модуль**: Компонент строки, дающий информацию на основе контекстной информации вашей ОС. Например, модуль "nodejs" показывает установленную версию NodeJS на вашем компьютере, если вы находитесь в директории проекта NodeJS.

**Сегмент**: Меньшие подкомпоненты, составляющие модуль. Например, сегмент "symbol" в модуле "nodejs" хранит символ, показываемый перед версией NodeJS (⬢, по умолчанию).

Вот представление модуля "nodejs". В примере, "symbol" и "version" - его сегменты. Также, каждый модуль имеет префикс и суффикс, являющиеся цветом терминала по умолчанию.

```
[prefix]      [symbol]     [version]    [suffix]
 "via "         "⬢"        "v10.4.1"       ""
```

### Стиль строк

В Starship, большинство модулей позволяют настроить стили отображения. Это делается записью (обычно называется `style`), которая представляет собой строку, определяющую конфигурацию. Ниже приведены несколько примеров стилей строк, а также, их действия. Подробнее о полном синтаксисе можно прочитать в [расширенном разделе конфигурации](/advanced-config/).

- `"fg:green bg:blue"` устанавливает зеленый текст на синем фоне
- `"bg:blue fg:bright-green"` устанавливает ярко-зеленый текст на синем фоне
- `"bold fg:27"` устанавливает жирный текст с [цветом ANSI](https://i.stack.imgur.com/KTSQa.png) 27
- `"underline bg:#bf5700"` устанавливает подчёркиваемый текст цвета сожженного апельсина
- `"bold italic fg:purple"` устанавливает жирный фиолетовый текст
- `""` выключает все стили

Обратите внимание на то, что, вид стиля зависит от вашего эмулятора терминала. Например, некоторые эмуляторы терминала будут использовать яркие цвета вместо жирного текста, и некоторые цветовые темы используют одинаковые значение для обычных и ярких цветов. Также, чтобы получить курсивный текст, ваш терминал должен поддерживать курсив.

## Командная строка

Ниже находится список опций, применяющихся для всей командной строки.

### Опции

| Переменная     | По умолчанию                    | Описание                                                 |
| -------------- | ------------------------------- | -------------------------------------------------------- |
| `add_newline`  | `true`                          | Добавление пустой строки перед началом командной строки. |
| `prompt_order` | [ссылка](#default-prompt-order) | Настройка порядка появления модулей командной строки.    |
| `scan_timeout` | `30`                            | Тайм-аут запуска сканирования файлов (в миллисекундах).  |

### Пример

```toml
# ~/.config/starship.toml

# Не добавлять пустую строку перед началом командной строки
add_newline = false
# Перезаписать default_prompt_order и использовать пользовательский prompt_order
prompt_order=["rust","line_break","package","line_break","character"]
# Ждать 10 миллисекунд перед запуском сканирования файлов.
scan_timeout = 10
```

### Порядок модулей командной строки по умолчанию

По умолчанию, `prompt_order` определеят порядок появления модулей командной строки, если `prompt_order` пустой или не объявлен. Значение по умолчанию:

```toml
prompt_order = [
    "username",
    "hostname",
    "kubernetes",
    "directory",
    "git_branch",
    "git_commit",
    "git_state",
    "git_status",
    "hg_branch",
    "package",
    "dotnet",
    "elm",
    "golang",
    "haskell",
    "java",
    "nodejs",
    "php",
    "python",
    "ruby",
    "rust",
    "terraform",
    "nix_shell",
    "conda",
    "memory_usage",
    "aws",
    "env_var",
    "crystal",
    "cmd_duration",
    "line_break",
    "jobs",
    "battery",
    "time",
    "character",
]
```

## AWS

Модуль `aws` показывает текущий регион и профиль AWS. Основано на `AWS_REGION`, `AWS_DEFAULT_REGION`, и `AWS_PROFILE` переменных окружения и файле`~/.aws/config`.

### Опции

| Переменная        | По умолчанию    | Описание                                                         |
| ----------------- | --------------- | ---------------------------------------------------------------- |
| `symbol`          | `"☁️ "`         | Символ перед отображением текущего профиля AWS.                  |
| `displayed_items` | `all`           | Выбор элементов. Возможные значения [`all`, `profile`, `region`] |
| `region_aliases`  |                 | Таблица региона псевдонимов, отображаемая вместе с именем AWS.   |
| `style`           | `"bold yellow"` | Стиль модуля.                                                    |
| `disabled`        | `false`         | Отключение модуля `AWS`.                                         |

### Пример

```toml
# ~/.config/starship.toml

[aws]
style = "bold blue"
symbol = "🅰 "
displayed_items = "region"
[aws.region_aliases]
ap-southeast-2 = "au"
us-east-1 = "va"
```

## Батарея

Модуль `battery` показывает насколько заряжена батарея девайса и статус зарядки на данный момент. Модуль виден только, если заряд батареи устройства меньше 10%.

### Опции

| Переменная           | По умолчанию             | Описание                                        |
| -------------------- | ------------------------ | ----------------------------------------------- |
| `full_symbol`        | `"•"`                    | Символ, отображаемый при полной батарее.        |
| `charging_symbol`    | `"⇡"`                    | Символ, показываемый при зарядке аккумулятора.  |
| `discharging_symbol` | `"⇣"`                    | Символ, показываемый при разрядке аккумулятора. |
| `display`            | [link](#battery-display) | Порог отображения и стиль для модуля.           |
| `disabled`           | `false`                  | Отключает модуль `battery`.                     |

<details>
<summary>Также, есть опции для некоторых нетипичных состояний батареи.</summary>

| Переменная       | Описание                                                |
| ---------------- | ------------------------------------------------------- |
| `unknown_symbol` | Символ, отображаемый при неизвестном состоянии батареи. |
| `empty_symbol`   | Символ, отображаемый при пустом состоянии батареи.      |

Примечание: Индикатор батареи будет скрыт при состоянии `unknown` или `empty`, если вы не указали параметр в настройках.

</details>

### Пример

```toml
# ~/.config/starship.toml

[battery]
full_symbol = "🔋"
charging_symbol = "⚡️"
discharging_symbol = "💀"
```

### Отображение батареи

Параметр `display` используется для определения того, когда индикатор батареи должен быть показан (threshhold) и как он выглядит (style). Если `display` не предоставлено. Значение по умолчанию:

```toml
[[battery.display]]
threshold = 10
style = "bold red"
```

#### Опции

Опция `display` представляет собой массив следующей таблицы.

| Переменная  | Описание                                                 |
| ----------- | -------------------------------------------------------- |
| `threshold` | Верхняя граница опции отображения.                       |
| `style`     | Используемый стиль, если используется опция отображения. |

#### Пример

```toml
[[battery.display]] # стиль "bold red" (жирный красный) если заряд между 0% и 10%
threshold = 10
style = "bold red"

[[battery.display]] # стиль "bold yellow" (жирный желтый) если заряд между 10% и 30%
threshold = 30
style = "bold yellow"

# когда заряд батареи больше 30%, индикатор батареи скрыт

```

## Символ

Модуль `character` показывает символ (обычно, стрелка) рядом с вводимым текстом в терминале.

Символ показывает, была ли последняя команда успешной или нет. Это возможно двумя способами: меняя цвет (красный/зеленый) или изменяя его форму (❯/✖). Последнее будет исполняться только в том случае, если переменной `use_symbol_for_status` установлено значение `true`.

### Опции

| Переменная              | По умолчанию   | Описание                                                                                                    |
| ----------------------- | -------------- | ----------------------------------------------------------------------------------------------------------- |
| `symbol`                | `"❯"`          | Символ, используемый перед вводом текста в командной строке.                                                |
| `error_symbol`          | `"✖"`          | Символ, используемый перед вводом текста, если предыдущая команда не удалась.                               |
| `use_symbol_for_status` | `false`        | Показывает статус ошибки путем изменения символа.                                                           |
| `vicmd_symbol`          | `"❮"`          | Символ, используемый перед вводом текста в строке, если командная строка находится в нормальном режиме vim. |
| `style_success`         | `"bold green"` | Используемый стиль, если последняя команда была успешной.                                                   |
| `style_failure`         | `"bold red"`   | Используемый стиль, если последняя команда была не успешной.                                                |
| `disabled`              | `false`        | Отключает модуль `character`.                                                                               |

### Пример

```toml
# ~/.config/starship.toml

[character]
symbol = "➜"
error_symbol = "✗"
use_symbol_for_status = true
```

## Длительность команды

Модуль `cmd_duration` показывает время исполнения последней команды. Модуль будет показан только, если команда заняла более двух секунд, или если задан параметр `min_time`.

::: warning Не подключайте ловушку DEBUG к Bash

Если вы испоьзуете Starship в `bash`, не подключайте ловушку `DEBUG` после запуска `eval $(starship init $0)`, иначе этот модуль сломается.

:::

Пользователи Bash, которым нужна функциональность, подобная preexec, могут использовать [фреймворк bash_preexec от rcaloras](https://github.com/rcaloras/bash-preexec). Просто определите массивы `preexec_functions` и `precmd_functions` перед запуском `eval $(starship init $0)`, а затем продолжайте нормально.

### Опции

| Переменная          | По умолчанию    | Описание                                                             |
| ------------------- | --------------- | -------------------------------------------------------------------- |
| `min_time`          | `2_000`         | Кратчайшая продолжительность для показа времени (в миллисекундах).   |
| `show_milliseconds` | `false`         | Показывать миллисекунды в дополнение к секундам в продолжительности. |
| `prefix`            | `took`          | Префикс, отображаемый перед продолжительностью команды.              |
| `style`             | `"bold yellow"` | Стиль модуля.                                                        |
| `disabled`          | `false`         | Отключает модуль `cmd_duration`.                                     |

### Пример

```toml
# ~/.config/starship.toml

[cmd_duration]
min_time = 500
prefix = "underwent "
```

## Конда

Модуль `conda` показывает текущее окружение conda, если `$CONDA_DEFAULT_ENV` присвоено значение.

::: tip Подсказка

Это не подавляет модификатор командной строки самой conda. Возможно, вы захотите запустить `conda config --set changeps1 False`.

:::

### Опции

| Переменная          | По умолчанию   | Описание                                                                                                                                                                                                     |
| ------------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `truncation_length` | `1`            | Количество каталогов, в которых путь к окружению должен быть усечен, если окружение было создано через `conda create -p [path]`. `0` означает без усечения. Также смотрите модуль [`directory`](#directory). |
| `symbol`            | `"C "`         | Символ перед названием окружения.                                                                                                                                                                            |
| `style`             | `"bold green"` | Стиль модуля.                                                                                                                                                                                                |
| `disabled`          | `false`        | Отключает модуль `conda`.                                                                                                                                                                                    |

### Пример

```toml
# ~/.config/starship.toml

[conda]
style = "dimmed green"
```

## Каталог

Модуль `directory` показывает путь к вашей текущей директории, усеченной до трех родительских папок. Ваш каталог также будет отсечен до корня git репозитория, в котором вы находитесь.

При использовании стиля оболочки fish, вместо скрытия усеченного каталога, вы увидите укороченное имя каталога, зависимое от числа символов вы установите для этой опции.

Например, возьмем `~/Dev/Nix/nixpkgs/pkgs` где `nixpkgs` является корневым репозиторием, и в опции установлено `1`. Вы увидите `~/D/N/nixpkgs/pkgs`, а до этого было бы `nixpkgs/pkgs`.

### Опции

| Переменная          | По умолчанию  | Описание                                                                     |
| ------------------- | ------------- | ---------------------------------------------------------------------------- |
| `truncation_length` | `3`           | Количество родительских папок, к которым должен быть усечен текущий каталог. |
| `truncate_to_repo`  | `true`        | Следует или нет обрезать до корня репозитория git, в котором вы находитесь.  |
| `prefix`            | `"in "`       | Префикс, отображаемый перед папкой.                                          |
| `style`             | `"bold cyan"` | Стиль модуля.                                                                |
| `disabled`          | `false`       | Отключает модуль `directory`.                                                |

<details>
<summary>Этот модуль имеет несколько расширенных опций конфигурации, которые контролируют отображение каталога.</summary>

| Переменная                  | По умолчанию | Описание                                                                          |
| --------------------------- | ------------ | --------------------------------------------------------------------------------- |
| `fish_style_pwd_dir_length` | `0`          | Количество символов, используемых при использовании логики создания пути из fish. |
| `use_logical_path`          | `true`       | Отображает логический путь от оболочки (`PWD`) вместо пути от ОС.                 |

`fish_style_pwd_dir_length` interacts with the standard truncation options in a way that can be surprising at first: if it's non-zero, the components of the path that would normally be truncated are instead displayed with that many characters. For example, the path `/built/this/city/on/rock/and/roll`, which would normally be displayed as as `rock/and/roll`, would be displayed as `/b/t/c/o/rock/and/roll` with `fish_style_pwd_dir_length = 1`--the path components that would normally be removed are displayed with a single character. For `fish_style_pwd_dir_length = 2`, it would be `/bu/th/ci/on/rock/and/roll`.

</details>

### Пример

```toml
# ~/.config/starship.toml

[directory]
truncation_length = 8
```

## Dotnet

The `dotnet` module shows the relevant version of the .NET Core SDK for the current directory. If the SDK has been pinned in the current directory, the pinned version is shown. Otherwise the module shows the latest installed version of the SDK.

This module will only be shown in your prompt when one of the following files are present in the current directory: `global.json`, `project.json`, `*.sln`, `*.csproj`, `*.fsproj`, `*.xproj`. You'll also need the .NET Core command-line tools installed in order to use it correctly.

Internally, this module uses its own mechanism for version detection. Typically it is twice as fast as running `dotnet --version`, but it may show an incorrect version if your .NET project has an unusual directory layout. If accuracy is more important than speed, you can disable the mechanism by setting `heuristic = false` in the module options.

### Опции

| Переменная  | По умолчанию  | Описание                                                          |
| ----------- | ------------- | ----------------------------------------------------------------- |
| `symbol`    | `"•NET "`     | Символ перед отображением текущей версии dotnet.                  |
| `heuristic` | `true`        | Использовать быстрое определение версии, для сохранения скорости. |
| `style`     | `"bold blue"` | Стиль модуля.                                                     |
| `disabled`  | `false`       | Отключает модуль `dotnet`.                                        |

### Пример

```toml
# ~/.config/starship.toml

[dotnet]
symbol = "🥅 "
style = "green"
heuristic = false
```

## Elm

The `elm` module shows the currently installed version of Elm. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит файл `elm.json`
- Текущий каталог содержит файл `elm-package.json`
- The current directory contains a `elm-stuff` folder
- The current directory contains a `*.elm` files

### Опции

| Переменная | По умолчанию  | Описание                                              |
| ---------- | ------------- | ----------------------------------------------------- |
| `symbol`   | `"🌳 "`        | The symbol used before displaying the version of Elm. |
| `style`    | `"bold cyan"` | Стиль модуля.                                         |
| `disabled` | `false`       | Disables the `elm` module.                            |


### Пример

```toml
# ~/.config/starship.toml

[elm]
symbol = " "
```

## Переменная Окружения

The `env_var` module displays the current value of a selected environment variable. The module will be shown only if any of the following conditions are met:

- Опция `variable` соответствует существующей переменной среды
- Опция `variable` не определена, но определена опция `default`

### Опции

| Переменная | По умолчанию     | Описание                                                           |
| ---------- | ---------------- | ------------------------------------------------------------------ |
| `symbol`   |                  | Символ, используемый перед отображением значения переменной.       |
| `variable` |                  | Отображаемая переменная окружения.                                 |
| `default`  |                  | Значение отображаемое, когда выбранная переменная не определена.   |
| `prefix`   | `""`             | Префикс, отображаемый, непосредственно перед значением переменной. |
| `suffix`   | `""`             | Префикс, отображаемый, непосредственно после значением переменной. |
| `style`    | `"dimmed black"` | Стиль модуля.                                                      |
| `disabled` | `false`          | Отключает модуль `env_var`.                                        |

### Пример

```toml
# ~/.config/starship.toml

[env_var]
variable = "SHELL"
default = "unknown shell"
```

## Ветвь Git

The `git_branch` module shows the active branch of the repo in your current directory.

### Опции

| Переменная          | По умолчанию    | Описание                                                                                                        |
| ------------------- | --------------- | --------------------------------------------------------------------------------------------------------------- |
| `symbol`            | `" "`          | Символ, используемый перед именем ветки репозитория в вашей текущей директории.                                 |
| `truncation_length` | `2^63 - 1`      | Отрезает ветку git до X графемов                                                                                |
| `truncation_symbol` | `"…"`           | Символ, используемый для обозначения усечения названия ветки. Вы можете использовать "", чтобы не видеть символ |
| `style`             | `"bold purple"` | Стиль модуля.                                                                                                   |
| `disabled`          | `false`         | Отключает модуль `git_branch`.                                                                                  |

### Пример

```toml
# ~/.config/starship.toml

[git_branch]
symbol = "🌱 "
truncation_length = 4
truncation_symbol = ""
```

## Коммит Git

The `git_commit` module shows the current commit hash of the repo in your current directory.

### Опции

| Переменная           | По умолчанию   | Описание                                              |
| -------------------- | -------------- | ----------------------------------------------------- |
| `commit_hash_length` | `7`            | Длина отображаемого хэша коммита git.                 |
| `prefix`             | `"("`          | Префикс, отображаемый сразу после коммита.            |
| `suffix`             | `")"`          | Суффикс, отображаемый сразу после коммита git.        |
| `style`              | `"bold green"` | Стиль модуля.                                         |
| `only_detached`      | `true`         | Only show git commit hash when in detached HEAD state |
| `disabled`           | `false`        | Disables the `git_commit` module.                     |

### Пример

```toml
# ~/.config/starship.toml

[git_commit]
commit_hash_length = 4
```

## Состояние Git

The `git_state` module will show in directories which are part of a git repository, and where there is an operation in progress, such as: _REBASING_, _BISECTING_, etc. If there is progress information (e.g., REBASING 3/10), that information will be shown too.

### Опции

| Переменная         | По умолчанию       | Описание                                                                                                      |
| ------------------ | ------------------ | ------------------------------------------------------------------------------------------------------------- |
| `rebase`           | `"REBASING"`       | Текст, отображаемый в процессе операции `rebase`.                                                             |
| `merge`            | `"MERGING"`        | Текст, отображаемый в процессе операции `merge`.                                                              |
| `revert`           | `"REVERTING"`      | Текст, отображаемый в процессе операции `revert`.                                                             |
| `cherry_pick`      | `"CHERRY-PICKING"` | Текст, отображаемый в процессе операции `cherry-pick`.                                                        |
| `bisect`           | `"BISECTING"`      | Текст, отображаемый в процессе операции `bisect`.                                                             |
| `am`               | `"AM"`             | Текст, отображаемый в процессе операции `apply-mailbox` (`git am`).                                           |
| `am_or_rebase`     | `"AM/REBASE"`      | Текст, отображаемый, когда выполняется неоднозначный процесс `apply-mailbox` или `rebase`.                    |
| `progress_divider` | `"/"`              | Символ или текст, который будет разделять текущую и общую сумму прогресса. (напр., `" из "`, для `"3 из 10"`) |
| `style`            | `"bold yellow"`    | Стиль модуля.                                                                                                 |
| `disabled`         | `false`            | Отключает модуль `git_state`.                                                                                 |

### Пример

```toml
# ~/.config/starship.toml

[git_state]
progress_divider = " of "
cherry_pick = "🍒 PICKING"
```

## Статус Git

The `git_status` module shows symbols representing the state of the repo in your current directory.

### Опции

| Переменная         | По умолчанию                 | Описание                                                         |
| ------------------ | ---------------------------- | ---------------------------------------------------------------- |
| `conflicted`       | `"="`                        | Эта ветка имеет конфликты слияния.                               |
| `conflicted_count` | [ссылка](#git-status-counts) | Оформленно показывать количество конфликтов.                     |
| `ahead`            | `"⇡"`                        | Эта ветка впереди отслеживаемой ветви.                           |
| `behind`           | `"⇣"`                        | Эта ветка позади отслеживаемой ветви.                            |
| `diverged`         | `"⇕"`                        | Эта ветка расходится от отслеживаемой ветки.                     |
| `untracked`        | `"?"`                        | В рабочей директории есть неотслеженные файлы.                   |
| `untracked_count`  | [ссылка](#git-status-counts) | Показывать в стиле количество неотслеженных файлов.              |
| `stashed`          | `"$"`                        | Для локального репозитория существует тайник.                    |
| `stashed_count`    | [ссылка](#git-status-counts) | Оформленно показывать количество тайников.                       |
| `modified`         | `"!"`                        | В рабочем директории есть изменения файлов.                      |
| `modified_count`   | [ссылка](#git-status-counts) | Оформленно показывать количество измененных файлов.              |
| `staged`           | `"+"`                        | В промежуточную область добавлен новый файл.                     |
| `staged_count`     | [ссылка](#git-status-counts) | Оформленно показывать количество файлов в промежуточной области. |
| `renamed`          | `"»"`                        | В промежуточную область добавлен переименованный файл.           |
| `renamed_count`    | [ссылка](#git-status-counts) | Оформленно показывать количество переименованных файлов.         |
| `deleted`          | `"✘"`                        | Удаление файла было добавлено в промежуточную область.           |
| `deleted_count`    | [ссылка](#git-status-counts) | Оформленно показывать количество удаленных файлов.               |
| `show_sync_count`  | `false`                      | Показывать счетчик впереди/позади для отслеживаемой ветки.       |
| `prefix`           | `[`                          | Префикс для отображения сразу перед статусом git.                |
| `suffix`           | `]`                          | Суффикс, отображаемый сразу после статуса git.                   |
| `style`            | `"bold red"`                 | Стиль модуля.                                                    |
| `disabled`         | `false`                      | Отключает модуль `git_status`.                                   |

#### Счетчик статуса Git

| Переменная | По умолчанию | Описание                                                |
| ---------- | ------------ | ------------------------------------------------------- |
| `enabled`  | `false`      | Показать количество файлов                              |
| `style`    |              | При желании, можно оформлять счетчик не так, как модуль |

### Пример

```toml
# ~/.config/starship.toml

[git_status]
conflicted = "🏳"
ahead = "🏎💨"
behind = "😰"
diverged = "😵"
untracked = "🤷‍"
stashed = "📦"
modified = "📝"
staged.value = "++"
staged.style = "green"
staged_count.enabled = true
staged_count.style = "green"
renamed = "👅"
deleted = "🗑"
```

## Golang

The `golang` module shows the currently installed version of Golang. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит файл `go.mod`
- Текущий каталог содержит файл `go.sum`
- Текущий каталог содержит файл `glide.yaml`
- Текущий каталог содержит файл `Gopkg.yml`
- Текущий каталог содержит файл `Gopkg.lock`
- Текущий каталог содержит папку `Godeps`
- Текущий каталог содержит файл с расширением `.go`

### Опции

| Переменная | По умолчанию  | Описание                                               |
| ---------- | ------------- | ------------------------------------------------------ |
| `symbol`   | `"🐹 "`        | Символ, используемый перед отображением версии Golang. |
| `style`    | `"bold cyan"` | Стиль модуля.                                          |
| `disabled` | `false`       | Отключает модуль `golang`.                             |

### Пример

```toml
# ~/.config/starship.toml

[golang]
symbol = "🏎💨 "
```
## Haskell

The `haskell` module shows the currently installed version of Haskell Stack version. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит файл `stack.yaml`

### Опции

| Переменная | По умолчанию | Описание                                          |
| ---------- | ------------ | ------------------------------------------------- |
| `symbol`   | `"λ "`       | Символ перед отображением текущей версии Haskell. |
| `style`    | `"bold red"` | Стиль модуля.                                     |
| `disabled` | `false`      | Отключает модуль `haskell`.                       |


### Пример

```toml
# ~/.config/starship.toml

[haskell]
symbol = " "
```

## Ветка Mercurial

The `hg_branch` module shows the active branch of the repo in your current directory.

### Опции

| Переменная          | По умолчанию    | Описание                                                                                 |
| ------------------- | --------------- | ---------------------------------------------------------------------------------------- |
| `symbol`            | `" "`          | Символ, используемый перед закладкой hg или именем ветки репозитория в текущем каталоге. |
| `truncation_length` | `2^63 - 1`      | Обрезает имя ветки hg до X графемов                                                      |
| `truncation_symbol` | `"…"`           | Символ, используемый для обозначения усечения названия ветки.                            |
| `style`             | `"bold purple"` | Стиль модуля.                                                                            |
| `disabled`          | `true`          | Отключает модуль `hg_branch`.                                                            |

### Пример

```toml
# ~/.config/starship.toml

[hg_branch]
symbol = "🌱 "
truncation_length = 4
truncation_symbol = ""
```

## Имя хоста

The `hostname` module shows the system hostname.

### Опции

| Переменная | По умолчанию          | Описание                                                                                                                                 |
| ---------- | --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `ssh_only` | `true`                | Показывать имя хоста только при подключении к SSH-сессии.                                                                                |
| `prefix`   | `""`                  | Префикс, отображаемый непосредственно перед именем хоста.                                                                                |
| `suffix`   | `""`                  | Суффикс, отображаемый непосредственно перед именем хоста.                                                                                |
| `trim_at`  | `"."`                 | Строка, по которую имя хоста будет сокращено после первого совпадения. `"."` остановится после первой точки. `"` отключит любое усечение |
| `style`    | `"bold dimmed green"` | Стиль модуля.                                                                                                                            |
| `disabled` | `false`               | Отключает модуль `hostname`.                                                                                                             |

### Пример

```toml
# ~/.config/starship.toml

[hostname]
ssh_only = false
prefix = "⟪"
suffix = "⟫"
trim_at = ".companyname.com"
disabled = false
```

## Задачи

The `jobs` module shows the current number of jobs running. The module will be shown only if there are background jobs running. The module will show the number of jobs running if there is more than 1 job, or more than the `threshold` config value, if it exists.

### Опции

| Переменная  | По умолчанию  | Описание                                                  |
| ----------- | ------------- | --------------------------------------------------------- |
| `symbol`    | `"✦"`         | Символ, используемый перед отображением количества работ. |
| `threshold` | `1`           | Показывать количество задач, если превышено.              |
| `style`     | `"bold blue"` | Стиль модуля.                                             |
| `disabled`  | `false`       | Отключает модуль `jobs`.                                  |

### Пример

```toml
# ~/.config/starship.toml

[jobs]
symbol = "+ "
threshold = 4
```

## Kubernetes

Displays the current Kubernetes context name and, if set, the namespace from the kubeconfig file. The namespace needs to be set in the kubeconfig file, this can be done via `kubectl config set-context starship-cluster --namespace astronaut`. If the `$KUBECONFIG` env var is set the module will use that if not it will use the `~/.kube/config`.

::: tip Подсказка

This module is disabled by default. To enable it, set `disabled` to `false` in your configuration file.

:::

### Опции

| Переменная | По умолчанию  | Описание                                                      |
| ---------- | ------------- | ------------------------------------------------------------- |
| `symbol`   | `"☸ "`        | Символ, используемый перед отображением информации о Cluster. |
| `style`    | `"bold blue"` | Стиль модуля.                                                 |
| `disabled` | `true`        | Отключает модуль `kubernetes`                                 |

### Пример

```toml
# ~/.config/starship.toml

[kubernetes]
symbol = "⛵ "
style = "dimmed green"
disabled = false
```

## Перевод Строки

The `line_break` module separates the prompt into two lines.

### Опции

| Переменная | По умолчанию | Описание                                                                 |
| ---------- | ------------ | ------------------------------------------------------------------------ |
| `disabled` | `false`      | Отключает модуль `line_break`, отображая командную строку в одну строку. |

### Пример

```toml
# ~/.config/starship.toml

[line_break]
disabled = true
```

## Nix-shell

The `nix_shell` module shows the nix-shell environment. The module will be shown when inside a nix-shell environment.

### Опции

| Переменная   | По умолчанию | Описание                      |
| ------------ | ------------ | ----------------------------- |
| `use_name`   | `false`      | Отображать имя nix-shell.     |
| `impure_msg` | `"impure"`   | Настроить сообщение "impure". |
| `pure_msg`   | `"pure"`     | Настройте сообщение "pure".   |
| `style`      | `"bold red"` | Стиль модуля.                 |
| `disabled`   | `false`      | Отключает модуль `nix_shell`. |

### Пример

```toml
# ~/.config/starship.toml

[nix_shell]
disabled = true
use_name = true
impure_msg = "impure shell"
pure_msg = "pure shell"
```

## Java

The `java` module shows the currently installed version of Java. Модуль будет показан, если любое из следующих условий соблюдено:

- The current directory contains a `pom.xml`, `build.gradle.kts` or `build.sbt` file
- The current directory contains a file with the `.java`, `.class`, `.gradle` or `.jar` extension

### Опции

| Переменная | По умолчанию   | Описание                                             |
| ---------- | -------------- | ---------------------------------------------------- |
| `symbol`   | `"☕ "`         | Символ, используемый перед отображением версии Java. |
| `style`    | `"dimmed red"` | Стиль модуля.                                        |
| `disabled` | `false`        | Отключает модуль `java`.                             |

### Пример

```toml
# ~/.config/starship.toml

[java]
symbol = "🌟 "
```

## Использование памяти

The `memory_usage` module shows current system memory and swap usage.

By default the swap usage is displayed if the total system swap is non-zero.

::: tip Подсказка

This module is disabled by default. To enable it, set `disabled` to `false` in your configuration file.

:::

### Опции

| Переменная        | По умолчанию          | Описание                                                                    |
| ----------------- | --------------------- | --------------------------------------------------------------------------- |
| `show_percentage` | `false`               | Отображать использование памяти в процентах от доступной памяти.            |
| `show_swap`       | `true`                | Отображать использование подкачки, если общая сумма подкачки не равна нулю. |
| `threshold`       | `75`                  | Скрывать использование памяти, если она не превышает этот процент.          |
| `symbol`          | `"🐏 "`                | Символ, используемый перед отображением использования памяти.               |
| `separator`       | `" | "`               | Символ или текст, который отделит использование памяти и подкачки.          |
| `style`           | `"bold dimmed white"` | Стиль модуля.                                                               |
| `disabled`        | `true`                | Отключает модуль `memory_usage`.                                            |

### Пример

```toml
# ~/.config/starship.toml

[memory_usage]
show_percentage = true
show_swap = true
threshold = -1
symbol = " "
separator = "/"
style = "bold dimmed green"
```

## Crystal

The `crystal` module shows the currently installed version of Crystal. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит файл `shard.yml`
- The current directory contains a `.cr` file

### Опции

| Переменная | По умолчанию | Описание                                                  |
| ---------- | ------------ | --------------------------------------------------------- |
| `symbol`   | `"🔮 "`       | The symbol used before displaying the version of crystal. |
| `style`    | `"bold red"` | Стиль модуля.                                             |
| `disabled` | `false`      | Disables the `crystal` module.                            |

### Пример

```toml
# ~/.config/starship.toml

[crystal]
symbol = "✨ "
style = "bold blue"
```

## NodeJS

The `nodejs` module shows the currently installed version of NodeJS. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит файл `package.json`
- Текущий каталог содержит каталог `node_modules`
- Текущий каталог содержит файл с расширением `.js`

### Опции

| Переменная | По умолчанию   | Описание                                               |
| ---------- | -------------- | ------------------------------------------------------ |
| `symbol`   | `"⬢ "`         | Символ, используемый перед отображением версии NodeJS. |
| `style`    | `"bold green"` | Стиль модуля.                                          |
| `disabled` | `false`        | Отключает модуль `nodejs`.                             |

### Пример

```toml
# ~/.config/starship.toml

[nodejs]
symbol = "🤖 "
```

## Версия пакета

The `package` module is shown when the current directory is the repository for a package, and shows its current version. The module currently supports `npm`, `cargo`, `poetry`, `composer`, and `gradle` packages.

- **npm** – Версия пакета `npm` из файла `package.json` в текущем каталоге
- **cargo** – Версия пакета `cargo` из файла `Cargo.toml` в текущем каталоге
- **poetry** – Версия пакета `poetry` из файла `pyproject.toml` в текущем каталоге
- **composer** – Версия пакета `composer` из `composer.json` в текущем каталоге
- **gradle** – The `gradle` package version is extracted from the `build.gradle` present

> ⚠ Показана версия пакета, исходный код которого находится в текущем каталоге, а не в менеджере пакетов.

### Опции

| Переменная | По умолчанию | Описание                                               |
| ---------- | ------------ | ------------------------------------------------------ |
| `symbol`   | `"📦 "`       | Символ, используемый перед отображением версии пакета. |
| `style`    | `"bold red"` | Стиль модуля.                                          |
| `disabled` | `false`      | Отключает модуль `package`.                            |

### Пример

```toml
# ~/.config/starship.toml

[package]
symbol = "🎁 "
```

## PHP

The `php` module shows the currently installed version of PHP. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит файл `composer.json`
- Текущий каталог содержит файл `.php`

### Опции

| Переменная | По умолчанию | Описание                                            |
| ---------- | ------------ | --------------------------------------------------- |
| `symbol`   | `"🐘 "`       | Символ, используемый перед отображением версии PHP. |
| `style`    | `"bold red"` | Стиль модуля.                                       |
| `disabled` | `false`      | Отключает модуль `php`.                             |

### Пример

```toml
# ~/.config/starship.toml

[php]
symbol = "🔹 "
```

## Python

The `python` module shows the currently installed version of Python.

If `pyenv_version_name` is set to `true`, it will display the pyenv version name.

Otherwise, it will display the version number from `python --version` and show the current Python virtual environment if one is activated.

Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит файл `.python-version`
- Текущий каталог содержит файл `requirements.txt`
- Текущий каталог содержит файл `pyproject.toml`
- Текущий каталог содержит файл с расширением `.py`
- Текущий каталог содержит файл `Pipfile`
- Текущий каталог содержит файл `tox.ini`
- Виртуальная среда в данный момент активирована

### Опции

| Переменная           | По умолчанию    | Описание                                                                              |
| -------------------- | --------------- | ------------------------------------------------------------------------------------- |
| `symbol`             | `"🐍 "`          | Символ перед отображением текущей версии Python.                                      |
| `pyenv_version_name` | `false`         | Использовать pyenv для получения версии Python                                        |
| `pyenv_prefix`       | `"pyenv "`      | Префикс перед отображением версии pyenv (отображение по умолчанию `pyenv MY_VERSION`) |
| `style`              | `"bold yellow"` | Стиль модуля.                                                                         |
| `disabled`           | `false`         | Disables the `python` module.                                                         |

### Пример

```toml
# ~/.config/starship.toml

[python]
symbol = "👾 "
pyenv_version_name = true
pyenv_prefix = "foo "
```

## Ruby

The `ruby` module shows the currently installed version of Ruby. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит файл `Gemfile`
- Текущий каталог содержит файл `.rb`

### Опции

| Переменная | По умолчанию | Описание                                             |
| ---------- | ------------ | ---------------------------------------------------- |
| `symbol`   | `"💎 "`       | Символ, используемый перед отображением версии Ruby. |
| `style`    | `"bold red"` | Стиль модуля.                                        |
| `disabled` | `false`      | Отключает модуль `ruby`.                             |

### Пример

```toml
# ~/.config/starship.toml

[ruby]
symbol = "🔺 "
```

## Rust

The `rust` module shows the currently installed version of Rust. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит файл `Cargo.toml`
- Текущий каталог содержит файл с расширением `.rs`

### Опции

| Переменная | По умолчанию | Описание                                             |
| ---------- | ------------ | ---------------------------------------------------- |
| `symbol`   | `"🦀 "`       | Символ, используемый перед отображением версии Rust. |
| `style`    | `"bold red"` | Стиль модуля.                                        |
| `disabled` | `false`      | Отключает модуль `rust`.                             |

### Пример

```toml
# ~/.config/starship.toml

[rust]
symbol = "⚙️ "
```

## Terraform

The `terraform` module shows the currently selected terraform workspace and version. By default the terraform version is not shown, since this is slow on current versions of terraform when a lot of plugins are in use. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий каталог содержит папку `.terraform`
- Текущий каталог содержит файл с расширением `.tf`

### Опции

| Переменная     | По умолчанию | Описание                                                                    |
| -------------- | ------------ | --------------------------------------------------------------------------- |
| `symbol`       | `"💠 "`       | Символ, используемый перед отображением рабочего пространства terraform.    |
| `show_version` | `false`      | Показать версию terraform. Очень медленно на больших рабочих пространствах. |
| `style`        | `"bold 105"` | Стиль модуля.                                                               |
| `disabled`     | `false`      | Отключает модуль `terraform`.                                               |

### Пример

```toml
# ~/.config/starship.toml

[terraform]
symbol = "🏎💨 "
```

## Время

The `time` module shows the current **local** time. The `format` configuration value is used by the [`chrono`](https://crates.io/crates/chrono) crate to control how the time is displayed. Take a look [at the chrono strftime docs](https://docs.rs/chrono/0.4.7/chrono/format/strftime/index.html) to see what options are available.

::: tip Подсказка

This module is disabled by default. To enable it, set `disabled` to `false` in your configuration file.

:::

### Опции

| Переменная        | По умолчанию  | Описание                                                                                                                                  |
| ----------------- | ------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `use_12hr`        | `false`       | Включить 12-часовое форматирование                                                                                                        |
| `format`          | см. ниже      | [Строка формата chrono](https://docs.rs/chrono/0.4.7/chrono/format/strftime/index.html), используемая для форматирования времени.         |
| `style`           | `bold yellow` | Стиль модуля времени                                                                                                                      |
| `utc_time_offset` | `local`       | Устанавливает смещение UTC. Диапазон -24 < x < 24. Разрешает числам с плавающей точкой встраивать 30/45-минутное смещение временной зоны. |
| `disabled`        | `true`        | Отключает модуль `time`.                                                                                                                  |

If `use_12hr` is `true`, then `format` defaults to `"%r"`. Otherwise, it defaults to `"%T"`. Manually setting `format` will override the `use_12hr` setting.

### Пример

```toml
# ~/.config/starship.toml

[time]
disabled = false
format = "🕙[ %T ]"
utc_time_offset = -5
```

## Имя пользователя

The `username` module shows active user's username. Модуль будет показан, если любое из следующих условий соблюдено:

- Текущий пользователь - root
- Текущий пользователь отличается от залогиненного
- Пользователь подключен к SSH-сессии
- Переменная `show_always` равна true

### Опции

| Переменная    | По умолчанию    | Описание                                                |
| ------------- | --------------- | ------------------------------------------------------- |
| `style_root`  | `"bold red"`    | Стиль, используемый для пользователя root.              |
| `style_user`  | `"bold yellow"` | Стиль, используемый для всех пользователей, кроме root. |
| `show_always` | `false`         | Всегда показывать модуль `username`.                    |
| `disabled`    | `false`         | Отключает модуль `username`.                            |

### Пример

```toml
# ~/.config/starship.toml

[username]
disabled = true
```
