#  Синхронность и асинхронность
- **Синхронным** называется событие которое выполняется последовательно друг за другом
- **Асинхронностью** назыввается событие которое выполняется по каким-то другим критериям, например по таймеру, при наведении мыши или по какому либо иному событию/

## setTimeout
    function showGreating(){
        console.log("Hello user");
    }

    setTimeout(showGreating, 2000);

    // Результат - Hello user

В функцию setTimeout не нужно передавать вызов функции, в этом случает вызов функции произойдет моментально и будет ошибка:

    setTimeout(showGreating(), 5000)
    VM12120:2 Hello user
    VM12307:1 Refused to evaluate a string as JavaScript because 'unsafe-eval' is not an allowed source of script in the 
    following Content Security Policy directive: "script-src 'nonce-G+Mxi7qRWl3fS7ojjIgrtg==' mc.yandex.com yastatic.net 
    yandex.ru mc.yandex.ru *.mc.yandex.ru adstat.yandex.ru".

Можно сразу вывести результат через стрелочную функцию:

    setTimeout(() => console.log("Hello user"), 3000);

Или вызовем другую функцию, например showGreating():

    setTimeout(() => showGreating(), 3000);  // в данном случае результат вывода будет одинаковый

Создадим цикл, который будет высчитывать разницу времени задержки между текущим временем и временем через сколько сработала задержка:

    for(let i = 0; i < 10; i++){
        const currentDate = new Date();
        setTimeout(() => {
            const now = new Date();
            console.log(now - currentDate);
        }, 3000);  // задержка 3 сек
    }

т.к. js работает в одном потоке задержка не будет ровно как указана (хотя на моем ПК все было очень близко).

Следующий пример, давайте преведущие вызовы запишим в функцию:

    function doAsincAction(){
        const currentDate = new Date();
        setTimeout(() => {
            const now = new Date();
            console.log(now - currentDate);
        }, 3000);  // задержка 3 сек
    }

// реализуем функцию которая будет добавлять бесполезную нагрузку, например функцию ссумирования:

    function sumChisel(){
        let sum = 0;
        for(let i = 0; i <= 10_000_000; i++){  // суммируем числа от 0 до 10 млн с шагом 1
            sum += i;
        }
        return sum;
    }

// теперь реализуем асинхронные действия вместе с бесполезными длительными вычислениями:

    for(let i = 0; i < 10; i++){
        doAsincAction();
        sumChisel();  // если убрать бесполезную нагрузку, то задержка уменьшиться
    }

Если мы уберем время задержи или выставим его в 0, то минимальная зедержка всеравно будет!


Для чего нужны остальные аргументы для функции setTimeout:

    function doAsincAction(){
        const currentDate = new Date();
        setTimeout((a,b) => {               // добавляем аргументы a,b
            const now = new Date();
            console.log(now - currentDate);
            console.log(a,b)                // выводим аргументы a,b
        }, 3000, "аргумент a", {a: 123, b: "stroki"});    // передаем аргументы в setTimeout a,b они могут быть разного типа!
    }

Или можно сразу передать эти аргументы в функцию при вводе:

    function doAsincAction(a,b){
        const currentDate = new Date();
        setTimeout((c,d) => {               // переименуем аргументы, они могут быть любыми
            const now = new Date();
            console.log(now - currentDate);
            console.log(c,d)                // переменуем аналогично сет таймауту
        }, 3000, a,b);                      // передаем аргументы в setTimeout a,b они могут быть разного типа!
    }

### Идентификаторы таймаута

    setTimeout(() => console.log("Асинхронное действие"), 5000);
    314  // идентификатор таймаута который можно использовать
    VM13748:1 Асинхронное действие // результат выведенный через 5 сек

Как использовать идентификатор таймаута:

например функция:

    clearTimeout(314) // передали значение идентификатора

после этого действия выполнение остановится (главное успеть в 5 секундный интервал или увеличить задержку)

## setInterval

setTimeout вызывает callback один раз, а setTimeout - многократно!

Функция setInterval также как и setTimeout принимает 2 обязательных аргумента это callback и время задержки

    setInterval(() => console.log("еще задерживаемся?"), 3000);  // задержка 3 сек
    322  // идентификатор
    8VM14175:1 еще задерживаемся?  // каждые 3 секунды появляется вывод колбека

как остановить это? :

    clearInterval(322)  // вводим значение идентификатора


замерием время между выводами:

    const now = new Date();  // записываем текущее время
    
    setInterval(() => {
        const callBackTime = new Date(); // записываем время вывода колбека
        console.log("уже приехали?");
        console.log(callBackTime - now);  // выводим разницу времени
    }, 2000);  // задержка 2 сек


## Работа с HTTP

Создадим объект который будет отправлять HTTP запросы:

    let xhr = new XMLHttpRequest();  // создали объект

    // создадим функцию которая будет обрабатывать ответ
    
    function processFinish(){
        console.log(xhr.responseText);
    }

    xhr.onload = processFinish;  // запишем в свойство onload функцию processFinish которая будет выполняться после выполнения запроса

    xhr.open("GET", "https://www.cbr-xml-daily.ru/daily_json.js");  // для отправки запроса откроем соединение с помощью метода open (получаем курсы валют)
    xhr.send();  // отправляеи запрос
