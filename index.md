
<ul id="tree" class="ztree" style=""></ul>

<article class='markdown-body'>
#LINGX技术文档
##目标
> 简化信息管理软件开发；抛开编码细节、专注业务逻辑。

##特色
###模型驱动
数据库表和视图来创建基本的数据模型，该模型具有属性、方法、解释器、验证器、执行器、配置等子模型；此外还有高级的查询模型、克隆模型。
###在线开发
WEB在线创建和管理模型、模型的关联关系、表单控件设置、功能权限设置、数据权限设置；还有一个简易的数据库管理工具。
###级联展示
视图模板主要分两大类：列表展示与树形展示；扩展为列表级联展示与树型级联展示（支持中间表级联，一个界面现可以达到5级联）、列表选择对话框与树形选择对话框（单选、多选）。
###分级授权
通过权限配置，可以实现分级管理人员，分级管理人员有独立的组织、角色、功能、菜单；包括业务模块的功能与数据均可分级授权。
###表单控件
基于ExtJS的控件扩展，控件参数与值都是动态填充，除了文本、密码、单选、多选外，特别加入对象模型选择控件，文件上传控件。
###插件管理
1、IPlugin的execute方法实现特殊功能
2、Spring的AOP功能，接管系统接口
3、Spring注解方式实现任务调度
###事件监听
平台内部逻辑会埋下事件点，事件监听就是找到相应的事件点，实现相应的接口，现有的事件点比较少，可通过反馈快速添加。
###在线更新
平台提供了类似于windows更新中心的功能，通过在线下载、安装补丁；来修复BUG与功能改进。将来计划可通过在线订购业务包或插件包，实现按需下载。

##第三方引用
- Spring 3.2:主要应用IOC、AOP、事件、JDBC、事务管理、工作调度
- druid 1.0:数据库连接池
- fastjson 1.1：JSON格式化工具
- log4j 2.3:日志工具
- guava 18.0:内部缓存
- ExtJS 4.2:前端展示
##运行环境
- 操作系统：Windows系列/Linux系列/MacOS 32位或64位都可以
- Java环境:JDK 1.6/1.7 32位或64位都可以
- WEB容器：Tomcat 6.0/7.0 是我的开发环境，理论上 Jetty、JBoss等也是可以的，LINGX是标准Servlet 2.5工程
- 数据库:MySQL 5.5+
- 浏览器:谷歌chrome、火狐Firefox


> 我的开发环境是：win7(64位)+JDK 1.7(64位)+Tomcat 7.0+MariaDB 5.5 (64位)
# 模型架构
## 对象
为了对象与数据库的方便管理与识别，建议数据库表名称与字段全部用小写，如果有多单词可下划线“_”划分；数据库表以t开头，视图以"v"开头，再接上系统名称、模块名称、子模块名称。
例如：

toa\_work\_rule OA系统的考勤规则

toa\_work\_rule\_date OA系统的考勤规则中的日期规则

更多资料请参考tlingx开头的表结构

### 表对象
表对象即为数据库表的对象模型，表字段为对象属性，字段名称为属性代码，字段注释为属性名称；默认情况下字段与属性一一映射，特殊情况下可以多个属性映射同一字段，或没有属性来映射这段。

在表对象生成期间会自动生成“添加、修改、删除”方法模型，来对该表数据进行增删改查。

表对象在使用列表展示模型时，有附加查询功能；该查询是组合查询，在查询关联对象只需要输关联对象的名称。

数据库表建议以“t”为前缀

### 视图对象
视图对象即为数据库视图的对象模型，只是引用对象不同。在平台的使用方式并无二致。
视图名称建议以“v” 为前缀。
 
### 查询对象
对SELECT语句进行封装，使其可查询、排序、展示
### 克隆对象
克隆对象是其他对象的完全复制品，克隆对象与原对象完全独立，在使用功能上与原对象无异；主要是对同一模型具有两种业务时进行克隆。
例如公共表附件，邮件附件与订单附件的隶属对象不一样，数据权限也不一样，所以需要克隆对象；使得在业务逻辑上是两个表，实际上是操作同一表。
克隆对象建议对象代码以“_clone”为后缀。

