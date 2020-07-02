
```说明：```

```singlexxx为单独使用日志的示例```

```slf4xxx为实现统一接口后的示例```

```combinexxx为合并使用的示例```

```
    在single示例中， log4j， log4j2都是默认为自己的log实现，没有实现标准。
    而logback则默认实现了slf4j的统一日志接口
    为了支持slf4j以统一接口，需要引入如下依赖：
    log4j:
                <dependency>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                    <version>1.7.25</version>
                </dependency>
              
    log4j2:
                <dependency>
                        <groupId>org.apache.logging.log4j</groupId>
                        <artifactId>log4j-slf4j-impl</artifactId>
                        <version>2.11.0</version>
                    </dependency>
    并且改代码中的LOGGER为slf4j接口Logger
```

```
    combineslf4juse中，引入的模块都是使用了实现了slf4j Logger接口的logger，由于存在多个bindings会根据类加载的顺序去打印
    这时候是会出现红色的警告的，如果想要去除警告，就需要在依赖中把其中的slf4j依赖exclude掉：
     <dependency>
         <groupId>log</groupId>
         <artifactId>slf4j4log4j</artifactId>
         <version>1.0-SNAPSHOT</version>
         <exclusions>
            <exclusion>
               <groupId>orh.slf4j</groupId>
               <artifactId>slf4j-log4j12</artifactId>
            </exclusion>
         </exclusions>
     </dependency>
    
    如果需要统一打印格式，添加或复制一个日志配置文件（bindings中的），来统一打印输出
    
    
    combinewithslfornot中，出现了没有实现slf4j接口的两个模块singlelog4j和singlelog4j2，如果执行main方法，
    他们的日志格式是不统一的，这时候就需要通过引入依赖：
    log4j:        
            <dependency>
                <groupId>org.slf4j</groupId>
                <artifactId>log4j-over-slf4j</artifactId>
                <version>1.7.25</version>
            </dependency>
            用于排除掉log4j的依赖，项目会最后加载这个依赖。发现找不到就不引入而去使用上面的log4j-over-slf4j
            <dependency>
                <groupId>log4j</groupId>
                <artifactId>log4j</artifactId>
                <version>99.99.99</version>
            </dependency>
    log4j2:
            <dependency>
                <groupId>org.apache.logging.log4j</groupId>
                <artifactId>log4j-to-slf4j</artifactId>
                <version>2.13.3</version>
            </dependency>
            <dependency>
                <groupId>org.apache.logging.log4j</groupId>
                <artifactId>log4j-core</artifactId>
                <version>99.99.99</version>
            </dependency>
    
    这是就可以统一日志输出了
```