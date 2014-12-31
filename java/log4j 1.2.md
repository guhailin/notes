##log4j定义配置文件

**log4j由三个重要的组件构成 ：**

-  日志信息的优先级，从高到低有：ERROR、WARN、 INFO、DEBUG，分别用来指定这条日志信息的重要程度；

-  日志信息的输出目的地，指定了日志将打印到控制台还是文件中；

-  日志信息的输出格式，控制了日志信息的显示内容。

**配置根Logger，其语法为 ：**

 log4j.rootLogger=[ level ] , appenderName, appenderName, …

level 是日志记录的优先级，分为OFF、FATAL、ERROR、WARN、INFO、DEBUG、ALL或者您定义的级别。Log4j建议只使用四个级别，优先级从高到低分别是ERROR、WARN、INFO、DEBUG。通过在这里定义的级别，您可以控制到应用程序中相应级别的日志信息的开关，只会打印定义级别及高于定义级别的日志。比如在这里定义了INFO级别，则应用程序中所有DEBUG级别的日志信息将不被打印出来。
 appenderName就是指日志信息输出到哪个地方。您可以同时指定多个输出目的地。
 
例如：
log4j.rootLogger=INFO, CONSOLE, LOGFILE

**配置日志信息输出目的地Appender，其语法为：**

log4j.appender.appenderName=[ fully.qualified.name.of.appender.class ]

**Log4j提供的appender有以下几种：**

- org.apache.log4j.ConsoleAppender，将日志打印到控制台

- org.apache.log4j.FileAppender，将日志输出到指定文件

- org.apache.log4j.DailyRollingFileAppender，每天产生一个日志文件

- org.apache.log4j.RollingFileAppender，文件大小到达指定尺寸的时候产生一个新的文件

- org.apache.log4j.WriterAppender，将日志信息以流格式发送到任意指定的地方
 
例如：
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender


###ConsoleAppender 选项
Threshold=WARN： 指定日志消息的输出最低层次  
ImmediateFlush=true： 默认值是true,意谓着所有的消息都会被立即输出  
Target=System.err： 默认情况下是：System.out,指定输出控制台

###FileAppender 选项
 
Threshold=WARN： 指定日志消息的输出最低层次。  
ImmediateFlush=true： 默认值是true,意谓着所有的消息都会被立即输出。  
File=mylog.txt： 指定消息输出到mylog.txt文件。  
Append=false： 默认值是true,即将消息增加到指定文件中，false指将消息覆盖指定的文件内容。    

###DailyRollingFileAppender 选项

Threshold=WARN： 指定日志消息的输出最低层次  
ImmediateFlush=true： 默认值是true,意谓着所有的消息都会被立即输出  
File=mylog.txt： 指定消息输出到mylog.txt文件  
Append=false： 默认值是true,即将消息增加到指定文件中，false指将消息覆盖指定的文件内容  
DatePattern='.'yyyy-ww： 每周滚动一次文件，即每周产生一个新的文件。当然也可以指定按月、周、天、时和分  
即对应的格式如下： 

1. yyyy-MM： 每月
2. yyyy-ww： 每周 
3. yyyy-MM-dd： 每天
4. yyyy-MM-dd-a： 每天两次
5. yyyy-MM-dd-HH： 每小时
6. yyyy-MM-dd-HH-mm： 每分钟

###RollingFileAppender 选项

Threshold=WARN： 指定日志消息的输出最低层次  
ImmediateFlush=true： 默认值是true,意谓着所有的消息都会被立即输出  
File=mylog.txt： 指定消息输出到mylog.txt文件  
Append=false： 默认值是true,即将消息增加到指定文件中，false指将消息覆盖指定的文件内容  
MaxFileSize=100KB： 后缀可以是KB, MB 或者是 GB. 在日志文件到达该大小时，将会自动滚动，即将原来的内容移到mylog.log.1文件  
MaxBackupIndex=2： 指定可以产生的滚动文件的最大数  

**配置日志信息的格式（布局），其语法为：**
 
log4j.appender.appenderName.layout=[ fully.qualified.name.of.layout.class ]
 
Log4j提供的layout有以下几种：   
　　org.apache.log4j.HTMLLayout，以HTML表格形式布局  
　　org.apache.log4j.PatternLayout，可以灵活地指定布局模式  
　　org.apache.log4j.SimpleLayout，包含日志信息的级别和信息字符串  
　　org.apache.log4j.TTCCLayout，包含日志产生的时间、线程、类别等等信息  
 
例如：
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout

**配置格式化信息，其语法为 ：**
 
log4j.appender. appenderName.layout.ConversionPattern= XXX XXX XXX
 
log4j提供的打印参数如下：  

