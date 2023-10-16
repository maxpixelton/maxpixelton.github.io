---
title: 数据结构：以空间换时间
author:
  name: superhsc
  link: https://github.com/maxpixelton
date: 2022-01-01 14:33:35 +0800
categories: [计算机基础, 数据结构和算法]
tags: [以空间换时间]
math: true
mermaid: true
---

针对低效代码，如何提高它们的效率 ？首先要知道，代码效率优化要将可行解提高到到更优解，最终目标是：**采用尽可能低的时间复杂度和空间复杂度，去完成一段代码的开发。**

# 昂贵的时间 VS 廉价的空间
一段代码会消耗计算时间和资源空间，从而产生时间复杂度和空间复杂度。

在尝试将时间复杂度和空间复杂进行对比，就会发现一个重要现象：

- **假设一段代码经过优化，虽然降低了时间复杂度，但依然需要消耗非常高的空间复杂度。**例如，对于固定数据量输入，这段代码需要消耗几十 G 内存空间，显然，普通计算机根本无法完成这样计算。如果一定要解决，一个最简单粗暴的办法就是，通过购买大量高性能计算机，来弥补空间性能的不足；
- **反过来，假设一段代码经过优化后，依然需要消耗非常高的时间复杂度。 **例如，对于固定数据量输入，这段代码需要消耗 1 年时间来完成计算。如果程序运行的 1 年时间内，出现了断电、断网或程序抛出异常等预期范围之外问题，很可能造成 1 年时间浪费的惨重后果。显然，用 1 年的时间去跑一段代码，对开发者和运维者而言都是极不友好的。

上面两个现象告诉一个现实问题：代码效率瓶颈可能发生在时间或者空间两个方面：

- 如果缺少计算空间，花钱买服务器就可以了，是个花钱就能解决的问题；
- 相反，如果是缺少计算时间，只能投入宝贵的人生去跑程序。即使有再多的钱、再多的服务器，也是毫无用处。

因此，相比于空间复杂度，降低时间复杂度显得非常重要了。进而得出结论：**空间是廉价的，时间是昂贵的。**
# 利用数据结构连接时空
假定在既不限制时间、也不限制空间的情况下，可以完成某个任务代码的开发（这就是通常所说的暴力解法，也是程序优化的起点）。

例如，如果要在 100 以内的正整数中，找到同时满足以下两个条件的最小数字：

- 能被 3 整除；
- 除 5 余 2。

最暴力的解法就是，从 1 开始到 100，每个数字都做一次判断。如果这个数字满足了上述两个条件，则返回结果。这是一种不计较任何时间复杂度或空间复杂度的、最直观的暴力解法。

当有了最暴力的解法后，就需要评估当前暴力解法的复杂度：

- 如果复杂度比较低或者可以接受，自然万事大吉；
- 如果暴力解法复杂度比较高，就要考虑采用**程序优化**的方法降低复杂度。

为了降低复杂度，一个直观的思路是：梳理程序，**看其流程中是否有无效的计算或者无效的存储。**

从时间复杂度和空间复杂度两个维度来考虑。常用的降低时间复杂度的方法有：

- 递归；
- 二分法；
- 排序算法；
- 动态规划等。

降低空间复杂度的方法，就要围绕数据结构做文章了，核心思路就是，**能用低复杂度的数据结构解决问题，就千万不要用高复杂度的数据结构。**

如果程序在时间和空间等方面的性能依然还有瓶颈，又该怎么办呢？前面提到过，空间是廉价的，最不济也是可以通过购买更高性能的计算机进行解决的。然而时间是昂贵的，如果无法降低时间复杂度，那系统的效率就永远无法得到提高。

此时有这样一个解决思路。**如果可以通过某种方式，把时间复杂度转移到空间复杂度的话，就可以把无价的东西变成有价了。这种时空转移的思想，在实际生活中也会经常遇到。**

例如，马路上的十字路口，所有车辆在通过红绿灯时需要分批次通行。这样，就消耗了所有车辆的通行时间。如果要降低这里的时间损耗，人们想到了修建立交桥。修建立交桥后，每个可能的转弯或直行的行进路线，都有专属的一条公路支持。这样，车辆就不需要全部去等待红绿灯分批通行了。最终，实现了用空间换取时间。

