# Таблица Котировок

Тестовое задание для JS-разработчиков.

## Описание

Нужно разработать простую таблицу котировок для криптовалютной биржи [fmfw.io](https://fmfw.io/) используя eё публичный [API](https://api.fmfw.io/).
В таблице должны отображаться и обновляться в реальном времени следующие данные для каждой пары валют: 
* Название или ID - Ticker
* цена покупки - Bid
* цена продажи Ask
* наибольшая цена за день - High
* наименьшая цена за день - Low
* последняя цена - Last

![](https://fintechytech.github.io/quote/pic.png?1)



### Демо

Демонстрацию работы такой таблицы котировок можете посмотреть здесь: [fintechytech.github.io/quote/](https://fintechytech.github.io/quote/),
но используйте эту демонстрацию лишь как пример и ориентир, но не как точное задание для выполнения, конкретные задачи по функциям таблицы немного отличаются от этой простой демонстрации.



## Задание

1. Реализовать таблицу котировок отображающую и обновляющую данные по каждой паре валют.
2. Добавить возможность сортировки по возрастанию и убыванию по каждому столбцу при клике на него (см. демонстрацию)
3. Добавьте переключатель ограничивающий список пар таблицы только 50-ю самыми дорогими по их последней цене (last) при этом оставьте возможность сортировки по другим столбцам. Другими словами, в каждый момент времени в таблице должны показываться только топ **50 самых дорогих** пар **отсортированых в соответсвии с выбранным столбцом**. Проверьте правильность выполнения этого задания например выводя не 50 а 5 пар, при сортировке по столбцам должен менятсья только порядок пар, но не набор, тоесть это должны всегда быть самые дорогие по last пары.

### Дополнительно

1. Добавьте анимацию при изменении значений в таблице, на свой вкус, можно подсвечивать отдельные ячейки как это сделано в демонстрации, можно подсвечивать строчку целиком например более контрастным фоном.
2. Добавьте ещё немного стилей и сделайте темный вариант таблицы (светлый текст на темном фоне), на свой вкус.

### Инструменты

Задание должно быть выполнено на Javascript или Typescript на выбор с использованием фреймворка [React](https://reactjs.org/), в остальном можете использовать любые дополнительные инструменты и библиотеки.

### Формат сдачи

Используя любой удобный вам инструмент сборки сделайте **production** сборку вашего приложения, в поддиректории dist/, архивируйте проект в формате **zip** и отправьте архив HR'у от кого вы получили это задание. Ответом на задание должно быть только одно react приложение. Обязательно архив должен содержать:

* production сборку в директории dist/ и соответственно файл index.html в ней, чтобы можно было просто включить http сервер в этой директории и запустить приложение.
* исходный код
* package.json

### Коментарии

* Дополнительно оптимизировать работу приложения под разные браузеры и устройства не нужно, задача будет проверяться в последней версии Chrome на разрешении FullHD
* Так как данные обновляются очень часто, особое внимание следует уделить производительности и скорости работы приложения, таблица должна обновляться визуально быстро, при этом не потребляя чрезмерно ресурсы, ориентируйтесь на 5% - 20% времени на scripting в профайлере или на то как на вашем девайсе работает демонстрация.
* Начните выполнение заданий начиная с первого, можете не выполнять дополнительные задания, однако их выполнение определенно будет большим плюсом, при проверке конечно будет учитываться что на их выполнение ушло больше времени. Помните что даже с основными заданиями важнее качество их выполнения, а не количество.
* Так как приложение довольно маленькое то корректность и правильность работы кода при проверке будут цениться больше чем его оформление, можете не тратить время на написание документации, однако помните что код всё же должен быть читаемым и понятным, без разных debug-конструкций и неиспользуемых блоков и переменных.
* Предпочтительнее использовать функциональные компоненты и [Hooks API](https://reactjs.org/docs/hooks-reference.html)
* Время на выполнение заданий не ограничено, но также будет приниматься во внимание.


### API

Рекомендуется использовать публичный WebSocket API биржи [fmfw.io](https://api.fmfw.io/), этот API работает по [JSON-RPC 2](https://www.jsonrpc.org/specification).

Для разработки вам потребуется метод [GetSymbols](https://api.fmfw.io/#get-symbols) с помощью которого можно получить список пар торгующихся на бирже.
Также вас интересуeт метод [subscribeTicker](https://api.fmfw.io/#subscribe-to-ticker) и соответствующие сообщения-нотификации в которых уже будут
присылаться и обновляться все необходимые данные. Подробнее о протоколе и форматах сообщений написано в документации по ссылкам выше.

Примерный алгоритм взаимодействия с API таков:

1. Получить список пар методом `GetSymbols`
2. Для всех пар подписаться на их котировки методом `subscribeTicker`

После этого в соединении начнут приходить сообщения `ticker` в которых будут все данные для таблицы, а именно:

* `symbol` - id пары к которой относятся данные, и одновременно имя которое стоит выводить в таблице
* `bid` - цена покупки Bid
* `ask` - цена покупки Ask
* `high` - наибольщная цена за день High
* `low` - наименьшая цена за день High
* `last` - последняя цена Last

Поле `symbol` в ответе `GetSymbols` и в сообщении `ticker` является первичным ключом, оно не будет меняться, и по нему всегда однозначно идентифицируется пара.
