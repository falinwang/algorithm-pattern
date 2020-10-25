# Javascript Data Structure 筆記

## Javascript 的 class

### 1. Reference Type

範例 1：
``` Javascript
[] === []     //false
[1] === [1]   //false
```
範例 2：
``` Javascript
var object1 = { value: 10 };
var object2 = object1;
var object3 = { value: 10 };
object1 === object2 //true
object1 === object3 //false

object1.value = 15;
object2.value // 15;
object3.value // 10;
```
Primitive Type

- Defined by the language
- JavaScript built-in types
  - boolean
  - number
  - bigint
  - string
  - null
  - undefined
  - symbol
  - object


Reference Type

- Created by programmers
- 在範例 2:
  - object 1 and object 2 are actually object 1
  - (object 2 references object 1)
  - object 3 is the new object and is actually object 3



### 2. Instantiation

ES6 範例：

``` JavaScript
class Player {
	constructor(name, type) {
		this.name = name;
		this.type = type;
	}
	introduce() {
		console.log(`Hi I am ${this.name}, I am a ${this.type}`);
	}
}

class Wizard extends Player {
  constructor(name, type) {
		super(name, type);
  }
  play() {
    console.log(`Coool I am a ${this.type}`);
  }
}

const wizard1 = new Wizard('Shelly', 'Healer');
const wizard2 = new Wizard('Shawn', 'Dark Magic');

wizard1.play(); // Coool I am a Healer
wizard1.introduce(); // Hi I am Shelly, I am a Healer
```

以前的作法 (不推薦)：

``` JavaScript
var Player = function(name, type) {
  this.name = name;
  this.type = type;
}

Player.prototype.introduce = function() {
  console.log(`Hi I am ${this.name}, I am a ${this.type}`);
} 

const wizard1 = new Wizard('Shelly', 'Healer');
const wizard2 = new Wizard('Shawn', 'Dark Magic');

wizard1.play = function() {
  console.log(`Coool I am a ${this.type}`);
}

wizard2.play = function() {
  console.log(`Coool I am a ${this.type}`);
}
```


---


## Array

### Complexity

- Lookup: O(1)
- Push: O(1)
- Insert: O(n)
- Delete: O(n)


### Traits

- organize data sequentially
- in-memory: one after another
- smallest footprint
- 內容可以是 string, numbers, objects 等

``` Javascript
const strings = ['a', 'b', 'c', 'd'];
const numbers = [1, 2, 3, 4, 5];
```

### Operations


- Index 取項目

``` Javascript
let first = strings[0]  // 'a'
let last = strings[strings.length - 1]  // 'd'
```

- 迴圈用 `forEach`


``` Javascript
// forEach

strings.forEach(function(item, index, array) {
  console.log(item, index);
})

// 'a' 0
// 'b' 1
// 'c' 2
// 'd' 3

// 或是用 ES6 Arrow Function

strings.forEach((item, index, array) => console.log(item, index))
```

- 基本 Manipulation

``` Javascript
// push 加在最後面
// O(1)

strings.push('e'); 

// ['a', 'b', 'c', 'd', 'e']


// pop 把最後面的東西丟掉
// O(1)

strings.pop(); 

// ['a', 'b', 'c', 'd']


// unshift 加東西在前面
// O(n) 因為要移動所有的內容到新的index

strings.unshift('x');

// ['x', 'a', 'b', 'c', 'd']


// shift 移除最前面的項目
// O(n)

let first = strings.shift();

// first: ['x']
// new array: ['a', 'b', 'c', 'd']

```

- [Splice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) 用法
  - `.splice(start, delCount, item1, item2, ...)` (start 可以是 -1，指最後面)
  - 可以加東西，也可以移除東西，並且指定移除開始的位置、插入的位置


  1. Insert

  ``` Javascript
  const months = ['Jan', 'March', 'April', 'June'];
  let removed = months.splice(2, 0, 'drum');

  console.log(months)
  // months   ["Jan", "March", "drum", "April", "June"]
  console.log(removed)
  // removed = []
  ```

  2. Remove
  ``` Javascript
  const months = ['Jan', 'March', 'April', 'June'];
  let removed = months.splice(2, 1, 'tree');

  console.log(months)
  // months = ["Jan", "March", "tree", "June"]
  console.log(removed)
  // removed  ["April"]
  ```

- [Slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) 用法
  - `.slice([start[, end]])`
    - Start: 開始的位置
    - End (optional): 會擷取在end前一個，-1代表最後面，如果不寫就是到 array.length
  - 把 array 切片取出 sub-array
  ``` JavaScript
  const animals = ['0', '1', '2', '3', '4'];

  console.log(animals.slice(2));
  // expected output: Array ["2", "3", "4"]

  console.log(animals.slice(2, 4));
  // expected output: Array ["2", "3"]

  console.log(animals.slice(1, 5));
  // expected output: Array ["1", "2", "3", "4"]
  ```

### Static vs. Dynamic Arrays

- **Static**: 在建立Array的時候先指定好了大小
  - C++: `int a[20]` or `int b[5] {1,2,3,4,5}`
- **Dynamic**: copy and rebuild the array at a new location with more memory
  - 所以 append (or push) 可能會是 O(n), 因為需要 loop 過一遍去複製到新的 array
  - 語言：Javascript, Python, Java 的 dynamic list
  - Big O
    - Lookup 查找: O(1)
    - Append 新增在後面: O(1), can be O(n)
    - Insert 加在任一位置: O(n)
    - Delete 刪除: O(b)