－X号: X信息输出时左对齐；   
%p： 输出日志信息优先级，即DEBUG，INFO，WARN，ERROR，FATAL,  
%d： 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyy MMM dd HH:mm:ss,SSS}，输出类似：2002年10月18日 22：10：28，921  
%r ： 输出自应用启动到输出该log信息耗费的毫秒数  
%c ： 输出日志信息所属的类目，通常就是所在类的全名  
%t ： 输出产生该日志事件的线程名  
%l  ：  输出日志事件的发生位置，相当于%C.%M(%F:%L)的组合,包括类目名、发生的线程，以及在代码中的行数。举例：Testlog4.main(TestLog4.java:10)  
%x ： 输出和当前线程相关联的NDC(嵌套诊断环境),尤其用到像java servlets这样的多客户多线程的应用中。  
%%： 输出一个"%"字符  
%F ： 输出日志消息产生时所在的文件名称  
%L ： 输出代码中的行号  
%m： 输出代码中指定的消息,产生的日志具体信息  
%n ： 输出一个回车换行符，Windows平台为"\r\n"，Unix平台为"\n"输出日志信息换行  
 
**可以在%与模式字符之间加上修饰符来控制其最小宽度、最大宽度、和文本的对齐方式 。如 ：**  
      1)%20c：指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，默认的情况下右对齐。  
      2)%-20c:指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，"-"号指定左对齐。  
      3)%.30c:指定输出category的名称，最大的宽度是30，如果category的名称大于30的话，就会将左边多出的字符截掉，但小于30的话也不会有空格。  
      4)%20.30c:如果category的名称小于20就补空格，并且右对齐，如果其名称长于30字符，就从左边交远销出的字符截掉。  

例如：  
log4j.appender. CONSOLE.layout.ConversionPattern=%d{yyyyMMdd-HH:mm:ss} %t %c %m%n

配置实例：
在工程目录src下创建名为 “log4j.properties” 文件，内容如下：


```
#Log4J配置文件实现了输出到控制台、文件、回滚文件、自定义标签等功能。仅供参考。   
log4j.rootLogger=DEBUG,CONSOLE,FILE,DLOGFILE,ROLLING_FILE  
log4j.addivity.org.apache=true   
#应用于控制台   
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender   
log4j.appender.CONSOLE.Threshold=DEBUG   
log4j.appender.CONSOLE.Target=System.out   
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout   
log4j.appender.CONSOLE.layout.ConversionPattern=%d{yyyyMMdd-HH:mm:ss} %t %c %m%n  
#应用于文件   
log4j.appender.FILE=org.apache.log4j.FileAppender   
log4j.appender.FILE.File=file.log   
log4j.appender.FILE.Append=false   
log4j.appender.FILE.layout=org.apache.log4j.PatternLayout   
log4j.appender.FILE.layout.ConversionPattern=%d{yyyyMMdd-HH:mm:ss} %t %c %m%n    
#应用于按日期生成文件   
log4j.appender.DLOGFILE=org.apache.log4j.DailyRollingFileAppender  
log4j.appender.DLOGFILE.File=c:\\test.log  
log4j.appender.DLOGFILE.Threshold=INFO  
log4j.appender.DLOGFILE.DatePattern='.'yyyy-MM-dd  
log4j.appender.DLOGFILE.layout=org.apache.log4j.PatternLayout  
log4j.appender.DLOGFILE.layout.ConversionPattern=%d{yyyyMMdd-HH:mm:ss} %t %c %m%n   
#应用于文件回滚   
log4j.appender.ROLLING_FILE=org.apache.log4j.RollingFileAppender   
log4j.appender.ROLLING_FILE.Threshold=ERROR   
log4j.appender.ROLLING_FILE.File=rolling.log  //文件位置,也可以用变量${java.home}、rolling.log  
log4j.appender.ROLLING_FILE.Append=true       //true:添加  false:覆盖  
log4j.appender.ROLLING_FILE.MaxFileSize=10KB   //文件最大尺寸  
log4j.appender.ROLLING_FILE.MaxBackupIndex=1  //备份数  
log4j.appender.ROLLING_FILE.layout=org.apache.log4j.PatternLayout   
log4j.appender.ROLLING_FILE.layout.ConversionPattern=%d{yyyyMMdd-HH:mm:ss} %t %c %m%n    
#自定义Appender   
# log4j.appender.im = net.cybercorlin.util.logger.appender.IMAppender   
# log4j.appender.im.host = mail.cybercorlin.net   
# log4j.appender.im.username = username   
# log4j.appender.im.password = password   
# log4j.appender.im.recipient = corlin@cybercorlin.net   
# log4j.appender.im.layout=org.apache.log4j.PatternLayout   
# log4j.appender.im.layout.ConversionPattern =[framework] %d - %c -%-4r [%t] %-5p %c %x - %m%n 
```