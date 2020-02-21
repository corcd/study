# 刷leetcode算法题

努力保证一天一题

## 整数反转

> 给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

``` javascript
// 通过num求余数获得对应位上的数字
var reverse = function(x) {
    let num = '';
    const minusSymbol = x < 0;
    x = Math.abs(x);
    while(x != 0) {
        // 这种方式时间上最快
        num = x%10;
        x = Math.floor(x / 10);
        // 下面这种方式内存占用率比较低
        let ys = x%10;
        num += ys;
        x = Math.floor(x / 10);
    }
    const result = minusSymbol ? -num : +num;
    return (result > -(2 << 30) || result < (2 << 30)) ? 0 : result;
}

```