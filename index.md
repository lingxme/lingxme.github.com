
<ul id="tree" class="ztree" style=""></ul>

<article class='markdown-body'>
#灵犀模驱 - 开源的模型驱动开发框架（低代码）
 ![](http://www.lingx.com/logo.png)
##系统目标
简化信息管理软件开发；抛开编码细节、专注业务逻辑。
##系统简介
 灵犀核心是模型驱动开发，有别于一般的代码生成系统。我们不生成代码、没有MVC、没有分层、没有前后端分离，非专业人员便可以做出一般信息管理系统。可以用于所有的Web应用程序，如网站管理后台，网站会员中心，CMS，CRM，OA。

官方网站：https://www.lingx.com

技术文档：http://docs.lingx.com

视频教程：https://space.bilibili.com/482658194

成品示例：https://www.longyaniot.com

账号:admin,密码:123456

##主要功能

-  在线开发：Web版的开发工具，可以进行模型的创建与编辑
-  在线预览：模型创建与编辑后，可立即预览；方便查看效果
-  功能权限：细粒度的功能管理，可管控用户管理的添加按钮是否给某角色或某人使用
-  数据权限：横向/纵向数据权限动态配置，用于不同人查看不同数据
-  远程提交：假如系统已经部署到服务器，可以通过此功能一键提交更新
-  远程更新：类似于window的系统更新，该系统不断维护，不定时发布新功能
-  导出模型：可以将开发好的模型、字典导出为ZIP压缩包
-  导入模型：可以将以上导出的ZIP压缩包进行导入
-  字典管理：研发过程中对数据字典的统一管理
-  权限管理：常规的用户、角色、功能、菜单管理
-  定时任务：可以指定某个时间或定时执行某项功能
-  操作日志：常规的操作日志
-  登录日志：常规的登录日志

##运行环境
- 操作系统：Windows系列/Linux系列/MacOS 32位或64位都可以
- Java环境：JDK 1.8 32位或64位都可以
- WEB容器：Tomcat 8.0 是我的开发环境，Jetty、JBoss等也是可以的，标准Servlet 2.5工程
- 数据库：MySQL 5.5、5.6、5.7，我是用MariaDB 5.5
- 浏览器：谷歌chrome、火狐Firefox


> 我的开发环境是：win7(64位)+JDK 1.8(64位)+Tomcat 8.0+MariaDB 5.5 (64位)
> 
> 一键启动版下载：http://mdd.lingx.com/20210115/lingx-mdd.zip

# 数据库建表约束
1. 当前只能支持MySQL数据库，支持版本 5.5、5.6、5.7 或 MariaDB 5.5
2. 每个表必须要有id字段做为主键，可以用数字(自增)或字符(UUID)
3. 时间字段不用Date类型，用char(8)、char(14)来表示日期与日期时间，例：20191115与20191115114450
4. 树型数据结构表，上级字段名必须用fid，有支持icon_cls（图标样式）,state（展开open、关闭close）字段，字典代码为：SXJDKG
# 开发工具
## 简单介绍
核心既然是模型，开发工具主要就是负责模型生命周期管理及配置管理。

基础功能：的创建、修改、删除、导入、导出。

高级功能：动态表单配置、数据权限配置、自定义查询、服务器远程提交。
## 功能详解
### 顶部工具栏
1. 新建模型
1. 删除模区
1. 复选框
1. 上移/下移
1. 数据权限
1. 纵向权限
1. 模型预览
1. 写入功能树
1. 自定义查询
1. 远程提交
1. 刷新界面
1. 参数配置
1. 插件管理
1. 超管授权
1. 模型备份
1. 执行SQL
1. 上传模型
1. 导入模型
1. 导出模型
1. 技术文档
### 左边模型区
1. 查询
1. 编辑
### 中部工作区
### 中部右键菜单
### 右边属性区
### 底部编辑区
# 系统菜单
## 系统管理
1. 组织管理
1. 用户管理
1. 菜单管理
1. 功能管理
1. 在线用户
1. 操作日志
1. 登陆日志
## 系统工具
1. 开发工具
1. 系统字典
1. 模型管理
1. 系统设置
1. 定时任务
1. 功能购买
1. 通知公告
1. 远程更新


# 模型架构

 主要有模型、属性、操作、验证器、解释器、执行器

 * 模型：现实或概念中具体存在的事物。例如：学生、图书、订单、日志。
 * 属性：模型客观存在的特征、参数、标识。例如：学生的姓名、年龄、编号。
 * 操作：对模型的各种操作。例如：添加、修改、删除、展示。
 * 验证器：在操作时需要对各种数据的有效性验证。例如：不可为空、必须为数字、必须为邮箱。
 * 解释器：将存储在数据库中的代码转换为便于人们理解的。例如：20191115 解释为 2019-11-15
 * 执行器：执行操作时的具体描述，这里一般用JavaScript代码书写。
## 模型

为了模型与数据库的方便管理与识别，建议数据库表名称与字段全部用小写，如果有多单词可下划线“_”划分；数据库表以t开头，视图以"v"开头，再接上系统名称、模块名称、子模块名称。
例如：

toa\_work\_rule OA系统的考勤规则

toa\_work\_rule\_date OA系统的考勤规则中的日期规则

更多资料请参考tlingx开头的表结构


### 表模型

表模型即为数据库表的模型，表字段为模型属性，字段名称为属性代码，字段注释为属性名称；默认情况下字段与属性一一映射，特殊情况下可以多个属性映射同一字段，或没有属性来映射这段。

在表模型生成期间会自动生成“添加、修改、删除”方法模型，来对该表数据进行增删改查。

表模型在使用列表展示模型时，有附加查询功能；该查询是组合查询，在查询关联模型只需要输关联模型的名称。

数据库表建议以“t”为前缀
#### 自动创建表模型
在线自动创建模型功能、在线添加属性。提交后自动生成为表对象。
#### Excel模型
与自动创建表模型功能类似，在这里不需要添加属性， 系统可以自动读取表格第一行为属性值

> Excel文件的第一行为模型属性，第二行开始为数据；只读取Sheet1。
### 视图模型
视图模型即为数据库视图的模型，只是引用模型不同。在平台的使用方式并无二致，但如果为多表关联只能展示，不能操作。
视图名称建议以“v” 为前缀。
 
### 克隆模型
克隆模型是其他模型的完全复制品，克隆模型与原模型完全独立，在使用功能上与原模型无异；主要是对同一模型具有两种业务时进行克隆。
例如公共表附件，邮件附件与订单附件的隶属模型不一样，数据权限也不一样，所以需要克隆模型；使得在业务逻辑上是两个表，实际上是操作同一表。
克隆模型建议模型代码以“_clone”为后缀。



### 模型详解
- 属性	说明
- **模型代码**	唯一标识，一般等同于数据库表名
- **模型名称**	模型方便展示的标识，一般等同数据库表备注
- **模型类型**	1、表模型，2、视图模型，3、克隆模型
- **数据表名**	映射到数据库的资源名称，当模型代码与数据库表名不一致时，在这里填写对应的表名
- **级联模型**	可以同时将有引用该模型的模型进行级联展示，最简单的一级级联方式：tlingx\_user(直接填写模型代码，前提是需要被级联模型有一属性指向当前模型)
> 一级级联 `[{name:'隶属分组',entity:'tgps_cargroup_car',method:'grid',rule:'',where:{}}]`
> 
> 二级级联 `[{name:'所属用户' ,entity:'tuser',method:'grid',rule:'torguser',where:{}}]`
> 
> rule是级联规则，目前只有二级级联中有用到
> 
> where是附加条件，值为JSON数据，例如:where:{"status":"1"}

- **查询条件** 	模型数据筛选， 以and 开头；该字段是表达式，所以常量需要声明，例如：' and status=1'(注意有引号)
- **显示模式**	列表展示或树型展示 grid/tree；目前在模型级联会用到
- **关联资源**   用于远程提交时，将关联的文件资源一起提交到远程服务器；例如：lingx\main.jsp
- **模型类型**   Entity 实体类型
	

## 配置
###列表参数配置
该参数的作用约等于EXTJS的GRID控件参数。只在列表展示的有效，个别参数在树型展示中也有效果，如排序，以下是常用属性

- **序号**	是否在列表显示中显示记录的序号从1开始；在树型结构数据中无效；默认开启
- **页码**	默认显示的页码，默认为1
- **行数**	每页显示的记录数，默认为20
- **排序字段** 可以是当前模型的任意字段代码，默认为id
- **排序方式** 升序asc,降序desc；默认是desc
- **查询字段**	在列表展示的右上角有一“查询”按钮，默认是可以用所有条件进行查询。该配置是指定哪些字段可进行查询，多字段用英文逗号隔开;例如：name,status，也可以用自定义查询，例如：
   
    [{"name":"分组","code":"group","xtype":"textfield","sql":"id in (select car_id from tgps_group_car where group_id in(select id from tgps_group where name like '%${group}%'))","url":""},{"name":"车牌","code":"carno","xtype":"textfield","sql":"carno like '%${carno}%'","url":""},{"name":"终端","code":"tid","xtype":"textfield","sql":"id like '%${tid}%'","url":""}]

> 因为字符编码为UTF-8，所以在中文排序时需要转GBK；例如：convert(name using gbk)

- **排序方式** asc 升序或desc 降序
- **查询字段** 默认不提供查询功能，必须在这里设置要查询的字段代码；多个字段用英文逗号隔开。树形展示不能查询
- **双击操作** 在双击列表项或树型节点时，对当前记录做对应操作，默认是view，可以修改为edit,del或其他自定义方法
- **双击窗体样式** 目前有四种：默认、只读、全屏1、全屏2;正常情况下是默认、只读是只能看不能提交、全屏1是默认的最大化版，全屏2是打开新的窗体有可能会被浏览器阻止
- **单行工具栏** 默认单行工具框，查询与操作按钮放在同一行
- **请求地址** 默认请求的地址为当前对像的grid方法，这里可以自定义，返回的格式为JSON：{rows:[{}],total:1}

> 其它没有说明的参数一般不建义修改

###数据权限配置
控制数据的横向权限，具体请移步 http://www.cnblogs.com/leoxie2011/archive/2012/03/20/2408542.html
## 属性
属性模型可以理解为数据库表字段的化身，数据元的载体；描述字段的类型、输入、输出、引用关系。

属性模型有验证器与解释器两种子模型。

主键字段/属性的字段名必须为id。

### 一般属性
一般属性是指映射到表字段且无外关联，在系统底层实现没有对其进行特殊处理；主要是对字符、数字类型进行管理。

一般属性可以添加解释器，对数据的展示进行格式转换。例如：日期的格式转换
### 日期属性
为了方便日期时间数据的存储和管理，在数据库中日期的数据格式为char(8)，日期时间的数据格式为char(14)，需添加日期解释器
### 模型属性
   模型属性是为表与表之间关联关系设计的属性

模型属性不可以使用解释器，平台会自动根据其引用情况自动对焦。

模型属性的输入控件有，单选、多选、列表、对话框列表选择、对话框树型选择
### 字典属性
在业务开发的过程中经常需要处理些像性别、状态。需要事先设定可选值；在LINGX中统一把这类型属性，叫字典属性。

字典属性不可以使用解释器，平台会自动根据其引用情况自动对焦。

字典属性需要在事先在系统字典表里加入要引用的字典项

以下是用户状态字段的引用情况：

**输入控件**为 “radio” 单选框

**控件参数**为“YHZT”用户状态的代码


**指向模型**必须为“tlingx_optionitem” 字典子项
### 树型属性
在一个系统中有许多树型结构的数据，为其定制了树型展示模版，同时对该数据表有一个要求，

- 上级ID的字段名必须为“fid”，根节点的值为0；
- 树型节点开关字段名state，值为open/close:打开（open）或关闭（close），可选；
- 节点的图标样式icon_cls，可以在数据库表字段中指定样式图标，可选。**详见tlingx\_org模型**

####fid 与本表的ID类型一至
根节点的fid必须为0
####state 字符串
树型节点的展开与关闭控制，只对根节点与中间节点有效，叶子节点都为关闭。
####iconCls 字符串
展示的图标样式，需要导入相应的CSS文件

### 属性模型详解

- **属性代码**	该属性的引用ID，一般等于数据库表字段名
- **属性名称**	该属性的名称
- **数据类型**	字符串或数字，主要在查询中判断是否要like
- **长度**	该属性可存储字符个数，在数据输入时，做长度校验
- **不可为空**	前端后端都会进行非空校验
- **输入控件**	该属性采用哪种输入控件，默认是文本框
- **控件参数**	部分控件支持参数设置
- **引用类型**	该属性在被引用时所扮演的角色，是显示、值、无关；默认情况下显示与值都是id
- **指向实体模型**	该属性引用的模型
- **是否可用**	是否在编辑界面中可用
- **是否可见**	是否在列表界面中可见
- **字段同步**	在修改同一模型下同名属性（模型属性和方法属性）时是否同步和被同步，默认是同步的
- **默认值**	在编辑时的默认值,可以是基本数据类型:1,张三；也可以用默认值表达式${CUser.getOrgId()}、${CDate}、${CDateTime}；还可以用Request模型的参数名${paramName}
- **列表宽度**	在列表展示时，该字段的显示宽度，单位px，例如：300
- **是否转义** 在指向模型的字段，在显示时默认会转义。比如：用户性别在数据库中值为1，在显示的时候会跟会字典的配置转化成男。
- **是否连接** 默认在转义的时候是带连接可以在界面中点击查看被连接的记录信息，在这里可以关闭。比如用户性别指向是字典信息，没有其他意义可关闭。所有的字典类转义都可以关闭连接，即设置false。
- **备注** 在可以这可填写备注信息
## 操作
方法模型代表着一个事件处理；例如：用户列表查询、添加用户、JSP界面跳转；描述事件的处理方式。

方法模型有两个子模型属性和执行器。

### 【类型】JSON操作
在一个REQUEST请求处理完成后返回JSON格式数据。大部分方法是JSON方法。
 
当JSON没有方法属性时，直接后台执行该方法。

### 【类型】JSP操作
在一个REQUEST请求处理完在后返回JSP界面，通常在自定义开发中会用到。
### 【类型】URL操作
系统会跳转到执行器的结果UR。
### 【类型】JavaScript操作
在一个REQUEST请求处理完成后，返回JavaScript，并执行。
### 默认生成操作
- 	模型添加add
- 	模型修改edit
- 	模型删除del

### 默认实现操作
- 模型展示view
- 模型列表grid(包含对话框列表**单选**)
- 模型树型tree(包含对话框树型**单选**)
- 模型选择combo
- 对话框列表**多选**combogrid2
- 对话框树型**多选**combotree2
- 数据导入import
- 数据导出excel
- 高级查询search

### 操作模型详解
- **操作代码**	该方法调用的唯一标识
- **操作名称**	对外显示的名称
- **是否验证**	是否启用验证，默认启用
- **当前记录**	是否需要当前记录作为参数
- **映射字段**	将当前记录的引用值，传给该字段
- **右键菜单**	是否把该功能加入右键菜单中
- **是否可见**	是否在界面中显示
- **是否可用**	是否可用与是否可见的区别是，不可用将显示灰色不可点击的按钮
- **窗体样式**   有四种默认、只读、全屏1、全屏2
- **返回类型**	返回类型json,jsp,javascript,url对应以上的四种方法
- **提示消息**	在编辑框头部的消息提示
- **操作确认消息**	在点“确定”按钮是弹出来的提示消息
- **自定义界面** 打开窗体时，指向的界面。例如：默认的lingx/template/edit.jsp
- **请求地址** 当该操作需要特殊操作，需要将数据提交到其他URI时填写
- **扩展参数**	是JSON模型，目前有属性height设置编辑框的高度，例如:{"height":600}
- **前置脚本**   在这里可以写启用该方法的条件，条件不足时。界面提示【前置消息】。需要时也可以在这里业务逻辑，相当于前置拦截器。
- **前置消息**   在前置脚本结果为flase时，界面上展示该值。详见用户管理的重置密码。
- **备注**	在这里填写备注信息
- **图标样式**	更改按钮上的图标，图标在界面的右下角需鼠标滚轮才可见，图标样式规则首字母大写其余全小写；例：Linkadd
## 验证器
验证器模型是在数据表单提交时，进行数据是否合格的检查；不合格将不作处理。属于属性模型的子模型。

如需指定返回提示消息,就修改返回消息的内容

**验证器需要手工添加在方法内部的属性模型中，在模型的属性模型设置验证器是不启作用的**
### 表达式验证器
默认验证器，需要在表达式写javascript代码

	name.length>=2
	
### 不可为空验证器
不可为空验证器，是前端后端都会验证；启用方法也比较特殊，只需要属性字段上“不可为空”设为true

### 正则验证器
通过正则表达式来校验数据的合法性，以下就是电子邮箱的验证正则表达式
	
	^([a-zA-Z0-9]*[-_]?[a-zA-Z0-9]+)*@([a-zA-Z0-9]*[-_]?[a-zA-Z0-9]+)+[\\.][A-Za-z]{2,3}([\\.][A-Za-z]{2})?$

### 电子邮箱验证器
验证类型	选择“电子邮箱验证” 

	^([a-zA-Z0-9]*[-_]?[a-zA-Z0-9]+)*@([a-zA-Z0-9]*[-_]?[a-zA-Z0-9]+)+[\\.][A-Za-z]{2,3}([\\.][A-Za-z]{2})?$
### 两数字之间
验证类型	选择“数字在两数字之间”

验证参数 例如：6,10

### 数字不大于{}
验证类型	选择“数字不大于{}”

验证参数 例如：10

### 数字不小于{}
验证类型	选择“数字不小于{}”

验证参数 例如：6

### 数字验证
必须为数字
### 字符串长度在两数字之间
验证类型	选择“字符串长度在两数字之间”

验证参数 例如：6,10
### 字符串长度不大于{}
验证类型	选择“字符串长度不大于{}”

验证参数 例如：10
### 字符串长度不小于{}
验证类型	选择“字符串长度不小于{}”

验证参数 例如：6
### 唯一验证
该验证器是通数据库检索来判断，当前是否是唯一；所以在记录数大于1万时，需要加入索引，最好是唯 一索引
### 自定义验证器
自定义验证器，需要继承于抽象类：com.lingx.support.model.validator.AbstractValidator

IContext 执行上下文，详见API

IPerformer 脚本执行，详见API

	//需要注册到Spring容器中
	@Component
	public class NotNullValidator extends AbstractValidator {
	
	public NotNullValidator(){
		this.setType("notnull");//调用键值，不可与其他的键值重复
		this.setName("不可为空");//显示值
	}
	private static final long serialVersionUID = 5104545985585423221L;

	@Override
	public boolean valid(String code,Object value, String param, //value要验证的值,param要验证的参数名
			IContext context,IPerformer performer) throws LingxScriptException {
		
		return value!=null&&!"".equals(value.toString());
	}
	}

## 解释器
解释器就是数据转换器，同时具有输入转换和输出转换；

例如：日期8 解释器

输入转换是将2015-07-16转换成20150716

输出转换是将20150716转换成2015-07-16
### 表达式解释器
JavaScript表达式，可以调用Spring中的所有资源进行处理
	
	var bean=SPRING.getBean("userService");//该获取Spring资源的方式在任意表达式都适用
	var ret="data";
	ret=ret+"1";
	ret

### 日期8
将20150716转为2015-07-16
### 日期时间14
将20150716220123转为2015-07-16 22:02:23
### 时间4
将1200转为12:00

### 自定义解释器
自定义解释器，需要继承于抽象类: com.lingx.support.model.interpreter.AbstractInterpreter 

IContext 执行上下文，详见API

IPerformer 脚本执行，详见API

	//需要注册到Spring容器中
	@Component
    public class Time4Interpreter extends AbstractInterpreter{

	public Time4Interpreter(){
		this.setType("time4");//调用键值，不可与其他的键值重复
		this.setName("时间4");//显示值
	}
	private static final long serialVersionUID = 3727950655388868120L;

	@Override
	public Object input(Object value, IContext context, IPerformer jsper)
			throws LingxScriptException {
		if(value!=null){
			String temp=value.toString();
			 temp=temp.replaceAll("[-]|[ ]|[:]", "");
			return temp;
		}else{
			return "";
		}
	}

	@Override
	public Object output(Object value, IContext context, IPerformer jsper)
			throws LingxScriptException {
		String temp=null;
		StringBuilder sb=new StringBuilder();
		if(value!=null){
			temp=value.toString();
			sb.append(temp.substring(0,2)).append(":").append(temp.substring(2));
		}else{
			return value;
		}
		return sb.toString();
	}
	}

## 执行器
业务逻辑执行，默认是表达式执行器。执行完成后将返回JSON数据。以下是简单的表达式执行器代码：

	JDBC.update('insert into tzf_wfjs_image(wfjs_id,path,remark,user_id,upload_time) values(?,?,?,?,?)'
	,[wfjs_id,path,remark,user_id,upload_time]);
	'操作成功';

JDBC是内置的API模型，可以进入与数据库的任务交互；JDBC等于Spring的JdbcTemplate具体使用方法请百度查询。

除了JDBC之外，还内置了

- CUser 当前用户，看后面章节的当前用户类介绍
- JDBC 数据库操作，Spring的JdbcTemplate具体使用方法请百度查询
- LINGX 综合API，看后面章节的LINGX接口介绍
- SPRING 资源提取，只有一个SPRING.getBean();方式
- Utils 工具类，看后面章节的当前工具类介绍
- REQUEST 客户端请求，封装后的Request不等同于HttpServletRequest，但同样可以var param=REQUEST.getParameter("param");
- ENTITY_CODE 当前的实体代码

### 执行结果说明
执行结果为JSON，格式为:{code:1,message:"操作成功"}，JSON内也可以添加额外数据

- code为1：提示、刷新、关窗 (一般用于操作成功)，可以直接返回'操作成功'，系统会自动封装为{code:1,message:"操作成功"}
- code为2：提示、关窗 (一般用于树形操作成功，不刷新树)，可以用API：LINGX.ret(2,"操作成功，请继续操作!");
- code为-1：只有提示 (一般用于操作错误提示)，可以用API：LINGX.retErr("操作失败，数据异常!");


###表达式执行器

上面介绍了简单的执行器代码编写，以下展示稍微复杂的代码，但实际上复杂的业务逻辑代码必须写在Java类中。通过Spring提取调用

	var newpass1=password1;
	var newpass2=password2;
	var ret=null;

	if(newpass1!=newpass2){
	ret={code:-1,message:"确认密码需等于新密码"};
	}else{
	var map=JDBC.queryForMap("select * from tlingx_user where id=?",[CUser.getId()]);
	var pass=(map.get('password'));
	var rand32=pass.substring(32);
	var inpass=LINGX.passwordEncode(oldpass,CUser.getAccount())
	if(!inpass.equals(pass)){
	ret={code:-1,message:"旧密码有误"};
	}else{
	JDBC.update("update tlingx_user set password=? where id=?",[LINGX.passwordEncode(password1,CUser.getAccount()),CUser.getId()]);
	ret='密码修改成功';
	}
	}
	ret;

###自定义执行器
自定义的执行器，必须实现接口IExecutor

	public class ComboExecutor extends AbstractModel implements IExecutor 

以上是完整的自定义执器

	public Object execute(IContext context, IPerformer performer)
			throws LingxScriptException {

		IEntity entity=(IEntity)context.getEntity();
		List<String> textField=modelService.getTextField(entity);
		String valueField=modelService.getValueField(entity);
		try {
			String sql=this.queryService.getSelectSql(context,performer);
			//System.out.println("--------ComboExecutor-----------");
			//System.out.println(sql);
			List<Map<String,Object>> list=jdbcTemplate.queryForList(LingxUtils.sqlInjection(sql));
			if(list!=null)
			for(Map<String,Object> map:list){
				map.put("value", map.get(valueField.toUpperCase()));
				StringBuilder sb=new StringBuilder();
				for(String s:textField){
					sb.append(map.get(s.toUpperCase())).append("-");
				}
				sb.deleteCharAt(sb.length()-1);
				map.put("text", sb.toString());
			}
			return list;
		} catch (Exception e) {
			throw new LingxScriptException("数据读取异常",e);
		} 
	}

在自定义执行器之后，需要根据 lingx-default-method.xml 里方式把组合成新方法配置到Spring容器中。平台就可以对其进行权限管理与执行

# 表单控件
基于EXTJS构建的表单数据输入控件，在非EXTJS构建的表单。不能直接使用以下控件，但可以变通使用

> 在设置模型或方法中任意一个属性时，会**自动同步**给其他同代码的字段。如不需同步，则设置属性中的**字段同步**为false
## 文本
### 单行文本
普通的单行文本输入框等于EXTJS中的textfield
### 多行文本
普通的多行文本输入框等于EXTJS中的textarea
## 密码
普通的密码文本输入框
## 选择
选择控件是一个改造后的combobox,支持任意模型的数据选择，显示与值可以在模型属性中配置；也可以选择字典

*模型选择设置方法*

- **输入控件**设置为"选择控件"
- **指向模型**设置为被选模型

*字典选择设置方法*

- **输入控件**设置为"选择控件"
- **控件参数**设置为字典代码，例如：SF。
- **指向模型**设置为“字典子项” 列表中的最后一个

## 单选

- **输入控件**设置为"单选框"
- **控件参数**设置为字典代码，例如：SF。
- **指向模型**设置为“字典子项” 列表中的最后一个
## 多选

- **输入控件**设置为"多选框"
- **控件参数**设置为字典代码，例如：SF。
- **指向模型**设置为“字典子项” 列表中的最后一个
## 数字
数字输入框
## 日期
日期在数据库存储格式为20160614，在显示或编辑中显示为2016-06-14；所以需要添加解释器（日期8,8是指字符长度）

> 模型属性是用于列表展示与查看，方法属性才是编辑用的，所以添加解释器时必须分别添加或复制
## 日期时间

日期在数据库存储格式为20160614220625，在显示或编辑中显示为2016-06-14 22:06:25；所以需要添加解释器（日期14,14是指字符长度）

> 模型属性是用于列表展示与查看，方法属性才是编辑用的，所以添加解释器时必须分别添加或复制
## 只读
如果某字段不允许修改，则可以设置为只读
## 文件
文件上传控件，服务端文件接收已固定，不需额外添加代码

控件参数:JSON或URL，默认JSON；两者区别在于返回值

**JSON**：

[{"text":"文件名称","value":"文件URL地址"}]

**URL**：

文件URL地址

## 图片
同文件控件，只是在列表会以小图展示


控件参数:JSON或URL，默认JSON；两者区别在于返回值

**JSON**：

[{"text":"文件名称","value":"文件URL地址"}]

**URL**：

文件URL地址


## 对话框
以下四个对话选择框，都可以在**控件参数**中设置筛选条件；参数类型为JSON

> 列表结构 - {"status":"1"}
> 树型结构 - {"fid":"f9779920-202b-41f5-8615-0776c88c78a8"}
### 列表单选对话框
### 列表多选对话框
### 树形单选对话框
### 树形多选对话框

#API
##转义表达式
使用场景
1. 在模型的属性设置默认值，方便编辑
例如：在添加用户时：有默认当前应用
${CUser.getApp().getId()}

2. 在菜单设置中加入筛选条件，在“备注”字段加入条件转义
例如：
某菜单下的用户列表，只能显示当前同一行政组织用户
${org_id=${CUser.getOrgId()}}

###通用转义值
 - CYear 当前年份
 - CDate 当前日期,20150222
 - CDateTime 当前日期时间,20150222175521
 - CUser.getId() 当前登录的用户ID
 - CUser.getId() 当前登录的用户姓名
 - CUser.getOrgId() 当前登录的组织ID
 - CUser.getOrgName() 当前登录的组织名称
 - CUser.getApp().getId() 当前应用ID
 - CUser.getApp().getName() 当前应用名称
###流程转义值
 - \_USER\_ID 发起人的用户ID
 - \_USER\_NAME 发起人的姓名
 - \_ORG\_ID 发起人的部门ID
 - \_ORG\_NAME 发起人的部门名称
###默认值扩展
实现接口  com.lingx.core.model.IValue，再把实现类注册到Spring
例如：

	public class DateValue implements IValue {
		public static final SimpleDateFormat sdfDate=new SimpleDateFormat("yyyyMMdd");
	
		@Override
		public String getSourceValue() {
			return "${CDate}";
		}
	
		@Override
		public String getTargetValue(IContext context) {
			return sdfDate.format(new Date());
		}
	
	}

##Java/Jsp开发的平台API
###取出Spring容器
在Java代码中不需要直接取Spring模型，直接使用@Service注解便可取得相应模型

在Jsp中使用以下方法

	ApplicationContext applicationContext = WebApplicationContextUtils.getRequiredWebApplicationContext(request.getSession().getServletContext());
	JdbcTemplate jdbc=applicationContext.getBean("jdbcTemplate",JdbcTemplate.class);
	
###综合API ILingxService
    
   
    
    /** 
     * @author www.lingx.com
     * @version 创建时间：2015年4月5日 上午11:30:24 
     * lingx框架的核心服务类
     */
    public interface ILingxService {
    	/**
    	 * 获取Spring环境
    	 * @return
    	 */
    	public ApplicationContext getSpringContext();
    	/**
    	 * 取得配置信息
    	 * @param key
    	 * @return
    	 */
    	public String getConfigValue(String key,String defaultValue);
    	/**
    	 * 取得配置信息
    	 * @param key
    	 * @return
    	 */
    	public int getConfigValue(String key,int defaultValue);
    	/**
    	 * 从Spring容器中取出模型
    	 * @param key
    	 * @return
    	 */
    	public Object getBean(String key);
    	/**
    	 * 根据ID取出系统插件
    	 * @param id
    	 * @return
    	 */
    	public IPlugin getPlugin(String id)throws LingxPluginException;
    	/**
    	 * 取得插件管理器
    	 */
    	public PluginManager getPluginManager();
    	/**
    	 * 调用模型中的方法
    	 * @param entityCode
    	 * @param methodCode
    	 * @param params
    	 * @return
    	 */
    	public Object call(String entityCode,String methodCode,Map<String,String> params,IContext context);
    	/**
    	 * 判断当前用户是否是超管人员
    	 * @param request
    	 * @return
    	 */
    	public boolean isSuperman(HttpServletRequest request);
    	/**
    	 * 统计外关联数
    	 * @param entityCode
    	 * @param id
    	 * @return
    	 */
    	public int countForeignKey(String entityCode, Object id);
    	/**
    	 * 生成UUID
    	 * @return
    	 */
    	public String uuid();
    	/**
    	 * 获取时间戳:20150616165723
    	 * @return
    	 */
    	public String ts();
    	/**
    	 * 获取时间戳:20150616165723
    	 * @return
    	 */
    	public String getTime();
    	/**
    	 * 密码加密
    	 * @param password 密码明文
    	 * @param userid 账号明文
    	 * @return
    	 */
    	public String passwordEncode(String password, String userid);
    	}

### IContent 执行上下文
	
	/**
	 * 存在request.attribute的客户端IP
	 */
	public static final String CLIENT_IP="ClientIP";
	/**
	 * 存在request.attrbute的项目运行根目录
	 */
	public static final String LOCAL_PATH="LocalPath";
	/**
	 * 获取RQEUEST模型，等同HttpServletRequest
	 * @return
	 */
	public IHttpRequest getRequest();
	/**
	 * 获取SESSION模型，等同HttpSession
	 * @return
	 */
	public Map<String,Object> getSession();
	/**
	 * 设置当前模型
	 * @param entity
	 */
	public void setEntity(IEntity entity);
	/**
	 * 设置当前方法模型
	 * @param method
	 */
	public void setMethod(IMethod method);
	/**
	 * 取出当前模型
	 * @return
	 */
	public IEntity getEntity();
	/**
	 * 取出当前方法模型
	 * @return
	 */
	public IMethod getMethod();
	/**
	 * 取出当前登录用户
	 * @return
	 */
	public UserBean getUserBean();
###IPerformer 脚本执行器
	/**
	 * 执行脚本
	 * @param script
	 * @param context
	 * @return
	 * @throws LingxScriptException
	 */
	public Object script(IScript script,IContext context) throws LingxScriptException;

##执行器中的内置模型API
	
###LINGX 核心处理类

	/**
	 * 获取Spring环境
	 * @return
	 */
	public ApplicationContext getSpringContext();
	/**
	 * 取得配置信息
	 * @param key
	 * @return
	 */
	public String getConfigValue(String key,String defaultValue);
	/**
	 * 取得配置信息
	 * @param key
	 * @return
	 */
	public int getConfigValue(String key,int defaultValue);
	/**
	 * 从Spring容器中取出模型
	 * @param key
	 * @return
	 */
	public Object getBean(String key);
	/**
	 * 根据ID取出系统插件
	 * @param id
	 * @return
	 */
	public IPlugin getPlugin(String id)throws LingxPluginException;
	/**
	 * 取得插件管理器
	 */
	public PluginManager getPluginManager();
	/**
	 * 调用模型中的方法
	 * @param entityCode
	 * @param methodCode
	 * @param params
	 * @return
	 */
	public Object call(String entityCode,String methodCode,Map<String,String> params,IContext context);
	/**
	 * 判断当前用户是否是超管人员
	 * @param request
	 * @return
	 */
	public boolean isSuperman(HttpServletRequest request);
	/**
	 * 统计外关联数
	 * @param entityCode
	 * @param id
	 * @return
	 */
	public int countForeignKey(String entityCode, Object id);
	/**
	 * 生成UUID
	 * @return
	 */
	public String uuid();
	/**
	 * 获取时间戳:20150616165723
	 * @return
	 */
	public String ts();
	/**
	 * 获取时间戳:20150616165723
	 * @return
	 */
	public String getTime();
	/**
	 * 密码加密
	 * @param password 密码明文
	 * @param userid 账号明文
	 * @return
	 */
	public String passwordEncode(String password, String userid);

###CUser 当前用户
当前登陆用户的API

	public String getId();//取ID
	public String getAccount();//取账号
	public String getName();//取名字
	public String getStatus();//取状态
	public String getOrg().getId();//取当前用户行政组织ID
	public String getOrg().getName();//取当前用户行政组织名称

###JDBC 数据库操作
详见以下链接：
> http://my.oschina.net/u/437232/blog/279530
> http://docs.spring.io/spring/docs/1.1.5/api/org/springframework/jdbc/core/JdbcTemplate.html

###REQUEST 请求模型
	
	/**
	 * 根据参数名获取请求参数值
	 * @param key
	 * @return
	 */
	String getParameter(String key);
	/**
	 * 根据参数名获取请求参数值，如果参数为空时，返回缺省值
	 * @param key
	 * @param defaultValue 
	 * @return
	 */
	String getParameter(String key,String defaultValue);
	/**
	 * 设置请求周期内属性
	 * @param key
	 * @param value
	 */
	void setAttribute(String key,Object value);
	/**
	 * 获取属性
	 * @param key
	 * @return
	 */
	Object getAttribute(String key);
	/**
	 * 获取请求所有参数值数组
	 * @param key
	 * @return
	 */
	String[] getParameterValues(String key);
	/**
	 * 获取所有属性
	 * @return
	 */
	Map<String,Object> getAttributes();
	/**
	 * 获取所有参数
	 * @return
	 */
	Map<String,String[]> getParameters();
	/**
	 * 获取所有参数名
	 * @return
	 */
	Set<String> getParameterNames();
###CONTEXT 执行上下文
	
	
	/**
	 * 获取RQEUEST模型，等同HttpServletRequest
	 * @return
	 */
	public IHttpRequest getRequest();
	/**
	 * 获取SESSION模型，等同HttpSession
	 * @return
	 */
	public Map<String,Object> getSession();
	/**
	 * 设置当前模型
	 * @param entity
	 */
	public void setEntity(IEntity entity);
	/**
	 * 设置当前方法模型
	 * @param method
	 */
	public void setMethod(IMethod method);
	/**
	 * 取出当前模型
	 * @return
	 */
	public IEntity getEntity();
	/**
	 * 取出当前方法模型
	 * @return
	 */
	public IMethod getMethod();
	/**
	 * 取出当前登录用户
	 * @return
	 */
	public UserBean getUserBean();

###自定义执行器中的API

	@Configuration
	public class ApiConfig {
	@Resource
	private ILingxService lingxService;
	
	@Bean(name="LINGX")//这里的LINGX就是在执行器中的内置模型名
	public IScriptApi getLingxApi(){
		DefaultScriptApi lingx=new DefaultScriptApi();
		lingx.setBean(this.lingxService);
		return lingx;
		
	}
	
	}

##前端开发中的JavaScript API
###页面通用API
> 必须引入 <%@ include file="/lingx/include/include_JavaScriptAndCss.jsp"%> 

    /**
     * 获取页面ID
     * @returns {___anonymous_pageId}
     */
    function getPageID()
    /**
     * 得到页面宽度
     * @returns
     */
    function getRootWidth()
    /**
     * 得到页面高度
     * @returns
     */
    function getRootHeight()
    /**
     * 得到工作区域的宽度
     */
    function getCenterWidth()
    /**
     * 得到工作区域的高度
     */
    function getCenterHeight()
    /**
     * 打开单个数据的展示窗口
     * @param ecode 模型代码
     * @param ename 模型名称
     * @param eid 模型ID
     */
    function openViewWindow(ecode,ename,eid)
    /**
     * 得到当前工作区域的TAB窗口模型
     */
    function getCurrentTabWindow()
    /**
     * 获得源页面的模型，通常在对话框中会调用
     * @param fromPageId
     */
    function getFromWindow(fromPageId)
    /**
     * 打开对话框，有“确定”和“取消”按钮
     * @param title
     * @param url
     */
    function openWindow(title,url)
    /**
     * 打开对话框，只有“关闭”按钮
     * @param title
     * @param url
     */
    function openWindow2(title,url)
    /**
     * 打开对话框，有“提交”、“确定”、“关闭”按钮
     * @param title
     * @param url
     */
    function openWindow3(title,url)
    /**
     * 打开查询对话框
     * @param queryField
     * @param fields
     */
    function openSearchWindow(queryField,fields)
    /**
     * 重置对话框的宽高
     * @param options
     */
    function resizeWindow(options)
    /**
     * 关闭当前对话框
     */
    function closeWindow()
    /**
     * 在顶部中间显示提示信息
     * @param msg
     */
    function showMessage(msg)
    /**
     * 在顶部中间显示提示信息
     * @param msg
     */
    function lgxInfo(msg)
    /**
     * 取得主操作页面
     */
    function getRootWindow()

### 对话框自定义的内置API
在对话框自定义界面中，为了响应对话框的“确定”按扭；需要在页面中定义lingxSubmit()函数

	function lingxSubmitFn(){
	// 非空检查 
	var fields=Ext.getCmp("form").getForm().getFields();
	var objCache={};
	for(var i=0;i<fields.items.length;i++){
		var obj=fields.items[i].getSubmitData();
		for(var temp in obj){
			objCache[temp]=obj[temp];
		}
	}
	for(var i=0;i<fieldsCache.length;i++){
		var f=fieldsCache[i];
		if(f.isNotNull&&!objCache[f.code]){
			lgxInfo(f.name+"不可为空");
			return ;
		}
	}
	// 非空检查 end
	Ext.getCmp("form").getForm().submit({
		params:{t:3},
		success: callbackFormSubmit,
		failure:callbackFormSubmit
	});
	}

#插件管理
## 插件安装
## 插件配置
## 插件开发
	
	package com.lingx.plugin.upload.ext;

	import java.util.Map;

	import javax.annotation.Resource;

	import org.apache.logging.log4j.LogManager;
	import org.apache.logging.log4j.Logger;
	import org.springframework.stereotype.Component;
	
	import com.lingx.core.plugin.IPlugin;
	import com.lingx.support.web.action.UploadAction;
	@Component
	public class UploadExtPlugin implements IPlugin {
	public static Logger logger = LogManager.getLogger(UploadExtPlugin.class);
	@Resource
	private IUploadExtService uploadExtService;
	@Resource
	private UploadAction uploadAction;
	private boolean enable=true;
	@Override
	public String getId() {
		return "upload-ext-plugin";
	}

	@Override
	public String getName() {
		return "文档外置上传插件";
	}

	@Override
	public String getDetail() {
		return "由于多容器署时，会导致文档上传混乱，且文档服务需要大容量硬盘；所以独立部署文档系统，通过使用本插件系统上传的文件，均转发到文档系统。\r\n需要参数：targetURL，流方式上传路径";
	}

	@Override
	public String getAuthor() {
		return "www.lingx.com";
	}

	@Override
	public String getVersion() {
		return "1.0";
	}

	@Override
	public boolean isEnable() {
		return enable;
	}

	@Override
	public void setEnable(boolean enable) {
		this.enable=enable;
	}

	@Override
	public void init(Map<String, Object> params) {
		try {
			String targetURL=params.get("targetURL").toString();
			this.uploadExtService.setTargetURL(targetURL);
			this.uploadAction.setType(2);//设置为从IUploadService
		} catch (Exception e) {
			logger.error("plugin init error:"+e.getMessage());
			//e.printStackTrace();
		}		
	}

	@Override
	public void start() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void stop() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void destory() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public Map<String, Object> execute(Map<String, Object> params) {
		// TODO Auto-generated method stub
		return null;
	}

	}

 </article>
<script src="//cdn.bootcss.com/jquery/1.11.3/jquery.min.js"></script>
<script type="text/javascript" src="javascripts/jquery.ztree.core-3.5.min.js"></script>
<script type="text/javascript" src="javascripts/ztree_toc.js"></script>
<link rel="stylesheet" href="stylesheets/zTreeStyle/zTreeStyle.css" type="text/css">

<SCRIPT type="text/javascript" >
 $(document).ready(function(){ $('#tree').ztree_toc({
 is_auto_number:true, 
is_highlight_selected_line:false,
documment_selector:'.markdown-body', 
ztreeStyle: { width:'260px', overflow: 'auto', position: 'fixed', 'z-index': 2147483647, border: '0px none', left: '0px', top: '0px' } 
});
$("title").text("灵犀 - 开源的轻量级模型驱动开发框架");
$("body").css("margin","0px");
$("body").css("margin-left","320px");
 });
</SCRIPT> 
<script>
var _hmt = _hmt || [];
(function() {
  var hm = document.createElement("script");
  hm.src = "//hm.baidu.com/hm.js?2624aa48241825af0869177ea4c22796";
  var s = document.getElementsByTagName("script")[0]; 
  s.parentNode.insertBefore(hm, s);
})();
</script>
