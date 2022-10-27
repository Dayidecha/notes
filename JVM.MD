![image-20210407190756542](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210407190756542.png)

+ 只要一个线程开始运行，**java虚拟机**就会给线程分配一块专属的内存空间，叫**线程栈**，线程运行时的局部变量存在栈中。

+ 一个方法对应一块栈帧内存区域

![image-20210407201125988](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210407201125988.png)

+ javap -c 对字节码文件(.class)进行反汇编生成更可读的

  `javap -c Math.class > math.txt`

