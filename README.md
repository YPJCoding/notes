# notes
开发过程中总结出的一些经验

# 小程序
## 小程序的双向绑定很鸡肋(model:value)，当页面中input较多时赋值就变得些许麻烦。
```
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
```
const num1 = ~~ 1.69;
const num2 = 1.69 | 0;
const num3 = 1.69 >> 0;
// num1 num2 num3 => 1 1 1
```
## 时间戳
```
const timestamp = +new Date("2021-06-19");
// timestamp => 1550102400000
```
## 精确小数
```
const RoundNum = (num, decimal) => Math.round(num * 10 ** decimal) / 10 ** decimal;
const num = RoundNum(1.69, 1);
// num => 1.7
```
## 优雅处理Async/Await参数
```
function AsyncTo(promise) {
    return promise.then(data => [null, data]).catch(err => [err]);
}
const [err, res] = await AsyncTo(Func());
```
