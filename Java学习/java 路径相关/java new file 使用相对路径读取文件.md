# java new file 使用相对路径读取文件
## 1. java project环境，使用java.io用相对路径读取文件的例子：
```
     *目录结构：
      DecisionTree
                |___src
                     |___com.decisiontree.SamplesReader.java
                |___resource
                     |___train.txt,test.txt
                     
     *SamplesReader.java:
      String filepath="resource/train.txt";//注意filepath的内容；
      File file=new File(filepath);
      ……
```
我们留意filepath的内容，java.io默认定位到当前用户目录("user.dir")下，即：工程根目录"D:DecisionTree"下，因此，此时的相对路径(以user.dir为基路径的路径)为"resource/train.txt"。这样，JVM就可以据"user.dir"与"resource/train.txt"得到完整的路径（即绝对路径）"D:DecisionTreeresourcetrain.txt"，从来找到train.txt文件。

注意：相对路径的起始处无斜杆"/";例如：
filepath="resource/train.txt";
而不是filepath="/resource/train.txt"; //error!

## 2、javaEE环境，使用Classloader用相对路径读取xml的例子：
 *参见之前写的文章“通过虚拟路径或相对路径读取一个xml文件，避免硬编码”。
 
 *内容如下：
 java使用相对路径读取xml文件：
    一、xml文件一般的存放位置有三个：
        1.放在WEB-INF下；
        2.xml文件放在/WEB-INF/classes目录下或classpath的jar包中；
        3.放在与解析它的java类同一个包中，不一定是classpath；
        
        
   二、相对应的两种使用相对路径的读取方法：
   
        方法一：（未验证）
            将xml文件放在WEB-INF目录下，然后
            程序代码：
            InputStream is=getServletContext().getResourceAsStream( "/WEB-INF/xmlfile.xml" );
            
        方法二：将xml文件放在/WEB-INF/classes目录下或classpath的jar包中，则可以使用ClassLoader的静态
            方法getSystemResourceAsStream(String s)读取；
            程序代码：
            String s_xmlpath="com/spf/web/ext/hotspot/hotspotxml/hotspot.xml";
            InputStream in=ClassLoader.getSystemResourceAsStream(s_xmlpath);
            
        方法三：xml在随意某个包路径下
            String s_xmlpath="com/spf/web/ext/hotspot/hotspotxml/hotspot.xml";
            ClassLoader classLoader=HotspotXmlParser.class.getClassLoader();
            InputStream in=classLoader.getResourceAsStream(s_xmlpath);



```
FileTest.class.getResource在所有项目上都好用：

1，FileTest.class.getResource("")
得到的是当前类FileTest.class文件的URI目录。不包括自己！
如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/com/test/

2，FileTest.class.getResource("/")
得到的是当前的classpath的绝对URI路径。
如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/

3，Thread.currentThread().getContextClassLoader().getResource("")
得到的也是当前ClassPath的绝对URI路径。
如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/

4，FileTest.class.getClassLoader().getResource("")
得到的也是当前ClassPath的绝对URI路径。
如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/

5，ClassLoader.getSystemResource("")
得到的也是当前ClassPath的绝对URI路径。
如：file:/D:/java/eclipse32/workspace/jbpmtest3/bin/
```
