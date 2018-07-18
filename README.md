# Udemy: The Coding Interview Bootcamp: Data structure and algorithm 总结
 一周左右的时间，迅速的把所有视频，用1.5的速度刷了一遍，感觉还是挺有收获的。整体上来说，这个课程不算很难，并没有讲特别复杂的数据结构和算法问题，当然这个课程本身也是用来对付interview过程中的数据结构和算法问题多的，并不是教科书式的课程。

所以，这里面主要还是很多解决问题的trick，将印象比较深刻的记录下：

## String Reverse

利用`reduce`实现:

```
function reverse(str) {
  return str.split('').reduce((rev, char) => char + rev, '');
}
```
## Paldinromes
利用并不常用的`every`函数实现：
```
function palindrome(str) {
  return str.split('').every((char, i) => {
    return char === str[str.length - i - 1];
  });
}
```
补充一个递归的方法如下：
```
function palindromeRecursive(str) {
  if (str.length <= 1) {
    return true;
  } else {
    if (str[0] !== str[str.length - 1]) {
      return false;
    } else {
      return palindromeRecursive(str.substring(1, str.length - 1))
    }
  }
}
```
## Longest Paldinromes substring

```
function longestPalin(str) {
  str = treatStr(str);
  console.log(str)
  let max =  0;
  for (let i  = 1; i < str.length - 1; i++) {
    let radius = Math.min (i, str.length - i - 1);
    console.log(radius)
    let currentMax = getMaxSubLength(str, i, radius);
    if (max < currentMax) {
      max = currentMax + 1;
    }
  }
  return max;
}

function getMaxSubLength(str, i, radius) {
  let max = 0;
  for (let j = 0; j <= radius; j++) {
    if (str[i - j] === str[i + j]) {
      max = j;
    } else {
      break;
    }
  }
  return max;
}

function treatStr(str) {
  let rtn = '';
  for (let i = 0; i < str.length; i++) {
    if (i === str.length - 1) {
      rtn = rtn + str[i];
    } else  {
      rtn = rtn + str[i] + '#';
    }
    
  }
  return rtn;
}
```

## Integer Reversal
将一个integer进行翻转，这里面的几个函数，对负数等边界条件进行了很好的处理：
```
// --- Directions
// Given an integer, return an integer that is the reverse
// ordering of numbers.
// --- Examples
//   reverseInt(15) === 51
//   reverseInt(981) === 189
//   reverseInt(500) === 5
//   reverseInt(-15) === -51
//   reverseInt(-90) === -9

function reverseInt(n) {
  const reversed = n
    .toString()
    .split('')
    .reverse()
    .join('');

  return parseInt(reversed) * Math.sign(n);
}
```
## Anagrams
判断两个字符串是否是由相同的字符组成。有两个方法，第一个方法是从字符串构造一个map对象，每个键值对用来记录对应字符出现的次数，然后比较两个map对象是否相等。比较两个map对象是否相等的trick是，先比较长度是否相等，这样就不需要两个map之间的相互映射对比了。<br> 
1. 方法一：
```
function anagrams(stringA, stringB) {
  const aCharMap = buildCharMap(stringA);
  const bCharMap = buildCharMap(stringB);

  if (Object.keys(aCharMap).length !== Object.keys(bCharMap).length) {
    return false;
  }

  for (let char in aCharMap) {
    if (aCharMap[char] !== bCharMap[char]) {
      return false;
    }
  }

  return true;
}

function buildCharMap(str) {
  const charMap = {};

  for (let char of str.replace(/[^\w]/g, '').toLowerCase()) {
    charMap[char] = charMap[char] + 1 || 1;
  }

  return charMap;
}
```
2. 方法二：
第二个方法更简单一点，把两个字符串进行重组对字符进行排序，然后再判断重组排序之后的字符串是否相等就行了。这个方法可以利用很多help function，代码上要简单很多。
```
function anagrams(stringA, stringB) {
  return cleanString(stringA) === cleanString(stringB);
}

function cleanString(str) {
  return str
    .replace(/[^\w]/g, '')
    .toLowerCase()
    .split('')
    .sort()
    .join('');
}
```
## Printing Steps
想要实现的是下面这样的效果：
```
--- Examples
  steps(2)
      '# '
      '##'
  steps(3)
      '#  '
      '## '
      '###'
  steps(4)
      '#   '
      '##  '
      '### '
      '####'
```
普通方法就不介绍了，记录下recursive的解法, recursive的方法通常都会带一些有默认值的参数，每步recursive的过程需要调整这些参数的值：
```
function steps(n, row = 0, stair = '') {
  if (n === row) {
    return;
  }

  if (n === stair.length) {
    console.log(stair);
    return steps(n, row + 1);
  }

  const add = stair.length <= row ? '#' : ' ';
  steps(n, row, stair + add);
}
```

