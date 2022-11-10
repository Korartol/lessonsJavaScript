# Работа с массивами

*Мутабельность = изменяемость*

- Cоздаем массив  

        const arr = [12,32,45,3] 

**arr** - просмотр что в массиве  
**arr.push(23, 543)** - добавляем в массив значения  
**arr.pop()** - удаляет последнее значение в массиве  
**arr.unshift(343)** - добавляет значение на первое место в массиве  
**arr.shift()** - удаляет первый элемент массива  

- Любой массив - это объект, для проверки введем:

        typeof arr

- Является ли элемент массивом

        Array.isArray(arr)

**A instances B** - поверяет является ли значение слева типом справа ( в данном случае - arr instanceof Array)  

**Array.from(«jdfdsfdh»)** - преобразует строку в массив посимвольно  

**arguments** - структура которая хранит все аргументы  

**arguments** - можно заменить на три точки … например  

- В рез-те все введенные аргументы преобразются в массив по типу Array.from

        function logArray(…arrs) {} 

- Варианты перебора значений в массиве:  

        Let numbers = [1,2,3,4,5]

- Самый простой вариант перебора:

      for(let i = 0; i < numbers.length; I++) {
      console.log(numbers[I]);
      }

По сути то же, перебираем число number из массива numbers:

        for(let number of numbers){
        console.log(number);
        }

Перебирает свойства объекта (в нашем примере - позиции элемента):

        for(let number in numbers){
        console.log(number);
        }

Чтобы получить из структуры numbers будем получать элемент который находится в переменной position:

        for(let position in numbers){
        console.log(numbers[position]);
        }

Перебор через callBack:

        numbers.forEach(item => console.log(item));  

# Поиск в массиве

Можно осуществить поиск в массиве через цикл:  

        let names = ["ivan", "zahar", "olga", "artem"];
        
        for(let name of names){
            if (name === "zahar"){
                console.log(name);
                break;
            }
        }

А можно через **indexOf** (indexOf покажет номер этого элемента (-1 - означает отсутствие элемента)):  

    names.indexOf("artem");

И еще через метод **includes** (он говорит есть ли этот элемент внутри массива true или false):  

    names.includes("artem");

**indexOf** ищет первый совпадающий элемент в массиве и возвращает его позицию, если одинаковых элементов несколько, то позицию последнего элемента можно найти через:  

    let numbers = [1,2,3,4,5,3,4,5,6,7,3,4,5,8,9];
    
    numbers.lastIndexOf(3); // результат - 10

# Расширенный поиск в массиве

Чтобы найти позицию в массиве исходя из условий (например что число должно делиться на 3) метод indexOf - не подойдет, необходимо использовать метод findIndex, но его можно использовать через функцию:

    let numbers = [1,3,3,4,5,6,4,4,4,5,6];
    function checkNumber(number){
      return number % 3 === 0
    }
    numbers.findIndex(checkNumber)  // результат 1

Либо можно сделать через формируемую функцию callBack:

    numbers.findIndex(number => number % 3 === 0);  // результат 1 (если такой позиции нет, результат будет -1)

Так же можно указать доп аргументы - это сам элемент **number**, его позиция **index** и массив **arr** :

    numbers.findIndex((number, index, arr) => number % 3 === 0);

Если нужно найти не позицию, а сам элемент - нужно использовать метод **find**:

    numbers.find(number => number % 3 === 0);  // результат 3 (если такого числа нет, результат будет undefined)

Если нужно проверить есть ли хоть один элемент удовлетворяющий требованием и если ДА, то вернет true, нужно использовать методо **some**:

    numbers.some(element => element % 3 === 0); // резульат true

Если нужно проверить подходят ли все элементы под наши условия - используем **every** :

    numbers.every(element => element % 3 === 0);  // результат false
    [2,4,6,10,20].every(element => element % 2 === 0); // результат true

Пример с другими данными:

    const empoyees = [
      {name: "Maria", departament: "IT", salary: 75000},
      {name: "Ivan", departament: "Sale", salary: 175000},
      {name: "Nikolay", departament: "IT", salary: 70000},
      {name: "Olga", departament: "IT", salary: 15000},
      {name: "Maria", departament: "Marketing", salary: 25000},
    ]  // заведем массив сотрудников
    
    empoyees.find(employee => employee.name === "Maria" && employee.departament === "IT")  // проверим есть ли мария с отдела АйТи
    
    empoyees.some(employee => employee.salary >= 90000)  // проверим есть ли хоть ОДИН кто получает больше или 90000
    
    empoyees.every(employee => employee.salary > 50000)  // проверим получают ли ВСЕ больше 50000

Метод **filter** вернет отфильтрованный массив:

    numbers.filter(number => number % 2 === 0)  // результат [4, 6, 4, 4, 4, 6]
    numbers.filter(number => number % 2 !== 0)  // результат [1, 3, 3, 5, 5]
    
    empoyees.filter(employer => employer.name === "Maria")  // результат 2-е марии
    

