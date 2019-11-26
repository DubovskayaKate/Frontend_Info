### Браузерное окружение, спецификации

window:
- DOM (Document object model)
    * document
- BOM (Browser Object Model)
    * navigator
    * screen
    * location
    * frames
    * history
    * XMLHttpRequest
- JS
    * Object
    * Array
    * Function

Существует 12 типов узлов. Но на практике мы в основном работаем с 4 из них

* EventTarget (абстрактный класс) - события
    - Node (абстрактный класс) -  базовая функциональность: parentNode, nextSibling, childNodes; nodeName
        * Text (elem.nodeType == 3) - навигацию на уровне элементов, методы поиска
        * Element (elem.nodeType == 1)  - tagName
            1. innerHTML - содержимое элемента (перезапись)
            2. outerHTML - HTML элемента целиком
            3. textContent - HTML за вычетом всех <тегов>
            4. elem.hasAttribute(name) – проверяет наличие атрибута.
            5. elem.getAttribute(name) – получает значение атрибута.
            6. elem.setAttribute(name, value) – устанавливает значение атрибута.
            7. elem.removeAttribute(name) – удаляет атрибут.
            - HTMLElement 
            - SVGElement            
        * Comment (elem.nodeType == 9)



#### Переход к соседям

* Для всех узлов: parentNode, childNodes, firstChild, lastChild, previousSibling, nextSibling.
* Только для узлов-элементов: parentElement, children, firstElementChild, lastElementChild, previousElementSibling, nextElementSibling.

#### Поиск элементов
Живая коллекция - коллекция отражающая текущее состояние документа и автоматически обновляются при его изменении.

|Метод|	Ищет по...|	Ищет внутри элемента?|	Возвращает живую коллекцию?|
|---|---|---|---|
|querySelector|	CSS-selector|✔|	-|
|querySelectorAll|	CSS-selector|✔|-|
|getElementById	|id	|-|	-|
|getElementsByName|	name|-	|✔|
|getElementsByTagName|tag or '*'|	✔	|✔|
|getElementsByClassName|class|	✔|	✔|

* Есть метод elem.matches(css), который проверяет, удовлетворяет ли элемент CSS-селектору.
* Метод elem.closest(css) ищет ближайшего по иерархии предка, соответствующему данному CSS-селектору. Сам элемент также включён в поиск.

### События

Есть три способа назначения обработчиков событий:

- Атрибут HTML: onclick="...".
- DOM-свойство: elem.onclick = function.
- elem.addEventListener(event, handler[, phase]) для добавления, removeEventListener для удаления.

### Всплытие
Всплытие -  на элементе происходит событие, обработчики сначала срабатывают на нём, потом на его родителе, затем выше и так далее, вверх по цепочке предков.
Самый глубокий элемент, который вызывает событие, называется целевым элементом, и он доступен через event.target.
Остановить всплытие - event.stopPropagation()

### Погружение

Стандарт DOM Events описывает 3 фазы прохода события:

1. Фаза погружения (capturing phase) – событие сначала идёт сверху вниз.(от Html до элемента)
2. Фаза цели (target phase) – событие достигло целевого(исходного) элемента.
3. Фаза всплытия (bubbling stage) – событие начинает всплывать.

Существуют два варианта значений опции capture - elem.addEventListener(..., {capture: true}):

* Если аргумент false (по умолчанию), то событие будет поймано при всплытии.
* Если аргумент true, то событие будет перехвачено при погружении.

elem.target - это «целевой» элемент, на котором произошло событие, в процессе всплытия он неизменен.
elem.currentTarget/this - это «текущий» элемент, до которого дошло всплытие, на нём сейчас выполняется обработчик.

### Делегирование событий

1. Вешаем обработчик на контейнер.
2. В обработчике проверяем исходный элемент event.target.
3. Если событие произошло внутри нужного нам элемента, то обрабатываем его.

Отмена действий браузера по умолчанию event.preventDefault()

### Event
```javascript
let event = new Event(type[, options]);
```

- type – тип события, строка, например "click" или же любой придуманный нами – "my-event".
- options – объект с двумя необязательными свойствами:
    * bubbles: true/false – если true, тогда событие всплывает.
    * cancelable: true/false – если true, тогда можно отменить действие по умолчанию. Позже мы разберём, что это значит для пользовательских событий.

elem.dispatchEvent(event) - вызов Event

## HTMl страница

У жизненного цикла HTML-страницы есть три важных события: DOMContentLoadedБ load, beforeunload/unload 

