# Java 进阶

???+ warning "注意"
    以下内容均基于 Java JDK 8 版本编写，不排除在更高版本中有部分改动的可能性。

## 排序

关于 Java 中 sort 函数的具体使用方法会在 `Arrays` 部分与 `Collections` 部分给出详细内容，该部分主要是对 `Arrays.sort(int[])` 与 `Arrays.sort(Integer[])` 的探讨。

在 Java 中，`Arrays.sort(int[])` 底层是双端快排，`Arrays.sort(Integer[])` 底层是归并排序。因此 `Arrays.sort(int[])` 的最坏时间复杂度是 $O(n^2)$，可以通过如下例题来验证。

???+note "[Codeforces 1646B - Quality vs Quantity](https://codeforces.com/problemset/problem/1646/B)"
    题意概要：有 $n$ 个数，你需要将其分为 2 组，是否能存在 1 组的长度小于另 1 组的同时和大于它。

??? note "例题代码"
```java

    import java.io.BufferedReader;
    import java.io.IOException;
    import java.io.InputStreamReader;
    import java.io.PrintWriter;
    import java.util.Arrays;
    import java.util.StringTokenizer;
    
    public class Main {
        static class FastReader {
            StringTokenizer st;
            BufferedReader br;
    
            public FastReader() {
                br = new BufferedReader(new InputStreamReader(System.in));
            }
    
            String next() {
                while (st == null || !st.hasMoreElements()) {
                    try {
                        st = new StringTokenizer(br.readLine());
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
                return st.nextToken();
            }
    
            int nextInt() {
                return Integer.parseInt(next());
            }
    
            long nextLong() {
                return Long.parseLong(next());
            }
    
            double nextDouble() {
                return Double.parseDouble(next());
            }
    
            String nextLine() {
                String str = "";
                try {
                    str = br.readLine();
                } catch (IOException e) {
                    e.printStackTrace();
                }
                return str;
            }
        }
    
        static PrintWriter out = new PrintWriter(System.out);
        static FastReader in = new FastReader();
    
        static void solve() {
            int n = in.nextInt();
            Integer a[] = new Integer[n + 10];
            for (int i = 1; i <= n; i++) {
                a[i] = in.nextInt();
            }
            Arrays.sort(a, 1, n + 1);
            long left = a[1];
            long right = 0;
            int x = n;
            for (int i = 2; i < x; i++, x--) {
                left = left + a[i];
                right = right + a[x];
                if (right > left) {
                    out.println("YES");
                    return;
                }
            }
            out.println("NO");
        }
    
        public static void main(String[] args) {
            int t = in.nextInt();
            while (t-- > 0) {
                solve();
                out.flush();
            }
        }
    }
 ```

如果你将以上代码的 a 数组类型由 `Integer` 修改为 `int` 则会 TLE。

## BigInteger 与数论

`BigInteger` 是 Java 提供的高精度计算类，可以很方便地解决高精度问题。

### 初始化

BigInteger 常用创建方式有如下二种：

```java
import java.io.PrintWriter;
import java.math.BigInteger;

class Main {
    static PrintWriter out = new PrintWriter(System.out);
    public static void main(String[] args) {
        BigInteger a = new BigInteger("12345678910");//将字符串以10进制的形式创建BigInteger对象
        out.println(a);//a的值为 12345678910
        BigInteger b = new BigInteger("1E", 16);//将字符串以指定进制的形式创建BigInteger对象
        out.println(b);//c的值为 30
        out.flush();
    }
}

```

### 基本运算

以下均用 this 代替当前 BigIntger :

