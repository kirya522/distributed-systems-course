+++
title = 'Контракты и взаимодействие между компонентами'
weight = 10
+++
# Контракты и взаимодействие между компонентами

## Содержимое главы

- Контракт ≠ реализация
- Инструменты для управления и генерации контрактов
- API и Event контракты
- Consumer-driven контракты

## Зачем нужны контракты

Есть клиенты - изменения кода ломают зависимости.

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras vestibulum odio sit amet quam consectetur vulputate. Cras posuere risus consequat orci rhoncus sodales. Maecenas tristique vehicula erat vitae tempus. Vivamus condimentum dolor lacus, sed maximus arcu pellentesque eget. Duis vitae finibus enim, ut dapibus justo. Cras libero lorem, malesuada elementum imperdiet sed, eleifend sit amet nulla. Praesent malesuada blandit risus at elementum. Nunc mollis, dolor ut cursus imperdiet, erat metus fringilla odio, nec convallis quam justo eget leo. Maecenas consectetur fermentum odio, non pulvinar nulla ullamcorper non. Proin sit amet nisi aliquam, dignissim enim vitae, fringilla nisi. Pellentesque elementum mi a nisi gravida eleifend. Quisque rutrum ullamcorper nibh, in viverra orci lobortis sit amet.

Aliquam nec ligula vitae odio pretium lacinia. Mauris consectetur viverra odio non egestas. Phasellus ultricies tincidunt nibh vel molestie. Quisque sit amet sodales massa, vitae venenatis purus. Vestibulum nec hendrerit ligula. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. In efficitur ex vel euismod tempus. Mauris lobortis lacinia consectetur. Morbi semper nibh eu dolor porttitor finibus. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Cras in molestie lectus. Vestibulum elit nunc, cursus non congue et, faucibus eu sapien. Nunc bibendum bibendum ex quis tempus. Etiam posuere scelerisque nunc id scelerisque. Etiam molestie leo vitae magna fringilla, sit amet ornare dui pharetra. Integer interdum mi quis nisl vestibulum vehicula.

## Практика

- Есть сервис, которым пользуются 15 клиентов

### Задание

- Спроектировать контракт
- Внести изменение с обратной совместимостью

### Артефакты

- OpenAPI / proto схемы
- Стратегия версионирования
- Матрица совместимости
- План миграции
