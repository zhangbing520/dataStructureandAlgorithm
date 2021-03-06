# JavaScript 实现集合与字典



### 一、集合结构

#### 1.1.简介

集合比较常见的实现方式是**哈希表**，这里使用JavaScript的Object类进行封装。

集合通常是由一组**无序的**、**不能重复**的元素构成。

- 数学中常指的集合中的元素是可以重复的，但是计算机中集合的元素不能重复。

**集合是特殊的数组：**

- 特殊之处在于里面的元素**没有顺序**，**也不能重复**。
- 没有顺序意味着**不能通过下标值进行访问**，不能重复意味着**相同的对象**在集合中只会**存在一份**。

**实现集合类**：

- 在ES6中的**Set**类就是一个集合类，这里我们重新封装一个Set类，了解集合的底层实现。
- JavaScript中的**Object**类中的**key**就是一个集合，可以使用它来封装集合类Set。

**集合常见的操作**：

- `add（value）`：向集合添加一个新的项；
- `remove（value）`：从集合中移除一个值；
- `has（value）`：如果值在集合中，返回`true`，否则返回`false`；
- `clear（）`：移除集合中的所有项；
- `size（）`：返回集合所包含元素的数量，与数组的`length`属性相似；
- `values（）`：返回一个包含集合中所有值的数组；

还有其他的方法，用的不多这里不做封装；

#### 1.2.代码实现

```js
    //封装集合类
    function Set() {
      //属性
      this.items = {}

      //方法
      //一.has方法
      Set.prototype.has = value => {
        return this.items.hasOwnProperty(value)
      }

      //二.add方法
      Set.prototype.add = value => {
        //判断集合中是否已经包含该元素
        if (this.has(value)) {
          return false
        }
        //将元素添加到集合中
        this.items[value] = value//表示该属性键和值都为value
        return true//表示添加成功
      }

      //三.remove方法
      Set.prototype.remove = (value) => {
        //1.判断集合中是否包含该元素
        if (!this.has(value)) {
          return false
        }

        //2.将元素从属性中删除
        delete this.items[value]
        return true
      }

      //四.clear方法
      Set.prototype.clear = () => {
        //原来的对象没有引用指向，会被自动回收
        this.items = {}
      }

      //五.size方法
      Set.prototype.size = () => {
        return Object.keys(this.items).length
      }

      //获取集合中所有的值
      //六.values方法
      Set.prototype.values = function() {
        return Object.keys(this.items)
      }
    }
```

**测试代码：**

```js
    //测试集合类
    //1.创建Set类对象
    let set = new Set()

    //添加元素
    //2.测试add方法
    console.log(set.add('a'));										
    console.log(set.add('a'));									
    console.log(set.add('b'));										
    console.log(set.add('c'));										
    console.log(set.add('d'));										

    //3.测试values方法
    console.log(set.values());										

    //删除元素
    //4.测试remove方法
    console.log(set.remove('a'));									
    console.log(set.remove('a'));									
    console.log(set.values());										

    //5.测试has方法
    console.log(set.has('b'));										

    //6.测试size方法和clear方法
    console.log(set.size());										
    set.clear()
    // 由于clear方法的实现原理为指向另外一个空对象，所以不影响原来的对象
    console.log(set.size());										
    console.log(set.values());										
```

**测试结果：**

