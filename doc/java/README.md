# 稀疏数组

## **基本介绍**:当一个数组但部分元素为0，或者为同一个值的数组时，可以使用稀疏数组来保存该数组

#### 稀疏数组的处理方法：

##### 1、记录数组一共有几行几列，有多少个不同的值

##### 2、把具有不同值的元素的行列及值记录在一个小规模的数组中，从而缩小程序的规模

##### **案列：**

​            ![image-20200528133318427](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528133318427.png)                   

##### **思路分析**：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528133332816.png" alt="image-20200528133332816" style="zoom:150%;" />

#### 代码实现：

```java
/**
 * 二维数组转为稀疏数组
 * 稀疏数组转为二维数组
 *
 */
public class SparseArray {
    public static void main(String[] args) {
        //创建一个原始的二维数组 11 * 11
        //0：没有棋子  1：黑色棋子  2：蓝色棋子
        int chessArr1 [][] =new int[11][11];
        chessArr1[1][2]=1;
        chessArr1[2][3]=2;
        chessArr1[5][2]=1;
        chessArr1[4][8]=2;
        System.out.println("原始的二维数组");
        for (int [] row : chessArr1){
            for (int data : row){
                System.out.printf("%d\t",data);
            }
            System.out.println();
        }

        //将二维数组 转 稀疏数组
        //1、先遍历二维数组 得到非0数据的个数
        int sum=0;
        for (int i = 0; i < 11; i++) {
            for(int j =0;j<11;j++){
                if (chessArr1[i][j] != 0){
                    sum++;
                }
            }
        }

        //2、创建对应的稀疏数组
        int sparseArr[][] = new int[sum+1][3];
        //给稀疏数组赋值
        sparseArr[0][0]=11;
        sparseArr[0][1]=11;
        sparseArr[0][2]=sum;

        // 遍历二维数组，将非0的值存到sparseArr中
        int count = 0;
        for (int i = 0; i < 11; i++) {
            for(int j=  0;j<11;j++){
                if (chessArr1[i][j] != 0){
                    count++;
                    sparseArr[count][0]=i;
                    sparseArr[count][1]=j;
                    sparseArr[count][2]=chessArr1[i][j];
                }
            }
        }

        //输出稀疏数组的形式
        System.out.println();
        System.out.println("得到的稀疏数组为-----");
        System.out.printf("行  列  值");
        System.out.println();
        for (int i = 0; i < sparseArr.length; i++) {
                System.out.printf("%d\t%d\t%d\t\n",sparseArr[i][0],sparseArr[i][1],sparseArr[i][2]);
        }
        System.out.println();

        //稀疏数组恢复为二维数组
        int chessArr2 [][] = new int [sparseArr[0][0]][sparseArr[0][1]];

        //将数据恢复到二维数组中
        for (int i=1 ;i<sparseArr.length;i++){
            chessArr2[sparseArr[i][0]][sparseArr[i][1]] = sparseArr[i][2];
        }
        System.out.println();
        System.out.println("恢复后的二维数组");

        for (int [] row : chessArr2){
            for (int data : row){
                System.out.printf("%d\t",data);
            }
            System.out.println();
        }
    }
}

```



# 队列

### #队列介绍：

###### 1、队列是一个**有序列表，**可以用**数组**或**链表**实现

###### 2、遵循**先入先出**的原则。即：**先存入队列的数据，要先取出。后存入后取出**

### #**数组模拟队列的思路：**

![image-20200528133227355](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528133227355.png)

##### **当我们将数据存入队列是称为“addQueue”的方法需要两个步骤：**

###### **1、** **将尾指针下·往后移：rear+1，当front==rear【空】**

###### **2、** **若尾指针rear小于队列的最大下标maxSize-1，则将数据存入rear所指的数组元素中，否则无法存入数据。rear == maxSize – 1【队列满】**

### #代码实现：

```java
public class ArrayQueueDemo {
    public static void main(String[] args) {
        ArrayQueue arrayQueue = new ArrayQueue(3);
        char key = ' ';
        Scanner sc = new Scanner(System.in);
        boolean loop =true;
        while (loop){
            System.out.println("s(show)：显示队列");
            System.out.println("e(exit)：推出程序");
            System.out.println("a(add)：添加数据到队列");
            System.out.println("g(get)：从队列中取得数据队列");
            System.out.println("h(head)：查看队列头的数据");

            key=sc.next().charAt(0);

            switch (key){
                case 's':
                    arrayQueue.showQueue();
                    break;
                case 'a':
                    System.out.println("请输入一个数字");
                    int value = sc.nextInt();
                    arrayQueue.addQueue(value);
                    break;
                case 'g':
                    try {
                        int res = arrayQueue.getQueue();
                        System.out.printf("取出的数据是%d\n",res);
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try {
                        int res = arrayQueue.headQueue();
                        System.out.printf("队列头部信息是%d\n",res);
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    sc.close();
                    loop=false;
                    break;
            }
        }
        System.out.println("程序推出--||--");
    }
}
//使用数组模拟队列-编写一个ArrayQueue类
class ArrayQueue{
    private int maxSize;//表示数组的最大容量
    private int front;//队列头
    private int rear;//队列尾
    private int[] arr;//该数组用于存放数据，模队列

    //创建队列的构造器
    public ArrayQueue(int arrMaxSize){
        maxSize = arrMaxSize;
        arr = new int [maxSize];
        front=-1;//指向队列头部，分析出front是指向队列头的前一个位置
        rear=-1;//指向队列尾，指向队列尾的数据(即就是队列最后一个数据)
    }

    //判断队列是否已经满
    public boolean isFull(){
        return  rear == maxSize - 1;
    }

    //判断队列是否为空
    public boolean isEmpty(){
        return rear == front;
    }

    //添加数据到队列
    public void addQueue(int n){
        //判断队列是否满
        if (isFull()){
            System.out.println("队列满，不能添加数据");
            return;
        }
        rear++;//让rear 后移
        arr[rear] = n;
    }

    //获取队列数据 出队列
    public int getQueue(){
        //判断是否空
        if (isEmpty()){
            //通过抛出异常来处理
            throw new RuntimeException("队列空，不能取数据");
        }
        front++; //front后移
        return arr[front];
    }

    //显示所有数据
    public void showQueue(){
        //遍历
        if (isEmpty()){
            System.out.println("队列空，无法取得数据");
            return;
        }
        for (int i = 0; i < arr.length; i++) {
            System.out.printf("arr[%d]=%d\n",i,arr[i]);
        }
    }

    //显示头部数据(不是取出数据)
    public int headQueue(){
        //判断
        if (isEmpty()){
            throw new RuntimeException("队列空，无法取得数据");
        }
        return arr[front + 1];
    }
}

```

## 

#### **问题分析并优化**:

###### **1****、目前数组使用一次就不能再次使用，没有达到复用的效果**

###### **2****、将这个数组使用算法，改进成一个环形的队列 通过取模：%**



## #**数组模拟环形数列**

###### 思路分析：

![image-20200528133754954](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528133754954.png)

###### 代码实现：

```java
/**
 * 数组模拟环形数列
 */
public class circleArrayQueueDemo {
    public static void main(String[] args) {
        System.out.println("数组模拟环形队列");
        //创建一个环形队列
        circleArray arrayQueue = new circleArray(4);//队列有效数据最大为 arrMaxSize - 1
        char key = ' ';
        Scanner sc=new Scanner(System.in);
        boolean loop = true;
        while (loop){
            System.out.println("s(show)：显示队列");
            System.out.println("e(exit)：推出程序");
            System.out.println("a(add)：添加数据到队列");
            System.out.println("g(get)：从队列中取得数据队列");
            System.out.println("h(head)：查看队列头的数据");

            key=sc.next().charAt(0);

            switch (key){
                case 's':
                    arrayQueue.showQueue();
                    break;
                case 'a':
                    System.out.println("请输入一个数字");
                    int value = sc.nextInt();
                    arrayQueue.addQueue(value);
                    break;
                case 'g':
                    try {
                        int res = arrayQueue.getQueue();
                        System.out.printf("取出的数据是%d\n",res);
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try {
                        int res = arrayQueue.headQueue();
                        System.out.printf("队列头部信息是%d\n",res);
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'e':
                    sc.close();
                    loop=false;
                    break;
        }

        }
        System.out.println("程序退出-- || --");
    }
}

//使用数组模拟队列-编写一个ArrayQueue类
class circleArray{
    private int maxSize;//表示数组的最大容量
    private int front;//指向队列的第一个元素，即arr[front]就是队列的第一个元素  front的初始值=0
    private int rear;//指向队列的最后一个元素的后一个位置，因为希望空出一个空间作为约定 rear的初始值=0
    private int[] arr;//该数组用于存放数据，模队列

    public circleArray(int arrMaxSize){
        maxSize=arrMaxSize;
        arr = new int [maxSize];
    }

    //判断是否满
    public boolean isFull(){
        return (rear+1) % maxSize == front;
    }

    //判断是否空
    public boolean isEmpty(){
        return rear == front;
    }

    //添加数据到队列
    public void addQueue(int n){
        //判断队列是否满
        if (isFull()){
            System.out.println("队列满，不能添加数据");
            return;
        }
        arr[rear] = n;//直接加入数据
        rear = (rear+1) % maxSize;//将rear后移 需要考虑取模 防止越界
    }

    //获取队列数据 出队列
    public int getQueue(){
        //判断是否空
        if (isEmpty()){
            //通过抛出异常来处理
            throw new RuntimeException("队列空，不能取数据");
        }
        /** front指向队列第一个元素
         *  1、先把front对应的值保存到一个临时变量
         *  2、将front后移，考虑取模 防止越界
         *  3、将临时变量返回
         */
        int value = arr[front];
        front = (front + 1) % maxSize;
        return value;
    }

    //求出当前数列有效数据个数
    public int size(){
        return (rear + maxSize - front) % maxSize;
    }

    //显示所有数据
    public void showQueue(){
        //遍历
        if (isEmpty()){
            System.out.println("队列空，无法取得数据");
            return;
        }
        for (int i = front; i < front + size(); i++) {
            System.out.printf("arr[%d]=%d\n",i % maxSize,arr[i%maxSize]);
        }
    }

    //显示头部数据(不是取出数据)
    public int headQueue(){
        //判断
        if (isEmpty()){
            throw new RuntimeException("队列空，无法取得数据");
        }
        return arr[front];
    }

```

------

# **链表(Linked List)**

![image-20200528133951229](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528133951229.png)

##### **小结：**

###### **1、** **链表是以节点的方式来存储，是链式储存**

###### **2、** **每个节点包含data域，next域：指向下一个节点**

###### **3、** **如图：发现链表的各个节点不一定是连续存储**

###### **4、** **链表分带头节点的链表和没有头节点的链表，根据实际需求来确定**



### #**单链表的创建示意图：(不考虑排序)**

![image-20200528134053211](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528134053211.png)



##### **代码实现(不考虑排序)**

```java
public class singleLinkedListDemo {
    public static void main(String[] args) {
        HeroNode heroNode1 = new HeroNode(1,"123","123");
        HeroNode heroNode2 = new HeroNode(2,"456","456");
        HeroNode heroNode3 = new HeroNode(3,"1","1");
        HeroNode heroNode4 = new HeroNode(4,"23","23");

        //创建要给的链表
        singleLinkedList singleLinkedList = new singleLinkedList();
        //加入
        singleLinkedList.add(heroNode1);
        singleLinkedList.add(heroNode3);
        singleLinkedList.add(heroNode2);
        singleLinkedList.add(heroNode4);

        //目前输出的顺序就时加入数据的顺序
        singleLinkedList.list();
    }
}

//定义singleLinkedList 管理我们的英雄
class singleLinkedList{
    //初始化一个头节点，头节点为固定值,不存放具体的数据
    private HeroNode head = new HeroNode(0,"","");

    //添加节点到单向链表
    /**
     * 思路：（不考虑编号时）
     * 1、找到当前链表的最后一个节点
     * 2、将最后这个节点的 next 指向新加的节点
     */
    public void add(HeroNode heroNode){
        //因为head节点不能动，因此需要一个辅助遍历temp
        HeroNode temp = head;
        //遍历链表，找最后的next
        while (true){
            if (temp.next == null){//temp.next值等于空说明找到最后一个值
                break;
            }
            //如果没有找到最后，则将temp后移
            temp=temp.next;
        }
        //当退出了while循环,temp就指向了链表最后
        //将最后这个节点的next指向 新的节点
        temp.next = heroNode;
    }

    //显示链表[遍历]通过一个辅助变量
    public void list(){
        //判断链表是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //因为头节点不能动，所以需要一个辅助变量来遍历
        HeroNode temp = head.next;
        while (true){
            //判断链表是否到最后
            if (temp == null){
                break;
            }
            //输出节点信息
            System.out.println(temp);
            //将next后移
            temp = temp.next;
        }
    }
}

//定义HeroNode，每个HeroNode对应一个节点
class HeroNode{
    public int no;
    public String name;
    public String nickname;
    public HeroNode next;//指向下一个节点
    //构造器
    public HeroNode(int no,String name,String nickname){
        this.no=no;this.name=name;this.nickname=nickname;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\'';
    }
}

    
```

##### 结果—(只按照加入数据得顺序)

![image-20200528134311331](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528134311331.png)

------

### #**单链表创建示意图：(排序)**

![image-20200528134433597](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528134433597.png)

##### **创建新的加入方法(代码实现)**：

```java
/**
 * 第二种添加方式，根据英雄排名将英雄插入到指定位置
 * (如果有这个排名，则添加失败，并给出提示)
 * 因为头节点不能动，需要创建一个变量来找到添加的位置
 * 因为是单链表，因此找的temp位于 添加位置的前一个节点 否则加入不了
 */
public void  addByOrder(HeroNode01 heroNode01){
    HeroNode01 temp = head;
    boolean flag=false;
    while (true){
        if (temp.next == null){//说明temp已经在链表的最后
            break;
        }
        if (temp.next.no > heroNode01.no){//位置找到，就在temp的后面插入
            break;
        }else if (temp.next.no == heroNode01.no){//说明添加的heroNode01的编号已经存在
            flag=true;//编号存在
            break;
        }
        temp = temp.next;//后移，遍历链表
    }
    //判断flag的值
    if (flag){//不能添加，编号已经存在
        System.out.printf("加入的数据编号已经存在%d，不能加入\n",heroNode01.no);
    }else{
        //插入到链表中，temp的后面
        heroNode01.next = temp.next;
        temp.next = heroNode01;
    }
}

```

### #**更新操作(找到指定的节点值，不能修改节点)**

##### **思路：**

1.**先找到该节点，通过遍历** 

2.**temp.name=newheronode.name;temp.nickname=newheronode.nickname;**

###### 代码实现：

