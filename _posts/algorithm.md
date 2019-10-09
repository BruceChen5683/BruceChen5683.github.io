##### 时间复杂度
一般情况下，算法中基本操作重复执行的次数是问题规模n的某个函数，用T(n)表示，若有某个辅助函数f(n),使得当n趋近于无穷大时，T(n)/f(n)的极限值为不等于零的常数，则称f(n)是T(n)的同数量级函数。记作T(n)=Ｏ(f(n)),称Ｏ(f(n)) 为算法的渐进时间复杂度，简称时间复杂度。

##### 空间复杂度


| 一个普通标题 | 一个普通标题 | 一个普通标题 |
| ------ | ------ | ------ |
| 短文本 | 中等文本 | 稍微长一点的文本 |
| 稍微长一点的文本 | 短文本 | 中等文本 |

| 排序法    |平均时间 | 最差情形  | 稳定度   | 额外空间  | 备注   |
| :------: | ------: | :------: |:------:  |:------: |:------: |
| 冒泡 |
| 交换 |
| 选择 |
| 插入 |
| 基数 |
|Shell |
| 快速 |
| 归并 |
| 堆   |

##### 排序
- 直接插入排序


##### 各种算法
1. binarySearch
```
static int binarySearch(int[] array,int size,int value){
    int lo = 0
    int hi = size - 1;
    while(lo <= hi){
        int mid = (lo+hi)>>1;
        int midVal = array[mid];
        if(midVal < value){
            lo = mid +1;
        }else if(midVal > value){
            hi = mid +1;
        }else{
            return mid;//value found
        } 
   }
   return ~lo;//value not found
}
```

2. [贝塞尔曲线](https://www.cnblogs.com/hnfxs/p/3148483.html)
```
https://blog.csdn.net/eclipsexys/article/details/51956908
```

3. [Deffie-Hellman 密钥交换算法](http://wsfdl.com/algorithm/2016/02/04/%E7%90%86%E8%A7%A3Diffie-Hellman%E5%AF%86%E9%92%A5%E4%BA%A4%E6%8D%A2%E7%AE%97%E6%B3%95.html)
```
假设 q 为素数，对于正整数 a,x,y，有：

(a^x mod p)^y mod p = a^(xy) mod p
证明如下：

令 a^x = mp + n， 其中 m, n 为自然数， 0 <= n < p，则有
C = (a^x mod p)^y mod p
  = ((mp + n) mod p)^y mod p
  = n^y mod p
  = (mp +n)^y mod p  如何得到此步******    ******    ******
  = a^(xy) mod p

```