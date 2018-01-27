## 常见数组处理

#### 1、数组去重
```
var unique = function (arr) {
let hashobj = {}
let result = []
for(let i=0,len=arr.length; i<len; i++){
  if(!hashobj[arr[i]]){
    hashobj[arr[i]] = true
    result.push(arr[i])
  }
}
return result
}
```
思想是利用object的key值唯一

#### 2、统计一个字符串中出现最多的字母
基于数组去重，将object[key]的值存入出现的次数。一遍循环后，将object循环比大小。

#### 3、不借助临时变量，交换两整数
```
function swap(a,b){
	b = b-a
	a = a+b
	b = a-b
	return [a,b]
}
```

#### 4、求正数组最大差值
相当于求数组中最大最小值，for循环一遍，找出当前的最小值，用当下的数与最小值做差，得到的差值求最大值。

#### 5、随机生成指定长度的字符串
```
function randomString(n){
	let str = 'abcdefghijklmnopqrstuvwxyz0123456789'
	let result = '',len=str.length
	for(let i=0; i<n; i++){
		result += str.charAt(Math.floor(Math.random * len))
	}
	return result
}
```