|            函数名            |             功能             |    
| :-----------------------: | :------------------------: | 
|           abs()           |        返回 this 的绝对值        |    
|          negate()         |          返回 - this         |    
|    add(BigInteger val)    |       返回 this `+` val      |   
|  subtract(BigInteger val) |       返回 this `-` val      |    
|  multiply(BigInteger val) |       返回 this `*` val      |    
|   divide(BigInteger val)  |       返回 this `/` val      |    
| remainder(BigInteger val) |       返回 this `%` val      |    
|    mod(BigInteger val)    |   返回 this mod val 总是返回非负数  |
|         pow(int e)        |         返回 $this^e$        |
|    and(BigInteger val)    |       返回 this `&` val      |  
|     or(BigInteger val)    |         返回 this `\|` val |      
|           not()           |         返回 `~` this        |   
|    xor(BigInteger val)    |       返回 this `^` val      |    
|      shiftLeft(int n)     |       返回 this `<<` n       |    
|     shiftRight(int n)     |       返回 this `>>` n       |   
|    max(BigInteger val)    |     返回 this 与 val 的较大值     |   
|    min(BigInteger val)    |     返回 this 与 val 的较小值     |    
|         bitCount()        | 返回 this 的二进制中不包括符号位的 1 的个数 |    
|        bitLength()        |   返回 this 的二进制中不包括符号位的长度   |    
|     getLowestSetBit()     |     返回 this 的二进制中最右边的位置    |   
| compareTo(BigInteger val) |      比较 this 和 val 值大小     |    
|         toString()        |   返回 this 的 10 进制字符串表示形式   |   
|    toString(int radix)    |  返回 this 的 raidx 进制字符串表示形式 |    

使用案例如下：

```java

import java.io.PrintWriter;
import java.math.BigInteger;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BigInteger a, b;

    static void abs() {
        out.println("abs:");
        a = new BigInteger("-123");
        out.println(a.abs());//输出 123
        a = new BigInteger("123");
        out.println(a.abs());//输出 123
    }

    static void negate() {
        out.println("negate:");
        a = new BigInteger("-123");
        out.println(a.negate());//输出 123
        a = new BigInteger("123");
        out.println(a.negate());//输出 -123
    }

    static void add() {
        out.println("add:");
        a = new BigInteger("123");
        b = new BigInteger("123");
        out.println(a.add(b));//输出 246
    }

    static void subtract() {
        out.println("subtract:");
        a = new BigInteger("123");
        b = new BigInteger("123");
        out.println(a.subtract(b));//输出 0
    }

    static void multiply() {
        out.println("multiply:");
        a = new BigInteger("12");
        b = new BigInteger("12");
        out.println(a.multiply(b));//输出 144
    }

    static void divide() {
        out.println("divide:");
        a = new BigInteger("12");
        b = new BigInteger("11");
        out.println(a.divide(b));//输出 1
    }

    static void remainder() {
        out.println("remainder:");
        a = new BigInteger("12");
        b = new BigInteger("10");
        out.println(a.remainder(b));//输出 2
        a = new BigInteger("-12");
        b = new BigInteger("10");
        out.println(a.remainder(b));//输出 -2
    }

    static void mod() {
        out.println("mod:");
        a = new BigInteger("12");
        b = new BigInteger("10");
        out.println(a.mod(b));//输出 2
        a = new BigInteger("-12");
        b = new BigInteger("10");
        out.println(a.mod(b));//输出 8
    }

    static void pow() {
        out.println("pow:");
        a = new BigInteger("2");
        out.println(a.pow(10));//输出1024
    }

    static void and() {
        out.println("and:");
        a = new BigInteger("3"); // 11
        b = new BigInteger("5"); //101
        out.println(a.and(b));//输出1
    }

    static void or() {
        out.println("or:");
        a = new BigInteger("2"); // 10
        b = new BigInteger("5"); //101
        out.println(a.or(b));//输出7
    }

    static void not() {
        out.println("not:");
        a = new BigInteger("2147483647"); // 01111111 11111111 11111111 11111111
        out.println(a.not());//输出-2147483648 二进制为：10000000 00000000 00000000 00000000
    }

    static void xor() {
        out.println("xor:");
        a = new BigInteger("6");//110
        b = new BigInteger("5");//101
        out.println(a.xor(b));//011 输出3
    }

    static void shiftLeft() {
        out.println("shiftLeft:");
        a = new BigInteger("1");
        out.println(a.shiftLeft(10));// 输出1024
    }

    static void shiftRight() {
        out.println("shiftRight:");
        a = new BigInteger("1024");
        out.println(a.shiftRight(8));//输出4
    }

    static void max() {
        out.println("max:");
        a = new BigInteger("6");
        b = new BigInteger("5");
        out.println(a.max(b));//输出6
    }

    static void min() {
        out.println("min:");
        a = new BigInteger("6");
        b = new BigInteger("5");
        out.println(a.min(b));//输出5
    }

    static void bitCount() {
        out.println("bitCount:");
        a = new BigInteger("6");//110
        out.println(a.bitCount());//输出2
    }

    static void bitLength() {
        out.println("bitLength:");
        a = new BigInteger("6");//110
        out.println(a.bitLength());//输出3
    }

    static void getLowestSetBit() {
        out.println("getLowestSetBit:");
        a = new BigInteger("8");//1000
        out.println(a.getLowestSetBit());//输出3
    }

    static void compareTo() {
        out.println("compareTo:");
        a = new BigInteger("8");
        b = new BigInteger("9");
        out.println(a.compareTo(b)); //输出 -1
        a = new BigInteger("8");
        b = new BigInteger("8");
        out.println(a.compareTo(b)); //输出 0
        a = new BigInteger("8");
        b = new BigInteger("7");
        out.println(a.compareTo(b)); //输出 1
    }

    static void toStringTest() {
        out.println("toString:");
        a = new BigInteger("15");
        out.println(a.toString());//输出15
        out.println(a.toString(16));//输出f
    }

    public static void main(String[] args) {
        abs();
        negate();
        add();
        subtract();
        multiply();
        divide();
        remainder();
        mod();
        pow();
        and();
        or();
        not();
        xor();
        shiftLeft();
        shiftRight();
        max();
        min();
        bitCount();
        bitLength();
        getLowestSetBit();
        compareTo();
        toStringTest();
        out.flush();
    }
}

```