## Enter the Matrix Spiral
接收一个整数N，返回一个NxN的矩阵，如下
```
--- Examples
  matrix(2)
    [[undefined, undefined],
    [undefined, undefined]]
  matrix(3)
    [[1, 2, 3],
    [8, 9, 4],
    [7, 6, 5]]
 matrix(4)
    [[1,   2,  3, 4],
    [12, 13, 14, 5],
    [11, 16, 15, 6],
    [10,  9,  8, 7]]
```
这个问题感觉就是硬看人的思维逻辑了，没啥技巧可言，如果能在面试过程中想到下面这个方法，应该是创造力很强的人了：
```
function matrix(n) {
  const results = [];

  for (let i = 0; i < n; i++) {
    results.push([]);
  }

  let counter = 1;
  let startColumn = 0;
  let endColumn = n - 1;
  let startRow = 0;
  let endRow = n - 1;
  while (startColumn <= endColumn && startRow <= endRow) {
    // Top row
    for (let i = startColumn; i <= endColumn; i++) {
      results[startRow][i] = counter;
      counter++;
    }
    startRow++;

    // Right column
    for (let i = startRow; i <= endRow; i++) {
      results[i][endColumn] = counter;
      counter++;
    }
    endColumn--;

    // Bottom row
    for (let i = endColumn; i >= startColumn; i--) {
      results[endRow][i] = counter;
      counter++;
    }
    endRow--;

    // start column
    for (let i = endRow; i >= startRow; i--) {
      results[i][startColumn] = counter;
      counter++;
    }
    startColumn++;
  }

  return results;
}
```
## Fibonacci
利用recursive的方法来解决Fibonacci的问题，本来也是常规手法，但是原始的递归方法性能太差了，几乎是指数式的复杂度。这里引入了一个很通用的优化技巧：Memoization，其实就是对之前已经计算过的值进行缓存，这样再次需要这个结果的时候，就不用重复浪费计算了。另外，这是闭包的一个应用实例。
```
function memoize(fn) {
  const cache = {};
  return function(...args) {
    if (cache[args]) {
      return cache[args];
    }

    const result = fn.apply(this, args);
    cache[args] = result;

    return result;
  };
}

function slowFib(n) {
  if (n < 2) {
    return n;
  }

  return fib(n - 1) + fib(n - 2);
}

const fib = memoize(slowFib);
```
对于fibonacci这个具体的问题，其实memoize函数可以简化为下面这样：
```
function memoize(fn) {
	const cache = {};
	return function(n) {
		if(cache[n]) {
			return cache[n];
		}
		const result = fn(n);
		cache[n] = result;
		
		return result;
	}
}
```
为了处理各种各样的问题，对memoize函数进行了抽象，其中...args和apply的使用是亮点：
```
function memoize(fn) {
  const cache = {};
  return function(...args) {
    if (cache[args]) {
      return cache[args];
    }

    const result = fn.apply(this, args);
    cache[args] = result;

    return result;
  };
}
```
## Queue and Stack
其实在原生js的array同时包含了stack和queue的所有功能，单独创建这两个数据集是为了区别它们的不同。
```
class Queue {
  constructor() {
    this.data = [];
  }

  add(record) {
    this.data.unshift(record);
  }

  remove() {
    return this.data.pop();
  }
}
```
```
class Stack {
  constructor() {
    this.data = [];
  }

  push(record) {
    this.data.push(record);
  }

  pop() {
    return this.data.pop();
  }

  peek() {
    return this.data[this.data.length - 1];
  }
}
```