```java
/**
 * 修改节点信息，根据no值来修改，所以no值不能修改
 * 根据newHeroNode的no值来修改即可
 * @param newheronode01
 */

public void update(HeroNode01 newheronode01){
    //判断是否为空
    if (head.next == null){
        System.out.println("链表为空");
        return;
    }
    //找到需要修改的节点，根据no值  定义一个辅助变量
    HeroNode01 temp = head.next;
    boolean flag = false;//表示是否找到节点
    while (true){
        if (temp == null){
            break;//已经遍历完链表
        }
        if (temp.no == newheronode01.no){
                flag = true;//找到
                break;
        }
        temp = temp.next;
    }
    //根据flag判断是否找到需要修改的节点
    if (flag){
        temp.name = newheronode01.name;
        temp.nickname = newheronode01.nickname;
    }else{//没有找到
        System.out.printf("没有找到 编号为%d 的节点，不能修改/n",newheronode01.no);

    }
}

```

### **删除操作**

#### 思路：

![image-20200528135042229](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528135042229.png)

##### 代码实现：

```java
/**
 * 删除节点
 * head不能动。因此需要一个temp辅助节点找到待删除的节点的前一个节点
 * 在比较时，temp.next.no  和 需要删除的节点的no比较
 */
 public void del(int no){
     HeroNode01 temp =head;
     boolean flag = false;  //标志是否找到待删除的节点
     while (true){
         if (temp.next == null){//已经到了链表的最后
             break;
         }
         if (temp.next.no == no){//找到的待删除的节点的前一个节点temp
            flag = true;
            break;
         }
         temp = temp.next; //temp后移，遍历
     }
     if (flag){//找到，可以删除
        temp.next = temp.next.next;
     }else{
         System.out.printf("要删除的 %d 节点不存在/n",no);
     }
 }

```

## 一些方法的实现：

### 1、获取单链表的节点个数(如果时戴头节点的链表，需求不统计头节点

```java
/**
 * 获取单链表的节点个数(如果时戴头节点的链表，需求不统计头节点)
 * @param head  链表的头节点
 * @return      返回链表有效节点的个数
 */
public static int getlength(HeroNode01 head){
    int len =0;
    HeroNode01 cur = head.next;
    if(head.next == null){
        return 0;
    }
    while (cur != null){
        len++;
        cur = cur.next;//遍历
    }
    return len;
}

```

### 2、查找单链表中的倒数第k个节点

```java
/** 查找单链表中的倒数第k个节点
 * 思路：1、编写方法，接收head节点，index
 *       2、index是表示倒数第index个节点
 *       3、先把链表遍历一遍，得到链表的总长度 (调用getLength()方法即可)
 *       4、得到size后，二次遍历 从链表的第一个到(size-index)个，就可以得到
 *       5、找到返回节点，否则返回null
 * @param head
 * @param index
 * @return
 */
public static HeroNode01 getLastIndexNode(HeroNode01 head,int index){
        if (head.next == null){
            return null;
        }
        //第一次遍历得到链表长度
        int size = getlength(head);
        //第二次遍历 size-index 位置，就是倒数第k个节点
        if (index <=0 || index > size){//校验index在·
            return null;
        }
        //定义辅助变量  for 循环定位到倒数的index
        HeroNode01 temp = head.next;
    for (int i = 0; i < size - index; i++) {
        temp = temp.next;
    }
    return temp;
}


```

### 3、将单链表反转

#### **思路分析：**

![image-20200528135257322](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528135257322.png)

#### 代码实现：

```java
**
 *  将单链表反转
 *  思路：1、定义一个节点 reverseHead = new   HeroNode01()
 *        2、将链表遍历一遍，每遍历一个节点 就将其取出 并放在新的链表reverseHead的最前端
 *        3、将原来的链表的head.next = reverseHead.next
 *
 * @param head
 */
public static void reverseList(HeroNode01 head){
    //判断链表是否为空，或者只有一个节点，无需反转直接返回
    if (head.next == null || head.next.next == null){
        return;
    }

    //定义一个辅助的指针(变量)，帮助遍历链表
    HeroNode01 cur = head.next;
    HeroNode01 next = null;//指向当前节点【cur】的下一个节点
    HeroNode01 reverseHead = new HeroNode01(0,"","");
    //遍历原链表，每遍历一个节点将其取出，并放到新的链表reverserHead的最前端
    while (cur != null){
        next = cur.next;//暂时保存当前节点的下一个节点
        cur.next = reverseHead.next;//将cur的下一个节点指向链表的最前端
        reverseHead.next = cur;//将cur 连接到新的链表上
        cur = next;//让cur后移
    }
    //将head.next 指向 reverser.next ，实现链表的反转
    head.next = reverseHead.next;
}


```

### 4、逆序打印单链表

#### **思路分析**：

![image-20200528135425219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528135425219.png)

#### 举例---栈：

```java
public class TestStack {
    public static void main(String[] args) {
        Stack<String> stack = new Stack<String>();
        //入栈
        stack.add("a");
        stack.add("b");
        stack.add("c");

        //出栈
        while (stack.size()>0){
            System.out.println(stack.pop());
        }
    }
}

```

#### 代码实现：

```java
/**
 * 逆序打印单链表
 * 思路：利用栈这个数据结构，将各个节点压入栈中，然后通过栈的先进后出的特点 实现逆序打印
 * @param head
 */
public static void reversePrint(HeroNode01 head){
        if (head.next == null){//链表为空
            return;
        }
    //创建一个栈，将各个节点压入栈中
    Stack<HeroNode01> stack = new Stack<HeroNode01>();
    HeroNode01 cur = head.next;
    while (cur != null){
        stack.push(cur);
        cur = cur.next;//cur后移，使得栈可以压入下一个节点
    }
    while (stack.size() > 0){//打印逆序后的单链表，通过栈的pop方法
        System.out.println(stack.pop());
    }
}

```

------

## **双向链表：**

### 创建双向链表分析图：

![image-20200528135824800](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528135824800.png)

### **代码实现**：

```java
public class DoubleLinkedListDemo {
    public static void main(String[] args) {
        HeroNode02 heroNode01 = new HeroNode02(1,"123","123");
        HeroNode02 heroNode02 = new HeroNode02(2,"456","456");
        HeroNode02 heroNode03 = new HeroNode02(3,"1","1");
        HeroNode02 heroNode04 = new HeroNode02(4,"23","23");

        //创建要给的链表
        DoubleLinkedList doubleLinkedList = new DoubleLinkedList();

        //添加
        doubleLinkedList.add(heroNode01);
        doubleLinkedList.add(heroNode02);
        doubleLinkedList.add(heroNode03);
        doubleLinkedList.add(heroNode04);

        doubleLinkedList.list();

        //修改
        HeroNode02 newheroNode = new HeroNode02(3,"0","0");
        doubleLinkedList.update(newheroNode);
        System.out.println("修改后的链表");
        doubleLinkedList.list();

        //删除
        doubleLinkedList.del(3);
        System.out.println("删除后的链表");
        doubleLinkedList.list();
    }
}

class DoubleLinkedList{
    //先初始化一个头节点 头节点不能动 不存放具体数据
    private  HeroNode02 head = new HeroNode02(0,"","");

    //返回头节点
    public HeroNode02 getHead(){
        return head;
    }

    //遍历双向链表
    public void list(){
        //判断链表是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //因为头节点不能动，所以需要一个辅助变量来遍历
        HeroNode02 temp = head.next;
        while (true){
            //判断链表是否到最后
            if (temp == null){
                break;
            }
            //输出节点信息
            System.out.println(temp);
            //将next后移
            temp = temp.next;
        }
    }

    //添加一个节点到双向链表的最后
    public void add(HeroNode02 heroNode02){
        //因为head节点不能动，因此需要一个辅助遍历temp
        HeroNode02 temp = head;
        //遍历链表，找最后的next
        while (true){
            if (temp.next == null){//temp.next值等于空说明找到最后一个值
                break;
            }
            //如果没有找到最后，则将temp后移
            temp=temp.next;
        }
        //当退出了while循环,temp就指向了链表最后
        //形成一个双向链表
        temp.next =heroNode02;
        heroNode02.pre = temp;

    }

    //修改一个节点的内容
    public void update(HeroNode02 newheronode02){
        //判断是否为空
        if (head.next == null){
            System.out.println("链表为空");
            return;
        }
        //找到需要修改的节点，根据no值  定义一个辅助变量
        HeroNode02 temp = head.next;
        boolean flag = false;//表示是否找到节点
        while (true){
            if (temp == null){
                break;//已经找到链表的最后
            }
            if (temp.no == newheronode02.no){
                flag = true;//找到
                break;
            }
            temp = temp.next;
        }
        //根据flag判断是否找到需要修改的节点
        if (flag){
            temp.name = newheronode02.name;
            temp.nickname = newheronode02.nickname;
        }else{//没有找到
            System.out.printf("没有找到 编号为%d 的节点，不能修改/n",newheronode02.no);

        }
    }

    /**
     * 从双向链表中删除一个节点
     * 说明：1、对于双向链表，可以直接找到需要删除的节点
     *       2、找到后，自我删除即可
     * @param no
     */
    public void del(int no){
        //判断链表是否为空
        if(head.next == null){
            System.out.println("链表为空，无法删除");
            return;
        }
        HeroNode02 temp =head.next;//辅助变量
        boolean flag = false;  //标志是否找到待删除的节点
        while (true){
            if (temp == null){//已经到了链表的最后
                break;
            }
            if (temp.no == no){//找到的待删除的节点的前一个节点temp
                flag = true;
                break;
            }
            temp = temp.next; //temp后移，遍历
        }
        //判断flag
        if (flag){
            temp.pre.next = temp.next;
            if (temp.next != null){//保证删除的节点不是最后一个节点，否者将出现空指针异常
                temp.next.pre = temp.pre;
            }
        }else{
            System.out.printf("要删除的 %d 节点不存在/n",no);
        }


    }
}
//定义HeroNode02，每个HeroNode02对应一个节点
class HeroNode02{
    public int no;
    public String name;
    public String nickname;
    public HeroNode02 next;//指向下一个节点 默认为null
    public HeroNode02 pre;//指向前一个节点  默认为null
    //构造器
    public HeroNode02(int no,String name,String nickname){
        this.no=no;this.name=name;this.nickname=nickname;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                ", nickname='" + nickname + '\''+'}';
    }
}


```

## **单向链表和双向链表比较**

### **单：**

**	只有一个指向下一个节点的指针**  

​       **优点：增加删除节点简单，遍历时不会死循环**

​       **缺点：只能从头到尾遍历。只能找到后继，无法找到前驱，也就是只能前进**

### **双：**

**	有两个指针，一个指向前一个节点，一个指向后一个节点**

​       **优点：可以找到前驱和后继，可进可退**

​       **缺点：增加删除节点复杂，需要多分配一个指针存储空间**

------

## **单向环形链表实现约瑟夫问题：**

### **约瑟夫问题分析**

![image-20200528140116277](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528140116277.png)

### **创建环形链表思路分析图：**

![image-20200528140139893](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528140139893.png)

### **代码实现：**

```java
public class Josepfu {
    public static void main(String[] args) {
        circleSingleLinkedList list = new circleSingleLinkedList();
        list.add(10);
        list.showBoy();
    }
}
//创建单向环形链表
class circleSingleLinkedList{
        //创建一个first节点，当前没有编号
        private Boy first = null;

        //添加节点，构成一个环形链表
        public void add(int nums){//传入需要添加节点的个数
            //需要先对nums做校验
            if (nums <1){
                System.out.println("nums的值不正确");
                return;
            }
            Boy curBoy = null;//辅助指针，帮助遍历循环
            //使用for循环来创建环形链表
            for (int i = 1; i <= nums; i++) {
                //根据编号，创建节点
                Boy boy = new Boy(i);
                //如果是第一个节点
                if (i == 1){
                    first = boy;
                    first.setNext(first);//构成换
                    curBoy = first;//让curBoy指向第一个节点
                }else{
                    curBoy.setNext(boy);
                    boy.setNext(first);
                    curBoy = boy;
                }
            }
        }

        //遍历环形链表
        public void showBoy(){
            //判断链表是否为空
            if (first == null){
                System.out.println("链表为空");
                return;
            }
            Boy curBoy = first;
            while (true){
                System.out.printf("节点的编号%d\n",curBoy.getNo());
                if (curBoy.getNext() == first){//已经遍历完成
                    break;
                }
                curBoy = curBoy.getNext();
            }
        }

}
//创建一个Boy类，表示一个节点
class Boy{
    private int no;//编号
    private Boy next;//指向下一个节点，默认为空

    public Boy(int no)  { this.no = no;}
    public int getNo()  {  return no;}
    public void setNo(int no) {   this.no = no; }
    public Boy getNext()   { return next; }
    public void setNext(Boy next)   {this.next = next;}
}

```

### **环形链表出圈示意图：**

![image-20200528140318147](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528140318147.png)

### 代码实现：

```java
/**
 * 根据用户输入计算出节点出圈的顺序
 * @param startNo    表示从第几个节点开始数数
 * @param countNum   表示数几下
 * @param nums       表示最初有几个小孩在圈中
 */
    public void countBoy(int startNo,int countNum,int nums){
        if (startNo <1 || startNo > nums || first == null){
            System.out.println("输入的参数有误，请重新输入");
            return;
        }
        //创建一个辅助指针
        Boy temp = first;
        //将创建的辅助指针指向环形链表的最后
        while (true){
            if (temp.getNext() == first){//说明temp指向最后
                break;
            }
            temp = temp.getNext();
        }
        //报数前，让first和temp指针先移动 startNo - 1次
        for (int i = 0; i < startNo - 1; i++) {
            first = first.getNext();
            temp = temp.getNext();
        }
        //报数时，让first和temp指针同时移动 countNum - 1次然后出圈，知      道圈中只剩一个节点    
        while (true){
            if (temp == first){//只剩一个节点
                break;
            }
            //让first 和 temp 同时移动countNum - 1
            for (int i = 0; i < countNum - 1; i++) {
                first = first.getNext();
                temp = temp.getNext();
            }
            //这时first指向的节点就时要出圈的节点
            System.out.printf("节点编号%d出圈\n",first.getNo());
            //这时将first指向的节点出圈
            first = first.getNext();
            temp.setNext(first);
        }
        System.out.printf("最后留在圈中节点编号为%d\n",temp.getNo());
    }


```

------

# 栈

### **栈的介绍**：

**1、** **栈是一个****先入后出****的有序链表；**

**2、** **栈是限制性表中元素的插入和删除****只能在线性表的同一端****进行的一种特殊线性表。允许插入和删除的一端，为变化的一端，称为****栈顶****；另一端为固定的一端，称为****栈底****；**

**3、** **根据定义知：最先放入栈的在栈底，最后放入的在栈顶；而删除时则相反，最后放入的最先删除，最先放入的最后删除；**

### **出栈(pop)和入栈(push)图解：**

![image-20200528142954381](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528142954381.png)

## **数组模拟栈思路分析：**

![image-20200528143126293](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528143126293.png)

### **代码实现：**

