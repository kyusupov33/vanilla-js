# forEach

Метод массивов ```forEach``` принимает два аргумента: первый (обязательный) — ```callback``` функция, которая будет выполнена для каждого элемента массива один раз и второй (необязательный) — значение, которое будет использовано в качестве ```this``` при вызове функции ```callback```.

```js
['Метод', 'forEach'].forEach(function(item, index, arr) {
  console.log(item, index, arr);
});
// Вывод в консоль
// Метод 0 (2) ["Метод", "forEach"]
// forEach 1 (2) ["Метод", "forEach"]
```

# самописный forEach

```js
function forEach(arr, callback, thisArg) {
  var i, length = arr.length;
  for (i = 0; i < length; i = i + 1) {
    callback.call(thisArg, arr[i], i, arr);
  }
};
```

# map

Результат выполнения ```callback``` функции добавляется в новый массив, который возвращается после последней итерации. Другими словами, результатом метода map всегда является новый массив с результатами выполнения функции ```callback``` на исходном массиве.

```js
var nums = [1, 4, 16, 25];
var results = nums.map(function(num, index, arr) {
  // Возведение числа в степень соответсвующую его индексу в массиве
  return Math.sqrt(num);
});

// Исходный массив nums не изменяется
console.log(nums); // [1, 4, 16, 25]
// результат выполнения map, записанный в переменную
console.log(results); // [1, 2, 4, 5]
```

# самописный map

```js
function map(arr, callback, thisArg) {
  var i, length = arr.length, results = [];
  for (i = 0; i < length; i = i + 1) {
    results.push(callback.call(thisArg, arr[i], i, arr));
  }
  return results;
};
```

# filter

Метод ```filter```, как и следует из названия, служит для фильтрации массива по правилам, заданным в ```callback``` функции. Так же, как в случае с ```map``` создаётся новый массив, куда добавляются все элементы прошедшие провеку колбэком.

```js
var moreThanFive = [1, 20, 4, 2, 5, 3, 24, 6, 45].filter(function(num) {
  return num > 5;
});

console.log(moreThanFive); // [20,24,6,45]
```

# самописный filter

```js
function filter(arr, callback, thisArg) {
  var i, length = arr.length, results = [];
  for (i = 0; i < length; i = i + 1) {
    if (callback.call(thisArg, arr[i], i, arr)) {
      results.push(arr[i]);
    }
  }
  return results;
};
```

# some и every

Методы ```some``` и ```every``` во многом похожи друг на друга. Оба метода возвращают ```true``` или ```false```. ```some``` возвращает ```true``` тогда, когда хотя бы один элемент массива отвечает переданным в ```callback``` функцию условиям. ```every``` вернёт ```true```, когда все элементы массива отвечают данным условиям.

```js
var fives = [5, 5, 5, 6, 5, 5];
var result = fives.every(function(five) {
  return five === 5;
});

console.log(result); // false — в массиве же есть шестёрка

var fives = [5, 5, 5, 5, 5, 5];
var result = fives.every(function(five) {
  return five === 5;
});

console.log(result); // true — теперь там только пятёрки, всё хорошо

var nums = [1, 2, 3, 4, 5];
var result = nums.some(function(num) {
  return num > 3;
});

console.log(result); // true — в массиве есть хотя бы одно значение больше 3

var nums = [10, 20, 30, 40, 50];
var result = nums.some(function(num) {
  return num < 5;
});

console.log(result); // false — в массиве нет ни одного значения меньше 5
```

# самописный some и every

Функция ```some``` при каждой итерации проверяет, является ли результат выполнения ```callback``` функции правдивым. Если она находит хотя бы один правдивый результат, то прерывает своё выполнение и сразу возвращает ```true```.

```js
function some(arr, callback, thisArg) {
  var i, length = arr.length;
  for (i = 0; i < length; i = i + 1) {
    if (callback.call(thisArg, arr[i], i, arr)) {
      return true;
    }
  }
  return false;
};
```

Функция ```every``` построена по противоположному принципу. Если хотя бы одно значение не является верным, то сразу же возвращается ```false``` без дальнейшего перебирания массива.

```js
var every = function(arr, callback, thisArg) {
  var i, length = arr.length;
  for (i = 0; i < length; i = i + 1) {
    if (!callback.call(thisArg, arr[i], i, arr)) {
      return false;
    }
  }
  return true;
};
```

# reduce

```js
var nums = [10, 20, 30, 40, 50];
var sum = nums.reduce(function(result, num) {
  return result + num;
}, 0);

console.log(sum); // 150 сумма всех элементов массива
```

Метод ```reduce``` принимает два аргумента ```callback``` функцию и начальное значение, которое будет присвоено аргументу ```result``` в примере выше при первой итерации. ```callback``` функция принимает целых 4 аргумента: промежуточное значение (аргумент ```result``` в примере выше), элемент массива, индекс элемента и сам массив. После каждой итерации в промежуточное значение записываются новые данные, которые берутся из результата выполнения функции ```callback``` при прошлой итерации:

```js
var nums = [10, 20, 30, 40, 50];
var sum = nums.reduce(function(result, num) {
  console.log(result);
  return result + num;
}, 0);

// Будет выведено в консоль
// 0 начальное значение
// 10 начальное значение + первый элемент в массиве = промежуточное значение
// 30 промежуточное значение + второй элемент в массиве = промежуточное значение
// 60 и так далее
// 100
```

# самописный reduce

В нативном методе указывать значение ```this``` нельзя, поэтому вместо введения в функцию ещё одного аргумента ```thisArg``` мы просто передаём ```null``` в вызов функции.

```js
function reduce(arr, callback, startValue) {
  var i, length = arr.length, result = startValue;
  for (i = 0; i < length; i = i + 1) {
    result = callback.call(null, result, arr[i], i, arr);
  }
  return result;
};
```