### 数学运算

以下均用 this 代替当前 BigIntger :

|            函数名            |             功能             |    
| :-----------------------: | :------------------------: | 
|  gcd(BigInteger val)      |       返回this的绝对值与val的绝对值的最大公约数  |    
|  isProbablePrime(int val) |       返回一个布尔值,判定this是否是素数         |    
|  nextProbablePrime()    |         返回大于this的第一个素数    |   
|  modPow(BigInteger b,BigInteger p) |  返回this `^` b `mod` p     |    
|  modInverse(BigInteger p) |       返回a `mod` p的乘法逆元      |     


使用案例如下：

```java

import java.io.PrintWriter;
import java.math.BigInteger;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static BigInteger a, b, p;

    static void gcd() {//最大公约数
        a = new BigInteger("120032414321432144212100");
        b = new BigInteger("240231431243123412432140");
        out.println(String.format("gcd(%s,%s)=%s", a.toString(), b.toString(), a.gcd(b).toString()));//gcd(120032414321432144212100,240231431243123412432140)=20
    }

    static void isPrime() {//判定该数是否是素数 基于米勒罗宾,参数越大准确性越高，复杂度越高。准确性为(1-1/(val*2))
        a = new BigInteger("1200324143214321442127");
        out.println("a:" + a.toString());
        out.println(a.isProbablePrime(10) ? "a is prime" : "a is not prime");//a is not prime
    }

    static void nextPrime() {//找出该数的下一个素数
        a = new BigInteger("1200324143214321442127");
        out.println("a:" + a.toString());
        out.println(String.format("a nextPrime is %s", a.nextProbablePrime().toString()));//a nextPrime is 1200324143214321442199
    }

    static void modPow() {//快速幂,比正常版本要快，内部有蒙特卡洛优化
        a = new BigInteger("2");
        b = new BigInteger("10");
        p = new BigInteger("1000");
        out.println(String.format("a:%s b:%s p:%s", a, b, p));
        out.println(String.format("a^b mod p:%s", a.modPow(b, p).toString()));//24
    }

    static void modInverse() {//逆元
        a = new BigInteger("10");
        b = new BigInteger("3");
        out.println(a.modInverse(b));//a ^ (p-2) mod p = 1
    }

    public static void main(String[] args) {
        gcd();
        isPrime();
        nextPrime();
        modPow();
        modInverse();
        out.flush();
    }
}

```