```java
import java.util.Scanner;

/**
 * 数组模拟栈
 */
public class ArrayStackDemo {
    public static void main(String[] args) {
        ArrayStack stack = new ArrayStack(4);
        String key = "";
        boolean loop = true;
        Scanner sc = new Scanner(System.in);
        while (loop){
            System.out.println("show:表示显示栈");
            System.out.println("exit:退出栈");
            System.out.println("push:表示加入数据到栈中");
            System.out.println("pop:表示从栈中取出数据");
            System.out.println("请输入你的选择");
            key = sc.next();
            switch (key){
                case "show":
                    stack.list();
                    break;
                case "exit":
                    sc.close();
                    loop = false;
                    break;
                case "push":
                    System.out.println("请输入你要添加的值");
                    int value = sc.nextInt();
                    stack.push(value);
                    break;
                case "pop":
                    try {
                        int res = stack.pop();
                        System.out.printf("输出的数据是%d\n",res);
                    }catch (Exception e){
                        System.out.println(e.getMessage());
                    }

                    break;
            }
        }
        System.out.println("程序退出");
    }
}

//定义一个 ArrayStack 表示栈
class ArrayStack{
    private int maxsize;//栈的最大值
    private int [] stack;//数组  数据存放在该数组
    private int top = -1;//栈顶 初始值为-1

    public ArrayStack(int maxsize){
        this.maxsize = maxsize;
        stack = new int [this.maxsize];
    }

    //判断栈是否满
    public boolean isFull(){
        return top == maxsize-1;
    }

    //判断栈是否空
    public boolean isEmpty(){
        return top == -1;
    }

    //入栈push
    public  void push(int value){
        if (isFull()){
            System.out.println("栈满");
            return;
        }
        top++;
        stack[top] = value;
    }

    //出栈pop
    public int pop(){
        if (isEmpty()){
            throw new RuntimeException("栈空，没有数据");
        }
        int value = stack[top];
        top--;
        return value;
    }

    //显示栈
    public void list(){
        if (isEmpty()){
            System.out.println("栈空，没有数据");
            return;
        }
//需要从栈顶开始显示数据
        for (int i = top; i >=0 ; i--) {
            System.out.printf("stack[%d] = %d\n",i,stack[i]);
        }
    }
}

```

## #**栈的应用：(使用栈完成表达式的计算)**

![image-20200528143305734](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528143305734.png)

### **代码实现：**

**Bug****：无法存入两位数字**

```java
public class Calculator {
    public static void main(String[] args) {
        String expression = "3+2*6-2";
        //创建连个栈
        ArrayStack1 numStack = new ArrayStack1(10);//数栈
        ArrayStack1 operStack = new ArrayStack1(10);//符号栈
        //定义相关变量
        int index=0;//用于扫描
        int num1=0;
        int num2=0;
        int oper=0;
        int res=0;
        char ch = ' ';//将每次扫描得到的char存到ch中
        //开始while循环扫描expression
        while (true){
            ch = expression.substring(index,index+1).charAt(0);//依次得到每一个字符
            if (operStack.isOper(ch)){//是运算符
                if(!operStack.isEmpty()){//判断当前符号栈是否为空
/** 如果符号栈中有符号则进行比较，
*   如果当前操作符的优先级小于或等于栈中的操作符，
*   就需要从数栈中pop出两个数，符号栈中pop出一个符号，
*   进行运算，得到结果，入数栈，当前符号入符号栈
                     */
                    if (operStack.priority(ch) <= operStack.priority(operStack.peek())){
                        num1 = numStack.pop();
                        num2 = numStack.pop();
                        oper = operStack.pop();
                        res = numStack.cal(num1,num2,oper);
                        numStack.push(res);//把运算结果存入数栈
                        operStack.push(ch);//把运算符号存入符号栈
                    }else {
                        operStack.push(ch);//如果当前操作符的优先级大于栈中的操作符,直接入栈
                    }
                }else{//如果为空直接入符号栈
                        operStack.push(ch);
                }
            }
//无法扫描多位数字
//------------------------------------------------------
else{//如果为数字，直接入栈
                numStack.push(ch - 48);
            }
//------------------------------------------------------

//------------------------------------------------------
/**  当处理多位数时，需要向expression表达式的后面在看一位，
 *   如果数字就扫描，是符号则入栈。因此需要定义一个变量用于拼接
 */
else {
    keepNum  += ch;
    if (index == expression.length()-1){
        numStack.push(Integer.parseInt(keepNum));
    }else if (operStack.isOper(expression.substring(index+1,index+2).charAt(0))){
        numStack.push(Integer.parseInt(keepNum));
        keepNum="";//必循将keepNum清空
    }
}
//------------------------------------------------------
            index++;//让index++，判断是否到expression最后
            if (index >= expression.length()){
                break;
            }
        }

        //当扫描完后，就顺序从 数栈和符号栈pop出数据进行计算
        while (true){
            //如果符号栈为空，则计算到最后的结果，此时数栈只有一个数字就是结果
            if (operStack.isEmpty()){
                break;
            }
            num1 = numStack.pop();
            num2 = numStack.pop();
            oper = operStack.pop();
            res = numStack.cal(num1,num2,oper);
            numStack.push(res);//把运算结果存入数栈
        }
        int res2 = numStack.pop();
        System.out.printf("表达式%s=%d",expression,res2);
    }
}

//定义一个 ArrayStack 表示栈
class ArrayStack1{
    private int maxsize;//栈的最大值
    private int [] stack;//数组  数据存放在该数组
    private int top = -1;//栈顶 初始值为-1

    public ArrayStack1(int maxsize){
        this.maxsize = maxsize;
        stack = new int [this.maxsize];
    }

    //判断栈是否满
    public boolean isFull(){
        return top == maxsize-1;
    }

    //判断栈是否空
    public boolean isEmpty(){
        return top == -1;
    }

    //入栈push
    public  void push(int value){
        if (isFull()){
            System.out.println("栈满");
            return;
        }
        top++;
        stack[top] = value;
    }

    //出栈pop
    public int pop(){
        if (isEmpty()){
            throw new RuntimeException("栈空，没有数据");
        }
        int value = stack[top];
        top--;
        return value;
    }

    //显示栈
    public void list(){
        if (isEmpty()){
            System.out.println("栈空，没有数据");
            return;
        }
        //需要从栈顶开始显示数据
        for (int i = top; i >=0 ; i--) {
            System.out.printf("stack[%d] = %d\n",i,stack[i]);
        }
    }

    //返回栈顶的值，但不是出栈
    public int peek(){
        return stack[top];
    }
    /**
     * 返回运算符的优先级，自己来确定，优先级使用数字表示
     * 数字越大，优先级越高
     * 目前表达式只有 +，-，*，/
     */
     public int priority(int oper){
        if (oper == '*' || oper == '/'){
            return 1;
        } else if (oper == '+' || oper == '-') {
            return 0;
        }else {
            return -1;
        }
     }

     //判断是否是运算符
    public boolean isOper(char val){
         return val=='+' || val=='-' || val=='*' || val=='/';
    }

    //计算方法
    public int cal(int num1,int num2,int oper){
        int res = 0;//存放结果
        switch (oper){
            case '+':
                res = num1 + num2;
                break;
            case '-':
                res = num2 - num1;
                break;
            case '*':
                res = num1 * num2;
                break;
            case '/':
                res = num2 / num1;
                break;
        }
        return res;
    }
}

```

## **前缀，中缀，后缀表达式(逆波兰表达式)**

### **前缀表达式(波兰表达式)：**

**1、**    **前缀表达式又称波兰式，前缀表达式的运算符位于操作数之前**

**2、**    **举例：(3+4)x5-6对应的前缀表达式为： - x + 3 4 5 6**

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528143454173.png" alt="image-20200528143454173" style="zoom:150%;" />

### **中缀表达式：**

**1、**    **中缀表达式就是最常见的运算表达式：(3+4)x5-6;**

**2、**    **计算机在计算中缀表达式时往往会将中缀表达式转   成其他表达式来操作(一般转为后缀表达式)**

### **后缀表达式(逆波兰表达式)：**

**1、**    **后缀表达式又称为逆波兰式，运算符位于数字后面**

**2、**    **举例：(3+4)x5-6 对应的后缀表达式为：3 4 + 5 x 6 –**

**3、**    **再比如：**![image-20200528143558336](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528143558336.png)

![image-20200528143613686](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528143613686.png)

## **逆波兰计算器实现思路：**

![image-20200528143640125](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200528143640125.png)

### **代码实现：**

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class PolandNotation {
    public static void main(String[] args) {
        //先定义一个逆波兰表达式
        //(3+4)x5-6 对应的后缀表达式为：3 4 + 5 x 6 –(为了方便数字和符号用空格隔开)
//        String suffixExpression = "30 4 + 5 * 6 -";
        String suffixExpression = "4 5 * 8 - 60 + 8 2 / +";
        /** 思路：
         *  1、先将“3 4 + 5 x 6 –” 放到ArrayList中
         *  2、将ArrayList 传递给一个方法，遍历ArrayList 配合栈 完成计算
         */

        List<String> list = getStringList(suffixExpression);
        System.out.println(list);
        int res = calcuLate(list);
        System.out.println("结果为："+res);
    }

    //将一个逆波兰表达式，一次将数字和符号存入到Array List中
    public static List<String> getStringList(String suffixExpression){
        String [] split = suffixExpression.split(" ");
        List<String> list  = new ArrayList<String>();
        for (String ele : split){
            list.add(ele);
        }
        return list;
    }

    //完成计算
    public static int calcuLate(List<String> ls){
        //创建栈，只需要一个即可
        Stack<String> stack = new Stack<String>();
        //遍历ls
        for (String item : ls){
            //使用正则表达式来取出数
            if (item.matches("\\d+")) {//匹配是多位数
                stack.push(item);//入栈
            }else {
                //pop出两个数进行运算之后再入栈
                int num2 = Integer.parseInt(stack.pop());
                int num1 = Integer.parseInt(stack.pop());
                int res = 0;
                if (item.equals("+")) res = num1 + num2;
                else if (item.equals("-")) res = num1 - num2;
                else if (item.equals("*")) res = num1 * num2;
                else if (item.equals("/")) res = num1 / num2;
                else throw new RuntimeException("运算符有误");
                stack.push(""+res);
            }
        }
        return Integer.parseInt(stack.pop());//
    }
}

```

## 中缀转后缀表达式思路分析：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200529111925284.png" alt="image-20200529111925284" style="zoom:200%;" />

### 代码实现：

```java
主程序：
public static void main(String[] args) {
        /** 完成将一个中缀表达式转后缀表达式得功能
         *  说明 ： 1、1 + ( ( 2 + 3 ) * 4 - 5 ) =》 转成 1 2 3 + 4 * + 5 -
         *          2、因为直接操作str不方便，需要将“1 + ( ( 2 + 3 ) * 4 - 5 )” =》 中缀表达式对应的List
         *          即，“1 + ( ( 2 + 3 ) * 4 - 5 )” =》 ArrayList [1,+,(,(,2,+,3,),*,4,-,5,)]
         *          3、将得到的中缀表达式的List =》 后缀表达式对应的List
         *          即，ArrayList[1, +, (, (, 2, +, 3, ), *, 4, -, 5, )] =》 ArrayList[1 , 2 , 3 , + , 4 ,* , + ,5 , -]
         */
        String expression = "1+((2+3)*4)-5";
        List<String> ls = toInfixExpressionList(expression);
        System.out.println("中缀表达式为="+ls);
        List<String> list = parseSuffExpressionList(ls);
        System.out.println("后缀表达式为="+list);//ArrayList[1 , 2 , 3 , + , 4 ,* , + ,5 , -]
        System.out.println(calcuLate(list));
}
//---------------------------------------------------------------------------------------
方法：
    /**
     * 将 中缀表达式转成对应得list s = " 1 + ( ( 2 + 3 ) * 4 - 5 )"
     * @param s
     * @return
     */
    public static List<String> toInfixExpressionList(String s){
        //定义一个List，存放中缀表达式 对应得内容
        List<String> ls = new ArrayList<String>();
        int i = 0;//辅助指针，用于遍历 中缀表达式字符串
        String str;//对多位数得拼接
        char c;//每遍历到一个字符，就放到c中
        do{
            //如果是一个数字，加入到ls中
            if ((c = s.charAt(i))<48 || (c = s.charAt(i)) > 57) {
                ls.add(""+c);
                i++;//需要后移，遍历完s
            }else{//如果是一个数，考虑多位数
                str="";
                while (i<s.length() && (c=s.charAt(i)) >=48 && (c=s.charAt(i)) <=57){
                    str +=c;//拼接
                    i++;
                }
                ls.add(str);
            }

        }while (i<s.length());
            return ls;
    }
//--------------------------
    /**
     *  ArrayList[1, +, (, (, 2, +, 3, ), *, 4, -, 5, )] =》 ArrayList[1 , 2 , 3 , + , 4 ,* , + ,5 , -]
     *  将得到的中缀表达式的List =》 后缀表达式对应的List
     * @param ls
     * @return
     */
    public static List<String> parseSuffExpressionList(List<String> ls) {
        //定义两个栈
        Stack<String> s1 = new Stack<String>();//符号栈
        //因为s2这个栈，在整个过程中没有pop操作，而且还需要逆序输出比较麻烦，则用List<String>代替
        List<String> s2 = new ArrayList<String>();//储存中间结果的List2
        //遍历ls
        for (String item : ls) {
            if (item.matches("\\d+")) {//通过正则表达式判断是否为数字
                s2.add(item);
            } else if (item.equals("(")) {
                s1.push(item);
            } else if (item.equals(")")) {
                //如果为右括号‘）’，依次弹出s1栈顶的运算符，并压入s2，直到遇到‘（’为止，将一对括号丢弃
                while (!s1.peek().equals("(")) {
                    s2.add(s1.pop());
                }
                s1.pop();//将‘（’弹出s1栈，消除小括号
            } else {
                //当item的优先级小于等于s1栈的运算符，将s1栈顶的运算符弹出并加入到s2中，再次与s1中新的栈顶运算符比较
  while (s1.size() != 0 && Operation.getValue(s1.peek()) >= Operation.getValue(item)) {
      //栈顶优先级大于item
                    s2.add(s1.pop());
                }
                s1.push(item);
            }
        }
        //将s1中剩余的运算符依次弹出并加入到s2中
        while (s1.size() != 0 ){
            s2.add(s1.pop());
        }
        return s2;//因为存放到的是List，因此按顺序输出就是对应的后缀表达式对应的List
    }
//---------------------------------------------------------------------------------------
辅助类：
    //编写一个类，用于返回y运算符对应的优先级
class Operation{
    private static int ADD=1;
    private static int SUB=1;
    private static int MUL=2;
    private static int DIV=3;

    //编写一个方法，返回对应的优先级数字
    public static int getValue(String operation){
        int res = 0;
        switch (operation){
            case "+":
                res = ADD;
                break;
            case "-":
                res = SUB;
                break;
            case "*":
                res = MUL;
                break;
            case "/":
                res = DIV;
                break;
            default:
                System.out.println("没有该运算符");
                break;
        }
        return res;
    }
}

