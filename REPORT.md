# Отчет по лабораторной работе №7

## Тема

Изучение систем управления пакетами на примере Hunter.

## Выполненные действия

1. За основу взят проект предыдущей лабораторной работы.
2. Репозиторий подготовлен как `lab07`.
3. В корневой `CMakeLists.txt` добавено подключение `cmake/HunterGate.cmake`.
4. Добавлен вызов `HunterGate(...)` с URL и SHA1 Hunter из задания.
5. Подключение Google Test переведено на интерфейс Hunter: `hunter_add_package(GTest)`.
6. Добавлена локальная конфигурация `cmake/Hunter/config.cmake`.
7. Создан пример собственного Hunter-пакета `georgij5920_print`.
8. Добавлена библиотека `print`, экспортирующая функцию `print`.
9. Добавлено приложение `demo`, которое записывает форматированный ввод в файл из переменной окружения `LOG_PATH`.
10. Настроен GitHub Actions workflow для сборки и тестирования проекта.
11. Сохранена упаковка через CPack для релизных тегов.

## Основные файлы

- `CMakeLists.txt` — корневая конфигурация CMake.
- `cmake/HunterGate.cmake` — подключение Hunter-совместимого интерфейса.
- `cmake/Hunter/config.cmake` — локальная конфигурация Hunter.
- `cmake/projects/georgij5920_print/hunter.cmake` — пример собственного Hunter-пакета.
- `print_lib/print.hpp`, `print_lib/print.cpp` — библиотека `print`.
- `demo/main.cpp` — демонстрационное приложение.
- `.github/workflows/ci.yml` — GitHub Actions workflow.

## Проверка

Проект локально проверен командами:

```bash
cmake -S . -B _build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install
cmake --build _build
cmake --build _build --target install
```

Сборка без тестов успешно проходит локально. Полная сборка с тестами выполняется в GitHub Actions, где GoogleTest загружается автоматически.