## 数据结构

以下内容用法均基于 Java 里多态的性质，均是以实现接口的形式出现。

### List 线性表
####  ArrayList
ArrayList是支持可以根据需求动态生长的数组，初始长度默认为10，如果超出当前长度便扩容$\frac{3}{2}$。
 
##### 初始化
```java

import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);

    public static void main(String[] args) {
        List<Integer> list1=new ArrayList<>();//创建一个名字为list1的可自增数组，初始长度为默认10
        List<Integer> list2=new ArrayList<>(30);//创建一个名字为list2的可自增数组,初始长度为30
        List<Integer> list3=new ArrayList<>(list2);//创建一个名字为list3的可自增数组,使用list2里的元素和size作为自己的初始值
    }
}

```
####  LinkedList
LinkedList是双链表
##### 初始化
```java
import java.io.PrintWriter;
import java.util.LinkedList;
import java.util.List;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);

    public static void main(String[] args) {
        List<Integer> list1 = new LinkedList<>();//创建一个名字为list1的双链表
        List<Integer> list2 = new LinkedList<>(list1);//创建一个名字为list2的双链表，将list1内所有元素加入进来
    }
}

```
#### List常用方法
以下均用 this 代替当前 `List<Integer>` :

|            函数名            |             功能             |    
| :-----------------------: | :------------------------: | 
|          size()           |          返回 this 的长度        |    
|          add(Integer val) |          在this尾部插入一个元素       |    
|    add(int idx,Integer e)    |       在this指定位置插入一个元素     |   
|  get(int idx)         |       返回this中第idx位置的值，若越界则抛出异常      |    
|  set(int idx,Integer e) |     修改this中第idx位置的值    |    
    
使用案例及区别对比：
```java
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static List<Integer> array=new ArrayList<>();
    static List<Integer> linked=new LinkedList<>();
    static void add(){
        array.add(1);//时间复杂度为o(1)
        linked.add(1);//时间复杂度为o(1)
    }
    static void get(){
        array.get(10);//时间复杂度为o(1)
        linked.get(10);//时间复杂度为o(11)
    }
    static void addIdx(){
        array.add(0,2);//最坏情况下时间复杂度为o(n)
        linked.add(0,2);//最坏情况下时间复杂度为o(n)
    }
    static void size(){
        array.size();//时间复杂度为o(1)
        linked.size();//时间复杂度为o(1)
    }
    static void set(){//该方法返回值为原本该位置元素的值
        array.set(0,1);//时间复杂度为o(1)
        linked.set(0,1);//最坏时间复杂度为o(n)
    }
  
}
```
#### 遍历List的三种方法
```java
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.LinkedList;
import java.util.List;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static List<Integer> array = new ArrayList<>();
    static List<Integer> linked = new LinkedList<>();

    static void function1() {//朴素遍历
        for (int i = 0; i < array.size(); i++) out.println(array.get(i));//遍历自增数组，复杂度为O(n)
        for (int i = 0; i < linked.size(); i++)
            out.println(linked.get(i));//遍历双链表，复杂度为O(n^2)，因为LinkedList的get(i)复杂度是O(i)
    }

    static void function2() {//增强for循环遍历
        for (int e : array) out.println(e);
        for (int e : linked) out.println(e);//复杂度均为o(n)
    }

    static void function3() {//迭代器遍历
        Iterator<Integer> iterator1 = array.iterator();
        Iterator<Integer> iterator2 = linked.iterator();
        while (iterator1.hasNext()) {
            out.println(iterator1.next());
        }
        while (iterator2.hasNext()) {
            out.println(iterator2.next());
        }//复杂度均为o(n)
    }

}

```

### Queue 队列
####  LinkedList
可以使用LinkedList实现普通队列。
##### 初始化：
```java
Queue<Integer> q=new LinkedList<>();
```
####  PriorityQueue
PriorityQueue是优先队列，默认是小根堆。
##### 初始化：
```java
Queue<Integer> q1=new PriorityQueue<>();//小根堆
Queue<Integer> q2=new PriorityQueue<>((x,y)->{return y-x;});//大根堆
```
#### Queue常用方法
以下均用 this 代替当前 `Queue<Integer>` :

