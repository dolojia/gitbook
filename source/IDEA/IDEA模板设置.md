### ***IDEA类和方法注释模板设置***

这里设置的注释模板采用Eclipse的格式，下面先贴出Eclipse的注释模板，我们就按照这种格式来设置：

类注释模板：  

```java
/**
 * @author 4005517
 * @ClassName xxxController
 * @Description TODO
 * @Date 2019/7/2 17:15
 */
```

方法注释模板：

```java
 /**
     * @Author 4005517
     * @Description //TODO 
     * @Date 2019/7/2 17:21
     * @Param [obj]
     * @return boolean
     */
```

#### 一、首先我们来设置IDEA中类的模板：（IDEA中在创建类时会自动给添加注释）

1、File-->settings-->Editor-->File and Code Templates-->Files

我们选择Class文件（当然你要设置接口的还也可以选择Interface文件）

（1）${NAME}：设置类名，与下面的${NAME}一样才能获取到创建的类名

（2）TODO：代办事项的标记，一般生成类或方法都需要添加描述

（3）${USER}、${DATE}、${TIME}：设置创建类的用户、创建的日期和时间，这些事IDEA内置的方法，还有一些其他的方法在绿色框标注的位置，比如你想添加项目名则可以使用${PROJECT_NAME}

（4）1.0：设置版本号，一般新创建的类都是1.0版本，这里写死就可以了

2、效果图展示

![1562059364946](..\images\file-and-code-templates.png)

#### 二、设置方法注释模板

IDEA还没有智能到自动为我们创建方法注释，这就是要我们手动为方法添加注释，使用Eclipse时我们生成注释的习惯是

/**+Enter，这里我们也按照这种习惯来设置IDEA的方法注释

##### 1、File-->Settings-->Editor-->Live Templates

![Live-Templates](..\images\Live-Templates.png)

###### (1）新建组：命名为` userDefine`

![Create-New-Group](..\images\Create-New-Group.png)

###### （2）新建模板：命名为` add`

因为IDEA生成注释的默认方式是：/*+模板名+快捷键（比如若设置模板名为add快捷键用Tab，则生成方式为

/*add+Tab），如果不采用这样的生成方式IDEA中没有内容的方法将不可用，例如获取方法参数的methodParameters(）、

获取方法返回值的methodReturnType(）

###### （3）设置生成注释的快捷键

![New-Templattes-Add](..\images\New-Templattes-Add.png)

###### （4）设置模板：模板内容如下

注意第一行，只有一个*而不是/*

在设置参数名时必须用${参数名}$的方式，否则第五步中读取不到你设置的参数名

```java
*
 * @Author 4005517
 * @Description //TODO $end$
 * @Date $date$ $time$
 * @Param $param$
 * @return $return$
 */    
```

生成的模板注释将会是如下效果：所以我们要去掉最前面的/*

###### （5）设置模板的应用场景

点击模板页面最下方的警告，来设置将模板应用于那些场景，一般选择EveryWhere-->Java即可

（如果曾经修改过，则显示为change而不是define）

![Templates-Everywhere](..\images\Templates-Everywhere.png)

###### （6）设置参数的获取方式

选择右侧的Edit variables按钮

![Edit-variables](..\images\Edit-variables.png)

PS:第五步和第六步顺序不可颠倒，否则第六步将获取不到方法



选择每个参数对应的获取方法（在下拉选择框中选择即可），网上有很多教程说获取param时使用脚本的方式，我试过使用脚本的方式不仅麻烦而且只能在方法内部使用注释时才能获取到参数

###### （7）效果图

创建方法，在方法上面写：/*+模板名(add)+Enter

```java
    /**
     * @Author 4005517
     * @Description //TODO 
     * @Date 2019/7/2 17:45
     * @Param [loginName, passWord]
     * @return java.lang.String
     */       
    public String test(String loginName,String passWord){
        return "test hello";
    }
```



---------------------
