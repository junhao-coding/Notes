#IO流
##流的分类
* 按操作数据不同，分为字节流和字符流。
* 按数据流的流向分为输出流和输入流。
* 按流的角色分为节点流和包装流。
  
|抽象基类|字节流|字符流|
|--|--|--|
|输入流|InputStreanm|Reader|
|输出流|OutputStreanm|Writer|
Java中跟io有关的类非常多，但是类的命名非常规则，所有的派生子类都是以上述四个基类作为后缀名来命名的。

##文件流
###FileInputStream
**FileInputStream** 用于从文件中按字节读取，一般针对视频，音频，图像等文件进行读取，使用read()方法，返回类型是int，如果读到文件末尾则放回-1。
~~~Java
read() //读取一个字节数据
read(Byte b[]) //最多读取b.length个字节数据
read(Byte b[], int off, int len) //最多读取len个字节数据到数组的off偏移位置
~~~
有时我们按照字节读取数据输出到控制台，**当最后一次读取时，字节数组不会被全部覆盖**，如果这时候直接用字节数组初始化一个字符串输出，那么**输出的内容会和期望的不一致**，所以采用读取多长就输出多长的方式。
~~~Java
while((readLen = stream.read(data)) != -1) {
    System.out.print(new String(data, 0, readlen));
}
~~~
###FileOutStream
**FileOutputStream**用于按字节写入内容到文件中，文件不存在时会自动创建文件，使用write()方法，写入文件有两种情况，一个时追加写，一个时覆盖写。
~~~Java
new FileOutputStream(filepath, true);//追加写
new FileOutputStream(filepath);//覆盖写
~~~
write()方法和前面的read()参数一致，同时也要注意一个问题，按照读取的长度来向文件写入数据。字符串转字节数组使用getBytes()方法

**注意：当完成数据的读写时，一定要关闭流，释放资源**

###FileReader
**FileReader** 是 **InputStramReader(转换流)** 的子类用于按字符读取文件内容，一般用于文本文件的处理。同样也使用read()方法读取。FileReader内没有read()方法，而是在父类中定义的。
~~~Java
read();//读取一个字符
read(char[] cbuf);//读取cbuf.length长度的字符
readn(char[] cbuf, int offset, int length)//从offset偏移的位置开始读取最多length长度的字符到数组中
~~~
可以将字符数组转为字符串类型
~~~Java
new String(char[]);
new String(char[], int, int);
~~~
###FileWriter
**FileWriter** 是 **InputStramWriter(转换流)** 的子类用于按字符写入文件，FileWriter内也没有write()方法，而是在父类中定义的。
~~~Java
new FileReader(filepath, true);//追加写
new FileWriter(filepath);//覆盖写
write(int);//写入一个字符
write(char[])
write(char[], int, int)
write(String);//写入字符串
write(String, int, int);//写入字符串的指定长度
~~~
**注意：必须关闭流或者调用flush()方法才能将数据真正写入文件中**

##节点流和处理流
节点流指从特定的数据来源中读取数据，比如上述的四个文件流，还有访问管道和数组的流等，处理流也称为包装流，它连接一个节点流或者处理流来提供更强大的读写功能。
###缓冲流

####BufferdReader
在**BufferedReader**内有一个 **Reader** in，可以通过构造器用一个任意的Reader来初始化。
~~~Java
BufferedReader bf = new BufferedReader(new FileReader(filePath)) // 关联一个文件流
~~~
BufferdReader提供readLine()按行读取可以方便的读取文件内容。
####BufferdWriter
同理在BufferedWriter中也有一个 **Writer** out。
~~~Java
BufferedWriter bf = new BufferedWriter(new FileWriter(filePath)) // 关联一个文件流
~~~
向文件中吸入数据是需要换行可以使用Writer提供的**newLine()**;

* 对于包装流只需要关闭外部流，即关闭BufferedReader即可，在底层会关闭节点流。
* **BufferedInputStream**和**BufferedOutputStream**是字节流对应的缓冲流，用法不再做具体的说明