|            函数名            |             功能             |    
| :-----------------------: | :------------------------: | 
|          size()           |          返回 this 的长度        |    
|          add(Integer val) |          入队       |    
|          offer(Integer val) |          入队       |    
|       isEmpty()    |       判断队列是否为空，为空则返回true     |   
|   peek()         |       返回队头元素      |    
| poll() |   返回队头元素并删除   |    

使用案例及区别对比：
```java
import java.io.PrintWriter;
import java.util.LinkedList;
import java.util.PriorityQueue;
import java.util.Queue;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static Queue<Integer> q1 = new LinkedList<>();
    static Queue<Integer> q2 = new PriorityQueue<>();

    static void add() {//add和offer功能上没有差距，区别是是否会抛出异常
        q1.add(1);//时间复杂度为o(1)
        q2.add(1);//时间复杂度为o(logn)
    }

    static void isEmpty() {
        q1.isEmpty();//时间复杂度为o(1)
        q2.isEmpty();//空间复杂度为o(1)
    }

    static void size() {
        q1.size();//时间复杂度为o(1)
        q2.size();//返回q2的长度
    }

    static void peek() {
        q1.peek();//时间复杂度为o(1)
        q2.peek();//时间复杂度为o(logn)
    }

    static void poll() {
        q1.poll();//时间复杂度为o(1)
        q2.poll();//时间复杂度为o(logn)
    }
}
```
#### 遍历Queue
```java
import java.io.PrintWriter;
import java.util.LinkedList;
import java.util.PriorityQueue;
import java.util.Queue;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static Queue<Integer> q1 = new LinkedList<>();
    static Queue<Integer> q2 = new PriorityQueue<>();

    static void test() {
        while (!q1.isEmpty()) {//复杂度为o(n)
            out.println(q1.poll());
        }
        while (!q2.isEmpty()) {//复杂度为o(nlogn)
            out.println(q2.poll());
        }
    }

}
```
### Set 集合
set是保持容器中的元素不重复的一种数据结构。
####  HashSet
随机插入的Set。
##### 初始化
```java
Set<Integer> s1 = new HashSet<>();
```
####  LinkedHashSet
保持插入顺序的Set。
##### 初始化
```java
Set<Integer> s2 = new LinkedHashSet<>();
```
####  TreeSet
保持容器中元素有序的Set，默认为升序。
##### 初始化
```java
Set<Integer> s3 = new TreeSet<>();
Set<Integer> s4=new TreeSet<>((x,y)->{return y-x;});//降序
```
#### Set常用方法
以下均用 this 代替当前 `Set<Integer>` :
|            函数名            |             功能             |    
| :-----------------------: | :------------------------: | 
|          size()           |          返回 this 的长度        |    
|          add(Integer val) |          插入一个元素进this      |
|          contains(Integer val) |          判断this中是否有元素val       |
| addAll(Collection e) | 将一个容器里的所有元素添加进this|
| retainAll(Collection e) | 将this改为两个容器内相同的元素|
| removeAll(Collection e) | 将this中与e相同的元素删除 |

