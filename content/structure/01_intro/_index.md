+++
title = 'Архитектурные подходы, декомпозиция и границы системы'
weight = 1
+++
# Архитектурные подходы, декомпозиция и границы системы

## Содержимое главы

- Почему не существует единственной правильной архитектуры систем и приложений
- Какие ограничения формируют архитектуру системы
- Современные архитектурные подходы: монолит, модульный монолит, микросервисы, распределенный монолит (distributed monolith)
- Когда и какие подходы применимы
- Декомпозиция и разделение данных по домену
- Границы системы и сервисов
- Владение данными (ownership)

## Как вообще думают про архитектуру сегодня

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras vestibulum odio sit amet quam consectetur vulputate. Cras posuere risus consequat orci rhoncus sodales. Maecenas tristique vehicula erat vitae tempus. Vivamus condimentum dolor lacus, sed maximus arcu pellentesque eget. Duis vitae finibus enim, ut dapibus justo. Cras libero lorem, malesuada elementum imperdiet sed, eleifend sit amet nulla. Praesent malesuada blandit risus at elementum. Nunc mollis, dolor ut cursus imperdiet, erat metus fringilla odio, nec convallis quam justo eget leo. Maecenas consectetur fermentum odio, non pulvinar nulla ullamcorper non. Proin sit amet nisi aliquam, dignissim enim vitae, fringilla nisi. Pellentesque elementum mi a nisi gravida eleifend. Quisque rutrum ullamcorper nibh, in viverra orci lobortis sit amet.

Aliquam nec ligula vitae odio pretium lacinia. Mauris consectetur viverra odio non egestas. Phasellus ultricies tincidunt nibh vel molestie. Quisque sit amet sodales massa, vitae venenatis purus. Vestibulum nec hendrerit ligula. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. In efficitur ex vel euismod tempus. Mauris lobortis lacinia consectetur. Morbi semper nibh eu dolor porttitor finibus. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Cras in molestie lectus. Vestibulum elit nunc, cursus non congue et, faucibus eu sapien. Nunc bibendum bibendum ex quis tempus. Etiam posuere scelerisque nunc id scelerisque. Etiam molestie leo vitae magna fringilla, sit amet ornare dui pharetra. Integer interdum mi quis nisl vestibulum vehicula.

## Практика

- Выбрать подход к развитию системы заказов(монолит, модульный монолит, микросервисы)

### Задание

- Декомпозиция
  - выделить домены
  - определить границы
  - описать ответственность
- Анализ рисков
  - где потенциальные проблемы
  - что будет сложно менять
- Ограничения
  - команда 8 человек
  - 2 релиза в неделю
  - нагрузка растёт, но не критично
  - высокая консистентность для платежей

### Артефакты

- Диаграмма сервисов и доменов
- Список рисков и трейдофов
- ADR для фиксации выбора решения

### Входные данные

Система:

- заказы
- оплата
- каталог
- доставка
- уведомления

Текущая реализация:

- монолит
- одна база данных