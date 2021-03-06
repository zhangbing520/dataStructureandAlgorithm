## JavaScript实现哈希表

### 一、哈希表简介

#### 1.1.认识哈希表

哈希表通常是基于**数组**实现的，但是相对于数组，它存在更多优势：

- 哈希表可以提供非常快速的**插入-删除-查找操作**；
- 无论多少数据，插入和删除值都只需要非常短的时间，即O(1)的时间级。实际上，只需要**几个机器指令**即可完成；
- 哈希表的速度比**树还要快**，基本可以瞬间查找到想要的元素。但是相对于树来说编码要简单得多。

**哈希表同样存在不足之处**：

- 哈希表中的数据是**没有顺序**的，所以不能以一种固定的方式（比如从小到大 ）来遍历其中的元素。
- 通常情况下，哈希表中的key是**不允许重复**的，不能放置相同的key，用于保存不同的元素。

**哈希表是什么？**

- 哈希表并不好理解，不像数组、链表和树等可通过图形的形式表示其结构和原理。
- 哈希表的结构就是**数组**，但它神奇之处在于对**下标值的一种变换**，这种变换我们可以称之为**哈希函数**，通过哈希函数可以获取**HashCode**。

**通过以下案例了解哈希表：**

- 案例一：公司想要存储1000个人的信息，每一个工号对应一个员工的信息。若使用数组，增删数据时比较麻烦；使用链表，获取数据时比较麻烦。有没有一种数据结构，能把某一员工的姓名转换为它对应的工号，再根据工号查找该员工的完整信息呢？没错此时就可以使用哈希表的哈希函数来实现。
- 案例二：存储联系人和对应的电话号码：当要查找张三（比如）的号码时，若使用数组：由于不知道存储张三数据对象的下标值，所以查找起来十分麻烦，使用链表时也同样麻烦。而使用哈希表就能通过哈希函数把张三这个名称转换为它对应的下标值，再通过下标值查找效率就非常高了。

也就是说：哈希表最后还是基于数据来实现的，只不过哈希表能够通过哈希函数把字符串转化为对应的**下标值**，建立字符串和下标值的对应关系。

#### 1.2.哈希化的方式

为了把字符串转化为对应的下标值，需要有一套编码系统，为了方便理解我们创建这样一套编码系统：比如**a为1，b为2，c为3，以此类推z为26，空格为27（不考虑大写情况）**。

有了编码系统后，将字母转化为数字也有很多种方式：

- **方式一**：数字相加。例如**cats转化为数字**：3+1+20+19=43，那么就把43作为cats单词的下标值储存在数组中；

  但是这种方式会存在这样的问题：很多的单词按照该方式转化为数字后都是43，比如was。而在数组中**一个下标值**只能**储存一个数据**，所以该方式不合理。

- **方式二**：幂的连乘。我们平时使用的**大于10的数字**，就是用**幂的连乘**来表示它的唯一性的。比如： 6543=6 * 103 + 5 * 102 + 4 * 10 + 3；这样单词也可以用该种方式来表示：cats = 3 * 273 + 1 * 272 + 20 * 27 + 17 =60337;

  虽然该方式可以保证字符的唯一性，但是如果是较长的字符（如aaaaaaaaaa）所表示的数字就非常大，此时要求很大容量的数组，然而其中却有许多下标值指向的是无效的数据（比如不存在zxcvvv这样的单词），造成了数组空间的浪费。

**两种方案总结：**

- 第一种方案（让数字相加求和）产生的**数组下标太少**；
- 第二种方案（与27的幂相乘求和）产生的**数组下标又太多**；

现在需要一种**压缩方法**，把幂的连乘方案系统中得到的**巨大整数范围**压缩到**可接受的数组范围**中。可以通过取余操作来实现。虽然取余操作得到的结构也有可能重复，但是可以通过其他方式解决。

**哈希表的一些概念：**