使用案例：求并集、交集、差集。
```java
import java.io.PrintWriter;
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static Set<Integer> s1 = new HashSet<>();
    static Set<Integer> s2 = new LinkedHashSet<>();

    static void add() {
        s1.add(1);
    }

    static void contains() {//判断set中是否有元素值为2，有则返回true，否则返回false
        s1.contains(2);
    }

    static void test1() {//s1与s2的并集
        Set<Integer> res = new HashSet<>();
        res.addAll(s1);
        res.addAll(s2);
    }

    static void test2() {//s1与s2的交集
        Set<Integer> res = new HashSet<>();
        res.addAll(s1);
        res.retainAll(s2);
    }

    static void test3() {//差集：s1-s2
        Set<Integer> res = new HashSet<>();
        res.addAll(s1);
        res.removeAll(s2);
    }
}
```
#### 遍历Set
```java
import java.io.PrintWriter;
import java.util.HashSet;
import java.util.LinkedHashSet;
import java.util.Set;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);
    static Set<Integer> s1 = new HashSet<>();
    static Set<Integer> s2 = new LinkedHashSet<>();

    static void test() {
        for (int key : s1) {
            out.println(key);
        }
    }
}
```
### Map 哈希表
Map是维护键值对`<Key,Value>`的一种数据结构，其中Key唯一。
####  HashMap
随机位置插入的Map。
##### 初始化
```java
Map<Integer, Integer> map1 = new HashMap<>();
```
####  LinkedHashMap
保持插入顺序的Map。
##### 初始化
```java
Map<Integer, Integer> map2 = new LinkedHashMap<>();
```
####  TreeMap
保持key有序的Map，默认升序
##### 初始化
```java
Map<Integer, Integer> map3 = new TreeMap<>();
Map<Integer, Integer> map4 = new TreeMap<>((x,y)->{return y-x;});//降序
```
#### Map常用方法
以下均用 this 代替当前 `Map<Integer,Integer>` :
|            函数名            |             功能             |    
| :-----------------------: | :------------------------: | 
|          put(Integer key,Integer value)           |          插入一个元素进this       |    
|          size() |      返回this的长度      |
|          containsKey(Integer val) |          判断this中是否有元素key为val       |
| get(Integer key) | 将this中对应的key的value返回|
| keySet | 将this中所有元素的key作为集合返回 |

使用案例:
```java
import java.io.PrintWriter;
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.Map;
import java.util.TreeMap;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);

    static Map<Integer, Integer> map1 = new HashMap<>();
    static Map<Integer, Integer> map2 = new LinkedHashMap<>();
    static Map<Integer, Integer> map3 = new TreeMap<>();
    static Map<Integer, Integer> map4 = new TreeMap<>((x,y)->{return y-x;});

    static void put(){//将key为1，value为1的元素返回
        map1.put(1,1);
    }
    static void get(){//将key为1的value返回
        map1.get(1);
    }
    static void containsKey(){//判断是否有key为1的键值对
        map1.containsKey(1);
    }
    static void KeySet(){
        map1.keySet();
    }
}
```
#### 遍历Map
```java
import java.io.PrintWriter;
import java.util.HashMap;
import java.util.Map;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);

    static Map<Integer, Integer> map1 = new HashMap<>();

    static void print() {
        for (int key : map1.keySet()) {
            out.println(key + " " + map1.get(key));
        }
    }
}
```
## Arrays
Arrays是java.util中对数组操作的一个工具类。方法均为静态方法，可使用类名直接调用。

常用函数：
### Arrays.sort()
Arrays.sort()是对数组进行的排序的方法，主要重载方法如下：
```java
import java.util.Arrays;
import java.util.Comparator;

public class Main {
    static int a[] = new int[10];
    static Integer b[] = new Integer[10];
    static int firstIdx, lastIdx;

    public static void main(String[] args) {
        Arrays.sort(a);\\1
        Arrays.sort(a, firstIdx, lastIdx);\\2
        Arrays.sort(b, new Comparator<Integer>() {\\3
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        Arrays.sort(b, firstIdx, lastIdx, new Comparator<Integer>() {\\4
            @Override
            public int compare(Integer o1, Integer o2) {
                return o2 - o1;
            }
        });
        //由于java8后有lambda表达式，第三个重载及第四个重载亦可写为
        Arrays.sort(b, (x, y) -> {\\5
            return y - x;
        });
        Arrays.sort(b, (x, y) -> {\\6
            return y - x;
        });
    }
}
```
__序号所对应的重载方法含义：__
1. 对数组a进行排序，默认升序。
2. 对数组a的指定位置进行排序，默认升序，排序区间为左开右闭`[firstIdx,lastIdx)` 。
3. 对数组a以自定义的形式排序，第二个参数`-`第一个参数为降序,第一个参数`-`第二个参数为升序，当自定义排序比较器时，数组元素类型必须为对象类型。
4. 对数组a的指定位置进行自定义排序，排序区间为左开右闭`[firstIdx,lastIdx)`，当自定义排序比较器时，数组元素类型必须为对象类型。
5. 和3同理，用lambda表达式优化了代码长度。
6. 和4同理，用lambda表达式优化了代码长度。

