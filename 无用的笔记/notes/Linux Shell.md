# Linux下的Shell编程
## 基础学习
### 进程管理工具(最重要的)
#### 进程查询
**查询所有的进程**
```shell
ps aux
```

**查询当前用户的进程**
```shell
ps ux
```

**查询用户war的进程**
```shell
ps -ef | grep war
ps -lu war
```

**查询所有进程的数目**
```shell
ps aux | wc -l
```

**查找进程id(适合只记得部分的进程字段)**
```shell
pgrep -l re
# 查找进程名中含有re的进程
```

**以完整的格式显示所有的进程**
```shell
ps -ajx
```

**显示进程的信息，并实时更新**
```shell
top
```

**查看端口占用的进程状态**
```shell
lsof -i:3306
```

**查看用户username的进程所打开的文件**
```shell
lsof -u username
```

**查询init进程当前打开的文件**
```shell
lsof -c init
```

**查询指定的进程ID(23295)打开的文件**
```shell
lsof -p 23295
```

**查询指定目录下被进程开启的文件(使用+D递归目录)**
```shell
lsof +d mydir1/
```

#### 终止进程
**杀死指定PID的进程**
```shell
kill PID
```

**杀死相关进程**
```shell
kill -9 3434
```

**杀死job工作**
```shell
kill %job
```

#### 进程监控
```shell
top
P:根据CPU使用百分比大小进行排序
M：根据驻留内存大小进行排序。
i：使top不显示任何闲置或者僵死进程。
```

#### 分析线程栈
pmap命令

#### 综合运用
```shell
# 将用户war下的所有进程名以av_开头的进程终止
ps -u war |  awk '/av_/ {print "kill -9 " $1}' | sh

# 将用户war下所有进程名中包含HOST的进程终止:
PS -ef｜ grep war| grep HOST | awk '{print $2}' | xargs kill -9
```


## 每日一个小命令
**2023/4/17**
```shell
# 找到当前目录下最大的3个文件
du -a . | sort -n -r | head -n 3
```


**2023/7/20**
```shell
# 找到当前目录和子目录下所有扩展名为.md的文件的行数
find . -name "*.md" | xargs wc -l
```