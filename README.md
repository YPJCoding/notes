# notes
开发过程中总结出的一些经验

# 小程序
## 小程序的双向绑定很鸡肋(model:value)，当页面中input较多时赋值就变得些许麻烦。
```javascript
<input  bindinput="input"   data-type="name" /> 
<input  bindinput="input"   data-type="phone" /> 
<input  bindinput="input"   data-type="password" /> 

data(){
  name: "",
  phone: "",
  password: ""
}

input(e) {
  this.setData({
    [`${e.target.dataset.type}`]: e.detail.value
  })
  console.log(this.data[`${e.target.dataset.type}`])
}
```
## 取整
```javascript
const num1 = ~~ 1.69;
const num2 = 1.69 | 0;
const num3 = 1.69 >> 0;
// num1 num2 num3 => 1 1 1
```
## 时间戳
```javascript
const timestamp = +new Date("2021-06-19");
// timestamp => 1550102400000
```
## 精确小数
```javascript
const RoundNum = (num, decimal) => Math.round(num * 10 ** decimal) / 10 ** decimal;
const num = RoundNum(1.69, 1);
// num => 1.7
```
## 优雅处理Async/Await参数
```javascript
function AsyncTo(promise) {
    return promise.then(data => [null, data]).catch(err => [err]);
}
const [err, res] = await AsyncTo(Func());
```

## 输入框 金额 两位小数
```javascript
/**
 * 金额输入 两位小数
 * @param {Number} val 
 */
export function money(val) {
  let num = val.toString(); //先转换成字符串类型
  if (num.indexOf('.') == 0) { //第一位就是 .
    num = '0' + num
  }
  num = num.replace(/[^\d.]/g, "");  //清除“数字”和“.”以外的字符
  num = num.replace(/\.{2,}/g, "."); //只保留第一个. 清除多余的
  num = num.replace(".", "$#$").replace(/\./g, "").replace("$#$", ".");
  num = num.replace(/^(\-)*(\d+)\.(\d\d).*$/, '$1$2.$3'); //只能输入两个小数
  if (num.indexOf(".") < 0 && num != "") {
    num = parseFloat(num);
  }
  return num
}
```

## 输入框 纯数字
```javascript
e.replace(/[^\d]/g, '')
```

## 当if的判断条件比较多时，或许更需要用Array.some | Array.every |Array.includes
```javascript
if(a === 1 || a === 2 || a ===3 || a ===4){
   //....
}

//更容易读的写法
if([1,2,3,4].includes(a)){
  //....
}

//多条件语句
if(condition1 && condition2 && condition3){
   //...
}

//更易读的写法
const conditions = [condition1, condition2, condition3];
if(conditions.every(c=>c)){
   //...
}

// 多条件或
if(condition1 || condition2 || condition3){
   //...
}

// 更易读的写法
const conditions = [condition1, condition2, condition3];
if(conditions.some(c=>c)){
   //...
}

```