```

------

# 递归

### 递归的概念：

递归就是方法自己调用自己，每次调用时传入不同的变量，**递归有助于编程者解决复炸的问题**，同时使代码变的简洁

### 递归调用机制示意图：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200529152142657.png" alt="image-20200529152142657" style="zoom:200%;" />

### 递归遵守的重要规则：

1、执行一个方法时，就创建一个新的受保护的独立空间(栈空间)

2、方法的局部变量是独立的，不会相互影响

3、如果方法中使用的是引用变量(比如数组)，就会共享该引用类型的数据

4、递归必**须向退出递归的条件逼近**，否则就是无限递归，出现StackOverflowError，死归了；

5、当一方法执行完毕，或者遇到return，就会返回，遵守谁调用就将结果返回给谁，同时当方法执行完毕或			  者返回时，该方法也就执行完毕了

## #迷宫回溯问题：

![image-20200605110730814](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200605110730814.png )

### 代码实现：

走的策略不同，小球的路径也不同。

```java
public class MiGong {
    public static void main(String[] args) {
        //创建一个二维数组，模拟迷宫
        int [][] map = new int [8][7];
        //使用1 代表墙
        //将上下全部置为1
        for (int i = 0; i < 7; i++) {
            map[0][i]=1;
            map[7][i]=1;
        }
        //将左右全部置为1
        for (int i = 0; i < 8; i++) {
            map[i][0]=1;
            map[i][6]=1;
        }
        map[3][1]=1;
        map[3][2]=1;


        //输出地图
        for (int i = 0; i < 8; i++) {
            for (int j = 0;j < 7; j++){
                System.out.print(map[i][j]+" ");
            }
            System.out.println();
        }

//        setWay(map,1,1);
        setWay2(map,1,1);
        System.out.println("找到出路后的地图");
        for (int i = 0; i < 8; i++) {
            for (int j = 0;j < 7; j++){
                System.out.print(map[i][j]+" ");
            }
            System.out.println();
        }
    }

    //使用递归回溯来给小球找路

    /** 说明：
     *  1、map表示地图
     *  2、i，j表示从地图的哪个位置开始出发
     *  3、如果小球能找到map[6][5] 的位置，说明找到出路
     *  4、约定：当map[i][j] 为0时表示该点没有走过；为1时表示墙；
     *      为2时表示通路可以走；为3表示该点走过，但是不通
     *  5、在走迷宫时，需要定一个策略(方法)，下->右->上->左,如果该点走不通，在回溯
     * @param map 表示地图
     * @param i   表示从哪个位置开始找
     * @param j
     * @return  如果找到通路返回true，没有则返回false；
     */
    public static boolean setWay(int [][] map ,int i ,int j){
        if (map[6][5]==2) {//已经找到通路
            return true;
        }else{
            if (map[i][j] == 0){//如果当前这个点还没有走过
                //按照策略 下->右->上->左
                map[i][j]=2;//假定该点可以走通
                    if (setWay(map,i+1,j)) {//向下走
                    return true;
                }else if(setWay(map,i,j+1)){//向右走
                    return true;
                }else if(setWay(map,i-1,j)){//向上走
                    return true;
                }else if(setWay(map,i,j-1)){//向左走
                    return true;
                }else{
                    map[i][j]=3;//说明该点是死了路，走不通
                    return false;
                    }
            }else{//如果map[i][j] ！= 0，可能为1，2，3
                return false;
            }
        }
    }

    public static boolean setWay2(int [][] map ,int i ,int j){
        if (map[6][5]==2) {//已经找到通路
            return true;
        }else{
            if (map[i][j] == 0){//如果当前这个点还没有走过
                //按照策略 上->右->下->左
                map[i][j]=2;//假定该点可以走通
                if (setWay2(map,i-1,j)) {//向上走
                    return true;
                }else if(setWay2(map,i,j+1)){//向右走
                    return true;
                }else if(setWay2(map,i+1,j)){//向下走
                    return true;
                }else if(setWay2(map,i,j-1)){//向左走
                    return true;
                }else{
                    map[i][j]=3;//说明该点是死了路，走不通
                    return false;
                }
            }else{//如果map[i][j] ！= 0，可能为1，2，3
                return false;
            }
        }
    }
}

```

## #八皇后问题(回溯算法)：

​    ![image-20200605111143758](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200605111143758.png)

### 思路分析：

![image-20200605112431409](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200605112431409.png)

![image-20200605112509990](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200605112509990.png)

### 代码实现：

```java
public class Queue8 {
    int max = 8;//定义一个max表示共有多少个皇后
    static int count=0;
    static int Judgecount=0;
    int [] array= new int[max];//定义数组array，保存皇后放置的位置的结果 比如: array={0,4,7,5,2,6,1,3};
    public static void main(String[] args) {
        Queue8 queue8 = new Queue8();
        queue8.check(0);
        System.out.printf("一共有%d种解法",count);
        System.out.printf("一共有%d次冲突",Judgecount);
    }

    //编写一个方法，放置第n个皇后
    private void check(int n){
        if(n==max){
            print();
            return;
        }
        //依次放入皇后，并判断是否冲突
        for (int i = 0; i < max; i++) {
            array[n]=i;//先将当前皇后n ， 放置在改行的第一列
            if (judge(n)){//不冲突，则继续下一个（n + 1）皇后
                check(n+1);
            }
            //如果冲突，就会继续执行array[n] = i;即将第n个皇后，放置在本行的 后移 一个位置
        }
    }

    /**
     *  查看当我们放置第n个皇后，就检测该皇后是否和前面已经摆放的皇后冲突
     * @param n  表示第n个皇后
     * @return
     */
    private boolean judge(int n){
        Judgecount++;
        //没有必要判断是否在同一行，n每次都在递增
        for (int i = 0; i < n; i++) {
            //array[i]==array[n]  判断第n个皇后是否和前面一个皇后在同一列
            //Math.abs(n-i)==Math.abs(array[n] - array[i]  判断第n个皇后是否和前面一个皇后在同一斜线
            if (array[i]==array[n] || Math.abs(n-i)==Math.abs(array[n] - array[i])) {
                return false;
            }
        }
        return true;
    }

    //将皇后摆放的位置输出
    private void print(){
        count++;
        for (int i = 0; i < array.length; i++) {
            System.out.print(array[i] + " ");
        }
        System.out.println();
    }
}

```

# 排序算法(排序)

https://www.cnblogs.com/onepixel/articles/7674659.html

## 介绍：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608102026569.png" alt="image-20200608102026569" style="zoom:200%;" />

## 算法的时间复杂度

### 时间频度：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608104320062.png" alt="image-20200608104320062" style="zoom:200%;" />



![image-20200608104744632](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608104744632.png)



![image-20200608104939427](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608104939427.png)



 <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608105240657.png" alt="image-20200608105240657" style="zoom:200%;" />



### 时间复杂度：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608105851777.png" alt="image-20200608105851777" style="zoom:200%;" />



<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608110249850.png" alt="image-20200608110249850" style="zoom:200%;" />



### 平均时间复杂度和最坏时间复杂度

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608111453122.png" alt="image-20200608111453122" style="zoom:200%;" />



## 算法的空间复杂度

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608111838407.png" alt="image-20200608111838407" style="zoom:200%;" />



------

## #冒泡排序

### 基本介绍：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608112112111.png" alt="image-20200608112112111" style="zoom:200%;" />

### 思路图解：

![image-20200608113028539](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200608113028539.png)

### 代码实现：

```java
/**
 * 冒泡排序
 *
 *
 */
public class BubbleSort {
    public static void main(String[] args) {
        //冒泡排序，时间复杂度O(n^2)
        int [] arr = {3,9,-1,10,-2};
        int temp=0;//临时变量存放数据
        boolean flag = false;//表示变量，判断是否进行过交换
        System.out.printf("原始数组");
        System.out.println(Arrays.toString(arr));
        for (int i = 0; i < arr.length - 1; i++) {
            for (int j = 0; j <arr.length -1 -i ;j++){
                if (arr[j] >arr[j+1]) {
                    flag = true;
                    temp = arr[j];
                    arr[j] = arr[j+1];
                    arr[j+1] = temp;
                }
            }
            System.out.printf("第"+(i+1)+"趟排序后的数组");
            System.out.println(Arrays.toString(arr));
            if (!flag){//在一趟排序中，一次都没有发生交换
                break;
            }else{
                flag = false;//重置flag，进行下一次判断
            }
        }

    }
}

```

### 时间比较：

```java
public static void testtime(){
        int [] arr = new int [80000];
        for (int i = 0; i < 80000; i++) {
            arr[i] = (int) (Math.random() * 80000);
        }
        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String str1 = simpleDateFormat.format(date1);
        System.out.println("排序前的时间是："+date1);

        bubbleSort(arr);

        Date date2 = new Date();
        String str2 = simpleDateFormat.format(date2);
        System.out.println("排序前的时间是："+date2);
    }

```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200609101556680.png" alt="image-20200609101556680" style="zoom:200%;" />

------

## #选择排序

### 思路图解：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200609151032544.png" alt="image-20200609151032544" style="zoom:200%;" />

### 代码实现：

```java
package com.cyq.Sort;
/**
 *
 * 选择排序
 *
 */

import java.util.Arrays;

public class SelectSort {
    public static void main(String[] args) {
        int [] arr = {101,34,119,1};
        selectSort(arr);

        System.out.println(Arrays.toString(arr));
    }
    public static void selectSort(int [] arr){
        for (int i = 0; i < arr.length - 1; i++) {
            int minarr = arr[i];
            int minindex = i;
            for (int j = i + 1; j < arr.length; j++) {
                if (minarr>arr[j]) {
                    minarr = arr[j];
                    minindex = j;
                }
            }
            if (minindex != i){
                arr[minindex] = arr[i];
                arr[i] = minarr;
            }
            }
    }
}

```

### 时间比较：

```java
public static void testTime(){
        int [] arr = new int [80000];
        for (int i = 0; i < 80000; i++) {
            arr[i] = (int) (Math.random() * 80000);
        }
        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String str1 = simpleDateFormat.format(date1);
        System.out.println("排序前的时间是："+date1);

        selectSort(arr);

        Date date2 = new Date();
        String str2 = simpleDateFormat.format(date2);
        System.out.println("排序后的时间是："+date2);

    }

```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200610141424123.png" alt="image-20200610141424123" style="zoom:150%;" />

## #插入排序：

### 思路图解：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200610142141754.png" alt="image-20200610142141754" style="zoom:200%;" />

### 代码实现：

```java
package com.cyq.Sort;

import java.util.Arrays;

public class InsertSort {
    public static void main(String[] args) {
        int [] arr = {101,34,119,1,-1,20,30};
        
        insertSort1(arr);
    }

    /**
     * 分步实现
     * @param arr
     */
    public static void insertSort(int [] arr){

        //定义待插入的数
		//第一步
        int insertVal = arr[1];
        int insertindex = 1-1;//insertVal前一个数的下标
        /**
         *  insertindex >= 0 保证插入的数字不越界
         *  insertVal < arr[insertindex] 待插入的数还没有找到要插入的位置
         *  需要将arr[insertindex]后移
         */
        while (insertindex >= 0 && insertVal < arr[insertindex] ) {
            arr[insertindex + 1] = arr[insertindex];
            insertindex--;
        }
        //当退出while循环时，说明插入的位置找到，insertindex + 1
        arr[insertindex+1] = insertVal;

        System.out.println("第一轮");
        System.out.println(Arrays.toString(arr));

        //第二步
        insertVal = arr[2];
        insertindex = 2-1;//insertVal前一个数的下标
        while (insertindex >= 0 && insertVal < arr[insertindex] ) {
            arr[insertindex + 1] = arr[insertindex];
            insertindex--;
        }
        //当退出while循环时，说明插入的位置找到，insertindex + 1
        arr[insertindex+1] = insertVal;

        System.out.println("第二轮");
        System.out.println(Arrays.toString(arr));

        //第三步
        insertVal = arr[3];
        insertindex = 3-1;//insertVal前一个数的下标
        while (insertindex >= 0 && insertVal < arr[insertindex] ) {
            arr[insertindex + 1] = arr[insertindex];
            insertindex--;
        }
        //当退出while循环时，说明插入的位置找到，insertindex + 1
        arr[insertindex+1] = insertVal;

        System.out.println("第三轮");
        System.out.println(Arrays.toString(arr));
    }


    //小 -> 大
    public static void insertSort1(int [] arr){
        for (int i = 1; i < arr.length; i++) {
            int insertVal = arr[i];
            int insertindex = i - 1;
            while (insertindex >= 0 && insertVal < arr[insertindex]) {
                arr[insertindex + 1] = arr[insertindex];
                insertindex--;
            }
            //优化
            if (insertindex + 1 != i){
                arr[insertindex+1] = insertVal;
            }
//            System.out.println("第"+i+"轮");
//            System.out.println(Arrays.toString(arr));
        }
    }

    //大 -> 小
    public static void insertSort2(int [] arr){
        for (int i = 1; i < arr.length; i++) {
            int insertVal = arr[i];
            int insertindex = i - 1;
            while (insertindex >= 0 && insertVal > arr[insertindex]) {
                arr[insertindex + 1] = arr[insertindex];
                insertindex--;
            }
            //优化
            if (insertindex + 1 != i){
                arr[insertindex+1] = insertVal;
            }
//            System.out.println("第"+i+"轮");
//            System.out.println(Arrays.toString(arr));
        }
    }
}


```

### 时间比较：

```java
public static void main(String[] args) {
//        int [] arr = {101,34,119,1,-1,20,30};
        int [] arr = new int [80000];
        for (int i = 0; i < 80000; i++) {
            arr[i] = (int) (Math.random() * 80000);
        }
        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String str1 = simpleDateFormat.format(date1);
        System.out.println("排序前的时间是："+date1);

        insertSort1(arr);

        Date date2 = new Date();
        String str2 = simpleDateFormat.format(date2);
        System.out.println("排序前的时间是："+date2);
    }
public static void insertSort1(int [] arr){
        for (int i = 1; i < arr.length; i++) {
            int insertVal = arr[i];
            int insertindex = i - 1;
            while (insertindex >= 0 && insertVal < arr[insertindex]) {
                arr[insertindex + 1] = arr[insertindex];
                insertindex--;
            }
            arr[insertindex+1] = insertVal;
//            System.out.println("第"+i+"轮");
//            System.out.println(Arrays.toString(arr));
        }
    }

