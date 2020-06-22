# Redis-String:

******

**单值单Value**

******

1. ## set/get/append/strlen:

两次set，后面的会覆盖前面的value

![Redis-String1](D:\Boke\Databases\Nosql\Redis\Basic\Redis-String1.png)

2. ## Incr/decr/incrby/decrby

   *****

   必须是数值才能向加减！！！

   ******

   ![Redis-String2](D:\Boke\Databases\Nosql\Redis\Basic\Redis-String2.png)

3. ## getrange/setrange

******

从0到-1代表全部

getrange：获取指定区间范围内的值，类似between……and

setrange: 设置指定区间范围内的值

******

### getrange：

![image-20200602094805165](D:\Boke\Databases\Nosql\Redis\Basic\Redis-String3.png)

### setrange:

![image-20200602095005187](C:\Users\W\AppData\Roaming\Typora\typora-user-images\image-20200602095005187.png)

4. ## setex(set with expire) ：setex key seconds value

   setnx(set if  not exsit) :如果不存在则起效 (setnx key value)

   如果起效则为1，不起效为0

   

   ![image-20200602095923645](D:\Boke\Databases\Nosql\Redis\Basic\Redis-String4.png)

5. ## mset/mget/msetnx(m:more )

![image-20200602151723208](D:\Boke\Databases\Nosql\Redis\Basic\Redis-String6.png)