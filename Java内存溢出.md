# Java内存溢出(Out of Memory)简称OOM

# 内存溢出

## **介绍：**

内存溢出是指程序在运行过程中，由于申请的内存空间超过了系统或虚拟机所能提供的最大限制，导致程序无法继续执行。内存溢出通常是由于申请的对象过多，而系统无法为它们提供足够的内存空间。

```java
package oom;

import java.util.ArrayList;
import java.util.List;

public class OutOfMemoryExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 1000000000000L; i++) {
            new Thread(()->{
                for (int j = 0; j < 10000; j++) {
                    Student student = new Student("1", "22");
                    list.add(student.name);
                }
            }).start();

            list.add("OutOfMemoryExample");  // 不断向列表中添加元素，可能导致内存溢出
            new Student("1","blake");
        }
    }
}
class Student  {
    String id;
    String name;

    public Student(String id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

## 使用分析工具：VisualVM

下载地址： https://visualvm.github.io/

![image-20240203111845394](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240203111845394.png)

注意： 如果笔记本下载下来无法安装，就下载一个之前的版本使用。

这里是我下载的版本：**可以下载“.zip” 包。**

![image-20240202231641211](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/Java内存溢出.png)

具体步骤： 

1. 下载zip文件并解压；
2. 编辑：**visualvm_211/etc/visualvm.conf** 文件；
3. <img src="/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240202232641142.png" alt="image-20240202232641142" style="zoom:100%;" />

4. **执行bin/visualvm**

![image-20240202232922954](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240202232922954.png)

5. 启动之后： 

![image-20240202233121773](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240202233121773.png)

## 测试

执行上述代码，使用工具查看分析：

1. 首先代码直接就终止进程了，OOM了。

![image-20240203110222488](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240203110222488-6929346.png)

## 分析VisualVM 找原因

![image-20240203105645312](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240203105645312.png)

![image-20240203110919244](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240203110919244.png)

![image-20240203111117963](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240203111117963.png)

![image-20240203111455306](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240203111455306.png)

![image-20240203111600200](/Users/blakeya/IdeaProjects/BlakeYa.github.io/Java内存溢出.assets/image-20240203111600200.png)



# 内存泄漏

内存泄露是指程序在运行过程中，由于某些原因，未能释放已经不再需要的对象或资源，导致这些对象一直占用着内存，使得可用内存逐渐减少。内存泄露通常是由于未正确释放对象的引用，使得垃圾回收器无法回收这些对象，最终导致程序消耗的内存不断增加。

示例：

```java
public class MemoryLeakExample {
    private static List<Object> memoryLeakList = new ArrayList<>();

    public void addToMemoryLeakList(Object obj) {
        memoryLeakList.add(obj);
    }

    // 没有正确释放对象引用，可能导致内存泄露
}
```
