# Конвертер файлов 1С в *.cfe

[![GitHub issues](https://img.shields.io/github/issues-raw/firstBitSportivnaya/files-converter?style=badge)](https://github.com/firstBitSportivnaya/files-converter/issues)
[![License](https://img.shields.io/github/license/firstBitSportivnaya/files-converter?style=badge)](https://github.com/firstBitSportivnaya/files-converter/blob/main/LICENSE)

## Обзор

**files-converter** - это инструмент для конвертации исходных файлов конфигурации или *.cf файла в *.cfe файл и обратно.

## Функциональность

- Реализована конвертация из *.cf файла в *.cfe файл.
- Реализована конвертация из исходных файлов (конфигурация) в *.cfe файл.
- Не реализована конвертация из *.cfe файла в *.cf файл.
- Не реализована конвертация из исходных файлов (расширение) в *.cf файл.

## Использование файла настроек

**files-converter** поддерживает файл настроек по умолчанию, который расположен в `$HOME/.files-converter/pkg/config/default.json`. Файл содержит настройки необходимые для конвертации в расширение (*.cfe).
Чтобы использовать свой конфигурационный файл, укажите путь к файлу с помощью флага `--config`. Если флаг не задан, приложение попытается использовать путь из переменной окружения **`CONFIG_PATH`**. Если путь не указан ни во флаге, ни в переменной окружения, программа завершится с ошибкой.

**Пример использования:**

1. Укажите путь с помощью флага:
```shell
files-converter --config="pathToConfig/config.json"
```

2. Если путь не указан во флаге, задайте переменную окружения:
```shell
export CONFIG_PATH="pathToConfig/config.json"
```

### Параметры конфигурационного файла

У проекта есть два файла настроек:
- default.json - содержит основные настройки для автоматической конвертации. Пример использования: [default.json](pkg/config/default.json)
- config.json  - пользовательский файл, где можно указать такие параметры, как пути, префикс, имя конфигурации, а также дополнительные XML-файлы для изменения определенных элементов, если требуется что-то дополнительно настроить поверх стандартной конвертации. Пример использования: [PSSL/cfe-converter-config.json](https://github.com/firstBitSportivnaya/PSSL/blob/develop/cfe-converter-config.json)

#### Обязательные

- **`platform_version`**: *(строка)* - версия платформы. (Обязательный параметр для `conversion_type`: `"cfConvert"`)
  - **Пример**: `"platform_version": "8.3.23"`

- **`extension`**: *(строка)* - имя расширения.
  - **Example**: `"extension": "PSSL"`

- **`prefix`**: *(строка)* - префикс, который используется для имени объектов метаданных.
  - **Example**: `"prefix": "пбп_"`

- **`input_path`**: *(строка)* - путь к каталогу или файлу для конвертации.
  - **Example**: `"input_path": "C:/path/to/input"`

- **`output_path`**: *(строка)* - путь, по которому будут сохранены преобразованные файлы.
  - **Example**: `"output_path": "C:/path/to/output"`

- **`conversion_type`**: *(строка)* - тип выполняемой конвертации. Поддерживаемые значения:
  - `"srcConvert"` - конвертация исходных файлов в *.cfe.
  - `"cfConvert"`  - конвертация конфигурационного файла *.cf в *.cfe.
  - **Example**: `"conversion_type": "srcConvert"`

#### Необязательные

- **`xml_files`**: *(массив)* - список файлов XML, в которые необходимо внести изменения. Это полезно, если требуется автоматически изменить конкретные элементы в XML-файлах, соответствующих настройкам конфигурации, без ручного редактирования. Например, можно добавить, изменить или удалить элементы, что делает процесс конвертации гибким и подстраиваемым под нужды проекта. Пример использования: [xml_files](pkg/config/default.json#3)
  - **`file_name`**: *(строка)* - имя XML-файла который нужно изменить.
    - **Example**: `"file_name": "example.xml"`
  - **`element_operations`**: *(массив)* - список операций, которые необходимо выполнить над элементами в XML-файле.
    - **`element_name`**: *(строка)* - имя элемента XML, который нужно изменить.
      - **Example**: `"element_name": "Global"`
    - **`operation`**: *(строка)* - тип операции. Поддерживаемые значения: 
      - `"add"`    - добавление элемента.
      - `"delete"` - удаление элемента.
      - `"modify"` - изменение элемента.
      - **Example**: `"operation": "modify"`
    - **`value`**: *(строка, опционально)* - новое значение, которое нужно установить для элемента. (используется с операциями: `add` и `modify`).
      - **Example**: `"value": "false"`

## Автоматическое определение версии платформы

Для типа конвертации (`conversion_type: "srcConvert"`) будет предпринята попытка определить версию платформы на основе **версии формата выгрузки**, указанной в файле `"Configuration.xml"`. Например для версии формата выгрузки `2.16` будет установлена версия платформы `8.3.23`. Подробнее см. файл соответствия [export_format_versions.json](pkg/export_format/export_format_versions.json). 

## Использование

Инструмент можно установить, запустив:

```shell
go install github.com/firstBitSportivnaya/files-converter@latest
```

**Примечание:** Для использования этой программы требуется соответствующая версия платформы (8.3.23).

## Справочная информация:

Для получения информации о доступных командах и параметрах:

```shell
files-converter --help
```

## Примеры

Использование конфигурационного файла:

```shell
files-converter --config="path/config.json"
```

Если флаг --config не указан, программа использует конфигурационный файл по умолчанию, расположенный в $HOME/.files-converter/configs/config.json:

```shell
files-converter
```

## Лицензия

Этот проект лицензирован по лицензии MIT. Подробнее см. файл [LICENSE](LICENSE).