Каждое из этих событий может быть полезно:

- Событие DOMContentLoaded – DOM готов, так что обработчик может искать DOM-узлы и инициализировать интерфейс. Срабатывает на объекте document.
- Событие load – внешние ресурсы были загружены, стили применены, размеры картинок известны и т.д. 
- Событие beforeunload – пользователь покидает страницу. Мы можем проверить, сохранил ли он изменения и спросить, на самом ли деле он хочет уйти.
- unload – пользователь почти ушёл, но мы всё ещё можем запустить некоторые операции, например, отправить статистику.

document.readyState – текущее состояние документа, изменения можно отследить с помощью события readystatechange:
* loading – документ грузится.
* interactive – документ прочитан, происходит примерно в то же время, что и DOMContentLoaded, но до него.
* complete – документ и ресурсы загружены, происходит примерно в то же время, что и window.onload, но до него.


||Порядок|	DOMContentLoaded|
|---|---|---|
|async	|Порядок загрузки (кто загрузится первым, тот и сработает).|	Не имеет значения. Может загрузиться и выполниться до того, как страница полностью загрузится. Такое случается, если скрипты маленькие или хранятся в кеше, а документ достаточно большой.
|defer	|Порядок документа (как расположены в документе).|Выполняется после того, как документ загружен и обработан (ждёт), непосредственно перед DOMContentLoaded.

### CORS (кросс-доменная политика)

Чтобы разрешить кросс-доменный доступ, нам нужно поставить тегу `<script>` атрибут crossorigin, и, кроме того, удалённый сервер должен поставить специальные заголовки.

Существует три уровня кросс-доменного доступа:

1. Атрибут crossorigin отсутствует – доступ запрещён.
2. crossorigin="anonymous" – доступ разрешён, если сервер отвечает с заголовком Access-Control-Allow-Origin со значениями * или наш домен. Браузер не отправляет авторизационную информацию и куки на удалённый сервер.
3. crossorigin="use-credentials" – доступ разрешён, если сервер отвечает с заголовками Access-Control-Allow-Origin со значением наш домен и Access-Control-Allow-Credentials: true. Браузер отправляет авторизационную информацию и куки на удалённый сервер.

### MutationObserver

MutationObserver может реагировать на изменения в DOM: атрибуты, добавленные/удалённые элементы, текстовое содержимое.

Объекты MutationObserver используют внутри себя так называемые «слабые ссылки» на узлы, за которыми смотрят. Так что если узел удалён из DOM и больше не достижим, то он будет удалён из памяти вне зависимости от наличия наблюдателя.

### Cookie, Local storage, IndexedDB

Куки – это небольшие строки данных, которые хранятся непосредственно в браузере. 

Запись в document.cookie обновит только упомянутые в ней куки, но при этом не затронет все остальные.

* path=/, по умолчанию устанавливается текущий путь, делает куки видимым только по указанному пути и ниже.
* domain=site.com, по умолчанию куки видно только на текущем домене, если явно указан домен, то куки видно и на поддоменах.
* expires или max-age устанавливает дату истечения срока действия, без них куки умрёт при закрытии браузера.
* secure делает куки доступным только при использовании HTTPS.
* samesite запрещает браузеру отправлять куки с запросами, поступающими извне, помогает предотвратить XSRF-атаки.
____

Объекты веб-хранилища localStorage и sessionStorage позволяют хранить пары ключ/значение в браузере.

- В отличие от куки, объекты веб-хранилища не отправляются на сервер при каждом запросе. Поэтому мы можем хранить гораздо больше данных. Большинство браузеров могут сохранить как минимум 2 мегабайта данных (или больше), и этот размер можно поменять в настройках.
- Ещё одно отличие от куки – сервер не может манипулировать объектами хранилища через HTTP-заголовки. Всё делается при помощи JavaScript.
- Хранилище привязано к источнику (домен/протокол/порт). Это значит, что разные протоколы или поддомены определяют разные объекты хранилища, и они не могут получить доступ к данным друг друга.


| localStorage|	sessionStorage|
|---|---|
|Совместно используется между всеми вкладками и окнами с одинаковым источником|	Разделяется в рамках вкладки браузера, среди ифреймов из того же источника|
|«Переживает» перезапуск браузера|	«Переживает» перезагрузку страницы (но не закрытие вкладки)
___

IndexedDB – это встроенная база данных, более мощная, чем localStorage.

