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
