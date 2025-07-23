# Linux 开发常用命令

```bash
# 1.1 文件与目录操作
# --------------------------
cp /usr/lib/jvm/ /tmp/arthas3.6.7/lib/   # 拷贝文件
cd /     # 进入根目录
cd ~     # 进入用户home目录
cd ..    # 返回上一级
cd -     # 返回上次目录
ls       # 基础列表
ls -l    # 详细列表
ls -a    # 显示隐藏文件
ls -lSh  # 按大小排序(带单位)
touch 1.txt 2.txt  # 同时创建两个文件
cat filename  # 读取文件内容
rm filename   # 删除文件
rm -rf dirname  # 强制递归删除目录
mv file1 file2  # 移动/重命名
pwd       # 显示当前路径
mkdir dirname  # 创建目录
find / -name fileName.xxx  # 查找文件
locate fileName.xxx  # 快速查找文件
stat FileName  # 查看文件详情
vi filename   # 文本编辑
vim filename  # 高级文本编辑

# 1.2 系统信息查询
# --------------------------
cat /proc/cpuinfo | grep 'process' | sort | uniq | wc -l  # 查看CPU核数
lscpu     # 查看CPU信息
free -h   # 查看内存使用
cat /proc/meminfo  # 查看内存详情
df -h     # 查看磁盘空间
df -h .   # 查看当前目录空间
df -hl    # 查看硬盘总容量
uname -r  # 查看内核版本
cat /etc/redhat-release  # 查看RedHat版本
who       # 查看登录用户
history   # 查看历史命令
apt list  # 显示安装包列表
cal -y    # 查看全年日历
exit      # 退出连接(Ctrl+D)

# ============================================
# 2. 高级操作
# ============================================

# 2.1 文件传输(SCP)
# --------------------------
scp /local/path/file root@IP:/remote/path  # 上传到Linux
scp root@IP:/remote/path /local/path       # 下载到本地
# 实际案例：
scp root@10.175.94.170:/data/gc/log/gc.log /C:/tmp/
scp root@10.175.94.169:/data/logs/backup.gz /C:/tmp/

# 2.2 权限管理
# --------------------------
chmod 744 filename  # rwxr--r--
chmod 644 filename  # rw-r--r--
chmod +x script.sh  # 添加执行权限

# 2.3 压缩包处理
# --------------------------
gzip -d file.gz     # 解压gzip(不保留原文件)
unzip package.zip   # 解压zip文件

# 2.4 VIM编辑
# --------------------------
vim filename  # 编辑文件
# 操作流程：
# 1. 按 i 进入插入模式
# 2. 编辑内容
# 3. 按 ESC 退出插入模式
# 4. 输入 :wq 保存退出

# ============================================
# 3. 系统诊断
# ============================================

# 3.1 进程分析
# --------------------------
top        # 查看所有进程
top -Hp pid  # 查看指定进程线程
ps -ef | grep PID  # 定位Java应用
printf "%x\n" 42818  # 十进制转十六进制
jstack 42818 | grep a742 -A 100  # 查看线程堆栈
```

# Windows 开发常用命令

```bash
# 进程相关
netstat -aon                 # 查看所有端口
netstat -an | findstr "8080" # 筛选指定端口
taskkill /PID 1234 /F        # 强制结束进程

# 网络测试
ping domain.com       # 网络连通性测试
telnet domain.com 443 # 端口测试
curl -vk https://domain.com  # HTTP请求测试

# JVM分析:本地生成堆快照hprof文件的相关指令-需要确保安装完整的jdk
jps -l # 显示当前运行的进程信息，可以获取对应应用的进程ID
jmap -dump:file=C:/tmp/dump.hprof 4040 # 导出对应进程ID(4040)的堆快照文件到指定的位置
使用jprofiler工具分析 # 用来定位可能引起OOM的大对象	
```

# 项目开发过程中问题汇总

## 数据库

oracle数据库模糊查找字段时，由于系统环境变量没有设值NLS_lAN,导致查询失败

## 平台|工具

### STS

```tex
本地连接测试/正式环境调试项目时配置如下：
    需要在STS中具体项目打开config后的arguments栏位下配置:--spring.profiles.active=prod-local/test2-local
注意：目前xylon框架下:eureka,config服务直接启动不用复制副本配置,framework,cas,ups-core-ws,ups-internal-ws,zuul-gateway,自己的项目web和report等均需要复制后配置
```

### VSCode-Python

