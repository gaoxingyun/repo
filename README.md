# maven仓库

#### 功能
- 在使用java编程过程中，经常遇到maven中央仓库中没有的jar包，如oracle驱动，各种sdk，自己写的jar包等。这种情况一般使用本地仓库或依赖本地文件，协作开发或更换电脑时，需要重新安装jar包到本地，非常麻烦。公司中一般使用nexus搭建私服来解决私有jar的问题，但是个人开发的情况下，使用云主机搭建私服成本很高，其实github同样可以作为maven仓库使用。


#### 使用
###### 添加自定义maven仓库到pom.xml文件中。
- maven
```xml
<project>
<!--Add repositories-->
 <repositories>
     <repository>
         <id>github-gaoxingyun-repo-mvn-repo</id>
         <name>github-gaoxingyun-maven-repo</name>
         <url>https://raw.github.com/gaoxingyun/repo/mvn-repo/</url>
     </repository>
 </repositories>
</project>
```
- gradle
```groovy
maven{ url 'https://raw.github.com/gaoxingyun/repo/mvn-repo'}
```


#### 搭建过程

1. 在github中创建一个新的仓库repo。
2. 进入本地maven仓库目录${HOME}/.m2/repository，初始化git仓库，添加远程仓库地址，并默认忽略所有文件。
```shell
cd ~/.m2/repository;# 进入maven仓库目录
git init;# 初始化目录为git仓库
git remote add origin git@github.com:gaoxingyun/repo.git;# 添加远程仓库地址
echo "*" >> .gitignore; # 创建并添加*到.gitignore文件，默认忽略所有文件。
git add .gitgnore;
git commit -m "init maven repository";
```
3. 安装本地jar文件到本地仓库，添加本地maven仓库中文件到git分支下，并推送到远程仓库。
```shell
mvn install:install -file -DgroupId=com.oracle -DartifactId=ojdbc14 -Dversion=9.0.2.0.0 -Dpackaging=jar -Dfile=ojdbc14.jar# 使用maven install命令安装jar文件到本地仓库
git add -f com/oracle;# 添加文件到git本地仓库
git commit -m "add oracle driver";
git push origin mvn-repo;# 推送本地git分支到远程mvn-repo分支
```
