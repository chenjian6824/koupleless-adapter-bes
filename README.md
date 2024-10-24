# koupleless-adapter-bes
组件是作为 [Koupleless](https://github.com/koupleless/koupleless) 项目的一个扩展组件，目的是适配宝蓝德（BES)容器。

由于BES是商业软件，目前维护在个人仓库中。

项目目前仅在BES 9.5.5.004 版本中验证过，其他版本需要自行验证，必要的话需要根据相同的思路进行调整。

如果不需要使用同一端口来安装BIZ模块,只需要关注下文安装依赖章节提到的注意事项即可，不需要引入本项目相关的依赖。

## 快速开始
### 1. 安装依赖
首先需要确保已经在maven仓库中导入了BES相关的依赖。
   (这里有个关键点，由于koupleless 2.2.14项目对于依赖包的识别机制与BES的包结构冲突，需要将BES的依赖包加上sofa-ark-的前缀,具体的识别机制可参考koupleless的com.alipay.sofa.ark.container.model.BizModel类)

参考导入脚本如下：
```angular2html
mvn install:install-file -Dfile=D:/software/xc/BES-EMBED/sofa-ark-bes-lite-spring-boot-starter-9.5.5.004.jar -DgroupId=com.bes.besstarter -DartifactId=sofa-ark-bes-lite-spring-boot-starter -Dversion=9.5.5.004 -Dpackaging=jar

mvn install:install-file -Dfile=D:/software/xc/BES-EMBED/bes-gmssl-9.5.5.004.jar -DgroupId=com.bes.besstarter -DartifactId=bes-gmssl -Dversion=9.5.5.004 -Dpackaging=jar

mvn install:install-file -Dfile=D:/software/xc/BES-EMBED/bes-jdbcra-9.5.5.004.jar -DgroupId=com.bes.besstarter -DartifactId=bes-jdbcra -Dversion=9.5.5.004 -Dpackaging=jar

mvn install:install-file -Dfile=D:/software/xc/BES-EMBED/bes-websocket-9.5.5.004.jar -DgroupId=com.bes.besstarter -DartifactId=bes-websocket -Dversion=9.5.5.004 -Dpackaging=jar
```
### 2. 编译安装本项目插件
进入本项目的bes9-web-adapter 目录执行mvn install命令即可。
项目将会安装“bes-web-ark-plugin” 和 “bes-sofa-ark-springboot-starter” 两个模块。

### 3. 使用本项目组件
首先需要根据koupleless的文档，[将项目升级为Koupleless基座](https://koupleless.io/docs/tutorials/base-create/springboot-and-sofaboot/)

然后将依赖中提到的
```
<dependency>
    <groupId>com.alipay.sofa</groupId>
    <artifactId>web-ark-plugin</artifactId>
    <version>${sofa.ark.version}</version>
</dependency>
```
替换为本项目的坐标
```
<dependency>
    <groupId>com.alipay.sofa</groupId>
    <artifactId>bes-web-ark-plugin</artifactId>
    <version>${sofa.ark.version}</version>
</dependency>
<dependency>
   <groupId>com.alipay.sofa</groupId>
   <artifactId>bes-sofa-ark-springboot-starter</artifactId>
   <version>${sofa.ark.version}</version>
</dependency>
```

引入BES相关依赖（同时需要exclude tomcat的依赖）。参考依赖如下：
```angular2html
       <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>com.bes.besstarter</groupId>
            <artifactId>sofa-ark-bes-lite-spring-boot-starter</artifactId>
            <version>9.5.5.004</version>
        </dependency>

        <dependency>
            <groupId>com.bes.besstarter</groupId>
            <artifactId>bes-gmssl</artifactId>
            <version>9.5.5.004</version>
        </dependency>

        <dependency>
            <groupId>com.bes.besstarter</groupId>
            <artifactId>bes-jdbcra</artifactId>
            <version>9.5.5.004</version>
        </dependency>

        <dependency>
            <groupId>com.bes.besstarter</groupId>
            <artifactId>bes-websocket</artifactId>
            <version>9.5.5.004</version>
        </dependency>
```

### 4. 完成
完成上述步骤后，即可在Koupleless中使用BES启动项目。