### 对象模型详解
- 属性	说明
- 对象代码	唯一标识
- 对象名称	对象方便展示的标识
- 对象类型	1、表对象，2、视图对象，3、查询对象、4、克隆对象
- 数据表名	映射到数据库的资源名称
- 级联对象	可以同时将有引用该对象的对象进行级联展示
- 一级级联 
`[{name:'隶属分组',entity:'tgps_cargroup_car',method:'grid',rule:''}]`

- 二级级联 `[{name:'所属用户' ,entity:'tuser',method:'grid',rule:'torguser'}]`
- 显示模式	列表展示或树型展示 grid/tree
- 目前在对象级联会用到
	

## 配置
###列表参数配置
该参数的作用约等于EXTJS的GRID控件参数。只在列表展示的有效，个别参数在树型展示中也有效果，如排序
###数据权限配置
控制数据的横向权限，具体请看 权限管理-横向数据权限 的章节
## 属性
属性模型可以理解为数据库表字段的化身，数据元的载体；描述字段的类型、输入、输出、引用关系。

属性模型有验证器与解释器两种子模型。

主键字段/属性的字段名必须为id。

### 一般属性
一般属性是指映射到表字段且无外关联，在系统底层实现没有对其进行特殊处理；主要是对字符、数字类型进行管理。

一般属性可以添加解释器，对数据的展示进行格式转换。例如：日期的格式转换
### 对象属性
   对象属性是为表与表之间关联关系设计的属性

对象属性不可以使用解释器，平台会自动根据其引用情况自动对焦。

对象属性的输入控件有，单选、多选、列表、对话框列表选择、对话框树型选择
### 字典属性
在业务开发的过程中经常需要处理些像性别、状态。需要事先设定可选值；在LINGX中统一把这类型属性，叫字典属性。

字典属性不可以使用解释器，平台会自动根据其引用情况自动对焦。

字典属性需要在事先在系统字典表里加入要引用的字典项

以下是用户状态字段的引用情况：
输入控件为 “radio” 单选框，
控件参数为“YHZT”用户状态的代码
指向对象模型必须为“tlingx_optionitem”
### 树型属性
在一个系统中有许多树型结构的数据，LINGX为其定制了树型展示模版，同时对该数据表有一个要求，上级ID的字段名必须为“fid”；
另外有两个可选字段：1、state节点打开（open）或关闭（close）；2、iconCls节点的图标样式。
####fid 与本表的ID类型一至
根节点的fid必须为0
####state 字符串
树型节点的展开与关闭控制，只对根节点与中间节点有效，叶子节点都为关闭。
####iconCls 字符串
展示的图标样式，需要导入相应的CSS文件

### 属性模型详解

- 属性代码	该属性的引用ID
- 映射字段	对应到数据库表字段名
- 属性名称	该属性的名称
- 数据类型	字符串或数字，主要在查询中判断是否要like
- 长度	该属性可存储字符个数，在数据输入时，做长度校验
- 不可为空	前端后端都会进行非空校验
- 输入控件	该属性采用哪种输入控件，默认是文本框
- 控件参数	部分控件支持参数设置
- 引用类型	该属性在被引用时所扮演的角色，是显示、值、无关；默认情况下显示与值都是id
- 指向对象模型	该属性引用的对象模型
- 是否可用	是否在编辑界面中可用
- 是否可见	是否在列表界面中可见
- 字段同步	在修改同一对象下同名属性（对象属性和方法属性）时是否同步和被同步
- 默认值	在编辑时的默认值
- 
## 方法
方法模型代表着一个事件处理；例如：用户列表查询、添加用户、JSP界面跳转；描述事件的处理方式。

方法模型有两个子模型属性和执行器。

### JSON方法
在一个REQUEST请求处理完成后返回JSON格式数据。大部分方法是JSON方法。
 
当JSON没有方法属性时，直接后台执行该方法。

### JSP方法
在一个REQUEST请求处理完在后返回JSP界面，通常在自定义开发中会用到。
### URL方法
该方法重定向到设置的URL。如下图重定向到百度
### JavaScript方法
在一个REQUEST请求处理完成后，返回JavaScript，并执行。
### 默认生成方法
- 	对象添加add
- 	对象修改edit
- 	对象删除del

### 默认实现方法
- 对象展示view
- 对象列表grid
- 对象树型tree
- 对象选择combo
- 对话框列表选择combogrid2
- 对话框树型选择combotree2
- 数据导出excel

