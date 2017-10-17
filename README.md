# bug-in-for
for循环中删除数组的bug
使用for删除数组指定元素的bug:
数组中没有直接删除数组指定元素的方法，因此需要在原型上添加：
```
Array.prototype.remove = function(val) {
    var index = this.indexOf(val);
    if (index > -1) {
    this.splice(index, 1);
  }
};

```
有以下数组，需要删除元素33和44
```
var emp=[11,22,33,44,55]
for(i in emp){
	console.log(emp[i])
	if(emp[i]==33||emp[i]==44){
		emp.remove(emp[i])
	}
}
```
打印emp的值为[11,22,44,55]
出现这种现象的原因在于for循环中，删除了某一元素，会导致后面的元素顶替被删除的元素，而循环的指针却没有发生相应的改变，进入下次循环后，会忽略掉被之前删除元素的下一个元素，因此会漏删。要解决漏删元素的问题可以采用以下的办法：
### 1.
```
var emp=[11,22,33,44,55]
var b=[]
for(i in emp){
	console.log(emp[i])
	if(emp[i]==33||emp[i]==44){
		b.push(emp[i])
	}
} 
for(i in b){
	emp.remove(b[i])
}
```
打印emp的值为[11,22,55]
### 2.lodash的数组remove方法：
```
 var emp=[11,22,33,44,55]
     _.remove(emp, function(n) {
           if(n==33 || n==44){
               console.log(n)
                return n ;
           }
          
        });
 ```
 打印emp的值为[11,22,55]