```py
安装Python环境
	1. 安装版本:Python 3.11.9	
	2.1 在Vscode中添加python插件:Python、Pylance、autopep8(格式化代码)
	2.2 用户变量中设置代理：
    	http_proxy:http://XXX:XXX@10.X.X.X:3128 		
        https_proxy:http://XXX:XXX@10.X.X.X:3128 
		no_proxy:10.1X6.1X9.1X2（专门为了兼容idea中git拉取推送）
		注意：设置本地用户变量代理后在vscode使用python发送https请求时报错：连接不上服务器；（目前采取删除本地用户环境变量）
	3.1 创建虚拟环境(隔离项目之间依赖冲突等): C+S+P(查看->命令面板)->Python:select Interpreter->创建虚拟环境->Venv【由于是纯Python项目,因此选择了pip+venv】
	3.2 使用CMD命令创建虚拟环境：cd /path/to/your/project -> python -m venv .venv ->.\.venv\Scripts\activate -> pip install -r requirements.txt
	3.3 注意: 
		安装依赖必须在虚拟环境中进行，否则会安装到全局当中，导致依赖管理混乱！！！
		开发环境建议使用pip install -e . :
	4. 开源项目使用流程
		4.1 先看README
		4.2 根据README完成环境配置
		4.3 程序运行测试，跑同官方Demo
		4.4 熟悉代码，二次开发
    5. 启动项目命令：uvicorn xxx:app --reload  : uvicorn为高性能ASGI服务器,表示热启动运行指定的python文件
```

### K8S-Kuborad

```tex
部署核心步骤=推送docker镜像到harbor仓库+改yaml文件(通过kuboard创建工作负载可视化操作代替)
1.工作负载类型:deployment[单实例：除了statefulSet外的服务其他服务均采用该类型]; StatefulSet[集群模式,数据持久化:包括elk、rabbitmq、redis]
2.其中在应用的configMap或者服务的config配置文件中我们采用服务名称代替服务所在服务器的ip,比如：rabbitmq-service代替10.100.111.222
```

## 知识点

```tex
1. pojo的类的主键Id：我们通过IdUitls封装生成唯一Id,其中采取了性能更好的ThreadLocalRandom生成randomUUID(JDK7之后提供并发产生随机数，能够解决多个线程发生的竞争争夺)；但是对于高并发场景则不适合,原因：无序的UUID新增时会产生页分裂, 高并发场景下在新增数据时会增极大增加磁盘IO压力,导致系统经常出现高延迟,因此此场景下需要使用有序的主键生成策略。
2. streamApi 最佳实践： 项目凡是使用到for循环处理数据的场景都使用它替换
3.SP&SF
	存储过程和存储函数都是对于Sql的封装,两者最大区别
	存储函数有返回值,存储过程则返回的是结果集,并非单个值
	实际开发中,存储过程用于业务的增删改;而存储函数用于业务的单值查询
4. JSON字符串、JSON数组、JSON对象格式
	JSON字符串: "{firstName:'John',lastName:'Doe'}"
	JSON对象:   { “firstName”:“John” , “lastName”:“Doe” }
	JSON数组: 数组可包含多个对象：{ “employees”: [{ “firstName”:“John” , “lastName”:“Doe” },{ “firstName”:“Anna” , “lastName”:“Smith” },{ “firstName”:“Peter” , “lastName”:“Jones” }] }
5.注解
	@RestController=@Controller+@ResponseBody。表示类中的方法返回的对象直接作为HTTP响应的正文返回而不需要View Resolver进行视图解析。
	@Controller 用于处理HTTP请求并返回视图名称并且由视图解析器解析为具体的视图最后渲染给客户端。
	@CrossOrigin注解用于启用跨域资源共享（CORS）,它允许JavaScript前端应用从不同的源（域名、协议或端口）访问服务器资源。这在开发现代Web应用时非常重要，因为出于安全考虑，浏览器默认会阻止跨域请求
6.若请求到controller的请求参数包括requestBody则统一使用@postMapping;仅包含@PathVariable获取信息就用@GetMapping,删除则用@DeletingMapping
7.mapper仅仅打印insert语句并未输出parame参数值就关闭sqlsession,原因在于插入数据时某个字段值为空导致!!!该问题在于必须要保证各个参数值按要求写入
8. web状态响应码
    403 Forbidden: 这个状态码表示服务器理解请求客户端的请求，但是拒绝执行此请求
    401 Unauthorized: 这个状态码表示请求未通过身份验证，即用户没有提供有效的认证凭证
    302 Found: 这个状态码表示请求的资源被临时移动到了由Location头部指定的URL。这是一种重定向响应，客户端应该使用Location头部中的URL来重新发起请求
9.vue前端项目命令
	在项目出现编译异常,我们会重新编译项目
	1. 清理项目    rm -r node_modules
	2. 删除配置项  rm package-lock.json
	3. 重新安装    npm install --force 
	4. 重新运行    npm run serve
	5. 打包build    npm run build
10.开发第三方接口注意事项
	1.必须和对方确认接口入参、接口请求头、接口数据返回值格式(需要注意:有时候需要硬编码设置响应数据格式ResponseEntity.ok()
						.contentType(org.springframework.http.MediaType.TEXT_PLAIN)
						.header("Content-Type", "application/json;charset=UTF-8")
						.body(JSONObject.toJSONString(new Result<>(todos, "输入参数错误，比如：缺少必要的参数", 0))))
	2.若不设置返回数据格式,有时候通过第三方接口调用我们接口返回数据时返回的格式不是'application/json;charset=UTF-8',有可能是'image.gif;charset=UTF-8'导致无法获取正确数据。
11.日常排查系统问题汇总
登录到服务器->然后使用top命令查看我们java应用占用CPU的资源,若持续超过100%,则可通过ps -ef | grep 'java' | grep 'PIDxxx'命令初步定位到具体的服务应用,再进行后续应用日志等排查

```