1. Подключить обёртку над промисами, например idb.
2. Открыть базу данных: idb.openDb(name, version, onupgradeneeded)
    * Создание хранилищ объектов и индексов происходит в обработчике onupgradeneeded.
    * Обновление версии – либо сравнивая номера версий, либо можно проверить что существует, а что нет.
3. Для запросов:
    * Создать транзакцию db.transaction('books') (можно указать readwrite, если надо).
    * Получить хранилище объектов transaction.objectStore('books').
4. Затем для поиска по ключу вызываем методы непосредственно у хранилища объектов.
5. Для поиска по любому полю объекта создайте индекс.

### Http/Https (HyperText Transfer Protocol /Secure/)
Защищенный протокол предлагает дополнительный уровень конфиденциальности, поскольку он использует SSL(Secure Sockets Layer)-сертификат для передачи данных.

HTTPS является защищенным протоколом, поскольку использует для передачи данных шифрованное соединение SSL.

Https гарантирует три основных уровня безопасности:
- Шифрование. При шифровании меняется информация, чтобы сохранить ее надежность.
- Целостность данных. Информация не может быть обнаружена или повреждена во время доставки, если ее не раскроют.
- Аутентификация устанавливает, что у ваших пользователей есть связь с сайтом.

HTTP использует порт 80, HTTPS — порт 443.

### REST / SOAP (Representational State Transfer)

REST - это архитектурный стиль и дизайн сетевых архитектур на основе программного обеспечения.

В общем случае REST является очень простым интерфейсом управления информацией без использования каких-то дополнительных внутренних прослоек. Каждая единица информации однозначно определяется глобальным идентификатором, таким как URL. 

Как происходит управление информацией ресурса — это целиком и полностью основывается на протоколе передачи данных. Наиболее распространенный протокол конечно же HTTP. Для HTTP действие над данными задается с помощью методов : GET (получить), PUT (добавить, заменить), POST (добавить, изменить, удалить), DELETE (удалить). Таким образом, действия CRUD (Create-Read-Update-Delete) могут выполняться как со всеми 4-мя методами, так и только с помощью GET и POST.

##### Основные принципы REST

* Stateless - каждый запрос клиента на сервер требует, чтобы его состояние было полностью представлено.
* Cacheable 
* Равномерный интерфейс - все компоненты должны взаимодействовать через единый унифицированный интерфейс.

SOAP (Simple Object Access Protocol) — протокол обмена структурированными сообщениями в распределённой вычислительной среде. Первоначально SOAP предназначался в основном для реализации удалённого вызова процедур. Протокол SOAP не различает вызов процедуры и ответ на него, а просто определяет формат послания (message) в виде документа XML. Послание может содержать вызов процедуры, ответ на него, запрос на выполнение каких-то других действий или просто текст. Спецификацию SOAP не интересует содержимое послания, она задает только его
оформление.

``` xml
<?xml version="1.0"?>

<soap:Envelope
xmlns:soap="http://www.w3.org/2003/05/soap-envelope/"
soap:encodingStyle="http://www.w3.org/2003/05/soap-encoding">

<soap:Header></soap:Header>
<soap:Body>
  <soap:Fault></soap:Fault>
</soap:Body>
</soap:Envelope>
```

### AJAX (Asynchronous JavaScript And XML)

При использовании AJAX нет необходимости обновлять каждый раз всю страницу, так как обновляется только ее конкретная часть.

#### XMLHttpRequst


XMLHttpRequest – это встроенный в браузер объект, который даёт возможность делать HTTP-запросы к серверу без перезагрузки страницы. XMLHttpRequest может работать с любыми данными, а не только с XML.

На сегодняшний день не обязательно использовать XMLHttpRequest, так как существует другой, более современный метод fetch.

В современной веб-разработке XMLHttpRequest используется, тк есть потребность в функционале, который fetch пока что не может предоставить, к примеру, отслеживание прогресса отправки на сервер.

XMLHttpRequest имеет два режима работы: синхронный и асинхронный.
Состояния запроса readyState
```
UNSENT = 0; // исходное состояние
OPENED = 1; // вызван метод open
HEADERS_RECEIVED = 2; // получены заголовки ответа
LOADING = 3; // ответ в процессе передачи (данные частично получены)
DONE = 4; // запрос завершён
```

- setRequestHeader(name, value)
- getResponseHeader(name)
- getAllResponseHeaders()

XMLHttpRequest может осуществлять запросы на другие сайты, используя ту же политику CORS, что и fetch.

Точно так же, как и при работе с fetch, по умолчанию на другой источник не отсылаются куки и заголовки HTTP-авторизации. Чтобы это изменить, установите xhr.withCredentials в true

