# Журнал изменений

Все значимые изменения проекта документируются в этом файле.

Формат основан на [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
проект следует [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

> Первоисточник — английский [CHANGELOG.md](CHANGELOG.md); этот перевод может немного отставать.

## [1.1.0] - 2026-10-22

### Добавлено
- **Скан зоны IE (zone map)** — находит записи zone-map для доменов Naverа
  (остатки «надёжных сайтов»); удаление сносит поддерево домена (сначала экспорт)
- **Скан RegisteredApplications** — находит записи Naverа в метаданных
  приложений по умолчанию; удаление стирает значение (сначала экспорт)
- **Скан MUICache** — находит следы запуска исполняемых файлов Naverа;
  удаление стирает значение кэша (сначала экспорт)
- **Следы инсталляторов в %TEMP%** — scan_traces теперь также репортит
  инсталляторы Naverа, оставшиеся во временной папке пользователя
- **Каталоги продуктов per-user** — scan_product_dirs теперь также покрывает
  `%LOCALAPPDATA%\Programs\Naver*` (legacy-установки Naver Музыки /
  Мессенджера / Переводчика), папка браузера исключена
- **Kill процессов по пути (Vanced)** — браузерный kill теперь также матчит
  процессы, чей exe лежит под путём NaverBrowser (crashpad, updater),
  а не только узкий список имён browser.exe/naver.exe
- **Скан hijack стартовых URL** — находит URL Naverа в enterprise-политикаx
  `RestoreOnStartupURLs` (Edge, Chrome, Chromium, Brave, Vivaldi, Opera,
  Naver.Браузер); удаление вычищает значения Naverа
- **Скан hijack домашней страницы Firefox** — находит
  `browser.startup.homepage` со ссылкой на Naver в `prefs.js`
  Firefox/Waterfox; удаление переписывает файл (сначала бэкап)

## [2.7.0] - 2026-10-21

### Добавлено
- **Скан Naver.DNS** (Extended) — находит сетевые адаптеры с DNS-серверами
  Naverа (77.88.8.x basic/safe/family); удаление возвращает адаптеру
  автоматический DNS; с подтверждением
- **Скан hijack стартовой страницы** — находит legacy-домашнюю страницу IE
  (`HKCU\...\Internet Explorer\Main\Start Page`) со ссылкой на Naver/naver.com;
  удаление возвращает дефолт (оба издания)

## [2.6.0] - 2026-10-21

### Добавлено
- **Extended-издание**: скан Store (Appx) приложений — находит Naver Музыку,
  Алису, Мессенджер и другие Store-приложения Naverа, удаляет через
  `Remove-AppxPackage`
- **Extended-издание**: скан каталогов небраузерных продуктов под
  `Program Files\Naver\` (Naver.Диск, Naver IME Switcher, IME, ...) с бэкапом
  перед удалением
- **Extended-издание**: скан локальных данных небраузерных продуктов под
  `%LOCALAPPDATA%\Naver\` (локальные данные Naver IME Switcher, апдейтер
  Naver.Диска, Beta-сборки) — с подтверждением, бэкап перед удалением
- **Extended-издание**: «Kill Processes» теперь завершает процессы всех
  продуктов Naverа (Naver.Диск, Naver IME Switcher, Store-приложения),
  а не только браузера

## [2.5.0] - 2026-10-21

### Добавлено
- Два издания из одной кодовой базы: **AntiNaverVanced** (удаление
  Naver.Браузера + Naver.Поиска) и **AntiNaverExtended** (все продукты
  Naverа) — профили продуктов в `profiles.py` выбирают охват скана, общий
  GUI в `app.py`, тонкие точки входа `main.py` / `main_extended.py`
- `build.bat` теперь собирает оба EXE

## [2.4.0] - 2026-10-21

### Добавлено
- 16-й–30-й векторы hijack: интеграция Naver.Диска в оболочку (иконки-оверлеи
  + обработчики контекстного меню), отключённый автозапуск (`StartupApproved`),
  `App Paths`, ярлыки (Рабочий стол / Меню Пуск / панель задач),
  каталоги-остатки (ProgramData, Roaming, данные Naver.Диска и
  синхронизированные файлы), записи `hosts`, правила брандмауэра Windows,
  обработчики URI-протоколов, расширения браузеров (манифесты Chromium +
  `.xpi` Firefox), куки naver.ru (Chromium + Firefox), ассоциации
  `OpenWithProgids`/`OpenWithList`, следы Prefetch / CrashDumps / WER,
  исключения Windows Defender, пользовательские раскладки клавиатуры
  (Naver IME)
- Подтверждение согласия: находки, затрагивающие пользовательские данные
  (файлы Naver.Диска, история Naver IME Switcher, расширения, куки, исключения
  Defender), требуют явного дополнительного подтверждения перед удалением
- Юнит-тесты для байт-детекта `.lnk`, фильтрации строк hosts, парсинга вывода
  брандмауэра и покрытия consent-префиксов

## [2.3.0] - 2026-10-20

### Добавлено
- 13-й–15-й векторы hijack: службы Windows (скан по реестру, удаление через
  `sc stop` + `sc delete`), Naver IME Switcher (детект по маркеру `ime`),
  файловые ассоциации (скан + удаление ProgId в `FileExts\<ext>\UserChoice`)
- Диалог выбора после скана: выбрать, какие именно находки удалять, вместо
  «всё или ничего»
- Исключения: снятые галочки можно сохранить в
  `%APPDATA%\AntiNaverVanced\exclusions.json` — будущие сканы автоматически
  их не выбирают, но они остаются доступными в диалоге; исключения можно
  сбросить из диалога
- Юнит-тесты персистентности исключений и маркера Naver IME Switcher

## [2.2.0] - 2026-10-20

### Добавлено
- Бэкап перед удалением: каждый затронутый ключ реестра / файл профиля
  экспортируется в `%APPDATA%\AntiNaverVanced\backup\<timestamp>\` перед
  удалением
- Журнал операций: `%APPDATA%\AntiNaverVanced\antinaver.log`
- 12-й вектор hijack: записи Планировщика задач Windows (задачи апдейтера
  Naverа)
- Скан/чистка HKCU-политик Chromium (per-user политики, раньше только HKLM)
- Проверка прав администратора при старте с предупреждением в статус-баре
- Предложение закрыть запущенные браузеры перед удалением hijack
  (предотвращает «database locked»)
- Предложение «открыть папку отчёта», когда часть hijack-находок не удалилась
- Юнит-тесты чистой логики (поиск движка, Firefox policies.json,
  LZ4-декомпрессор)

### Исправлено
- Все обновления UI из фоновых потоков теперь маршалируются в главный поток
  через `root.after` (tkinter не потокобезопасен)
- Голые `except:` заменены на `except OSError`
- Чистка Web Data теперь удаляет ровно найденные строки (по `rowid`), а не по
  подстроке URL — детект и удаление согласованы
- SQLite-соединения закрываются на error-путях (`finally`)
- Firefox `policies.json` больше не содержит `null` `SuggestURLTemplate`
- Политики Firefox больше не пишутся в несуществующие папки установки
- Захардкоженная русская строка ошибки админа заменена на переводимый ключ
  во всех 10 языках UI
- `shutil.rmtree` теперь репортит, какие файлы заблокированы, вместо общей
  ошибки
- Битые LZ4-блоки с невалидными смещениями вызывают исключение вместо возврата
  мусора

## [2.1.2] - 2026-10-20

### Изменено
- Заголовок приложения сокращён до «Удаление Naver» / «Remove Naver»
  (ru/en/uk)
- Футер прижат к правому нижнему углу: автор, версия и кликабельная ссылка
  на GitHub
- Ссылка репозитория установлена на `https://github.com/retvil/AntiNaverVanced`

## [2.1.0] - 2026-10-20

### Добавлено
- Вертикальная компоновка окна (560×700): центрированный стек действий,
  карточки секций на всю ширину
- Светлая тема UI на **ttkbootstrap** (тема `flatly`) — замена CustomTkinter
  на более лёгкий ttk-стек
- Футер с автором, версией и кликабельной ссылкой на репозиторий GitHub

### Изменено
- Миграция UI-библиотеки: tkinter → CustomTkinter → ttkbootstrap
- Селектор языка перенесён в правый верхний угол

## [2.0.0] - 2026-10-20

Форк [retvil/AntiYandexVanced](https://github.com/retvil/AntiYandexVanced) 1.3.

### Добавлено
- **Сканер hijack поиска** — детектит 11 векторов, которыми Naver
  прописывается дефолтным поиском/браузером:
  - каталоги установки на диске
  - записи Uninstall в реестре (HKLM, WOW6432Node, HKCU)
  - ассоциация браузера по умолчанию Windows (`UserChoice` ProgId для
    http/https)
  - регистрации клиентов браузеров `StartMenuInternet`
  - отдельные деревья ключей `Software\Naver` (HKCU/HKLM/WOW6432Node)
  - hijack через enterprise-политики `DefaultSearchProvider*` (жёсткий способ
    закрепления Naverа)
  - per-profile JSON `Preferences` Chromium (Chrome, Edge, Brave, Vivaldi,
    Naver.Браузер)
  - строки ключевых слов поисковиков в SQLite-базах `Web Data` Chromium
  - Firefox/Waterfox `search.json.mozlz4` (со встроенным pure-Python
    LZ4-декомпрессором блоков)
  - legacy-записи `SearchScopes` IE/Windows
  - записи автозапуска (`Run`/`RunOnce`)
- **Удаление hijack** — удаление всех найденных векторов одним кликом,
  у каждого вектора свой remover: рекурсивное удаление ключей реестра,
  сброс `UserChoice`, чистка `Preferences`, `DELETE FROM keywords` в Web Data,
  сброс `search.json.mozlz4` (Firefox пересоздаёт дефолты), удаление значений
  автозапуска. Заблокированные базы/файлы (браузер запущен) репортятся,
  без крашей.
- **Жёсткая политика поиска по умолчанию** — закрепляет один из 10
  поисковиков (Google, Bing, DuckDuckGo, Brave Search, Startpage, Mojeek,
  Qwant, Ecosia, Yahoo, SearXNG):
  - семейство Chromium через HKLM `SOFTWARE\Policies\...` значения
    `DefaultSearchProvider*` (Chrome, Edge, Brave, Vivaldi, Opera,
    Naver.Браузер)
  - Firefox/Waterfox через `distribution\policies.json` (`SearchEngines` +
    `LockDefaultSearchEngine` + `PreventSearchEngineInstall`)
- Кнопка **«Удалить политики Naverа»** — вычищает значения политик
  `DefaultSearchProvider*` Naverа из реестра
- **10 языков UI** (ru, en, uk, kk, de, fr, es, pl, tr, zh) в отдельном модуле
  `translations.py`; язык системы определяется автоматически через Windows UI
  language API (`GetUserDefaultUILanguage`), фолбэк на английский
- Выпадающий список смены языка в UI

### Изменено
- Код разбит на модули: `engines.py`, `naver_detect.py`, `policies.py`,
  `translations.py`, `main.py`

## [1.3.0] - upstream

База [retvil/AntiYandexVanced](https://github.com/retvil/AntiYandexVanced):
- Завершение процессов Naver.Браузера (`browser.exe`, `naver.exe`)
- Поиск путей установки
- Чистка записей Uninstall в реестре
- Удаление папки браузера
- Переключатель интерфейса RU/EN

[Unreleased]: https://github.com/retvil/AntiNaverVanced/compare/v2.1.2...HEAD
[2.1.2]: https://github.com/retvil/AntiNaverVanced/compare/v2.1.0...v2.1.2
[2.1.0]: https://github.com/retvil/AntiNaverVanced/compare/v2.0.0...v2.1.0
[2.0.0]: https://github.com/retvil/AntiNaverVanced/compare/v1.3.0...v2.0.0


