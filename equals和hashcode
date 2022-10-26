
# equals和hashcode

 
<font size=3><br>
#### 为什么重写equals( )方法和hashCode( )方法要同时重写

##### 默认的equals( )方法
默认的equals( )方法继承于Object类，用于判断两个对象的地址是否相同<br><br>
误区：<br><br>
仅通过默认的equals( )方法比较两个对象就判断两对象相同是片面的，需要按照实现方法的不同进行重写<br><br>

##### .java.lang.String的equals( )方法源码
<font size=5>String类的equals( )方法实现，不是Object类中默认的equals( )方法！</font><br><br>
<font size=4>[ 时间复杂度为O(n) ]</font>
```
public boolean equals(Object anObject) {  
        if (this == anObject) {           //判断两个对象的存储地址是否相同，如果是直接返回true
            return true;
        }       
        if (anObject instanceof String) {       //判断anObject是否为String的一个实例，如果不是直接返回false
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {  //判断两个字符串长度是否相等，如果不是则这两个对象不同
                char v1[] = value;                  // 将对应字符串转化为char数组进行一一比较
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {                    
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
```
###### 算法流程
1. 使用==运算符比较两个对象的内存地址，如一致则两个对象为同一对象<br><br>
2. 当1.中所比较的两个对象内存地址不同时，继续比较这两个对象中的anObject对象(用于和原对象进行比较的对象)是否为String类的一个实例，<br><br>也就是判断anObject是否为一个String对象，由于.java.lang.String中的equals( )方法只允许用来比较两个对象是否为同一String对象，<br><br>当anObject对象不为String对象时则无法调用该方法，所以判断anObject对象是否为String对象这一步必不可少<br><br>
3. 当满足2.中的条件时，再比较两个String对象中的字符串长度是否相同，如果不相同则没必要进一步比较下去<br><br>
4. 当3.中比较出两个String对象中的字符串长度相同时，再把两个字符串转换为两个char数组，<br><br>再把两个char数组中的元素分别进行一一比较，直到比较出有不同或者完全相同为止

---

##### hashCode( )方法：
###### 基本概念
默认hashCode( )方法可以用来返回对象的哈希值，存于Object类中，所以hashCode( )方法为native声明的本地方法，<br><br>
因为java中任何对象都会有一个native的hashCode( )方法，所以java中的任何一个对象都会有一个默认的哈希值<br><br>
在hashCode( )方法不进行重写的时候，它会将内存地址转换为int数值进行返回，值即为该对象的哈希值<br><br>

###### 默认的hashCode( )方法应用
<font size=4>[ 时间复杂度为O(1) ]</font><br><br>
hashCode( )方法可以在散列集合诸如HashTable与HashMap中使用：当散列集合添加元素的并判断该元素是否已经存在<br><br>
比如在HashMap中，如果待加入的元素是一个对象并且想要判断该元素是否存在于map中，<br><br>
使用equals( )方法则需要将待加入的元素与已有元素进行一一比较，效率太低<br><br>
此时系统可以通过默认hashCode( )方法获取该对象的哈希值，<br><br>
然后使用该哈希值与HashMap中的map长度进行取余运算并查看取余后的存放位置是否已经存在有元素，<br><br>
如果该位置为空则直接存入，不再需要进行任何的比较，不为空则进行进一步的比较<br><br>
取余后的值即为该元素在散列表中存放的位置<br><br>

###### hashCode值重复问题
hashCode的值默认由JVM使用随机数生成的，即便是两个不同的对象，也可能会出现hashCode值相同的情况，即哈希冲突<br><br>但两个完全相同的对象，它们的内存地址指向相同，则它们的hashCode值一定是相同的<br><br>
所以建立在这个条件下：当map或者table中，已经存在了与待存入元素相同的hashCode值或者防止哈希冲突带来不可预料的异常时，<br><br>则会调用equals( )方法将对象进行比较，<br><br>
即比较两个对象是否完全相同，当这两个对象并不是完全相同时，即便它们的hashCode值相同，后来的元素也会被散列到其他的地址中

---
#### 关于两个方法为什么要同时重写
###### 基本概念 
1. 完全相同的两个对象，内存地址指向是相同的，所以他们的hashCode值，也就是哈希值，是一定相同的<br><br>
2. 两个对象即便是hashCode值相同，在不经过equals( )方法比较的前提下，也无法确定它们两个是否是相同的<br><br>



###### 对象存入map或者table的基础逻辑流程：
```mermaid
graph TD;
A["对象(有对应的哈希值)"]--> E["系统调用默认hashCode()方法获取该对象的哈希值"]
E --对其进行取余运算并待存入map或table中--> B["系统判断map或table中该位置是否已经存在元素"]
B --不存在--> C["存入"]
B --存在--> D["发生哈希冲突"]
D --> F["系统调用equals()方法判断两对象是否完全相同"]
F --相同--> G["存入"]
F --不相同--> H["使用相关的解决方案(如链表，线性探测)"]
```

###### 实际使用时的逻辑
<font size=5>上述流程的equals( )方法为String类中的实现方法，hashCode( )方法为Object类中的默认方法，<br><br>
是在非重写的前提下！</font><br><br>
1. 当我们只重写了hashCode( )方法时，对象默认的hashCode值经由这个方法后发生了改变，如果不重写equals( )方法，仅比较这两个对象的地址是否相同<br><br>即便这两个对象一样，但由于equals( )方法中比较出的地址不同，系统还是会将这两个对象当作是不同的对象来判断，可能会引起内存泄漏或越界等问题<br><br>
2. 当我们只重写equals( )方法时，不管这个重写的equals( )是基于什么标准实现的，如果保留原来的hashCode( )方法不进行重写，<br><br>就有可能会出现这两个对象经由重写后的equals( )方法判断为相等，但是hashCode却不同，这就出现了代码逻辑上的错误

###### 混淆点
1. 系统自动调用的equals( )方法和hashCode( )的方法是Object类中的方法，是有固定规范和实现的<br><br>
2. 代码编写时重写的equals( )方法和hashCode( )的方法是我们自己定义的方法，即便方法名相同，返回的数值类型相同，<br><br>但内部逻辑已经被我们给重写，为了保证数据存储时不会出现难以预测的异常，<br><br>这两个方法只要有一个重写了，另一个都必须跟着重写<br><br>

<a href="https://www.bilibili.com/video/BV1NW4y1J7So/?spm_id_from=333.1007.tianma.3-3-9.click&vd_source=3305ed7407299e8eba839b981fe89d7f">参考视频1</a><br><br>
<a href="https://www.bilibili.com/video/BV1Y44y1P7d5/?spm_id_from=..search-card.all.click&vd_source=3305ed7407299e8eba839b981fe89d7f">参考视频2</a><br><br>
<a href="https://www.bilibili.com/video/BV1u44y1K7GA/?spm_id_from=333.788.recommend_more_video.0&vd_source=3305ed7407299e8eba839b981fe89d7f">参考视频3</a><br><br>
<a href="https://www.cnblogs.com/yaobolove/p/5086510.html">参考文章1</a><br><br>
<a href="https://blog.csdn.net/qq_41864967/article/details/103095959">参考文章2</a>



------------------------------------------------------------
Generated with [Mybase Desktop 8.2 Beta-10](http://www.wjjsoft.com/mybase.html?ref=markdown_export)
