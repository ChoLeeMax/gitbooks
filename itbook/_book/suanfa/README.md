## 数组去重

```js
function qc(arr1) {
  //创建一个新的数组
  let arr = [];
  //遍历数组arr1
  for (let i = 0; i < arr1.length; i++) {
    //如果arr1不在arr中 会返回-1 那么将和这个元素存在新建的arr中
    if (arr.indexOf(arr1[i]) == -1) {
      arr.push(arr1[i]);
    }
  }
  //返回arr
  return arr;
}
arr1 = ["1", "1", "3", "5", "2", "24", "4", "4", "a", "a", "b"];

console.log(qc(arr1));
//输出结果["1", "3", "5", "2", "24", "4", "a", "b"]
```

## 冒泡排序

```js
function sort(elements) {
  for (var i = 0; i < elements.length - 1; i++) {
    for (var j = 0; j < elements.length - i - 1; j++) {
      //两个for循环 依次比较 把较大的数记录下来给后面 依次类推 达到排序的结果
      if (elements[j] > elements[j + 1]) {
        var swap = elements[j];
        elements[j] = elements[j + 1];
        elements[j + 1] = swap;
      }
    }
  }
  return elements;
}
var elements = [3, 1, 5, 9, 6];
console.log(sort(elements));
//输出结果[1, 3, 5, 6, 9]
```

## 快速排序

```js
var quicksort = function (arr) {
  if (arr.length <= 1) {
    return arr;
  }
  //取中间值作为一个标杆进行比较
  var midIndex = Math.floor(arr.length / 2);
  var midIndexVal = arr.splice(midIndex, 1);
  //新建两个数组存放比中间值大的或者小的
  var left = [];
  var right = [];
  for (var i = 0; i < arr.length; i++) {
    if (arr[i] < midIndexVal) {
      //如果比中间值小放在左面
      left.push(arr[i]);
    } else {
      //如果比中间值大放在右面
      right.push(arr[i]);
    }
  }
  //依次将两边的数组在调用quicksort函数最后进行concat连接数组
  return quicksort(left).concat(midIndexVal, quicksort(right));
};
var b = [1, 6, 2, 8, 4, 3, 70, 50, 30];
console.log(quicksort(b));
//[1, 2, 3, 4, 6, 8, 30, 50, 70]
```

## 冒泡

```js
//阶乘算法
function jiecheng(num) {
  if (num == 1) {
    return 1;
  }
  return num * jiecheng(num - 1);
}
console.log(jiecheng(5));

//斐波拉契题 1， 1， 2， 3， 5， 8， 13， 21 。。。

function fei(num) {
  if (num == 0 || num == 1) {
    return 1;
  }
  return fei(num - 1) + fei(num - 2);
}
console.log(fei(5));
```
