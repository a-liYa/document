# Java hashCode() 和 equals()


## Java对equals()的要求
有以下几点：

```
1. 对称性：如果x.equals(y)返回是"true"，那么y.equals(x)也应该返回是"true"。
2. 反射性：x.equals(x)必须返回是"true"。
3. 类推性：如果x.equals(y)返回是"true"，而且y.equals(z)返回是"true"，那么z.equals(x)也应该返回是"true"。
4. 一致性：如果x.equals(y)返回是"true"，只要x和y内容一直不变，不管你重复x.equals(y)多少次，返回都是"true"。
5. 非空性，x.equals(null)，永远返回是"false"；x.equals(和x不同类型的对象)永远返回是"false"。
```

## equals() 和 hashCode() 规则

```
1. equals()为true，则hashCode()一定相等;
2. hashCode()相等, 则equals()不一定为true;
```

## hashCode覆写规范
摘自《Effective Java》

```
1. 初始化一个整形变量，为此变量赋予一个非零的常数值，比如int result = 17;

2. 选取equals方法中用于比较的所有域，然后针对每个域的属性进行计算：
  (1) 如果是boolean值，则计算 f ? 1 : 0

  (2) 如果是byte\char\short\int，则计算(int)f

  (3) 如果是long值，则计算(int)(f ^ (f >>> 32))

  (4) 如果是float值，则计算Float.floatToIntBits(f)

  (5) 如果是double值，则计算Double.doubleToLongBits(f)，然后返回的结果是long，再用规则(3)去处理long，得到int

  (6)如果是对象应用，如果equals方法中采取递归调用的比较方式，那么hashCode中同样采取递归调用hashCode的方式。否则需要为这个域计算一个范式，比如当这个域的值为null的时候，那么hashCode值为0

  (7)如果是数组，那么需要为每个元素当做单独的域来处理。如果你使用的是1.5及以上版本的JDK，那么没必要自己去重新遍历一遍数组，java.util.Arrays.hashCode方法包含了8种基本类型数组和引用数组的hashCode计算，算法同上，
```