# Laboratory work VII

[![CI](https://github.com/georgij5920/lab07/actions/workflows/ci.yml/badge.svg)](https://github.com/georgij5920/lab07/actions/workflows/ci.yml)

Данная лабораторная работа посвящена изучению систем управления пакетами на примере Hunter.

## Что сделано

- Проект перенесен из `lab06` в `lab07`.
- Добавлен файл `cmake/HunterGate.cmake` и вызов `HunterGate(...)` в корневом `CMakeLists.txt`.
- Подключение Google Test перенесено на форму из задания: `hunter_add_package(GTest)` и `find_package(GTest CONFIG REQUIRED)`.
- Добавлена локальная конфигурация Hunter: `cmake/Hunter/config.cmake`.
- Добавлен пример собственного Hunter-пакета: `cmake/projects/georgij5920_print/hunter.cmake`.
- Добавлена библиотека `print` с заголовком `print.hpp`.
- Добавлено приложение `demo`, которое читает слова из `stdin` и пишет форматированный результат в файл из переменной окружения `LOG_PATH`.
- Сохранены тесты из предыдущих лабораторных работ.
- CI настроен через GitHub Actions для Linux GCC, Linux Clang и Windows MSVC.
- Сохранена упаковка через CPack для релизных тегов `v*`.

## Сборка

```bash
cmake -S . -B _build -DBUILD_TESTS=ON -DCMAKE_INSTALL_PREFIX=_install
cmake --build _build
ctest --test-dir _build --output-on-failure
cmake --build _build --target install
```

## Проверка demo

```bash
cmake -S . -B _build -DCMAKE_INSTALL_PREFIX=_install
cmake --build _build --target demo
LOG_PATH=demo.log ./_build/demo/demo
```

После запуска можно ввести несколько слов и завершить ввод. Результат будет записан в `demo.log`.

## Hunter

В корневом `CMakeLists.txt` добавлен блок:

```cmake
include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"
    SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"
    LOCAL
)
```

Для тестов используется интерфейс Hunter:

```cmake
hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)
```

Локальная конфигурация находится в `cmake/Hunter/config.cmake`. Пример собственного пакета находится в `cmake/projects/georgij5920_print/hunter.cmake`.

## Создание пакета вручную

```bash
cmake -S . -B _build -DCPACK_GENERATOR="TGZ"
cmake --build _build
cmake --build _build --target package
```

## Создание релиза

```bash
git tag v0.1.0
git push origin v0.1.0
```

После пуша тега GitHub Actions соберет пакеты и прикрепит их к GitHub Release.
