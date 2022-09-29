#Java数组
##声明和初始化
###动态初始化
~~~Java
//声明同时分配好存储空间
int[] array = new int[5];
//先声明再分配存储空间
int[] array;
array = new int[5];
~~~
###静态初始化
~~~Java
int[] array = {1,2,3,4,5}
~~~
**注意：**
数组创建后没有赋值，会采用默认值
|数组类型|默认初始值|
|---|---|
|整形|0|
|浮点数类型|0.0|
|boolean|false|
|引用类型|null|
##数组的赋值机制
对于一般数据类型的赋值是值传递，数组类型的数据是引用类型，在赋值时是引用传递
<img src="img/屏幕截图%202022-07-06%20165922.jpg">
##数组的拷贝
在前面说过，由于数组在赋值时是引用传递，所以在更改其中一个数组时，另外一个数组也会发生变化，因为它们都指向了同一块存储区域。如果需要拷贝一份相同的数组，并且两个数组不会互相影响，可以使用java.util.Arrays的copyof()方法

~~~Java
//拷贝数组
int[] array = {1,2,3,4,5};
int[] newarray = Arrays.copyof(array,array.length);
//数组扩容
int[] newarray = Arrays.copyof(array,array.length*2);
~~~

##数组排序
除了拷贝数组，Arrays还提供了数组排序方法sort()
~~~Java
Arrays.sort(array);
~~~

##数组for-each循环
相比于for循环，for-each更加便利，这种方式能避免使用下标带来的错误
~~~java
String[] books = {"射雕英雄出","天龙八部","笑傲江湖","神雕侠侣"}
for(String book:books)
{
    System.out.println(book);
}
~~~
##二维数组
不同于C和C++的二维数组时连续分配存储空间的，Java的二维数组是非连续分配的，因此Java的二维数组不要求每一维的大小都相同。实质上，Java的二维数组还是一维数组，只是每一个数据都是低一维数组这种引用类型。
~~~java
//定义一个二维的3*3数组
int [][] twoDArray = new int[3][3];
//可以只指定一个维度大小，另一维可以在之后再分配空间
int [][] towDArray = new int[5][]
for(int i=0;i<5;i++)
{
    twoDArray[i]=new int[i+1];
}
~~~
最后附上一个简单的二维数组的应用案例：
**使用二维数组打印杨辉三角**
~~~java
public class array01 {
    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        System.out.println("please input lines of Yang Hui Triangle");
        int lines = input.nextInt();
        int [][] array = new int [lines][];
        array[0] = new int[] {1};//初始化第一行
        for(int i = 1; i < lines ;i++){
            array[i] = new int[i+1];//初始化一行数据
            array[i][0] = 1;
            for(int j=1; j<array[i].length; j++){
                if(j==array[i].length-1){
                    array[i][j] = 1;
                }
                else {
                    array[i][j] = array[i-1][j-1]+array[i-1][j];//根据杨辉三角的定义求解
                }
            }
        }
        for(int [] line:array){
            System.out.println(Arrays.toString(line));//以字符串形式输出一行的数据
        }
    }
}
~~~