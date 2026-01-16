# Курс: Паттерны и компоненты современных распределенных систем

сайт курса [https://kirya522.tech/distributed-systems-course/](https://kirya522.tech/distributed-systems-course/)

## Автор

Кирилл Грищук

## Документация для темы

https://github.com/alex-shpak/hugo-book?tab=readme-ov-file

## Документация по разработке сайта

Установить [hugo](https://gohugo.io/installation/windows/)
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

Публикация артефактов, в actions используется `-minify`
```sh
hugo
```