```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200610152946606.png" alt="image-20200610152946606" style="zoom:150%;" />

### 存在的问题：	

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200612141132177.png" alt="image-20200612141132177" style="zoom:150%;" />

## #希尔排序：

### 示意图：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200612141655769.png" alt="image-20200612141655769" style="zoom:150%;" />

### 应用实例：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200612142018988.png" alt="image-20200612142018988" style="zoom:150%;" />

## #希尔排序----交换法：

### 代码实现：

```java
//对有序序列在插入时采用交换法
	public class ShellSort {
    public static void main(String[] args) {
        int [] arr = {8,9,1,7,2,3,5,4,6,0};

//        shellSort(arr);

        shellSort1(arr);
    }

    //逐步推导编写希尔排序
    //插入时交换的方法
    public static void shellSort(int [] arr){
        int temp =0;
        //希尔排序的第一轮
        //因为希尔排序第一轮将10个数据分成了5组 (arr.length/2)
        for (int i = 5; i < arr.length; i++) {
            //遍历各组中所有的元素(共五组，每组两个元素)，步长5
            for (int j = i - 5;j>=0;j -= 5){
                if (arr[j] > arr[j+5]){
                    //如果当前元素大于加上步长后的那个元素，说明需要交换
                    temp = arr[j];
                    arr[j]=arr[j+5];
                    arr[j+5]=temp;
                }
            }
        }
        System.out.println("希尔排序第一轮："+ Arrays.toString(arr));

        for (int i = 2; i < arr.length; i++) {
            for (int j = i -2;j>=0;j -=2){
                if (arr[j] > arr[j+2]){
                    temp = arr[j];
                    arr[j]=arr[j+2];
                    arr[j+2]=temp;
                }
            }
        }
        System.out.println("希尔排序第二轮："+ Arrays.toString(arr));

        for (int i = 1; i < arr.length; i++) {
            for (int j = i -1;j>=0;j -=1){
                if (arr[j] > arr[j+1]){
                    temp = arr[j];
                    arr[j]=arr[j+1];
                    arr[j+1]=temp;
                }
            }
        }
        System.out.println("希尔排序第三轮："+ Arrays.toString(arr));
    }

    //插入时交换的方法
    public static void shellSort1(int [] arr){
        int temp = 0;
        for (int gap = arr.length / 2;gap>0;gap /=2){
            for (int i = gap; i < arr.length; i++) {
                for (int j = i -gap;j>=0;j -=gap){
                    if (arr[j] > arr[j+gap]){
                        //如果当前元素大于加上步长后的那个元素，说明需要交换
                        temp = arr[j];
                        arr[j]=arr[j+gap];
                        arr[j+gap]=temp;
                    }
                }
            }
        }
        System.out.println(Arrays.toString(arr));
    }
}

```

### 时间比较：

```java
public static void testTime(){
        int [] arr = new int [80000];
        for (int i = 0; i < 80000; i++) {
            arr[i] = (int) (Math.random() * 80000);
        }
        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String str1 = simpleDateFormat.format(date1);
        System.out.println("排序前的时间是："+date1);

        shellSort1(arr);

        Date date2 = new Date();
        String str2 = simpleDateFormat.format(date2);
        System.out.println("排序前的时间是："+date2);
    }

```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200612145646222.png" alt="image-20200612145646222" style="zoom:150%;" />

### 弊端：

发现一组数据再进行交换，时间变得更加长

## #希尔排序----移位法：

### 代码实现：

```java
//对交换式排序的希尔排序进行优化-->移位法
    public static void shellSort2(int [] arr){
        //增量gap，并逐步减小增量
        for (int gap = arr.length;gap > 0;gap /=2){
            //从第gap个元素，逐个对其所在组进行直接插入排序
            for (int i = gap; i < arr.length; i++) {
                int j =i;
                int temp = arr[i];
                if (arr[j] < arr[j-gap]) {
                    while (j - gap >=0 && temp < arr[j-gap]) {
                        //移动
                        arr[j]=arr[j-gap];
                        j -= gap;
                    }
                    //当退出while循环，就给temp找到插入的位置
                    arr[j]=temp;
                }
            }
        }
    }

```

### 时间比较：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200612152832009.png" alt="image-20200612152832009" style="zoom:150%;" />

## #快速排序：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200612153337924.png" alt="image-20200612153337924" style="zoom:150%;" />

### 思路分析：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200612154214686.png" alt="image-20200612154214686" style="zoom:150%;" />

### 算法描述：

**快速排序使用分治法来把一个串（list）分为两个子串（sub-lists）。具体算法描述如下：**

1. **从数列中挑出一个元素，称为 “基准”（pivot）；**
2. **重新排序数******列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；****
3. **递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。**

### 代码实现：

```java
import java.io.PipedOutputStream;
import java.text.SimpleDateFormat;
import java.util.Arrays;
import java.util.Date;
import java.util.PrimitiveIterator;

/**
 * 快速排序
 *
 */

public class QuickSort {
    public static void main(String[] args) {
        int [] arr = {-9,78,0,23,-567,70,-5,489};
        quickSort(arr,0,arr.length-1);
        System.out.println(Arrays.toString(arr));

//        testTime();
    }

    public static void quickSort(int [] arr,int left,int right){
        int l = left;
        int r = right;
        int pivot = arr[(left+right) / 2];//中轴值
        int temp = 0;//临时变量
        //while循环的目的是把比pivot小的值 放到pivot的左边
        while (l < r){
            //在pivot的左边一直找，找到大于等于pivot的值才退出
            while(arr[l] < pivot){
                l +=1;
            }
            //在pivot的右边一直找，找到小于等于pivot的值才退出
            while (arr[r] > pivot){
                r -=1;
            }
            /**
             *  如果 l >= r 说明pivot的左右值，已经按照
             *  左边全部小于等于pivot，右边全部大于等于pivot
             */
            if (l >= r){
                break;
            }
            //交换
            temp = arr[l];
            arr[l]=arr[r];
            arr[r]=temp;

            //如果交换完后 发现arr[l] == pivot的值  r--，前移
            if (arr[l] == pivot){
                r -= 1;
            }
            //如果交换完后 发现arr[r] == pivot的值  l++，后移
            if (arr[r] == pivot){
                l += 1;
            }
        }
        // 如果 l == r ，必须l++    r-- ,否则会出现栈溢出
        if (l == r){
            l +=1;
            r -=1;
        }
        //向左递归
        if(left < r){
            quickSort(arr,left,r);
        }
        //向右递归
        if(right > r){
            quickSort(arr,l,right);
        }
    }

//测试时间方法
    public static void testTime(){
        int [] arr = new int [80000];
        for (int i = 0; i < 80000; i++) {
            arr[i] = (int) (Math.random() * 80000);
        }
        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String str1 = simpleDateFormat.format(date1);
        System.out.println("排序前的时间是："+date1);

        quickSort(arr,0,arr.length-1);

        Date date2 = new Date();
        String str2 = simpleDateFormat.format(date2);
        System.out.println("排序前的时间是："+date2);
    }
}

```

### 时间比较：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200612162037824.png" alt="image-20200612162037824" style="zoom:150%;" />

## #归并排序：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200622144457954.png" alt="image-20200622144457954" style="zoom:150%;" />

### 思路分析：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200628143317878.png" alt="image-20200628143317878" style="zoom:200%;" />

### 代码实现：

```java
import java.text.SimpleDateFormat;
import java.util.Arrays;
import java.util.Date;

public class MergerSort {
    public static void main(String[] args) {
        int [] arr = {8,4,5,7,1,3,6,2};
        int [] temp = new int[arr.length];
        mergersort(arr,0,arr.length-1,temp);
        System.out.println(Arrays.toString(arr));
//        testTime();
    }

    //分+合的方法
    public static void mergersort(int [] arr,int left,int right,int [] temp){
        if (left <right){
            int mid = (left+right) / 2;//中间索引
            //向左递归进行分解
            mergersort(arr,left,mid,temp);
            //向有递归进行分解
            mergersort(arr,mid+1,right,temp);
            //合并
            merge(arr,left,mid,right,temp);
        }
    }
    /**
     *  合并的方法
     * @param arr   排序的原始数组
     * @param left  左边有数序列的初始索引
     * @param mid   中间索引
     * @param right 右边索引
     * @param temp  做中转的数组
     */
    public static void merge(int [] arr,int left,int mid,int right,int [] temp){
        int i = left;  //初始化i，左边有序数列的初始索引
        int j = mid+1; //初始化j，右边有序数列的初始索引
        int t = 0;     //指向temp数组的当前索引

        /**
         一、
            先把左右两边(有序)的数据按照规则填充到temp数组
            直到左右两边的有序序列，有一边处理完毕为止
         */
        while (i<=mid && j<=right){
            /**如果左边的有序序列的当前元素，小于等于右边有序序列的当前元素
               即将左边的当前元素，填充到temp数组中。然后后移 即t++ i++  */
            if (arr[i] <= arr[j]){
                temp[t]=arr[i];
                t +=1;
                i +=1;
            }else{//反之，将右边有序序列的当前元素，填充到temp数组
                temp[t]=arr[j];
                t +=1;
                j +=1;
            }
        }

        /**
         二、
            把有剩余数据的一边的数据依次全部填充到temp
         */
        //左边有序序列有剩余元素，就全部填充到temp数组中
        while (i <= mid){
            temp[t]=arr[i];
            t +=1;
            i +=1;
        }
        //右边有序序列有剩余元素，就全部填充到temp数组中
        while (j <= right){
            temp[t]=arr[j];
            t +=1;
            j +=1;
        }

        /**
         三、
            将temp数组的元素拷贝到arr，并不是每次都拷贝所有
         */
        t = 0;
        int tempLeft = left;
        //第一次合并 tempLeft=0，t=1；二 tempLeft=2，t=3；三 tempLeft=0，t=3
        // 最后一次 tempLeft=0，t=arr.length-1；
//        System.out.println("tempLeft="+tempLeft + "right=" + right);
        while (tempLeft <= right){
            arr[tempLeft] = temp[t];
            t +=1;
            tempLeft +=1;
        }
    }

    //时间测试
    public static void testTime(){
        int [] arr = new int [80000];
        int [] temp = new int[arr.length];
        for (int i = 0; i < 80000; i++) {
            arr[i] = (int) (Math.random() * 80000);
        }
        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String str1 = simpleDateFormat.format(date1);
        System.out.println("排序前的时间是："+date1);

        mergersort(arr,0,arr.length-1,temp);

        Date date2 = new Date();
        String str2 = simpleDateFormat.format(date2);
        System.out.println("排序前的时间是："+date2);
    }
}


```

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200628151802345.png" alt="image-20200628151802345" style="zoom:100%;" />

### 时间比较：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200628151910615.png" alt="image-20200628151910615" style="zoom:150%;" />

## #基数排序(桶排序)：

### 介绍：

![image-20200705160704485](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200705160704485.png)

### 思路分析：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200705172610074.png" alt="image-20200705172610074" style="zoom:150%;" />![image-20200705172656725](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200705172656725.png)

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200705172724072.png" alt="image-20200705172724072" style="zoom:150%;" />

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200705172849425.png" alt="image-20200705172849425" style="zoom:150%;" />

### 代码实现：

```java
package com.cyq.Sort;

import java.text.SimpleDateFormat;
import java.util.Arrays;
import java.util.Date;

/**
 * 基数排序是使用空间换时间的经典算法
 *
 */
public class RadixSort {
    public static void main(String[] args) {
        int [] arr = {53,3,542,748,14,214};
//        radixSortTest(arr);
//        radixSort(arr);
            testTime();
    }

    //基数排序
    public static void radixSortTest(int [] arr){
        /** 定义一个二维数组，表示10个桶，每个桶就是一个一维数组
            二维数组包含10个一维数组，为了防止放数据的时候，数据溢出
            每一个一维数组的大小定义为arr.length
         */
        int [][] bucket = new int [10][arr.length];
        //定义一个一维数组，用来记录实际存放了多少个数据
        int [] bucketCount = new int [10];

        //第一轮(针对每个元素的个位进行排序处理)
        for (int j = 0;j<arr.length;j++){
            int bitElement = arr[j] % 10;
            bucket[bitElement][bucketCount[bitElement]] = arr[j];
            bucketCount[bitElement]++;
        }
        //按照这个桶的顺序(一维数组的数据依次取出数据，放回到原来数组)
        int index = 0;
        //遍历每一个桶，并将有数据的桶中的元素，放回到原来的数组中
        for (int k = 0;k<bucketCount.length;k++){
            //如果桶中有数据才放入原数组
            if (bucketCount[k] != 0 ){
                //循环该桶即第k个桶(即第k个一维数组)
                for (int l = 0;l<bucketCount[k];l++){
                    //取出元素放到arr中
                    arr[index]=bucket[k][l];
                    index++;
                }
            }
            //第一轮处理后，需要将每个bucketCount[k]置为0
            bucketCount[k] = 0;
        }
        System.out.println("第一轮处理后的arr："+Arrays.toString(arr));

//=================================================

        //第二轮(针对每个元素的个位进行排序处理)
        for (int j = 0;j<arr.length;j++){
            int bitElement = arr[j] /10 % 10;
            bucket[bitElement][bucketCount[bitElement]] = arr[j];
            bucketCount[bitElement]++;
        }
        //按照这个桶的顺序(一维数组的数据依次取出数据，放回到原来数组)
        index = 0;
        //遍历每一个桶，并将有数据的桶中的元素，放回到原来的数组中
        for (int k = 0;k<bucketCount.length;k++){
            //如果桶中有数据才放入原数组
            if (bucketCount[k] != 0 ){
                //循环该桶即第k个桶(即第k个一维数组)
                for (int l = 0;l<bucketCount[k];l++){
                    //取出元素放到arr中
                    arr[index]=bucket[k][l];
                    index++;
                }
            }
            //第二轮处理后，需要将每个bucketCount[k]置为0
            bucketCount[k] = 0;
        }
        System.out.println("第二轮处理后的arr："+Arrays.toString(arr));

//================================================

        //第三轮(针对每个元素的个位进行排序处理)
        for (int j = 0;j<arr.length;j++){
            int bitElement = arr[j] /100 % 10;
            bucket[bitElement][bucketCount[bitElement]] = arr[j];
            bucketCount[bitElement]++;
        }
        //按照这个桶的顺序(一维数组的数据依次取出数据，放回到原来数组)
        index = 0;
        //遍历每一个桶，并将有数据的桶中的元素，放回到原来的数组中
        for (int k = 0;k<bucketCount.length;k++){
            //如果桶中有数据才放入原数组
            if (bucketCount[k] != 0 ){
                //循环该桶即第k个桶(即第k个一维数组)
                for (int l = 0;l<bucketCount[k];l++){
                    //取出元素放到arr中
                    arr[index]=bucket[k][l];
                    index++;
                }
            }
            //第三轮处理后，需要将每个bucketCount[k]置为0
            bucketCount[k] = 0;
        }
        System.out.println("第三轮处理后的arr："+Arrays.toString(arr));

    }
    public static void radixSort(int [] arr){
        //得到最大的数的位数
        int max = arr[0];
        for (int i = 1; i < arr.length; i++) {
            if (arr[i]>max){
                max = arr[i];
            }
        }
        //得到最大数的是几位数
        int maxLength = (max+"").length();
        int [][] bucket = new int [10][arr.length];
        int [] bucketCount = new int [10];
        for (int i = 0,n = 1; i < maxLength; i++,n *= 10) {
            for (int j = 0;j<arr.length;j++){
                int bucketElement = arr[j] / n % 10;
                bucket[bucketElement][bucketCount[bucketElement]] = arr[j];
                bucketCount[bucketElement]++;
            }
            int index = 0;
            for (int k =0;k<bucketCount.length;k++){
                if (bucketCount[k] != 0){
                    for (int l =0;l<bucketCount[k];l++){
                        arr[index]=bucket[k][l];
                        index++;
                    }
                }
                bucketCount[k] = 0;
            }
//            System.out.println("第"+i+"轮排序后arr："+Arrays.toString(arr));
        }
//        System.out.println("排序后arr："+Arrays.toString(arr));
    }