- **哈希化：**将**大数字**转化成**数组范围内下标**的过程，称之为**哈希化**；
- **哈希函数：**我们通常会将**单词**转化成**大数字**，把**大数字**进行**哈希化**的代码实现放在一个函数中，该函数就称为**哈希函数**；
- **哈希表：**对最终数据插入的**数组**进行整个**结构的封装**，得到的就是**哈希表**。

**仍然需要解决的问题**：

- 哈希化过后的下标依然可能**重复**，如何解决这个问题呢？这种情况称为**冲突**，冲突是**不可避免**的，我们只能**解决冲突**。

#### 1.3.解决冲突的方法

**解决冲突常见的两种方案：**

- 方案一：**链地址法**（**拉链法**）；

如下图所示，我们将每一个数字都对**10**进行取余操作，则余数的范围**0~9**作为数组的下标值。并且，数组每一个下标值对应的位置存储的不再是一个数字了，而是存储由经过取余操作后得到相同余数的数字组成的**数组**或**链表**。

[![image-20200229002550499](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/1.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/1.png)





这样可以根据下标值获取到整个数组或链表，之后继续在数组或链表中查找就可以了。而且，产生冲突的元素一般不会太多。

**总结：**链地址法解决冲突的办法是**每个数组单元**中存储的不再是**单个数据**，而是一条**链条**，这条链条常使用的数据结构为**数组或链表**，两种数据结构查找的效率相当（因为链条的元素一般不会太多）。

- 方案二：**开放地址法**；

开放地址法的主要工作方式是**寻找空白的单元格**来放置**冲突**的数据项。

[![image-20200229083553133](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/2.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/2.png)





根据探测空白单元格位置方式的不同，可分为三种方法：

- **线性探测**
- **二次探测**
- **再哈希法**

#### 1.4.寻找空白单元格的方式

##### 线性探测

**当插入13时**：

- 经过哈希化（对10取余）之后得到的下标值index=3，但是该位置已经放置了数据33。而线性探测就是从**index位置+1**开始向后一个一个来查找**合适的位置**来放置13，所谓合适的位置指的是**空的位置**，如上图中index=4的位置就是合适的位置。

**当查询13时**：

- 首先13经过哈希化得到index=3，如果index=3的位置存放的数据与需要查询的数据13相同，就直接返回；
- 不相同时，则线性查找，从**index+1**位置开始一个一个位置地查找数据13；
- 查询过程中不会遍历整个哈希表，只要查询到**空位置，就停止**，因为插入13时不会跳过空位置去插入其他位置。

**当删除13时**：

- 删除操作和上述两种情况类似，但需要注意的是，删除一个数据项时，**不能**将该位置下标的内容**设置为null**，否则会**影响到之后其他的查询操作**，因为一遇到为null的位置就会停止查找。
- 通常**删除一个位置的数据项**时，我们可以**将它进行特殊处理**（比如设置为-1），这样在查找时遇到-1就知道要**继续查找**。

**线性探测存在的问题**：

- 线性探测存在一个比较严重的问题，就是**聚集**；
- 如哈希表中还没插入任何元素时，插入23、24、25、26、27，这就意味着下标值为3、4、5、6、7的位置都放置了数据，这种**一连串填充单元**就称为**聚集**；
- 聚集会影响哈希表的**性能**，无论是插入/查询/删除都会影响；
- 比如插入13时就会发现，连续的单元3~7都不允许插入数据，并且在插入的过程中需要经历多次这种情况。二次探测法可以解决该问题。

[![image-20200306181129217](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/3.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/3.png)





##### 二次探测

上文所说的**线性探测存在的问题**：

- 如果之前的数据是**连续插入**的，那么新插入的一个数据可能需要**探测很长的距离**；

  二次探测是在线性探测的基础上进行了**优化**：

- **线性探测**：我们可以看成是**步长为1**的探测，比如从下表值x开始，那么线性探测就是按照下标值：x+1、x+2、x+3等依次探测；

- **二次探测**：对步长进行了优化，比如从下标值x开始探测：x+12、x+22、x+33 。这样**一次性探测比较长的距离**，避免了数据聚集带来的影响。

**二次探测存在的问题**：

- 当插入数据分布性较大的一组数据时，比如：13-163-63-3-213，这种情况会造成**步长不一的一种聚集**（虽然这种情况出现的概率较线性探测的聚集要小），同样会影响性能。

##### 再哈希化

在开放地址法中寻找空白单元格的最好的解决方式为**再哈希化**：

- 二次探测的步长是固定的：1，4，9，16依次类推；
- 现在需要一种方法：产生一种**依赖关键字(数据)的探测序列**，而不是每个关键字探测步长都一样；
- 这样，**不同的关键字**即使映射到**相同的数组下标**，也可以使用**不同的探测序列**；
- 再哈希法的做法为：把关键字用**另一个**哈希函数，**再做一次哈希化**，用这次哈希化的**结果作为该关键字的步长**；

**第二次哈希化需要满足以下两点**：

- 和**第一个哈希函数不同**，不然哈希化后的结果仍是原来位置；
- **不能输出为0**，否则每次探测都是原地踏步的死循环；

**优秀的哈希函数**：

- **stepSize = constant - （key % constant）**；
- 其中constant是**质数**，且小于数组的容量；
- 例如：stepSize = 5 - （key % 5），满足需求，并且结果不可能为0；

**哈希化的效率**

哈希表中执行插入和搜索操作效率是非常高的。

- 如果没有**发生冲突**，那么效率就会更高；
- 如果**发生冲突**，存取时间就依赖后来的探测长度；
- 平均探测长度以及平均存取时间，取决于**填装因子**，随着填装因子变大，探测长度会越来越长。

理解概念**装填因子**：

- 装填因子表示当前哈希表中已经**包含的数据项**和**整个哈希表长度**的**比值**；
- **装填因子 = 总数据项 / 哈希表长度**；
- **开放地址法的装填因子**最大为**1**，因为只有空白的单元才能放入元素；
- **链地址法的装填因子**可以**大于1**，因为只要愿意，拉链法可以无限延伸下去；

#### 1.5.不同探测方式性能的比较

- **线性探测：**

可以看到，随着装填因子的增大，平均探测长度呈指数形式增长，性能较差。实际情况中，最好的装填因子取决于存储效率和速度之间的平衡，随着装填因子变小，存储效率下降，而速度上升。

[![image-20200229101035976](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/4.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/4.png)





- **二次探测和再哈希化的性能**：

二次探测和再哈希法性能相当，它们的性能比线性探测略好。由下图可知，随着装填因子的变大，平均探测长度呈指数形式增长，需要探测的次数也呈指数形式增长，性能不高。

[![image-20200229100339679](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/5.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/5.png)





- **链地址法的性能：**

可以看到随着装填因子的增加，平均探测长度呈线性增长，较为平缓。在开发中使用链地址法较多，比如Java中的HashMap中使用的就是**链地址法**。

[![image-20200229100712464](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/6.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/6.png)





#### 1.6.优秀的哈希函数

哈希表的优势在于它的速度，所以哈希函数不能采用消耗性能较高的复杂算法。提高速度的一个方法是在哈希函数中**尽量减少乘法和除法**。

性能高的哈希函数应具备以下两个优点：

- **快速的计算**；
- **均匀的分布**；

##### 快速计算

**霍纳法则**：在中国霍纳法则也叫做**秦久韶算法**，具体算法为：

[![image-20200229105546884](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/7.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/7.png)





求多项式的值时，首先计算最内层括号内一次多项式的值，然后由内向外逐层计算一次多项式的值。这种算法把求n次多项式f(x)的值就转化为求n个一次多项式的值。

**变换之前**：

- 乘法次数：n（n+1）/2次；
- 加法次数：n次；

**变换之后：**

- 乘法次数：n次；
- 加法次数：n次；

如果使用大O表示时间复杂度的话，直接从变换前的**O(N2)**降到了**O(N)**。

##### 均匀分布

为了保证数据在哈希表中**均匀分布**，当我们需要**使用常量的地方**，尽量使用**质数**；比如：哈希表的长度、N次幂的底数等。

Java中的HashMap采用的是链地址法，哈希化采用的是公式为：**index = HashCode（key）&（Length-1）**

即将数据化为二进制进行**与**运算，而不是取余运算。这样计算机直接运算二进制数据，效率更高。但是JavaScript在进行叫大数据的**与**运算时会出现问题，所以以下使用JavaScript实现哈希化时还是采用取余运算。

### 二、初步封装哈希表

**哈希表的常见操作为：**

- put（key，value）：插入或修改操作；
- get（key）：获取哈希表中特定位置的元素；
- remove（key）：删除哈希表中特定位置的元素；
- isEmpty（）：如果哈希表中不包含任何元素，返回trun，如果哈希表长度大于0则返回false；
- size（）：返回哈希表包含的元素个数；
- resize（value）：对哈希表进行扩容操作；

#### 2.1.哈希函数的简单实现

首先使用霍纳法则计算hashCode的值，通过取余操作实现哈希化，此处先简单地指定数组的大小。

```js
    //设计哈希函数
    //1.将字符串转成比较大的数字：hashCede
    //2.将大的数字hasCode压缩到数组范围(大小)之内
    function hashFunc(str, size){
      //1.定义hashCode变量
      let hashCode = 0

      //2.霍纳法则，计算hashCode的值
      //cats -> Unicode编码
      for(let i = 0 ;i < str.length; i++){
        // str.charCodeAt(i)//获取某个字符对应的unicode编码
        hashCode = 37 * hashCode + str.charCodeAt(i)
      }

      //3.取余操作
      let index = hashCode % size
      return index
    }
```

**测试代码：**

```js
    //测试哈希函数
    console.log(hashFunc('123', 7));
    console.log(hashFunc('NBA', 7));
    console.log(hashFunc('CBA', 7));
    console.log(hashFunc('CMF', 7));
```

**测试结果：**

[![image-20200306183709654](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/8.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/8.png)





#### 2.2.创建哈希表

封装哈希表的数组结构模型：

[![image-20200229135924674](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/9.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/9.png)





首先创建哈希表类HashTable，并添加必要的属性和上面实现的哈希函数，再进行其他方法的实现。

```js
    //封装哈希表类
    function HashTable() {
      //属性
      this.storage = []
      this.count = 0//计算已经存储的元素个数
      //装填因子：loadFactor > 0.75时需要扩容；loadFactor < 0.25时需要减少容量
      this.limit = 7//初始长度

      //方法
      //哈希函数
      HashTable.prototype.hashFunc = function(str, size){
      //1.定义hashCode变量
      let hashCode = 0

      //2.霍纳法则，计算hashCode的值
      //cats -> Unicode编码
      for(let i = 0 ;i < str.length; i++){
        // str.charCodeAt(i)//获取某个字符对应的unicode编码
        hashCode = 37 * hashCode + str.charCodeAt(i)
      }

      //3.取余操作
      let index = hashCode % size
      return index
    }
```

#### 2.3.put(key,value)

哈希表的插入和修改操作是同一个函数：因为，当使用者传入一个<key，value>时，如果原来不存在该key，那么就是插入操作，如果原来已经存在该key，那么就是修改操作。

[![image-20200229145513769](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/10.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/10.png)





**实现思路：**

- 首先，根据key获取索引值index，目的为将数据插入到storage的对应位置；
- 然后，根据索引值取出bucket，如果bucket不存在，先创建bucket，随后放置在该索引值的位置；
- 接着，判断新增还是修改原来的值。如果已经有值了，就修改该值；如果没有，就执行后续操作。
- 最后，进行新增数据操作。

**代码实现：**

```js
    //插入&修改操作
    HashTable.prototype.put = function (key, value){
      //1.根据key获取对应的index
      let index = this.hashFunc(key, this.limit)

      //2.根据index取出对应的bucket
      let bucket = this.storage[index]

      //3.判断该bucket是否为null
      if (bucket == null) {
        bucket = []
        this.storage[index] = bucket
      }

      //4.判断是否是修改数据
      for (let i = 0; i < bucket.length; i++) {
        let tuple = bucket[i];
        if (tuple[0] == key) {
          tuple[1] = value
          return//不用返回值
        }
      }

      //5.进行添加操作
      bucket.push([key, value])
      this.count += 1
    }
```

**测试代码：**

```js
    //测试哈希表
    //1.创建哈希表
    let ht = new HashTable()

    //2.插入数据
    ht.put('class1','Tom')
    ht.put('class2','Mary')
    ht.put('class3','Gogo')
    ht.put('class4','Tony')
    ht.put('class4', 'Vibi')
    console.log(ht);
```

**测试结果：**

[![image-20200306195830876](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/11.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/11.png)





#### 2.4.get(key)

**实现思路**：

- 首先，根据key通过哈希函数获取它在storage中对应的索引值index；
- 然后，根据索引值获取对应的bucket；
- 接着，判断获取到的bucket是否为null，如果为null，直接返回null；
- 随后，线性遍历bucket中每一个key是否等于传入的key。如果等于，直接返回对应的value；
- 最后，遍历完bucket后，仍然没有找到对应的key，直接return null即可。

```js
   //获取操作
    HashTable.prototype.get = function(key){
      //1.根据key获取对应的index
      let index = this.hashFunc(key, this.limit)

      //2.根据index获取对应的bucket
      let bucket = this.storage[index]

      //3.判断bucket是否等于null
      if (bucket == null) {
        return null
      }

      //4.有bucket，那么就进行线性查找
      for (let i = 0; i < bucket.length; i++) {
        let tuple = bucket[i];
        if (tuple[0] == key) {//tuple[0]存储key，tuple[1]存储value
          return tuple[1]
        }
      }

      //5.依然没有找到，那么返回null
      return null
    }
```

**测试代码：**

```js
    //测试哈希表
    //1.创建哈希表
    let ht = new HashTable()
    
	//2.插入数据
    ht.put('class1','Tom')
    ht.put('class2','Mary')
    ht.put('class3','Gogo')
    ht.put('class4','Tony')
    
    //3.获取数据
    console.log(ht.get('class3'));
    console.log(ht.get('class2'));
    console.log(ht.get('class1'));
```

**测试结果：**

[![image-20200306195441780](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/12.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/12.png)





#### 2.5.remove(key)

**实现思路**：

- 首先，根据key通过哈希函数获取它在storage中对应的索引值index；
- 然后，根据索引值获取对应的bucket；
- 接着，判断获取到的bucket是否为null，如果为null，直接返回null；
- 随后，线性查找bucket，寻找对应的数据，并且删除；
- 最后，依然没有找到，返回null；

**代码实现：**

```js
   //删除操作
    HashTable.prototype.remove = function(key){
      //1.根据key获取对应的index
      let index = this.hashFunc(key, this.limit)

      //2.根据index获取对应的bucket
      let bucket = this.storage[index]

      //3.判断bucket是否为null
      if (bucket == null) {
        return null
      }

      //4.有bucket,那么就进行线性查找并删除
      for (let i = 0; i < bucket.length; i++) {
        let tuple = bucket[i]
        if (tuple[0] == key) {
          bucket.splice(i,1)
          this.count -= 1 
          return tuple[1]
        }
    }

      //5.依然没有找到，返回null
      return null
    }
```

**测试代码：**

```js
    //测试哈希表
    //1.创建哈希表
    let ht = new HashTable()
    
	//2.插入数据
    ht.put('class1','Tom')
    ht.put('class2','Mary')
    ht.put('class3','Gogo')
    ht.put('class4','Tony')
    
    //3.删除数据
    console.log( ht.remove('class2'));
    console.log(ht.get('class2'));
```

**测试结果：**

[![image-20200306200014491](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/13.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/13.png)





#### 2.6.其他方法的实现

其他方法包括：**isEmpty()、size()**：

**代码实现：**

```js
//判断哈希表是否为null
  HashTable.prototype.isEmpty = function(){
    return this.count == 0
  }

  //获取哈希表中元素的个数
  HashTable.prototype.size = function(){
    return this.count
  }
```

**测试代码：**

```js
    //测试哈希表
    //1.创建哈希表
    let ht = new HashTable()

    //2.插入数据
    ht.put('class1','Tom')
    ht.put('class2','Mary')
    ht.put('class3','Gogo')
    ht.put('class4','Tony')
    
    //3.测试isEmpty()
    console.log(ht.isEmpty());
    //4.测试isEmpty()
    console.log(ht.size());
    console.log(ht);
```

**测试结果：**

[![image-20200306200301853](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/14.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/14.png)





### 三、哈希表的扩容

#### 3.1.扩容与压缩

为什么需要扩容？

- 前面我们在哈希表中使用的是**长度为7的数组**，由于使用的是**链地址法，装填因子(loadFactor)**可以大于1，所以这个哈希表可以无限制地插入新数据。
- 但是，随着**数据量的增多**，storage中每一个index对应的bucket数组（链表）就会越来越长，这就会造成哈希表**效率的降低**

什么情况下需要扩容？

- 常见的情况是**loadFactor > 0.75**的时候进行扩容；

如何进行扩容？

- 简单的扩容可以直接扩大**两倍**（关于质数，之后讨论）；
- 扩容之后**所有的**数据项都要进行**同步修改**；

**实现思路:**

- 首先，定义一个变量，比如oldStorage指向原来的storage；
- 然后，创建一个新的容量更大的数组，让this.storage指向它；
- 最后，将oldStorage中的每一个bucket中的每一个数据取出来依次添加到this.storage指向的新数组中；

[![image-20200229165155853](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/15.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/15.png)





代码实现：

```js
  //哈希表扩容
  HashTable.prototype.resize = function(newLimit){
    //1.保存旧的storage数组内容
    let oldStorage = this.storage

    //2.重置所有的属性
    this.storage = []
    this.count = 0
    this.limit = newLimit

    //3.遍历oldStorage中所有的bucket
    for (let i = 0; i < oldStorage.length; i++) {
      //3.1.取出对应的bucket
      const bucket = oldStorage[i];

      //3.2.判断bucket是否为null
      if (bucket == null) {
        continue
      }      

      //3.3.bucket中有数据，就取出数据重新插入
      for (let j = 0; j < bucket.length; j++) {
        const tuple = bucket[j];
        this.put(tuple[0], tuple[1])//插入数据的key和value
      }
    }
  }
```

上述定义的哈希表的resize方法，既可以实现哈希表的**扩容**，也可以实现哈希表容量的**压缩**。

> **装填因子** = 哈希表中数据 / 哈希表长度，即 loadFactor = count / HashTable.length。

- 通常情况下当**装填因子laodFactor > 0.75**时，对哈希表进行扩容。在哈希表中的添加方法（push方法）中添加如下代码，判断是否需要调用扩容函数进行扩容：

```js
     //判断是否需要扩容操作
      if(this.count > this.limit * 0.75){
        this.resize(this.limit * 2)
      }
```

- 当**装填因子laodFactor < 0.25**时，对哈希表容量进行压缩。在哈希表中的删除方法（remove方法）中添加如下代码，判断是否需要调用扩容函数进行压缩：

```js
    //缩小容量
    if (this.limit > 7 && this.count < this.limit * 0.25) {
      this.resize(Math.floor(this.limit / 2))
    }
```

#### 3.2.选择质数作为容量

**质数的判断**

首先我们来复习一下，判断质数的方法：

> 注意1不是质数

- 方法一：针对质数的特点：只能被1和num整除，不能被2 ~ (num-1)整除。遍历2 ~ (num-1) 。

```js
    function isPrime(num){
      if(num <= 1 ){
        return false
      } 
      for(let i = 2; i <= num - 1; i++){
        if(num % i ==0){
          return false
        }
      }
        return true
    }
```

这种方法虽然能实现质数的判断，但是效率不高。

- 方法二：只需要遍历2 ~ num的平方根即可。

```js
   function isPrime(num){
      if (num <= 1) {
        return false
      }
      //1.获取num的平方根:Math.sqrt(num)
      //2.循环判断
      for(var i = 2; i<= Math.sqrt(num); i++ ){
        if(num % i == 0){
          return false;
        }
      }
        return true;
    }
```

**实现扩容后的哈希表容量为质数**

**实现思路：**

2倍扩容之后，通过循环调用isPrime判断得到的容量是否为质数，不是则+1，直到是为止。比如原长度：7，2倍扩容后长度为14，14不是质数，14 + 1 = 15不是质数，15 + 1 = 16不是质数，16 + 1 = 17是质数，停止循环，由此得到质数17。

**代码实现：**

- **第一步：**首先需要为HashTable类添加判断质数的isPrime方法和获取质数的getPrime方法：

```js
  //判断传入的num是否质数
  HashTable.prototype.isPrime = function(num){
      if (num <= 1) {
        return false
      }
      //1.获取num的平方根:Math.sqrt(num)
      //2.循环判断
      for(var i = 2; i<= Math.sqrt(num); i++ ){
        if(num % i == 0){
          return false;
        }
      }
        return true;
    }

    //获取质数的方法
    HashTable.prototype.getPrime = function(num){
       //7*2=14,+1=15,+1=16,+1=17(质数)
      while (!this.isPrime(num)) {
        num++
      }
      return num
    }
```

- **第二步：**修改添加元素的put方法和删除元素的remove方法中关于数组扩容的相关操作：

在put方法中添加如下代码：

```js
      //判断是否需要扩容操作
      if(this.count > this.limit * 0.75){
        let newSize = this.limit * 2
        let newPrime = this.getPrime(newSize)
        this.resize(newPrime)
      }
```

在remove方法中添加如下代码：

```js
          //缩小容量
          if (this.limit > 7 && this.count < this.limit * 0.25) {
            let newSize = Math.floor(this.limit / 2)
            let newPrime = this.getPrime(newSize)
            this.resize(newPrime)
          }
```

**测试代码：**

```js
    let ht = new HashTable()

    ht.put('class1','Tom')
    ht.put('class2','Mary')
    ht.put('class3','Gogo')
    ht.put('class4','Tony')
    ht.put('class5','5')
    ht.put('class6','6')
    ht.put('class7','7')
    ht.put('class8','8')
    ht.put('class9','9')
    ht.put('class10','10')
    console.log(ht.size());//10
    console.log(ht.limit);//17
```

**测试结果：**

[![image-20200229203540957](https://gitee.com/ahuntsun/BlogImgs/raw/master/%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%B8%8E%E7%AE%97%E6%B3%95/%E5%93%88%E5%B8%8C%E8%A1%A8/16.png)](https://gitee.com/ahuntsun/BlogImgs/raw/master/数据结构与算法/哈希表/16.png)





### 四、哈希表的完整实现

```js
	//封装哈希表类
    function HashTable() {
      //属性
      this.storage = []
      this.count = 0//计算已经存储的元素个数
      //装填因子：loadFactor > 0.75时需要扩容；loadFactor < 0.25时需要减少容量
      this.limit = 7//初始长度

      //方法
      //哈希函数
      HashTable.prototype.hashFunc = function(str, size){
      //1.定义hashCode变量
      let hashCode = 0

      //2.霍纳法则，计算hashCode的值
      //cats -> Unicode编码
      for(let i = 0 ;i < str.length; i++){
        // str.charCodeAt(i)//获取某个字符对应的unicode编码
        hashCode = 37 * hashCode + str.charCodeAt(i)
      }

      //3.取余操作
      let index = hashCode % size
      return index
    }

    //一.插入&修改操作
    HashTable.prototype.put = function (key, value){
      //1.根据key获取对应的index
      let index = this.hashFunc(key, this.limit)

      //2.根据index取出对应的bucket
      let bucket = this.storage[index]

      //3.判断该bucket是否为null
      if (bucket == null) {
        bucket = []
        this.storage[index] = bucket
      }

      //4.判断是否是修改数据
      for (let i = 0; i < bucket.length; i++) {
        let tuple = bucket[i];
        if (tuple[0] == key) {
          tuple[1] = value
          return//不用返回值
        }
      }

      //5.进行添加操作
      bucket.push([key, value])
      this.count += 1

      //6.判断是否需要扩容操作
      if(this.count > this.limit * 0.75){
        let newSize = this.limit * 2
        let newPrime = this.getPrime(newSize)
        this.resize(newPrime)
      }
    }

    //二.获取操作
    HashTable.prototype.get = function(key){
      //1.根据key获取对应的index
      let index = this.hashFunc(key, this.limit)

      //2.根据index获取对应的bucket
      let bucket = this.storage[index]

      //3.判断bucket是否等于null
      if (bucket == null) {
        return null
      }

      //4.有bucket，那么就进行线性查找
      for (let i = 0; i < bucket.length; i++) {
        let tuple = bucket[i];
        if (tuple[0] == key) {//tuple[0]存储key，tuple[1]存储value
          return tuple[1]
        }
      }

      //5.依然没有找到，那么返回null
      return null
    }

    //三.删除操作
    HashTable.prototype.remove = function(key){
      //1.根据key获取对应的index
      let index = this.hashFunc(key, this.limit)

      //2.根据index获取对应的bucket
      let bucket = this.storage[index]

      //3.判断bucket是否为null
      if (bucket == null) {
        return null
      }

      //4.有bucket,那么就进行线性查找并删除
      for (let i = 0; i < bucket.length; i++) {
        let tuple = bucket[i]
        if (tuple[0] == key) {
          bucket.splice(i,1)
          this.count -= 1 
          return tuple[1]

          //6.缩小容量
          if (this.limit > 7 && this.count < this.limit * 0.25) {
            let newSize = Math.floor(this.limit / 2)
            let newPrime = this.getPrime(newSize)
            this.resize(newPrime)
          }
        }
    }

      //5.依然没有找到，返回null
      return null
    }

  /*------------------其他方法--------------------*/
  //判断哈希表是否为null
  HashTable.prototype.isEmpty = function(){
    return this.count == 0
  }

  //获取哈希表中元素的个数
  HashTable.prototype.size = function(){
    return this.count
  }


  //哈希表扩容
  HashTable.prototype.resize = function(newLimit){
    //1.保存旧的storage数组内容
    let oldStorage = this.storage

    //2.重置所有的属性
    this.storage = []
    this.count = 0
    this.limit = newLimit

    //3.遍历oldStorage中所有的bucket
    for (let i = 0; i < oldStorage.length; i++) {
      //3.1.取出对应的bucket
      const bucket = oldStorage[i];

      //3.2.判断bucket是否为null
      if (bucket == null) {
        continue
      }      

      //3.3.bucket中有数据，就取出数据重新插入
      for (let j = 0; j < bucket.length; j++) {
        const tuple = bucket[j];
        this.put(tuple[0], tuple[1])//插入数据的key和value
      }
    }
  }

  //判断传入的num是否质数
  HashTable.prototype.isPrime = function(num){
      if (num <= 1) {
        return false
      }
      //1.获取num的平方根:Math.sqrt(num)
      //2.循环判断
      for(var i = 2; i<= Math.sqrt(num); i++ ){
        if(num % i == 0){
          return false;
        }
      }
        return true;
    }

    //获取质数的方法
    HashTable.prototype.getPrime = function(num){
       //7*2=14,+1=15,+1=16,+1=17(质数)
      while (!this.isPrime(num)) {
        num++
      }
      return num
    }
 }
```

> 参考资料:[JavaScript数据结构与算法](https://www.bilibili.com/video/BV1x7411L7Q7?from=search&seid=3912456004602554239)