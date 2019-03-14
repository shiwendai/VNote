# @Scheduled创建定时任务
SpringBoot为我们内置了定时任务，我们只需要一个注解就可以开启定时为我们所用了。

文章开头我说到了SpringBoot为我们内置了`@Scheduled`定时任务，下面我们就来配置下这个注解，找到入口程序`SolrApplication`添加注解`@EnableScheduling`，如
```
@SpringBootApplication
@EnableScheduling
public class SolrApplication {

	@RequestMapping("/hello")
	public String hello(){
		return "hello world";
	}

	public static void main(String[] args) {
		SpringApplication.run(SolrApplication.class, args);
	}
}
```
可以看到上图2内我们添加注解后SpringBoot就已经认定了我们要使用定时任务来完成一些业务逻辑了，内部会对应原始配置定时任务添加对应的配置文件。

## @Scheduled
`@scheduled`注解用来配置到方法上来完成对应的定时任务的配置，如执行时间，间隔时间，延迟时间等等，下面我们就来详细的看下对应的属性配置。

我们先来创建一个测试的定时任务实体，如
```
@Component
public class PrintTask {

	@Scheduled(cron = "0 10 * * * *")
	public void cron() throws Exception{
		System.out.println("执行测试cron时间：" + new Date(System.currentTimeMillis()));
	}




}
```
## cron属性
这是一个时间表达式，可以通过简单的配置就能完成各种时间的配置，我们通过CRON表达式几乎可以完成任意的时间搭配，它包含了六或七个域：

Seconds : 可出现", - * /"四个字符，有效范围为0-59的整数
Minutes : 可出现", - * /"四个字符，有效范围为0-59的整数
Hours : 可出现", - * /"四个字符，有效范围为0-23的整数
DayofMonth : 可出现", - * / ? L W C"八个字符，有效范围为0-31的整数
Month : 可出现", - * /"四个字符，有效范围为1-12的整数或JAN-DEc
DayofWeek : 可出现", - * / ? L C #"四个字符，有效范围为1-7的整数或SUN-SAT两个范围。1表示星期天，2表示星期一， 依次类推
Year : 可出现", - * /"四个字符，有效范围为1970-2099年

下面简单举几个例子：

"0 0 12 * * ?"    每天中午十二点触发
"0 15 10 ? * *"    每天早上10：15触发
"0 15 10 * * ?"    每天早上10：15触发
"0 15 10 * * ? *"    每天早上10：15触发
"0 15 10 * * ? 2005"    2005年的每天早上10：15触发
"0 * 14 * * ?"    每天从下午2点开始到2点59分每分钟一次触发
"0 0/5 14 * * ?"    每天从下午2点开始到2：55分结束每5分钟一次触发
"0 0/5 14,18 * * ?"    每天的下午2点至2：55和6点至6点55分两个时间段内每5分钟一次触发
"0 0-5 14 * * ?"    每天14:00至14:05每分钟一次触发
"0 10,44 14 ? 3 WED"    三月的每周三的14：10和14：44触发
"0 15 10 ? * MON-FRI"    每个周一、周二、周三、周四、周五的10：15触发

## fixedRate属性
该属性的含义是上一个调用开始后再次调用的延时（不用等待上一次调用完成），这样就会存在重复执行的问题，所以不是建议使用，但数据量如果不大时在配置的间隔时间内可以执行完也是可以使用的
```
	@Scheduled(fixedRate = 1000*1)
	public void fixedRate() throws Exception{
		Thread.sleep(2000);
		System.out.println("执行测试fixedRate时间：" + new Date(System.currentTimeMillis()));
	}
```

##fixedDelay属性
该属性的功效与上面的fixedRate则是相反的，配置了该属性后会等到方法执行完成后延迟配置的时间再次执行该方法。
```
	@Scheduled(fixedDelay = 1000*1)
	public void fixedDelay() throws Exception{
		Thread.sleep(3000);
		System.out.println("执行测试fixeDelay时间：" + new Date(System.currentTimeMillis()));
	}
```

## initialDelay属性
该属性跟上面的fixedDelay、fixedRate有着密切的关系，为什么这么说呢？该属性的作用是第一次执行延迟时间，只是做延迟的设定，并不会控制其他逻辑，所以要配合fixedDelay或者fixedRate来使用
```
	/**
	 * @throws Exception
	 */
	@Scheduled(initialDelay = 1000*10, fixedDelay = 1000*2)
	public void initialDelay() throws Exception{
		System.out.println("执行测试initalDelay时间: " + new Date(System.currentTimeMillis()));
	}
```
## 总结
上述内容就是本章的所有讲解内容，本章主要讲解了SpringBoot项目内的定时任务如果配置使用，上述的属性是我们实际项目中最常用到的，可根据项目实际情况进行选择配置。