    //时间测试
    public static void testTime(){
        int [] arr = new int [80000];
        int [] temp = new int[arr.length];
        for (int i = 0; i < 80000; i++) {
            arr[i] = (int) (Math.random() * 80000);
        }
        Date date1 = new Date();
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String str1 = simpleDateFormat.format(date1);
        System.out.println("排序前的时间是："+date1);

        radixSort(arr);

        Date date2 = new Date();
        String str2 = simpleDateFormat.format(date2);
        System.out.println("排序前的时间是："+date2);
    }
}


```

### 时间比较：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200705173004309.png" alt="image-20200705173004309" style="zoom:150%;" />

#### 注意点：

1. 基数排序是对传统桶排序的扩展，速度很快

2. 基数排序是经典的空间换时间的算法，占用内存较大，当对海量数据排序时，容易造成 OutOfMemoryError

   <img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200705173504857.png" alt="image-20200705173504857" style="zoom:150%;" />

# 查找算法：

## #线性(顺序)查找：

### 代码实现：

```java
public class SeqSearch {
    public static void main(String[] args) {
        int [] arr = {12,43,-12,4,67,8};
        int index = seqSearch(arr,-12);
        if (index == -1){
            System.out.println("没有找到");
        }else{
            System.out.println("找到了下标为："+index);
        }
    }
    public static int seqSearch(int [] arr,int value){
        for (int i = 0; i < arr.length; i++) {
            if(arr[i] == value){
                return i;
            }
        }
        return -1;
    }
}

```

## #二分查找：

### 思路分析：

![image-20200723111955678](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200723111955678.png)

### 数组必须为有序数组！！！

### 代码实现：

```java
package com.cyq.Search;

import java.util.ArrayList;
import java.util.List;

public class BinarySearch {
    public static void main(String[] args) {
        int [] arr = {1,8,10,89,1000,1000,1000,1000,1234};
//        int resindex = binarySearch(arr,0,arr.length-1,8);
//        System.out.println(resindex);
        List list = binarySearch2(arr,0,arr.length-1,1000);
        System.out.println(list);


    }

    /**
     *
     * @param arr      数组
     * @param left     左索引
     * @param right    右索引
     * @param findval  要查找得值
     * @return
     */
    public static int binarySearch(int [] arr,int left,int right,int findval){
        //如果 left > right时，说明递归整个数组，但是没有找到
        if (left > right){
            return -1;
        }
        int mid = (left + right) / 2;
        int midval = arr[mid];
        if (findval > midval){// 向 右 递归
            return binarySearch(arr,mid + 1,right,findval);
        }else if (findval < midval){// 向 左 递归
            return binarySearch(arr,left,mid - 1,findval);
        }else{
            return mid;
        }
    }

    /**
     当一个有序数组中有多个相同的值时，如何将所有的值都查找到并返回下标
        思路分析：
            1、在找到mid索引值，不要马上返回
            2、向mid值索引值的左边扫描，将所有满足1000的元素下标，加入到集合ArrayList
            3、向mid值索引值的右边扫描，将所有满足1000的元素下标，加入到集合ArrayList
            4、将ArrayList返回

     */
    public static List<Integer> binarySearch2(int [] arr,int left,int right,int findval){
        //如果 left > right时，说明递归整个数组，但是没有找到
        if (left > right){
            return new ArrayList<Integer>();
        }
        int mid = (left + right) / 2;
        int midval = arr[mid];
        if (findval > midval){// 向 右 递归
            return binarySearch2(arr,mid + 1,right,findval);
        }else if (findval < midval){// 向 左 递归
            return binarySearch2(arr,left,mid - 1,findval);
        }else{
            List<Integer> list = new ArrayList<>();
//            向mid值索引值的左边扫描，将所有满足1000的元素下标，加入到集合ArrayList
            int temp = mid - 1;
            while (true){
                if (temp < 0 || arr[temp] != findval){
                    break;
                }
                //否则temp 放到list
                list.add(temp);
                temp -= 1;
            }
                list.add(mid);

//            向mid值索引值的右边扫描，将所有满足1000的元素下标，加入到集合ArrayList
            temp = mid + 1;
            while (true){
                if (temp > arr.length - 1 || arr[temp] != findval){
                    break;
                }
                //否则temp 放到list
                list.add(temp);
                temp += 1;
            }
            return list;
        }
    }
}


```

## #插值查找：

### 思路分析：

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200727143107284.png" alt="image-20200727143107284" />

![image-20200727143140081](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200727143140081.png)

### 注意点：

1. 对于数据量大，关键字分布比较均匀的情况下，采用插值查找，速度较快
2. 关键字分布不均匀的情况下，该方法不一定比折半查找方法好

### 代码实现：

```java
package com.cyq.Search;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class InsertValueSearch {
    public static void main(String[] args) {
        int [] arr = new int [100];
        for (int i = 0; i < 100; i++) {
            arr[i] =  i + 1;
        }
        System.out.println(Arrays.toString(arr));
    }


    /**
     *
     * @param arr
     * @param left
     * @param right
     * @param findVal
     * @return
     */
    public static int insertValueSearch(int [] arr,int left,int right,int findVal){
        //注意：findVal < arr[0] 和 findVal > arr[arr.length - 1] 是必须的不然求出的mid可能越界
        if (left > right || findVal < arr[0] || findVal > arr[arr.length - 1]){
            return -1;
        }
        int mid = left + (right - left) * (findVal - arr[left]) / (arr[right] - arr[left]);
        int midVal = arr[mid];
        if (findVal > midVal){
            return insertValueSearch(arr,mid+1,right,findVal);
        }else if (findVal < midVal){
            return insertValueSearch(arr,left,mid-1,findVal);
        }else {
            return mid;
        }
    }

    
    public static List<Integer> insertValueSearch2(int [] arr, int left, int right, int findVal){
        //注意：findVal < arr[0] 和 findVal > arr[arr.length - 1] 是必须的不然求出的mid可能越界
        if (left > right || findVal < arr[0] || findVal > arr[arr.length - 1]){
            new ArrayList<Integer>();
        }
        int mid = left + (right - left) * (findVal - arr[left]) / (arr[right] - arr[left]);
        int midVal = arr[mid];
        if (findVal > midVal){
            return insertValueSearch2(arr,mid+1,right,findVal);
        }else if (findVal < midVal){
            return insertValueSearch2(arr,left,mid-1,findVal);
        }else{
            List<Integer> list = new ArrayList<>();
//            向mid值索引值的左边扫描，将所有满足1000的元素下标，加入到集合ArrayList
            int temp = mid - 1;
            while (true){
                if (temp < 0 || arr[temp] != findVal){
                    break;
                }
                //否则temp 放到list
                list.add(temp);
                temp -= 1;
            }
            list.add(mid);

//            向mid值索引值的右边扫描，将所有满足1000的元素下标，加入到集合ArrayList
            temp = mid + 1;
            while (true){
                if (temp > arr.length - 1 || arr[temp] != findVal){
                    break;
                }
                //否则temp 放到list
                list.add(temp);
                temp += 1;
            }
            return list;
        }
    }
}


```

## #斐波那契(黄金分割法)查找：	

### 介绍：

![image-20200727155900118](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200727155900118.png)

### 思路分析：

![image-20200727160025413](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200727160025413.png )

### 代码实现：

```java
package com.cyq.Search;

import java.util.Arrays;

public class FibonacciSearch {
    public static int maxsize = 20;
    public static void main(String[] args) {
        int [] arr = {1,8,10,89,1000,1234};

        System.out.println(fibSearch(arr,1234));
    }
    //mid = low + F[k-1]-1,需要使用到斐波那契数列
    //非递归的方法得到一个斐波那契数列
    public static int [] fib(){
        int [] f = new int [maxsize];
        f[0] = 1;
        f[1] = 1;
        for (int i = 2; i < maxsize; i++) {
            f[i] = f[i-1]+f[i-2];
        }
        return f;
    }

    /**
     *  编写斐波那契查找算法(使用非递归写算法)
     * @param a   数组
     * @param key 需要查找的关键码(值)
     * @return    返回对应下标，没有则返回-1
     */
    public static int fibSearch (int [] a,int key){
        int low = 0;
        int high= a.length - 1;
        int k =0; //表示斐波那契分割数值的下标
        int mid = 0;//存放中间值
        int [] f = fib();//获得斐波那契数列
        //获取斐波那契分割数值的下标
        while (high > f[k] - 1){
            k++;
        }
        //因为 f[k] 值可能大于a的长度，因此需要使用Arrays类 构造一个新的数组，并指向a[]
        //不足的部分会使用0填充
        int [] temp = Arrays.copyOf(a,f[k]);
        //实际上需求使用a数组最后的数填充temp
        //eg：temp = {1,8,10,89,1000,1234,0,0,0} => {1,8,10,89,1000,1234,1234,1234,1234}
        for (int i = high+1; i < temp.length; i++) {
            temp[i] = a[high];
        }

        //使用while来循环处理，找到我们的数key
        while(low <= high){
            mid = low + f[k-1] -1;
            if (key < temp[mid]){ //需要向数组的前面查找(左边)
                high = mid - 1;
                k--;
                /*  k--
                    1、全部元素 = 前面元素 + 后边元素
                    2、f[k] = f[k-1] + f[k-2];
                    3、因为后面有f[k-2]所以可以继续拆分f[k-1] = f[k-2] + f[k-3];
                    4、即在f[k-1]的前面继续查找 k--;
                    5、即下次循环 mid = f[k-1-1] - 1;
                 */
            }else if (key > temp[mid]){//需要向数组的后面查找(右边)
                low = mid + 1;
                k -=2;
                /*  k -=2
                    1、全部元素 = 前面元素 + 后边元素
                    2、f[k] = f[k-1] + f[k-2];
                    3、因为后面有f[k-2]所以可以继续拆分f[k-1] = f[k-3] + f[k-4];
                    4、即在f[k-2]的前面继续查找 k -=2;
                    5、即下次循环 mid = f[k-1-2] - 1;
                 */
            }else{ //找到
                //需要确定返回的是哪个下标
                if(mid <= high){
                    return mid;
                }else {
                    return high;
                }
            }
        }
            return -1;
    }
}

```

# 哈希表：

![image-20200809160830025](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200809160830025.png)

## 哈希表的基本介绍：

![image-20200809160918202](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200809160918202.png)



![image-20200809160950800](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200809160950800.png)

## 示例题：

![image-20200809161147431](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200809161147431.png)

## 思路分析：

![image-20200809161222011](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200809161222011.png)

## 代码实现：

```java
import java.util.Scanner;
public class HashTabDemo {
    public static void main(String[] args) {

        HashTab hashTab = new HashTab(7);

        String key = "";
        Scanner scanner = new Scanner(System.in);
        while (true){
            System.out.println("add： 添加雇员");
            System.out.println("list：显示雇员");
            System.out.println("find：查找雇员");
            System.out.println("exit：退出系统");

            key = scanner.next();
            switch (key){
                case "add":
                    System.out.println("请输入id");
                    int id = scanner.nextInt();
                    System.out.println("请输入姓名");
                    String name = scanner.next();
                    Emp emp = new Emp(id,name);
                    hashTab.add(emp);
                    break;
                case "list":
                    hashTab.list();
                    break;
                case "find":
                    System.out.println("请输入要找找的id号");
                    int findId = scanner.nextInt();
                    hashTab.findEmpById(findId);
                    break;
                case "exit":
                    scanner.close();
                    System.exit(0);
                default:
                    break;
            }
        }
    }
}
class HashTab{//创建HashTab 管理多条链表
    private EmpLinkedList [] empLinkedListArray;
    private int size;//表示有多少条链表

    //构造器 初始化empLinkedListArray
    public HashTab(int size){
        this.size = size;
        empLinkedListArray = new EmpLinkedList[size];
        for (int i = 0; i < size; i++) {
            empLinkedListArray[i] = new EmpLinkedList();
        }
    }

    //添加雇员  根据雇员id，得到雇员应该添加到哪条链表
    public void add(Emp emp){
        int empLinkedListNo = hashFun(emp.id);
        empLinkedListArray[empLinkedListNo].add(emp);
    }

    //遍历链表，遍历hashtab
    public void list(){
        for (int i = 0; i < size; i++) {
            empLinkedListArray[i].list(i+1);
        }
    }

    //根据输入的id查找雇员
    public void findEmpById(int id){
        int empLinkedListNo = hashFun(id);
        Emp emp = empLinkedListArray[empLinkedListNo].findEmpById(id);
        if (emp != null){
            System.out.printf("在第%d条链表中找到id = %d的雇员",(empLinkedListNo+1),id);
            System.out.println();
        }else{
            System.out.println("在哈希表中，没有找到该雇员");
        }
    }
    //编写散列函数 使用取模法
    public int hashFun(int id){
        return id % size;
    }
}


class Emp{
    public int id;
    public String name;
    public Emp next;

    public Emp(int id,String name) {
        super();
        this.id=id;
        this.name=name;
    }
}
//创建EmpLinkedList ， 表示链表
class EmpLinkedList{
    //头指针，执行第一个Emp，因此我们这个链表的head 是指向第一个Emp
    private Emp head;//默认为null

    /* 添加雇员到链表
    说明：  假定，当添加雇员时，id是自增长，即id的分配总是从小到大
           因此将雇员直接加入到链表最后即可
     */
    public void add(Emp emp){
        //如果添加的是第一个员工
        if (head == null){
            head = emp;
            return;
        }
        //如果不是第一个员工，则需要辅助指针定位到最后
        Emp curEmp = head;
        while (true){
            if (curEmp.next == null){//到链表最后
                break;
            }
            curEmp = curEmp.next;//后移
        }
        //退出时直接将emp加入链表
        curEmp.next = emp;
    }

    //遍历链表
    public void list(int no){
        if (head == null){
            System.out.println("第"+no+"链表为空");
            return;
        }
        System.out.print("第"+no+"链表信息为");
        Emp curEmp = head;//辅助指针
        while (true){
            System.out.printf("=> id=%d name=%s\t",curEmp.id,curEmp.name);
            if (curEmp.next == null){//已经到链表最后
                break;
            }
            curEmp = curEmp.next;//后移，完成遍历
        }
        System.out.println();
    }

    //根据id查找雇员
    public Emp findEmpById(int id){
        if (head  == null){
            System.out.println("链表为空");
            return null;
        }
        Emp curEmp = head;
        while (true){
            if (curEmp.id == id){
                break;
            }
            if (curEmp.next == null){//没有找到雇员，退出
                curEmp=null;
                break;
            }
            curEmp = curEmp.next;
        }
        return curEmp;
    }
}