###对象流
有时我们保存一个数据，除了它的数值外还想保存它的类型，比如对于100，它可能是一个int类型，也有可能是字符串类型。再比如说，我们想要保存一个对象，它有多个对象，我们想要**保存这个对象的完整内容，并且可以从文件中恢复**。针对这个问题，Java提供了**对象流**。
####序列化和反序列化
* 保存数据时保存它的数据类型和值，称为**序列化**
* 通过文件恢复数据的类型和值称为 **反序列化**。
* 如果需要某个对象支持序列化，它的类必须实现**Serializabel**，这个接口是一个标记接口，没有实现方法。

####ObjectOutputStream
对象流也是包装流，创建对象时，也需要提供一个字节输出流初始化。
~~~Java
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath));
~~~
通过对象流可以向文件中写入基本数据类型的数据，字符串类型或者对象等
~~~Java
oos.writeInt(100);// int -> Integer (实现了 Serializable)
oos.writeBoolean(true);// boolean -> Boolean (实现了 Serializable)
oos.writeChar('a');// char -> Character (实现了 Serializable)
oos.writeDouble(9.5);// double -> Double (实现了 Serializable)
oos.writeUTF("yjh学java");//String
oos.writeObject(new Person("junhao", 21, "杭州"));
~~~

####ObjectInputStream
使用对象输入流可以从文件中**按照顺序**读取对象，这里读出的顺序必须和保存顺序是一致的。
~~~Java
System.out.println(ois.readInt());
System.out.println(ois.readBoolean());
System.out.println(ois.readChar());
System.out.println(ois.readDouble());
System.out.println(ois.readUTF());
System.out.println(ois.readObject());
~~~

####其它说明
* 序列化对象时，用关键字static和transient修饰的成员不会被序列化。
* 序列化对象的属性也必须实现了了Serializable。
* 序列化的类中条件SerialVersion可以提高版本兼容性。

###标准输入输出流
|标准输入输出流|类型|设备|
|--|--|--|
|System.out|OutputStream|显示器|
|System.in|InputStream|键盘|

###转换流
转换流是为了解决乱码的问题，通关转换流可以将字节流转换成字符流，将字符流转换成字符流。
####InputStreamReader
**InputStreamReader** 将字节输入流转换为字符输入流，并且可以指定编码方式，比如指定gbk编码这样就很好解决了读取数据输出后中文乱码问题。
~~~Java
//将文件字节输入流转为字符流，指定编码方式gbk
InputStreamReader isr = new InputStreamReader(new FileInputStream(filePath), "gbk");
//可以继续包装为BufferedReader
BufferedReader br = new BufferedReader(isr);
//可以直接写成这样
BufferedReader br = new BufferedReader(new InputStreamReader(
                                            new FileInputStream(filePath), "gbk"));
~~~

####OutputStreamWriter
**OutputStreamWriter** 将字节输出流转为字符输出流。
~~~Java
BufferedWriter bw = new BufferedWriter (new OutputStreamWriter(
                                            new FileOutputStream(filePath), "gbk"));
~~~

###打印流
默认情况下我们使用print函数，会输出内容到控制台上，而如果想要重定向输出，即想要输出内容到文件中，可以使用 **System.setOut(PrintStream out)** 方法。
~~~Java
//打印内容到指定文件中
System.setOut(new PrintStream("filePath"));
~~~

##Properties
在集合中介绍过这是专门用来读取配置文件的集合类，先看看配置文件的格式：**键=值** 中间不能有空格。用Properties类的load方法来加载文件信息。
~~~Java
//1. 创建 Properties 对象
Properties properties = new Properties();
//2. 加载指定配置文件
properties.load(new FileReader("src\\mysql.properties"));
~~~
|方法|说明|
|--|--|
|list()|显示数据到指定的设备(控制台System.out)|
|getProperty()|根据键获取值|
|setProperty()|设置键值对|
|store()|保存文件到配置文件中，中文存储为unicode码|