![这是一张图片](https://maxpixelton.github.io/images/assert/structures/0201.gif)

其实，程序开发也是可以借鉴这里的思想的。**在程序开发中，连接时间和空间的桥梁就是<font color="red">数据结构</font>。**

对于一个开发任务，如果能找到一种高效的数据组织方式，采用合理的数据结构，就可以实现时间复杂度的再次降低。同样的，这通常会增加数据的存储量，也就是增加了空间复杂度。

以上就是程序优化的最核心的思路，简单梳理如下：

1. **暴力解法。**在没有任何时间、空间约束下，完成代码任务的开发；
1. **无效操作处理。**将代码中的无效计算、无效存储剔除，降低时间或空间复杂度；
1. **时空转换。**设计合理数据结构，完成时间复杂度向空间复杂度的转移。

# 栗子

### 案例一
假设有任意多张面额为 2 元、3 元、7 元的货币，现要用它们凑出 100 元，求总共有多少种可能性。
假设写了下面的代码：
```java
public static void s2_1() {
    int count = 0;
    for (int i = 0; i <= (100 / 7); i++) {
        for (int j = 0; j <= (100 / 3); j++) {
            for (int k = 0; k <= (100 / 2); k++) {
                if (i * 7 + j * 3 + k * 2 == 100) {
                    count += 1;
                }
            }
        }
    }
    System.out.println(count);
}
```
上面代码中，使用了 3 层的 for 循环。从结构上来看，是很显然的 $O( n³ )$ 时间复杂度。然而，仔细观察会发现，代码中最内层的 for 循环是多余的。因为，当确定了要用 i 张 7 元和 j 张 3 元时，只需要判断用有限个 2 元能否凑出 $100 - 7 \times i - 3 \times j$ 元就可以了。因此，代码改写如下：
```java
public static void s2_2() {
    int count = 0;
    for (int i = 0; i <= (100 / 7); i++) {
        // 下面的 for 中 j 的最大值可以改为 100-7*i) / 3);
        // 虽然可以提供性能，但是时间复杂度不变
        for (int j = 0; j <= (100 / 3); j++) {
            int tmp = 100 - i * 7 - j * 3;
            if ((tmp >= 0) && (tmp % 2 == 0)) {
                count++;
            }
        
        }
    }
    System.out.println(count);
}
```
经过改造后，代码的结构由 3 层 for 循环，变成了 2 层 for 循环。显然，时间复杂度就变成了$O(n^2)$ 。这样的代码改造，就是利用了方法论中的步骤二，将代码中的无效计算、无效存储剔除，降低时间或空间复杂度。
### 案例二
查找出一个数组中，出现次数最多的那个元素的数值。例如，输入数组 `a = [1,2,3,4,5,5,6 ]` 中，查找出现次数最多的数值。从数组中可以看出，只有 5 出现了 2 次，其余都是 1 次。显然 5 出现的次数最多，则输出 5。

首先想到的解决方法是，采用两层的 for 循环完成计算：

- 第一层循环，对数组每个元素遍历；
- 第二层循环，是对第一层遍历的数字，去遍历计算其出现的次数。这样，全局再同时缓存一个出现次数最多的元素及其次数就可以了。具体代码如下：
```java
public static void s2_3() {
    int[] a = { 1, 2, 3, 4, 5, 5, 6 };
    int valMax = 0;
    int timeMax = 0;
    int timeTmp = 0;
    for (int k : a) {
        timeTmp = 0;
        for (int i : a) {
            if (k == i) {
                timeTmp += 1;
            }
            if (timeTmp > timeMax) {
                timeMax = timeTmp;
                valMax = k;
            }
        }
    }
    System.out.println(valMax);
}
```
在这段代码中，采用了两层的 for 循环，很显然时间复杂度就是 $O(n^2)$。而且代码中，几乎没有冗余的无效计算。如果还需要再去优化，就要考虑采用一些数据结构方面的手段，来把时间复杂度转移到空间复杂度了。

先思考一下，这个问题能否通过一次 for 循环就找到答案呢？一个直观的想法是，一次循环的过程中，同步记录下每个元素出现的次数。最后，再通过查找次数最大的元素，就得到了结果。

具体而言，定义一个 k-v 结构的字典，用来存放元素-出现次数的 k-v 关系。那么步骤如下：

1. 通过一次循环，将数组转变为“元素-出现次数”的一个字典；

2. 再去遍历一遍这个字典，找到出现次数最多的那个元素，就能找到最后的结果了。

   ![这是一张图片](https://maxpixelton.github.io/images/assert/structures/0202.gif)

具体代码如下：

```java
public static void s2_4() {
    int[] a = { 1, 2, 3, 4, 5, 5, 6 };
    
    Map<Integer, Integer> d = new HashMap<>();
    for (int i : a) {
        if (d.containsKey(i)) {
            d.put(i, d.get(i) + 1);
        } else {
            d.put(i, 1);
        }
    }
    
    int valMax = -1;
    int timeMax = 0;
    
    for (Integer key: d.keySet()) {
        if ( d.get(key) > timeMax) {
            timeMax = d.get(key);
            valMax = key;
		}
    }
    System.out.println(valMax);
}
```
这种方法的时空复杂度。代码结构上，有两个 for 循环。不过，这两个循环不是嵌套关系，而是顺序执行关系。其中：

- 第一 个循环实现了数组转字典的过程，也就是 $O(n)$ 的复杂度；
- 第二个循环再次遍历字典找到出现次数最多的那个元素，也是一个 $O(n)$ 的时间复杂度。

因此，总体的时间复杂度为 $O(n) + O(n)$，就是 $O(2n)$，根据复杂度与具体的常系数无关的原则，也就是$O(n) $的复杂度。空间方面，由于定义了 k-v 字典，其字典元素的个数取决于输入数组元素的个数。因此，空间复杂度增加为 $O(n)$。

这段代码的开发，就是借鉴了方法论中的步骤三，通过采用更复杂、高效的数据结构，完成了时空转移，提高了空间复杂度，让时间复杂度再次降低。

# 总结
无论什么难题，降低复杂度的方法就是这三个步骤：

1. **暴力解法（暴解）。**在没有任何时间、空间约束下，完成代码任务的开发。此步骤没有太多的套路，只要围绕着面临的问题出发，大胆想象，去尝试解决即可；
2. **无效操作处理（剔除）。**将代码中的无效计算、无效存储剔除，降低时间或空间复杂度。此步骤需要掌握递归、二分法、排序算法、动态规划等常用的算法思维；
3. **时空转换（时间转空间）。**设计合理数据结构，完成时间复杂度向空间复杂度的转移。此步骤需要对数据的操作进行细分，全面掌握常见数据结构的基础知识。再围绕问题，有针对性的设计数据结构、采用合理的算法思维，去不断完成时空转移，降低时间复杂度。