[![image-20200228183431635](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E9%9B%86%E5%90%88%E5%92%8C%E5%AD%97%E5%85%B8/1.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/集合和字典/1.png)





#### 1.3.集合间的操作

**集合间操作：**

- **并集**：对于给定的两个集合，返回一个包含两个集合中所有元素的新集合；
- **交集**：对于给定的两个集合，返回一个包含两个集合中共有元素的新集合；
- **差集**：对于给定的两个集合，返回一个包含所有存在于第一个集合且不存在于第二个集合的元素的新集合；
- **子集**：验证一个给定集合是否是另一个集合的子集；

[![image-20200228210239984](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E9%9B%86%E5%90%88%E5%92%8C%E5%AD%97%E5%85%B8/2.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/集合和字典/2.png)





**并集的实现：**

实现思路：创建集合C代表集合A和集合B的并集，先将集合`A`中的所有元素添加到集合`C`中，再遍历集合`B`，如果是集合`C`所没有的元素就把它添加到集合`C`中。

```js
 Set.prototype.union = otherSet => {
      // this:集合对象A
      // otherSet:集合对象B
      //1.创建一个新的集合
      let unionSet = new Set()

      //2.将A集合中的所有元素添加到新集合中
      let values = this.values()
      // for(let i of values){
      //   unionSet.add(i)
      // }
      for(let i = 0;i < values.length;i++){
        unionSet.add(values[i])
      }

      //3.取出B集合中的元素,判断是否需要加到新集合中
      values = otherSet.values()
      // for(let i of values){
      //   //由于集合的add方法已经对重复的元素进行了判断,所以这里可以直接添加
      //   unionSet.add(i)
      // }
      for(let i = 0;i < values.length;i++){
        unionSet.add(values[i])
      }
      return unionSet
    }
```

**交集的实现：**

实现思路：遍历集合A，当取得的元素也存在于集合B时，就把该元素添加到另一个集合C中。

```js
 Set.prototype.intersection = otherSet => {
      // this:集合A
      // otherSet:集合B
      //1.创建新的集合
      let intersectionSet = new Set()
      
      //2.从A中取出一个元素，判断是否同时存在于集合B中，是则放入新集合中
      let values = this.values()
      for(let i =0 ; i < values.length; i++){
        let item = values[i]
        if (otherSet.has(item)) {
          intersectionSet.add(item)
        }
      }
      return intersectionSet
    }
```

**差集的实现：**

实现思路：遍历集合A，当取得的元素不存在于集合B时，就把该元素添加到另一个集合C中。

```js
Set.prototype.diffrence = otherSet => {
        //this:集合A
        //otherSet:集合B
        //1.创建新的集合
        var diffrenceSet = new Set()

        //2.取出A集合中的每一个元素，判断是否同时存在于B中，不存在则添加到新集合中
        var values = this.values()
        for(var i = 0;i < values.length; i++){
          var item = values[i]
          if (!otherSet.has(item)) {
            diffrenceSet.add(item)
          }
        }
        return diffrenceSet
      }
```

**子集的实现：**

实现思路：遍历集合A，当取得的元素中有一个不存在于集合B时，就说明集合A不是集合B的子集，返回false。

```js
 Set.prototype.subset = otherSet => {
        //this:集合A
        //otherSet：集合B
        //遍历集合A中的所有元素，如果发现，集合A中的元素，在集合B中不存在，那么放回false，如果遍历完整个集合A没有返回false，就返回true
        let values = this.values()
        for(let i = 0; i < values.length; i++){
          let item = values[i]
          if(!otherSet.has(item)){
            return false
          }
        }
        return true
      }
```

### 二、字典结构

#### 2.1.简介

**字典的特点**：

- 字典存储的是键值对，主要特点是一一对应；
- 比如保存一个人的信息：数组形式：[19，‘Tom’，1.65]，可通过下标值取出信息；字典形式：{"age"：19，"name"："Tom"，"height"：165}，可以通过key取出value。
- 此外，在字典中**key**是**不能重复**且**无序**的，而**Value**可以**重复**。

**字典和映射的关系**：

- 有些编程语言中称这种**映射关系**为**字典**，如Swift中的Dictonary，Python中的dict；
- 有些编程语言中称这种**映射关系**为**Map**，比如Java中的HashMap&TreeMap等；

**字典类常见的操作**：

- set(key,value)：向字典中添加新元素。
- remove(key)：通过使用键值来从字典中移除键值对应的数据值。
- has(key)：如果某个键值存在于这个字典中，则返回`true`，反之则返回`false`。
- get(key)：通过键值查找特定的数值并返回。
- clear()：将这个字典中的所有元素全部删除。
- size()：返回字典所包含元素的数量。与数组的`length`属性类似。
- keys()：将字典所包含的所有键名以数组形式返回。
- values()：将字典所包含的所有数值以数组形式返回。

#### 2.2.封装字典

字典类可以基于JavaScript中的对象结构来实现，比较简单，这里直接实现字典类中的常用方法。

```js
//封装字典类
function Dictionary(){
  //字典属性
  this.items = {}

  //字典操作方法
  //一.在字典中添加键值对
  Dictionary.prototype.set = function(key, value){
    this.items[key] = value
  }

  //二.判断字典中是否有某个key
  Dictionary.prototype.has = function(key){
    return this.items.hasOwnProperty(key)
  }

  //三.从字典中移除元素
  Dictionary.prototype.remove = function(key){
    //1.判断字典中是否有这个key
    if(!this.has(key)) return false

    //2.从字典中删除key
    delete this.items[key]
    return true
  }

  //四.根据key获取value
  Dictionary.prototype.get = function(key){
    return this.has(key) ? this.items[key] : undefined
  }

  //五.获取所有keys
  Dictionary.prototype.keys = function(){
    return Object.keys(this.items)
  }

  //六.size方法
  Dictionary.prototype.keys = function(){
    return this.keys().length
  }

  //七.clear方法
  Dictionary.prototype.clear = function(){
    this.items = {}
  }
}
```

> 参考资料:[JavaScript数据结构与算法](https://www.bilibili.com/video/BV1x7411L7Q7?from=search&seid=3912456004602554239)