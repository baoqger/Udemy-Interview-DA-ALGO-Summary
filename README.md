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