## Two Become One
所谓的Two Become one，就是说用两个stack来模拟queue的行为（Implement a Queue datastructure using two stacks）。实现起来，需要在两个stack之间，把数据倒来倒去的，如下：
```
const Stack = require('./stack');

class Queue {
  constructor() {
    this.first = new Stack();
    this.second = new Stack();
  }

  add(record) {
    this.first.push(record);
  }

  remove() {
    while (this.first.peek()) {
      this.second.push(this.first.pop());
    }

    const record = this.second.pop();

    while (this.second.peek()) {
      this.first.push(this.second.pop());
    }

    return record;
  }

  peek() {
    while (this.first.peek()) {
      this.second.push(this.first.pop());
    }

    const record = this.second.peek();

    while (this.second.peek()) {
      this.first.push(this.second.pop());
    }

    return record;
  }
}
```
## Linked List
Linked List的实现如下：
```
class Node {
  constructor(data, next = null) {
    this.data = data;
    this.next = next;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
  }

  insertFirst(data) {
    this.head = new Node(data, this.head);
  }

  size() {
    let counter = 0;
    let node = this.head;

    while (node) {
      counter++;
      node = node.next;
    }

    return counter;
  }

  getFirst() {
    return this.head;
  }

  getLast() {
    if (!this.head) {
      return null;
    }

    let node = this.head;
    while (node) {
      if (!node.next) {
        return node;
      }
      node = node.next;
    }
  }

  clear() {
    this.head = null;
  }

  removeFirst() {
    if (!this.head) {
      return;
    }

    this.head = this.head.next;
  }

  removeLast() {
    if (!this.head) {
      return;
    }

    if (!this.head.next) {
      this.head = null;
      return;
    }

    let previous = this.head;
    let node = this.head.next;
    while (node.next) {
      previous = node;
      node = node.next;
    }
    previous.next = null;
  }

  insertLast(data) {
    const last = this.getLast();

    if (last) {
      // There are some existing nodes in our chain
      last.next = new Node(data);
    } else {
      // The chain is empty!
      this.head = new Node(data);
    }
  }

  getAt(index) {
    let counter = 0;
    let node = this.head;
    while (node) {
      if (counter === index) {
        return node;
      }

      counter++;
      node = node.next;
    }
    return null;
  }

  removeAt(index) {
    if (!this.head) {
      return;
    }

    if (index === 0) {
      this.head = this.head.next;
      return;
    }

    const previous = this.getAt(index - 1);
    if (!previous || !previous.next) {
      return;
    }
    previous.next = previous.next.next;
  }

  insertAt(data, index) {
    if (!this.head) {
      this.head = new Node(data);
      return;
    }

    if (index === 0) {
      this.head = new Node(data, this.head);
      return;
    }

    const previous = this.getAt(index - 1) || this.getLast();
    const node = new Node(data, previous.next);
    previous.next = node;
  }

  forEach(fn) {
    let node = this.head;
    let counter = 0;
    while (node) {
      fn(node, counter);
      node = node.next;
      counter++;
    }
  }

  *[Symbol.iterator]() {
    let node = this.head;
    while (node) {
      yield node;
      node = node.next;
    }
  }
}
```
## Find the midpoint
这是一个有意思的问题，利用刚刚创建的Linked List，寻找一个链表的中心点。要求利用链表自己的API，不能用coutner计数器变量来实现，也不能获取链表的size信息，只能用逐个遍历的方法。
``` 
--- Directions
Return the 'middle' node of a linked list.
If the list has an even number of elements, return
the node at the end of the first half of the list.
*Do not* use a counter variable, *do not* retrieve
the size of the list, and only iterate
through the list one time.
--- Example
  const l = new LinkedList();
  l.insertLast('a')
  l.insertLast('b')
  l.insertLast('c')
  midpoint(l); // returns { data: 'b' }
```
实现方法如下：综合考虑奇、偶两种情况，需要对while循环的判断条件设计下。
```
function midpoint(list) {
  let slow = list.getFirst();
  let fast = list.getFirst();

  while (fast.next && fast.next.next) {
    slow = slow.next;
    fast = fast.next.next;
  }

  return slow;
}
```
