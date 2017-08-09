# flume-file-sink_header-to-filename
这个sink可以根据header中的不同的value作为文件名，创建多个不同文件。每天会生成一个文件

#sources desc
a1.sources.r1.type=avro
a1.sources.r1.bind=0.0.0.0
a1.sources.r1.port=22223

a1.sources.r1.interceptors =i5

a1.sources.r1.interceptors.i5.type = regex_extractor
a1.sources.r1.interceptors.i5.regex = (?<=\\{type:)(.+?)(?=\\})
a1.sources.r1.interceptors.i5.serializers = s1
a1.sources.r1.interceptors.i5.serializers.s1.name = log_type

#-----bydayfile
a1.sinks.k1.type = flume.mysink.RollingByTypeAndDayFileSink
a1.sinks.k1.sink.directory =  /usr/local/software/apache-flume-1.7.0-bin/log_file
a1.sinks.k1.sink.headerkeyname = log_type
a1.sinks.k1.sink.rollInterval = 0
#-----------------------


以上是部分配置文件，在i5拦截器中会拦截body中 {type:aaa} 这样的字符串并且放入到header中，header中会增加  log_type=aaa 这样的键值对，
那么这类日志生成的文件名就是aaa.20170808 这样的日志文件。 
应用场景：
在同一tomcat日志中输出的日志信息，由于类型不同,在传输后会分发到不同的日志文件中，
 body中 添加{type:aaa} 发送到aaa文件
 body中 添加{type:bbb} 发送到bbb文件

我编译时的pom文件。
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <flume.version>1.7.0</flume.version>
  </properties>

  <dependencies>
    <dependency>  
      <groupId>org.apache.flume</groupId>  
      <artifactId>flume-ng-sdk</artifactId>  
      <version>${flume.version}</version>  
    </dependency> 
    <dependency>  
      <groupId>org.apache.flume</groupId>  
      <artifactId>flume-ng-core</artifactId>  
      <version>${flume.version}</version>  
    </dependency>  
    <!-- https://mvnrepository.com/artifact/com.google.guava/guava -->
    <dependency>
        <groupId>com.google.guava</groupId>
        <artifactId>guava</artifactId>
        <version>19.0</version>
    </dependency>
  </dependencies>
  
  欢迎大家提出问题，结局问题，提交，维护..。