### 方法模型详解
- 方法代码	该方法调用的唯一标识
- 方法名称	对外显示的名称
- 是否验证	是否启用验证，默认启用
- 当前记录	是否需要当前记录作为参数
- 是否可见	是否可见
- 是否可用	是否可用，与是否可见的区别是，不可用将显示灰色不可点击的按钮
- 返回类型	返回类型json,jsp,javascript,url对应以上的四种方法

## 验证器
验证器模型是在数据表单提交时，进行数据是否合格的检查；不合格将不作处理。属于属性模型的子模型。
### 表达式验证器
默认验证器，需要在表达式写javascript代码

	name.length>=2
	
### 不可为空验证器
不可为空验证器，是前端后端都会验证；启用方法也比较特殊，只需要属性字段上“不可为空”设为true
### 正则验证器
通过正则表达式来校验数据的合法性，以下就是电子邮箱的验证正则表达式
	
	^([a-zA-Z0-9]*[-_]?[a-zA-Z0-9]+)*@([a-zA-Z0-9]*[-_]?[a-zA-Z0-9]+)+[\\.][A-Za-z]{2,3}([\\.][A-Za-z]{2})?$

### 电子邮箱验证器
### 两数字之间
### 数字不大于{}
### 数字不小于{}
### 数字验证
### 字符串长度在两数字之间
### 字符串长度不大于{}
### 字符串长度不小于{}
### 唯一验证
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
### 表达式验证器
### 日期解释器1
将20150716220123转为2015-07-16
### 日期解释器2
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
###表达式执行器
###对象视图展示执行器
###列表数据展示执行器
###树形数据展示执行器
###选择数据获取执行器
###自定义执行器
# 视图模版
## 列表模版
## 树形模版
## 列表级联
## 树形级联
## 多重级联
# 表单控件
## 文本
### 单行文本
### 多行文本
## 密码
## 单选
## 多选
## 数字
## 日期
## 只读
## 文件
## 对话框
### 列表单选对话框
### 列表多选对话框
### 树形单选对话框
### 树形多选对话框
# 权限管理
## 功能权限
###模型操作权限
###JSP扩展权限
## 数据权限
###横向数据权限
###纵向数据权限
##分级授权
### 组织授权
### 角色授权
### 菜单授权
### 功能授权
#工作流程
##流程元素
###开始节点
###结束节点
###手动任务
###自动任务
###会签任务
###子流程任务
###前进线
###后退线
##表单控件
###表格 table
###列表 list
###单行文本框 text
###多行文本框 textarea
###选择项 select
###签章 image
###用户单选
###用户多选
###组织单选
###组织多选
###外部链接
### 日期 yyyy-MM-dd
### 日期时间 yyyy-MM-dd HH:mm
###提示框
###内容Well
###红色标志
###环境变量
##在线表单设计
##在线流程设计
#数据库规则
- 目前只支持MYSQL5.5/5.6
- 主键名字必须为id,类型可以为自增或UUID
- 日期/时间类型必须用字符来表示，日期时间建议设置为char(14)，例：20150217152536；日期设置为char(8)，例：20150217
- 树型数据结构表，上级字段名必须用fid，有支持icon_cls（图标样式）,state（展开open、关闭close）字段
#开发环境
## 本地集在开发环境-Eclipse
### 创建Web项目
### 下载并导入相关jar
### 安装数据库
### 配置数据库
## 在线集在开发环境-WEB
![](http://lingx-gy.oss-cn-hangzhou.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720150925145207.png)
### 工具栏区
在上图的页面头部区域是工具栏区
#### 创建对象
- 创建数据表对象
- 创建数据视图对象
- 创建查询对象
- 创建克隆对象
#### 规则编辑
>权限管理-横向数据权限

![](http://lingx-gy.oss-cn-hangzhou.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720150925164746.png)

左右两边都是数据权限元数据，中间是逻辑运算符；可以进行条件分组。条件分组之间也可以进行逻辑运算
#### 纵向权限
>权限管理-纵向数据权限

![](http://lingx-gy.oss-cn-hangzhou.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720150925165353.png)

根据角色来指定模型对该角色的不可见字段，如果用户有多个角色；字段可见是或关系。
#### 对象预览
![](http://lingx-gy.oss-cn-hangzhou.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720150925170812.png)

在线开发的功能可直接预览，所见即所得。

#### 写入功能树
![](http://lingx-gy.oss-cn-hangzhou.aliyuncs.com/QQ%E5%9B%BE%E7%89%8720150925171056.png)

只有打勾的功能点才能进行授权与操作。
#### 插件管理
功能插件的启用、禁用操作；插件参数的设置与修改
#### 参数设置
系统全局参数的设置与修改。可直接刷新内存中的缓存数据，立即生效。

通过LINGX或ILingxService的getContfigValue()来获取。
###对象区
在集成开发环境的页面左部区域是对象列表区，简称对象区
###工作区
在集成开发环境的页面中部区域是工作区
####添加属性
####添加方法
####添加验证器
####添加解释器
####添加执行器
####复制
####粘贴
####删除
###属性区
在集成开发环境的页面右部区域是属性区

对象、属性、方法、验证器、解释器、执行器等，都有自己的属性。这里是相应的属性设置区
###代码编辑区
在集成开发环境的页面底部区域是代码编辑区
####编辑框
代码编辑框
####API
执行器内置对象的简易说明
#API
##转义表达式
使用场景
1. 在对象的属性设置默认值，方便编辑
例如：在添加用户时：有默认当前应用
${CUser.getApp().getId()}

2. 在菜单设置中加入筛选条件，在“备注”字段加入条件转义
例如：
某菜单下的用户列表，只能显示当前同一行政组织用户
${org_id=${CUser.getOrgId()}}

###通用转义值
 - CDate 当前日期,20150222
 - CDateTime 当前日期时间,20150222175521
 - CUser.getId() 当前登录的用户ID
 - CUser.getId() 当前登录的用户姓名
 - CUser.getOrgId() 当前登录的组织ID
 - CUser.getOrgName() 当前登录的组织名称
 - CUser.getApp().getId() 当前应用ID
 - CUser.getApp().getName() 当前应用名称
###流程转义值
 - _USER_ID 发起人的用户ID
 - _USER_NAME 发起人的姓名
 - _ORG_ID 发起人的部门ID
 - _ORG_NAME 发起人的部门名称
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
在Java代码中不需要直接取Spring对象，直接使用@Service注解便可取得相应对象

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
    	 * 从Spring容器中取出对象
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
	 * 获取RQEUEST对象，等同HttpServletRequest
	 * @return
	 */
	public IHttpRequest getRequest();
	/**
	 * 获取SESSION对象，等同HttpSession
	 * @return
	 */
	public Map<String,Object> getSession();
	/**
	 * 设置当前对象模型
	 * @param entity
	 */
	public void setEntity(IEntity entity);
	/**
	 * 设置当前方法模型
	 * @param method
	 */
	public void setMethod(IMethod method);
	/**
	 * 取出当前对象模型
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

##执行器中的内置对象API
	
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
	 * 从Spring容器中取出对象
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

###JDBC 数据库操作
详见以下链接：
> http://docs.spring.io/spring/docs/1.1.5/api/org/springframework/jdbc/core/JdbcTemplate.html

###REQUEST 请求对象
	
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
	 * 获取RQEUEST对象，等同HttpServletRequest
	 * @return
	 */
	public IHttpRequest getRequest();
	/**
	 * 获取SESSION对象，等同HttpSession
	 * @return
	 */
	public Map<String,Object> getSession();
	/**
	 * 设置当前对象模型
	 * @param entity
	 */
	public void setEntity(IEntity entity);
	/**
	 * 设置当前方法模型
	 * @param method
	 */
	public void setMethod(IMethod method);
	/**
	 * 取出当前对象模型
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
	
	@Bean(name="LINGX")//这里的LINGX就是在执行器中的内置对象名
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
     * @param ecode 对象代码
     * @param ename 对象名称
     * @param eid 对象ID
     */
    function openViewWindow(ecode,ename,eid)
    /**
     * 得到当前工作区域的TAB窗口对象
     */
    function getCurrentTabWindow()
    /**
     * 获得源页面的对象，通常在对话框中会调用
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
###IPlugin接口的execute方法
###AOP切面编程
###工作调度@Scheduled
# 在线更新
##在线打包
##在线部署
 </article>
<script type="text/javascript" src="javascripts/jquery-1.4.4.min.js"></script>
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
$("title").text("LINGX模型驱动开发平台-抛开编码细节、专注业务逻辑");
$("body").css("margin","0px");
$("body").css("margin-left","320px");
 });
</SCRIPT> 
