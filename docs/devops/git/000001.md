创建git父子模块管理项目

在远程先创立对应模块

按照单独创建project方式建立模块后 

1.（先进入项目文件夹）通过命令 git init 把这个目录变成git可以管理的仓库。

```
git init
```

2.把文件添加到版本库中，使用命令 git add .添加到暂存区里面去，不要忘记后面的小数点“.”，意为添加文件夹下的所有文件(夹)。

```
git add .
```

3.commit到主分支

```
git commit -m "描述" 
```

4.登录github，把本地仓库提交至远程仓库。

接下来你要做的就是复制那个地址，然后你将本地仓库个远程仓库连接起来。

```
git remote add origin git@github.com:yourname/仓库名.git  
```

5.进行第一次提交

```
git push -u origin master  
```

6.如果报'agriculture-share-common' already exists in the index ，执行下面删除后重新执行加入父模块命令

```
git rm -rf --cached agriculture-service-common
```
7.将子模块加入到父模块（在总项目里执行）

```
git submodule add git@code.aliyun.com:work_shop_daheyun/b2b2c-service-information.git b2b2c-service-information
```

8.更新总项目

```
git submodule update --init --recursive --remote
```