```mermaid
flowchart TD
    A[登录Linux服务器] --> B[执行 top 命令]--> C{Java进程 CPU >100%?}
    C -->|是| D[记录PID: 12345]
    C -->|否| B
    D --> E[执行定位命令:grep 'PID12345']
    E --> F[获取服务名称]
    F --> G[检查应用日志]
    G --> H[分析异常原因]
    H --> I[制定解决方案]
```

```txt
12.Git
	当出现提交代码或者拉取代码异常时,可通过如下命令解决
	git status -> git stash ->git pull -> git stash pop -> git add ->git commit 
13.SecurityContextHolder
	Spring Security框架中的一个核心类，用于存储当前安全上下文(SecurityContext)的信息，主要作用是：
        保存当前用户的安全信息：包括认证信息(Authentication)、权限信息等
        提供线程绑定的安全上下文：确保在多线程环境下也能正确获取当前用户的安全信息
        作为安全信息的访问入口：应用程序可以通过它获取当前用户的认证和授权信息	
```

## 其它

```tex
1.网络协议只分配了从0到65535的端口号,因此在分配规划自定义微服务端口时需要在该范围内,否则启动失效
2.外网想要访问内网搭建的应用服务,除了通过购买云服务外,也可以通过第三方工具作为中介穿透内网进而访问。
3.DEMO项目搭建发布总结
	一.开发环境下注意事项
	1.web在启动时找不到主类(如DemoWebApplication)时,一般在于web类名和启动设置时的Main Type类名不一致,可以在Run Configurations在检查
	2.config配置文件在修改后,重启本应用服务即可(因为该应用每次重启时会从svn服务器拉取最新的配置文件)
	3.demo项目对外暴露的端口为41300

	二.发布至测试/正式环境注意事项
	1.确保微服务名称以及端口正确,并及时申请登记:https://10.175.94.135/!/#Xtrack/view/head/XTRACK4.0_Doc/Subsystem/NewFremeWork/release/micro_service_port.xlsx
	2.密钥加密相关配置jasypt统一放至到bootstrap.yml中,并确保各服务密码加密正确
	3.确认Logback.xml文件是否配置正确,关注点在于value需和项目名称一致<property name="log.file" value="logback_demo_web_amqp"></property>
	4.若项目common、pojo、mapper、service包中的内容发生变化,必须maven deploy推送至Nexus,指令包括clean compile install deploy;否则不用deploy(据说是因为构建镜像时首先从maven仓库获取)
	5.确保Dockerfile中的配置正确,关注点在于'EXPOSE 41300'-暴露端口正确(即和申请登记时保持一致)
	6.推送docker镜像到docker仓库,参考命令:clean compile install docker:build(若common、pojo、mapper、service包中的内容发生变化,必须先deploy推送至Nexus,即命令:clean compile install deploy docker:build)
	7.确定发布的服务器(即服务器ip)并登记:https://10.175.94.135/!/#Xtrack/view/head/XTRACK4.0_Doc/Subsystem/NewFremeWork/Deploy/A-02-deploy_top.xlsx
	8.编写启动应用的shell脚本(参考当前最新版本https://10.175.94.135/!\SVN_Repository\Xtrack\XTRACK4.0_Doc\Subsystem\NewFremeWork\release\test\docker-run-shell\demo),并上传到服务器
	9.Zull-gateway配置文件增加本应用的路由,并通过UPS->系统监控->微服务管理->只刷新网关->刷新zullgateway的IP+端口(10.175.94.160:50000和10.175.94.163:50001)
	10.UPS配置用户角色(系统运维),然后验证web应用是否访问正常 
4.navicat
对整个schema强制刷新统计信息：为了查看当前数据库各种表最新数据记录
	BEGIN
	  DBMS_STATS.GATHER_SCHEMA_STATS(
		ownname => 'BBPO',
		estimate_percent => DBMS_STATS.AUTO_SAMPLE_SIZE,
		method_opt => 'FOR ALL COLUMNS SIZE AUTO',
		cascade => TRUE,
		force => TRUE
	  );
	END;
5.redis常用指令
    0.whereis redis命令找到redis的位置
    1.dbsize--redis存在的key数量
    2.info keyspace --redis数据库key的数量信息
    3.keys：该数据库有多少个key
    4.expires：设置了过期时间的key的个数
    5.avg_ttl：所有key的平均过期时间毫秒数
    6.type key ：当 KEY 不存在时，TYPE 命令返回 none	
6.快捷键
	Ctrl + F5 强制刷新
	Visual Studio 多行注释：Ctrl+K -> Ctrl+C
	Visual Studio 取消注释：Ctrl+K -> Ctrl+U
	NotePdd++ 列模式与行模式切换快捷键:  Alt + Q
	STS: Ctrl + Shift + / -> Ctrl + Shift + \
	Vscode: 格式化代码快捷键 Shift + Alt + F
	IDEA: C+S+F/R--全局查找/替换 
7.个人提升及收藏网址
	postman工具
		其中发请求的Body栏位各个属性选择：
			文件上传 → 用 form-data。
			简单表单 → 用 x-www-form-urlencoded。
			结构化数据（如 API 请求） → 用 raw（JSON/XML）。
			直接发送文件 → 用 binary。
			GraphQL 查询 → 用专用选项。
	Mobaxterm/finalXshell---远程终端工具
	LeetCode热题HOT 100
	Github上的刷题笔记
	查看当前语言热度权威网站: https://www.tiobe.com/tiobe-index/
	查看java当前最新版本: https://docs.oracle.com/javase/specs/index.html
	谷歌地图: https://earth.google.com/web/
	Netty实现聊天室案例: https://blog.csdn.net/weixin_43333483/article/details/127716359#comments_25224363
	easyExcel学习网址: https://www.yuque.com/easyexcel/doc/about
	中国海峡人才市场档案查询系统: https://app.hxrc.com/hxrcservices/dacx/dacx_nor.htm
	Json web token解析: https://jwt.io/
	xidian URL: https://yjsxk.xidian.edu.cn/yjsxkapp/sys/xsxkapp/index.html
	阿里Springcloud alibaba URL:
		https://sca.aliyun.com/docs/2023/overview/what-is-sca/?spm=5176.29160081.0.0.74801d28fdRGMs
		https://sca.aliyun.com/ai/
	Maven 仓库URl: https://mvnrepository.com/artifact/org.redisson/redisson-spring-boot-starter/3.32.0
	vue 官网Url: https://cn.vuejs.org/
	Vxe table官网: https://vxetable.cn/v3.8/#/table/api?filterName=row-config
	iview 官网: https://v4.iviewui.com/docs/introduce
	drawio 官网:用来画流程图以及ER图 https://app.diagrams.net/?src=about
	process on Url: https://www.processon.com/diagraming/65b98667d40f6010366e3a5d
	navicat安装后下载补丁即可无限期使用,获取补丁地址(需关注公众号获取下载码)：https://task.csdn.net/
	typora1.9.5版本激活教程: https://blog.csdn.net/2302_81286569/article/details/144246649
	
	//-----------------------------------AI-----------------------------------//
	AI大模型魔塔社区：https://www.modelscope.cn/papers
	开源的ai助手对话Url: https://front.aibeehive.icu/main/openaiImg?roomId=1781203921566470146
	开源的AI聊天产品合集: https://github.com/xx025/carrot
	AI 大模型最新进展: https://www.datalearner.com/ai-models/pretrained-models
```

# 私有信息

````java
```
````

# <span style="color:orange">README</span>

<span Style="color:red">MD语法</span>

- 标题: Ctrl +  1/2/3/4/5/6(分别对应1-6级标题)
- 代码块语言类型: 三个反引号 ```，可选类型包括tex、java、python、
- 表格: Ctrl + T
- 字体颜色： <span style="color:orange">只能在代码块外面生效</span>
