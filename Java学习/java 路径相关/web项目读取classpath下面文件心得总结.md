# web项目读取classpath下面文件心得总结

首先分两大类按web容器分类

## 一、普通的web项目,如Tomcat容器
特点是压缩包随着容器的启动会解压缩成一个文件夹，项目访问的时候，实际是去访问文件夹，而不是jar或者war包。
这种的无论你是用获取路径的方法
```
        this.getClass().getResource("/")+fileName
```
获取流的方法
```
        this.getClass().getResourceAsStream(failName);
```
屡试不爽，都行，这种没什么可注意的，大多数项目都是这种。

上方便的工具类吧

```
import org.springframework.util.ResourceUtils;

File file= ResourceUtils.getFile("classpath:test.txt");
```
或者

```
ClassPathResource classPathResource = new ClassPathResource("test.txt");
//获取文件
classPathResource .getFile();
//获取文件流
classPathResource .getInputStream();
```



## 二、 内嵌web容器
其特点是只有一个jar文件，在容器启动后不会解压缩，项目实际访问时jar包或者war包

**这种最容易遇坑，最大的坑就是，用第一种方式读取，在eclipse，本地调试，完美运行，到linux环境下，就GG**

线上内嵌的工程，我们只会放一个jar文件上去，jar里面的路径是获取不到的，jar是封闭性东西吧，不像文件夹，总不能c:/home/xx.jar/file.txt。
**总结：读取jar里面的文件，我们只能用流去读取，不能用file，文件肯定要牵扯路径**

jar里面文件读取方式：
```
ClassPathResource classPathResource = new ClassPathResource("test.txt");
//获取文件流
classPathResource .getInputStream();
```