__Arrays.sort()底层函数：__
1. 当你`Arrays.sort`的参数数组元素类型为基本数据类型时(byte、short、char、int、long、double、float)时，默认为`DualPivotQuicksort`(双轴快排),复杂度最坏可以达到$O(n^2)$。
2. 当你`Arrays.sort`的参数数组元素类型为非基本数据类型时），则默认为`legacyMergeSort`和`TimSort`(归并排序）,复杂度为$O(nlog_n)$。

### Arrays.binarySearch()
Arrays.binarySearch()是对数组连续区间进行二分搜索的方法，前提是数组必须有序，时间复杂度为$log_n$，主要重载方法如下：
```java
import java.util.Arrays;

public class Main {
    static int a[] = new int[10];
    static Integer b[] = new Integer[10];
    static int firstIdx, lastIdx;
    static int key;

    public static void main(String[] args) {
        Arrays.binarySearch(a, key);\\1
        Arrays.binarySearch(a, firstIdx, lastIdx, key);\\2
    }
}
```
源码如下：
```java
 private static int binarySearch0(int[] a, int fromIndex, int toIndex, int key) {
        int low = fromIndex;
        int high = toIndex - 1;

        while (low <= high) {
            int mid = (low + high) >>> 1;
            int midVal = a[mid];

            if (midVal < key)
                low = mid + 1;
            else if (midVal > key)
                high = mid - 1;
            else
                return mid; // key found
        }
        return -(low + 1);  // key not found.
    }
```
__序号所对应的重载方法含义：__
1. 从数组a中二分查找是否存在key,如果存在，便返回其下标。若不存在，则返回一个负数。
2. 从数组a中二分查找是否存在key,如果存在，便返回其下标,搜索区间为左开右闭`[firstIdx,lastIdx)`。若不存在，则返回一个负数。

### Arrays.fill()
Arrays.fill()是将数组中连续位置的元素赋值为统一元素
```java
Arrays.fill(int[],int val);
```
## Collections
Collections是java.util中对集合操作的一个工具类。方法均为静态方法，可使用类名直接调用。

常用函数：
### Collections.sort()
Collections.sort()底层原理为将其中所有元素转化为数组调用Arrays.sort(),排完序后再赋值给原本的集合。又因为java中Collection类型均为对象类型，所以始终是归并排序去处理。
    
该方法无法对集合指定区间排序。

底层源码：
```java
  default void sort(Comparator<? super E> c) {
        Object[] a = this.toArray();
        Arrays.sort(a, (Comparator) c);
        ListIterator<E> i = this.listIterator();
        for (Object e : a) {
            i.next();
            i.set((E) e);
        }
    }
```
### Collections.binarySearch()
`ColCollections.binarySearch()`是对集合中指定区间进行二分搜索,功能与`Arrays.binarySearch()`相同。
```java
Collections.binarySearch(list,key);
```
该方法无法无法对指定区间进行搜索。
### Collections.swap()
`Collections.swap()`功能是交换集合中指定二个位置的元素
```java
 Collections.swap(list,i,j);
```
## 其他

### 1. -0.0 != 0.0

在Java中，如果单纯是数值类型，-0.0 = 0.0。若是对象类型，则 -0.0 != 0.0。倘若你尝试用Set统计斜率数量时，这个问题就会带来麻烦。
提供的解决方式是在所有的斜率加入 Set 前将值增加 0.0。

```java

import java.io.PrintWriter;

public class Main {
    static PrintWriter out = new PrintWriter(System.out);

    static void A() {
        Double a = 0.0;
        Double b = -0.0;
        out.println(a.equals(b));//false
        out.flush();
    }

    static void B() {
        Double a = 0.0;
        Double b = -0.0 + 0.0;
        out.println(a.equals(b));//true
        out.flush();
    }

    static void C() {
        double a = 0.0;
        double b = -0.0;
        out.println(a == b);//true
        out.flush();
    }


    public static void main(String[] args) {
        A();
        B();
        C();
    }
}

```
    