```

# 树结构：

## 二叉树：

![image-20200902103839445](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200902103839445.png)

#### 数组存储方式：

![image-20200902103953146](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200902103953146.png)

#### 链表存储方式：

![image-20200902104057750](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200902104057750.png)

#### 二叉排序树：

![image-20200902104219079](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200902104219079.png)

#### 树示意图：

![image-20200902104954345](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200902104954345.png)

#### 二叉树的概念：

![image-20200902105150085](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200902105150085.png)

![image-20200902105206234](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200902105206234.png)

## 二叉树遍历说明：

![image-20200902105530182](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200902105530182.png)



### 思路分析：

```tex
分析二叉树的前序、中序、后序的遍历步骤
1、创建一颗二叉树
-----------------------------------
2.前序遍历：
2.1先输出当前节点(初始的时候是root节点)
2.2如果当前节点的左节点不为空，则递归继续前序遍历
2.3如果当前节点的右节点不为空，则递归继续前序遍历
-----------------------------------
3.中序遍历：
3.1如果当前节点的左节点不为空，则递归继续中序遍历
3.2输出当前节点
3.3如果当前节点的右节点不为空，则递归继续中序遍历
-----------------------------------
4.后序遍历：
4.1如果当前节点的左节点不为空，则递归继续后序遍历
4.2如果当前节点的右节点不为空，则递归继续后序遍历
4.3输出当前节点

```

### 代码实现：

```java
public class BinaryTreeDemo {
    public static void main(String[] args) {
        BinaryTree tree = new BinaryTree();

        HeroNode root = new HeroNode(1,"a");
        HeroNode node1 = new HeroNode(2,"b");
        HeroNode node2 = new HeroNode(3,"c");
        HeroNode node3 = new HeroNode(4,"d");
        HeroNode node4 = new HeroNode(5,"e");

        tree.setRoot(root);
        root.setLeft(node1);
        root.setRight(node2);
        node2.setRight(node3);
        node2.setLeft(node4);

        System.out.println("前序遍历");
        tree.preOrder();

        System.out.println("中序遍历");
        tree.infixOrder();

        System.out.println("后序遍历");
        tree.postOrder();
    }
}

//定义BinaryTree二叉树
class BinaryTree{
    private HeroNode root;

    public void setRoot(HeroNode root) {
        this.root = root;
    }

    //前序遍历
    public void preOrder(){
        if (this.root != null){
            this.root.preOrder();
        }else{
            System.out.println("当前二叉树为空");
        }
    }

    //中序遍历
    public void infixOrder(){
        if (this.root != null){
            this.root.infixOrder();
        }else{
            System.out.println("当前二叉树为空");
        }
    }

    //后序遍历
    public void postOrder(){
        if (this.root != null){
            this.root.postOrder();
        }else{
            System.out.println("当前二叉树为空");
        }
    }
}


//先创建HeroNode节点
class HeroNode{
    private int no;
    private String name;
    private HeroNode left;
    private HeroNode right;

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public HeroNode getLeft() {
        return left;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public HeroNode getRight() {
        return right;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }

    //前序遍历
    public void preOrder(){
        //先输出父节点
        System.out.println(this);
        //递归向左子树前序遍历
        if (this.left != null){
            this.left.preOrder();
        }
        //递归向右子树前序遍历
        if (this.right != null){
            this.right.preOrder();
        }
    }

    //中序遍历
    public void infixOrder(){
        //递归向左子树中序遍历
        if (this.left != null){
            this.left.infixOrder();
        }
        //输出父节点
        System.out.println(this);
        //递归向右子树中序遍历
        if (this.right != null){
            this.right.infixOrder();
        }
    }

    //后序遍历
    public void postOrder(){
        //递归向左子树后序遍历
        if (this.left != null){
            this.left.postOrder();
        }
        //递归向右子树后序遍历
        if (this.right != null){
            this.right.postOrder();
        }
        //输出父节点
        System.out.println(this);
    }
}

```

## 二叉树查找:

### 思路分析：

![image-20201009151321836](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201009151321836.png)

### 代码实现：

```java
package com.cyq.Tree;

public class BinaryTreeDemo {
    public static void main(String[] args) {
        BinaryTree tree = new BinaryTree();

        HeroNode root = new HeroNode(1,"a");
        HeroNode node1 = new HeroNode(2,"b");
        HeroNode node2 = new HeroNode(3,"c");
        HeroNode node3 = new HeroNode(4,"d");
        HeroNode node4 = new HeroNode(5,"e");

        tree.setRoot(root);

        root.setLeft(node1);
        root.setRight(node2);

        node2.setRight(node3);
        node2.setLeft(node4);

        System.out.println("前序遍历");
        tree.preOrder();

        System.out.println("中序遍历");
        tree.infixOrder();

        System.out.println("后序遍历");
        tree.postOrder();

        System.out.println("====================");
        System.out.println("前序查找");
        HeroNode resNode = tree.preOrderSearch(5);
        if (resNode != null){
            System.out.printf("找到了，信息： no=%d name=%s",resNode.getNo(),resNode.getName());
        }else{
            System.out.printf("没有找到编号为%d",5);
        }

        System.out.println();
        System.out.println("====================");
        System.out.println("中序查找");
        resNode = tree.infixOrderSearch(5);
        if (resNode != null){
            System.out.printf("找到了，信息： no=%d name=%s",resNode.getNo(),resNode.getName());
        }else{
            System.out.printf("没有找到编号为%d",5);
        }

        System.out.println();
        System.out.println("====================");
        System.out.println("后序查找");
        resNode = tree.postOrderSearch(5);
        if (resNode != null){
            System.out.printf("找到了，信息： no=%d name=%s",resNode.getNo(),resNode.getName());
        }else{
            System.out.printf("没有找到编号为%d",5);
        }
    }
}

//定义BinaryTree二叉树
class BinaryTree{
    private HeroNode root;

    public void setRoot(HeroNode root) {
        this.root = root;
    }

    //前序遍历
    public void preOrder(){
        if (this.root != null){
            this.root.preOrder();
        }else{
            System.out.println("当前二叉树为空");
        }
    }

    //中序遍历
    public void infixOrder(){
        if (this.root != null){
            this.root.infixOrder();
        }else{
            System.out.println("当前二叉树为空");
        }
    }

    //后序遍历
    public void postOrder(){
        if (this.root != null){
            this.root.postOrder();
        }else{
            System.out.println("当前二叉树为空");
        }
    }

    //前序查找
    public HeroNode preOrderSearch(int no){
        if (root != null){
            return  root.preOrderSearch(no);
        }else{
            return null;
        }
    }

    //前序查找
    public HeroNode infixOrderSearch(int no){
        if (root != null){
            return  root.infixOrderSearch(no);
        }else{
            return null;
        }
    }

    //前序查找
    public HeroNode postOrderSearch(int no){
        if (root != null){
            return  root.postOrderSearch(no);
        }else{
            return null;
        }
    }
}


//先创建HeroNode节点
class HeroNode{
    private int no;
    private String name;
    private HeroNode left;
    private HeroNode right;

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public HeroNode getLeft() {
        return left;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public HeroNode getRight() {
        return right;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }

    //前序遍历
    public void preOrder(){
        //先输出父节点
        System.out.println(this);
        //递归向左子树前序遍历
        if (this.left != null){
            this.left.preOrder();
        }
        //递归向右子树前序遍历
        if (this.right != null){
            this.right.preOrder();
        }
    }

    //中序遍历
    public void infixOrder(){
        //递归向左子树中序遍历
        if (this.left != null){
            this.left.infixOrder();
        }
        //输出父节点
        System.out.println(this);
        //递归向右子树中序遍历
        if (this.right != null){
            this.right.infixOrder();
        }
    }

    //后序遍历
    public void postOrder(){
        //递归向左子树后序遍历
        if (this.left != null){
            this.left.postOrder();
        }
        //递归向右子树后序遍历
        if (this.right != null){
            this.right.postOrder();
        }
        //输出父节点
        System.out.println(this);
    }

    /**
     *  前序遍历查找
     * @param no    需要查找的序号 no
     * @return      如果找到则返回HeroNode，否则返回null
     */
    public HeroNode preOrderSearch(int no){
        System.out.println("前序遍历查找~~~");
        //比较当前节点是不是
        if (this.no == no){
            return this;
        }
        //先判断当前节点的做子节点是否为空，不为空则递归前序查找
        //如果前序查找 找到节点 则返回
        HeroNode resNode = null;
        if (this.left != null){
            resNode = this.left.preOrderSearch(no);
        }
        if (resNode != null){
            return resNode;
        }
        //若左边前序查找完没有找到，先判断当前节点的右子节点是否为空 不为空则递归前序查找
        //找到则返回 没有则返回null
        if (this.right != null){
            resNode = this.right.preOrderSearch(no);
        }
        return resNode;
    }

    /**
     * 中序查找
     * @param no    需要查找的序号 no
     * @return      如果找到则返回HeroNode，否则返回null
     */
    public HeroNode infixOrderSearch(int no){
        HeroNode resNode = null;
        //向左遍历查找
        if (this.left != null){
            resNode = this.left.infixOrderSearch(no);
        }
        if (resNode != null){
            return resNode;
        }
        System.out.println("中序遍历查找~~~");
        //当前节点
        if (this.no == no){
            return this;
        }
        //向右遍历查找
        if (this.right != null){
            resNode = this.right.infixOrderSearch(no);
        }
        return resNode;
    }

    /**
     * 后序查找
     * @param no    需要查找的序号 no
     * @return      如果找到则返回HeroNode，否则返回null
     */
    public HeroNode postOrderSearch(int no){
        HeroNode resNode = null;
        //向左遍历查找
        if (this.left != null){
            resNode = this.left.postOrderSearch(no);
        }
        if (resNode != null){
            return resNode;
        }
        //向右遍历查找
        if (this.right != null){
            resNode = this.right.postOrderSearch(no);
        }
        if (resNode != null){
            return resNode;
        }
        System.out.println("后序遍历查找~~~");
        //左右子树都没，查找当前节点
        if (this.no == no){
            return this;
        }
        return resNode;
    }
}

```

## 二叉树删除节点：

### 思路分析：

![image-20201009162310883](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201009162310883.png)

### 代码实现：

```java
package com.cyq.Tree;

public class BinaryTreeDemo {
    public static void main(String[] args) {
        BinaryTree tree = new BinaryTree();

        HeroNode root = new HeroNode(1, "a");
        HeroNode node1 = new HeroNode(2, "b");
        HeroNode node2 = new HeroNode(3, "c");
        HeroNode node3 = new HeroNode(4, "d");
        HeroNode node4 = new HeroNode(5, "e");

        tree.setRoot(root);

        root.setLeft(node1);
        root.setRight(node2);

        node2.setRight(node3);
        node2.setLeft(node4);

        

        System.out.println("=======================");
        System.out.println("删除前，前序遍历");
        tree.preOrder();
//        tree.delNode(5);
        tree.delNode(3);
        System.out.println("删除后，前序遍历");
        tree.preOrder();
    }
}

//定义BinaryTree二叉树
class BinaryTree {
    private HeroNode root;

    public void setRoot(HeroNode root) {
        this.root = root;
    }

    

    //删除节点
    public void delNode(int no){
        if (root != null){
            //如果root只有一个节点，立即判断root是否就是需要删除的节点
            if(root.getNo() == no){
                root =null;
            }else{
                //递归删除
                root.delNode(no);
            }
        }else{
            System.out.println("空树不能删除");
        }
    }
}


//先创建HeroNode节点
class HeroNode {
    private int no;
    private String name;
    private HeroNode left;
    private HeroNode right;

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public HeroNode getLeft() {
        return left;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public HeroNode getRight() {
        return right;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }


    /*
      递归删除节点
        1、如果删除的节点是叶子节点，则删除该节点
        2、如果删除的节点是飞叶子节点，则删除该子树
    */
    public void delNode(int no) {
        //如果当前节点的左子节点不为空，并且左子节点 就是要删除的节点 将this.left = null;并且返回(结束递归)
        if (this.left != null && this.left.no == no) {
            this.left = null;
            return;
        }
        //如果当前节点的右子节点不为空，并且右子节点 就是要删除的节点 将this.right = null;并且返回(结束递归)
        if (this.right != null && this.right.no == no) {
            this.right = null;
            return;
        }
        //如果前面都没找到需要删除的节点 就需要向左子树进行递归删除
        if (this.left != null) {
            this.left.delNode(no);
        }
        //如果前面都没找到需要删除的节点 就需要向左子树进行递归删除
        if (this.right != null) {
            this.right.delNode(no);
        }
    }
}

```

## 顺序存储二叉树：

![image-20201009165558518](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201009165558518.png)

### 顺序二叉树存储特点：（应用实例堆排序）

1. 顺序二叉树通常只考虑完全二叉树
2. 第n个元素的左子节点为2 * n + 1
3. 第n个元素的右子节点为2 * n + 2
4. 第n个元素的父子节点为 ( n - 1 ) / 2
5. n:表示二叉树中的第几个元素(按0开始编号)

```java
package com.cyq.Tree;

public class ArrayBinaryTreeDemo {
    public static void main(String[] args) {
        int arr [] = {1,2,3,4,5,6,7};
        ArrayBinaryTree tree = new ArrayBinaryTree(arr);

        System.out.println("前序遍历");
        tree.preOrder();

        System.out.println();
        System.out.println("中序遍历");
        tree.infixOrder();

        System.out.println();
        System.out.println("后序遍历");
        tree.postOrder();
    }
}

class ArrayBinaryTree{
    private int [] arr;//存储数据节点的数组

    public ArrayBinaryTree(int[] arr) {
        this.arr = arr;
    }

    public void preOrder(){
        this.preOrder(0);
    }

    public void infixOrder(){
        this.infixOrder(0);
    }

    public void postOrder(){
        this.postOrder(0);
    }

    /**
     *  顺序存储二叉树的前序遍历
     * @param index 数组的下标
     */
    public void preOrder(int index){
        //如果数组为空，或者arr.length = 0
        if(arr == null || arr.length == 0){
            System.out.println("数组为空，不能进行遍历");
        }
        //输出当前这个节点
        System.out.print(arr[index]+" ");
        //向左递归遍历
        if ((index * 2 + 1) < arr.length){
            preOrder(index * 2 + 1);
        }
        //向右递归遍历
        if ((index * 2 + 2) <arr.length){
            preOrder(index * 2 + 2);
        }
    }

    /**
     *  顺序存储二叉树的中序遍历
     * @param index 数组的下标
     */
    public void infixOrder(int index){
        //如果数组为空，或者arr.length = 0
        if(arr == null || arr.length == 0){
            System.out.println("数组为空，不能进行遍历");
        }
        //向左递归遍历
        if ((index * 2 + 1) < arr.length){
            preOrder(index * 2 + 1);
        }
        //输出当前这个节点
        System.out.print(arr[index]+" ");
        //向右递归遍历
        if ((index * 2 + 2) <arr.length){
            preOrder(index * 2 + 2);
        }
    }