#### Fetch 

Метод fetch() — современный и очень мощный способ для асинхронных сетевых запросов в JavaScript. 

``` javascript
let response = await fetch(url, options); // завершается с заголовками ответа
let result = await response.json(); // читать тело ответа в формате JSON
```
Процесс получения ответа обычно происходит в два этапа:

- Во-первых, promise выполняется с объектом встроенного класса Response в качестве результата, как только сервер пришлёт заголовки ответа.
- Во-вторых, для получения тела ответа нам нужно использовать дополнительный вызов метода.

* *response.text()* – читает ответ и возвращает как обычный текст,
* *response.json()* – декодирует ответ в формате JSON,
* *response.formData()* – возвращает ответ как объект FormData,
* *response.blob()* – возвращает объект как Blob (бинарные данные с типом),
* *response.arrayBuffer()* – возвращает ответ как ArrayBuffer (низкоуровневое представление бинарных данных),
* помимо этого, *response.body* – это объект ReadableStream, с помощью которого можно считывать тело запроса по частям, по мере поступления. 

Для Post запросов:
Для отправки POST-запроса или запроса с другим методом, нам необходимо использовать fetch параметры:

- method – HTTP метод, например POST,
- body – тело запроса

##### Прерывание запроса

``` javascript
// 1. Создание контроллера
let controller = new AbortController();
let signal = controller.signal;

// срабатывает при вызове controller.abort()
signal.addEventListener('abort', () => alert("отмена!"));
controller.abort(); // отмена!
alert(signal.aborted); // true

//2. Передача свойства signal опцией в метод
let controller = new AbortController();
fetch(url, {
  signal: controller.signal
});
//3. Прерывание выполнения
controller.abort();
```
* При вызове abort():
    - генерируется событие с именем abort на объекте controller.signal
    - свойство controller.signal.aborted становится равным true.

Когда fetch отменяется, его промис завершается с ошибкой AbortError. AbortController – масштабируемый, он позволяет отменить несколько вызовов fetch одновременно.

##### Запросы на другие сайты

Есть два вида запросов на другой источник:

- Простые.
- Все остальные.
Простые запросы будут попроще, поэтому давайте начнём с них.

**Простой запрос** – это запрос, удовлетворяющий следующим условиям:

1. Простой метод: GET, POST или HEAD
2. Простые заголовки – разрешены только:
    - Accept,
    - Accept-Language,
    - Content-Language,
    - Content-Type со значением application/ x-www-form-urlencoded, multipart/form-data или text/plain.

Любой другой запрос считается «непростым». Например, запрос с методом PUT или с HTTP-заголовком API-Key не соответствует условиям.

Принципиальное отличие между ними состоит в том, что «простой запрос» может быть сделан через `<form>` или `<script>`, без каких-то специальных методов.

Особые заголовки: 
- Access-Control-Allow-Origin - согласие принять такой запрос
- Access-Control-Expose-Headers - скрипту разрешено получить заголовки Content-Length и API-Key ответа

**Непростые запросы**

Мы можем использовать любой HTTP-метод: не только GET/POST, но и PATCH, DELETE и другие. Браузер не делает «непростые» запросы (которые нельзя было сделать в прошлом) сразу. Перед этим он посылает предварительный запрос, спрашивая разрешения.

Предварительный запрос использует метод OPTIONS, у него нет тела, но есть два заголовка:
```
Access-Control-Request-Method содержит HTTP-метод «непростого» запроса.
Access-Control-Request-Headers предоставляет разделённый запятыми список его «непростых» HTTP-заголовков.
```
Если сервер согласен принимать такие запросы, то он должен ответить без тела, со статусом 200 и с заголовками:
```
Access-Control-Allow-Methods должен содержать разрешённые методы.
Access-Control-Allow-Headers должен содержать список разрешённых заголовков.
```
Коме того, заголовок Access-Control-Max-Age может указывать количество секунд, на которое нужно кешировать разрешения. Так что браузеру не придётся посылать предзапрос для последующих запросов, удовлетворяющих данным разрешениям.

### WebSocket Server Sent Event

| WebSocket| 	EventSource|
|---|---|
|Двунаправленность: и сервер, и клиент могут обмениваться сообщениями|	Однонаправленность: данные посылает только сервер|
|Бинарные и текстовые данные|	Только текст|
|Протокол WebSocket|	Обычный HTTP|

EventSource, как и fetch, поддерживает кросс-доменные запросы. Мы можем использовать любой URL: