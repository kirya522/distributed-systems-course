# Курс: Паттерны и компоненты современных распределенных систем

сайт курса [https://kirya522.tech/distributed-systems-course/](https://kirya522.tech/distributed-systems-course/)

## Автор

Кирилл Грищук

## Документация по разработке сайта

Установить hugo
```sh
brew install hugo
```

Инициализация submodule
```
git submodule update --init
```

Запуск сервера
```sh
hugo server --buildDrafts
```
server will start at http://localhost:1313/

Создать новый пост
```sh
hugo new posts/<name>.md
```

Публикация артефактов, в actions используется `-minify`
```sh
hugo
```