    /**
     *  顺序存储二叉树的后序遍历
     * @param index 数组的下标
     */
    public void postOrder(int index){
        //如果数组为空，或者arr.length = 0
        if(arr == null || arr.length == 0){
            System.out.println("数组为空，不能进行遍历");
        }
        //向左递归遍历
        if ((index * 2 + 1) < arr.length){
            preOrder(index * 2 + 1);
        }
        //向右递归遍历
        if ((index * 2 + 2) <arr.length){
            preOrder(index * 2 + 2);
        }
        //输出当前这个节点
        System.out.print(arr[index]+" ");
    }
}

```

## 线索化二叉树：

![image-20201104161737802](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201104161737802.png)

### 思路分析：

![image-20201104162401256](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201104162401256.png)

### 代码实现：

```java
package com.cyq.Tree;

/**
 * @Author cxy
 * @Date 2020/11/4 16:25
 * @Version 1.0
 * 线索化二叉树
 */
public class ThreadedBinaryTreeDemo {
    public static void main(String[] args) {
        ThreadedHeroNode root = new ThreadedHeroNode(1, "一休");
        ThreadedHeroNode node2 = new ThreadedHeroNode(3, "三弟");
        ThreadedHeroNode node3 = new ThreadedHeroNode(6, "六狗");
        ThreadedHeroNode node4 = new ThreadedHeroNode(8, "八哥");
        ThreadedHeroNode node5 = new ThreadedHeroNode(10, "十子");
        ThreadedHeroNode node6 = new ThreadedHeroNode(14, "十四爹");

        root.setLeft(node2);
        root.setRight(node3);
        node2.setLeft(node4);
        node2.setRight(node5);
        node3.setLeft(node6);

        ThreadedBinaryTree binaryTree = new ThreadedBinaryTree();
        binaryTree.setRoot(root);
        binaryTree.infixthreadedNodes();

        ThreadedHeroNode node5Left = node5.getLeft();
        ThreadedHeroNode node5Right = node5.getRight();
        System.out.println("node5前驱节点==" + node5Left);
        System.out.println("node5后继节点==" + node5Right);
    }
}

//定义ThreadedBinaryTree二叉树 实现了线索化功能的二叉树
class ThreadedBinaryTree {
    private ThreadedHeroNode root;
    //为了实现线索化，需要创建一个指向当前节点的前驱节点的指针
    //在递归进行线索化是，pre总是保留前一个节点
    private ThreadedHeroNode pre = null;

    public void setRoot(ThreadedHeroNode root) {
        this.root = root;
    }

    public void prethreadedNodes1() {
        this.prethreadedNodes1(root);
    }

    public void infixthreadedNodes() {
        this.infixthreadedNodes(root);
    }

    public void postthreadedNodes() {
        this.postthreadedNodes(root);
    }


    /**
     * 编写对二叉树进行前序线索化的方法
     *
     * @param node 当前需要线索化的节点
     */
    public void prethreadedNodes1(ThreadedHeroNode node) {
        if (node == null) {
            return;
        }
        //2、线索化当前节点
        //处理当前节点的前驱节点
        if (node.getLeft() == null) {
            //让当前节点的左指针指向前驱节点
            node.setLeft(pre);
            //修改当前节点的做指针类型，指向前驱节点
            node.setLeftType(1);
        }
        //处理后继节点
        if (pre != null && pre.getRight() == null) {
            //让前驱节点的右指针指向当前节点
            pre.setRight(node);
            //修改前驱节点的右指针类型
            pre.setRightType(1);
        }
        //***没处理一个节点后，让当前节点是下一个节点的前驱节点
        pre = node;
        //1、线索化左子树
        prethreadedNodes1(node.getLeft());

        //3、线索化右子树
        prethreadedNodes1(node.getRight());
    }

    /**
     * 编写对二叉树进行中序线索化的方法
     *
     * @param node 当前需要线索化的节点
     */
    public void infixthreadedNodes(ThreadedHeroNode node) {
        if (node == null) {
            return;
        }
        //1、线索化左子树
        infixthreadedNodes(node.getLeft());
        //2、线索化当前节点
        //处理当前节点的前驱节点
        if (node.getLeft() == null) {
            //让当前节点的左指针指向前驱节点
            node.setLeft(pre);
            //修改当前节点的做指针类型，指向前驱节点
            node.setLeftType(1);
        }
        //处理后继节点
        if (pre != null && pre.getRight() == null) {
            //让前驱节点的右指针指向当前节点
            pre.setRight(node);
            //修改前驱节点的右指针类型
            pre.setRightType(1);
        }
        //***没处理一个节点后，让当前节点是下一个节点的前驱节点
        pre = node;
        //3、线索化右子树
        infixthreadedNodes(node.getRight());
    }

    /**
     * 编写对二叉树进行后序线索化的方法
     *
     * @param node 当前需要线索化的节点
     */
    public void postthreadedNodes(ThreadedHeroNode node) {
        if (node == null) {
            return;
        }
        //1、线索化左子树
        postthreadedNodes(node.getLeft());
        //3、线索化右子树
        postthreadedNodes(node.getRight());
        //2、线索化当前节点
        //处理当前节点的前驱节点
        if (node.getLeft() == null) {
            //让当前节点的左指针指向前驱节点
            node.setLeft(pre);
            //修改当前节点的做指针类型，指向前驱节点
            node.setLeftType(1);
        }
        //处理后继节点
        if (pre != null && pre.getRight() == null) {
            //让前驱节点的右指针指向当前节点
            pre.setRight(node);
            //修改前驱节点的右指针类型
            pre.setRightType(1);
        }
        //***没处理一个节点后，让当前节点是下一个节点的前驱节点
        pre = node;

    }

    //前序遍历
    public void preOrder() {
        if (this.root != null) {
            this.root.preOrder();
        } else {
            System.out.println("当前二叉树为空");
        }
    }

    //中序遍历
    public void infixOrder() {
        if (this.root != null) {
            this.root.infixOrder();
        } else {
            System.out.println("当前二叉树为空");
        }
    }

    //后序遍历
    public void postOrder() {
        if (this.root != null) {
            this.root.postOrder();
        } else {
            System.out.println("当前二叉树为空");
        }
    }

    //前序查找
    public ThreadedHeroNode preOrderSearch(int no) {
        if (root != null) {
            return root.preOrderSearch(no);
        } else {
            return null;
        }
    }

    //前序查找
    public ThreadedHeroNode infixOrderSearch(int no) {
        if (root != null) {
            return root.infixOrderSearch(no);
        } else {
            return null;
        }
    }

    //前序查找
    public ThreadedHeroNode postOrderSearch(int no) {
        if (root != null) {
            return root.postOrderSearch(no);
        } else {
            return null;
        }
    }

    //删除节点
    public void delNode(int no) {
        if (root != null) {
            //如果root只有一个节点，立即判断root是否就是需要删除的节点
            if (root.getNo() == no) {
                root = null;
            } else {
                //递归删除
                root.delNode(no);
            }
        } else {
            System.out.println("空树不能删除");
        }
    }
}


//先创建HeroNode节点
class ThreadedHeroNode {
    private int no;
    private String name;
    private ThreadedHeroNode left;
    private ThreadedHeroNode right;
    /*
        1、如果leftType == 0 表示指向的是左子树，如果是 1 表示指向前驱节点
        2、如果rightType == 0 表示指向的是右子树，如果是 1 表示指向后继节点
     */
    private int leftType;
    private int rightType;

    public ThreadedHeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public ThreadedHeroNode getLeft() {
        return left;
    }

    public void setLeft(ThreadedHeroNode left) {
        this.left = left;
    }

    public ThreadedHeroNode getRight() {
        return right;
    }

    public void setRight(ThreadedHeroNode right) {
        this.right = right;
    }

    public int getLeftType() {
        return leftType;
    }

    public void setLeftType(int leftType) {
        this.leftType = leftType;
    }

    public int getRightType() {
        return rightType;
    }

    public void setRightType(int rightType) {
        this.rightType = rightType;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }

    //前序遍历
    public void preOrder() {
        //先输出父节点
        System.out.println(this);
        //递归向左子树前序遍历
        if (this.left != null) {
            this.left.preOrder();
        }
        //递归向右子树前序遍历
        if (this.right != null) {
            this.right.preOrder();
        }
    }

    //中序遍历
    public void infixOrder() {
        //递归向左子树中序遍历
        if (this.left != null) {
            this.left.infixOrder();
        }
        //输出父节点
        System.out.println(this);
        //递归向右子树中序遍历
        if (this.right != null) {
            this.right.infixOrder();
        }
    }

    //后序遍历
    public void postOrder() {
        //递归向左子树后序遍历
        if (this.left != null) {
            this.left.postOrder();
        }
        //递归向右子树后序遍历
        if (this.right != null) {
            this.right.postOrder();
        }
        //输出父节点
        System.out.println(this);
    }

    /**
     * 前序遍历查找
     *
     * @param no 需要查找的序号 no
     * @return 如果找到则返回HeroNode，否则返回null
     */
    public ThreadedHeroNode preOrderSearch(int no) {
        System.out.println("前序遍历查找~~~");
        //比较当前节点是不是
        if (this.no == no) {
            return this;
        }
        //先判断当前节点的做子节点是否为空，不为空则递归前序查找
        //如果前序查找 找到节点 则返回
        ThreadedHeroNode resNode = null;
        if (this.left != null) {
            resNode = this.left.preOrderSearch(no);
        }
        if (resNode != null) {
            return resNode;
        }
        //若左边前序查找完没有找到，先判断当前节点的右子节点是否为空 不为空则递归前序查找
        //找到则返回 没有则返回null
        if (this.right != null) {
            resNode = this.right.preOrderSearch(no);
        }
        return resNode;
    }

    /**
     * 中序查找
     *
     * @param no 需要查找的序号 no
     * @return 如果找到则返回HeroNode，否则返回null
     */
    public ThreadedHeroNode infixOrderSearch(int no) {
        ThreadedHeroNode resNode = null;
        //向左遍历查找
        if (this.left != null) {
            resNode = this.left.infixOrderSearch(no);
        }
        if (resNode != null) {
            return resNode;
        }
        System.out.println("中序遍历查找~~~");
        //当前节点
        if (this.no == no) {
            return this;
        }
        //向右遍历查找
        if (this.right != null) {
            resNode = this.right.infixOrderSearch(no);
        }
        return resNode;
    }

    /**
     * 后序查找
     *
     * @param no 需要查找的序号 no
     * @return 如果找到则返回HeroNode，否则返回null
     */
    public ThreadedHeroNode postOrderSearch(int no) {
        ThreadedHeroNode resNode = null;
        //向左遍历查找
        if (this.left != null) {
            resNode = this.left.postOrderSearch(no);
        }
        if (resNode != null) {
            return resNode;
        }
        //向右遍历查找
        if (this.right != null) {
            resNode = this.right.postOrderSearch(no);
        }
        if (resNode != null) {
            return resNode;
        }
        System.out.println("后序遍历查找~~~");
        //左右子树都没，查找当前节点
        if (this.no == no) {
            return this;
        }
        return resNode;
    }

    /*
      递归删除节点
        1、如果删除的节点是叶子节点，则删除该节点
        2、如果删除的节点是飞叶子节点，则删除该子树
    */
    public void delNode(int no) {
        //如果当前节点的左子节点不为空，并且左子节点 就是要删除的节点 将this.left = null;并且返回(结束递归)
        if (this.left != null && this.left.no == no) {
            this.left = null;
            return;
        }
        //如果当前节点的右子节点不为空，并且右子节点 就是要删除的节点 将this.right = null;并且返回(结束递归)
        if (this.right != null && this.right.no == no) {
            this.right = null;
            return;
        }
        //如果前面都没找到需要删除的节点 就需要向左子树进行递归删除
        if (this.left != null) {
            this.left.delNode(no);
        }
        //如果前面都没找到需要删除的节点 就需要向左子树进行递归删除
        if (this.right != null) {
            this.right.delNode(no);
        }
    }
}

```

# 堆排序：

 堆排序(Heap Sort)是指利用堆这种数据结构所设计的一种排序算法。堆是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于(或者大于)它的父节点

## 堆：

 堆是一个树形结构，其实堆的底层是一棵完全二叉树。而完全二叉树是一层一层按照进入的顺序排成的

![](https://gitee.com/telei/picgoimage/raw/master//img/image-20210302130832863.png)

## 大顶堆与小顶堆

### 大顶堆原理：

根结点（亦称为堆顶）的关键字是堆里所有结点关键字中最大者，称为大顶堆。大顶堆要求根节点的关键字既大于或等于左子树的关键字值，又大于或等于右子树的关键字值。   

### 小顶堆原理：

根结点（亦称为堆顶）的关键字是堆里所有结点关键字中最小者，称为小顶堆。小堆堆要求根节点的关键字既小于或等于左子树的关键字值，又小于或等于右子树的关键字值。

![大顶堆-小顶堆](https://img2018.cnblogs.com/blog/1469176/201903/1469176-20190329000535427-914276300.png)

## 堆排序的基本思想：

![](https://gitee.com/telei/picgoimage/raw/master//img/image-20210302131317321.png)

## 堆排序动画演示：

![堆排序动画演示](https://img2018.cnblogs.com/blog/1469176/201903/1469176-20190329000555410-1254067522.gif)

## 堆排序图解：

![案例1](https://img2018.cnblogs.com/blog/1469176/201903/1469176-20190329000537287-1110479343.png)

## 代码实现：

```java
import org.junit.validator.PublicClassValidator;

import java.util.Arrays;

/**
 * @Author cxy
 * @Date 2021/3/2 21:55
 * @Version 1.0
 */
public class HeapSort {
    public static void main(String[] args) {
        //将数组进行升序排序
        int [] arr = {4, 6, 8, 5, 9};
        heapSort(arr);
    }

    //堆排序的方法
    public static void heapSort(int [] arr){
        int temp = 0;
        System.out.println("堆排序=====");

//        adjustHeap(arr, 1, arr.length);
//        System.out.println("第一次排序:"+ Arrays.toString(arr));
//        adjustHeap(arr, 0, arr.length);
//        System.out.println("第二次排序:"+ Arrays.toString(arr));

        //将无序数组构建成一个堆
        for (int i = arr.length / 2 - 1; i >= 0; i--) {
            adjustHeap(arr, i, arr.length);
        }

        for (int j = arr.length - 1; j > 0; j--) {
            temp = arr[j];
            arr[j] = arr[0];
            arr[0] = temp;
            adjustHeap(arr, 0, j);
        }

        System.out.println(Arrays.toString(arr));

    }

    /**
     *
     *
     * @param arr       待调整的数组
     * @param i         非叶子节点在数组中的索引
     * @param length    对多少个元素继续调整，逐渐减少
     * 将一个数组(二叉树)，调整为大顶堆
     */
    public static void adjustHeap(int [] arr, int i, int length){
        int temp = arr[i];//取出当前元素保存在临时变量
        //j = i * 2 + 1 | j是i的左子节点
        for (int j = i * 2 + 1; j < length; j = j * 2 + 1) {
            if (j+1 < length && arr[j] < arr[j+1]) {//左子节点的值小于右子节点的值
                j++; // j 指向右子节点
            }
            if (arr[j] > temp) {//如果子节点大于父节点
                arr[i] = arr[j];//将较大的值赋给当前节点
                i = j;          //i 指向 j 继续循环比较
            }else {
                break;
            }
            //for结束后，已经将以 i 为父节点的数的最大值 放在了最顶（局部）
            arr[i] = temp;//将temp的值放到调整后的位置
        }
    }
}

```







