#PHP设计模式

      这段时间在学习PHP的设计模式，于是把所有设计模式重新学了一遍，并做一下整理，统一进行注解。
      我认为如果PHP是一本武功秘籍，那么设计模式便是那招式，把招式学好，那么便是为学好PHP这本武功秘籍，打下良好的基础。
      成为绝世高手，指日可待！
      Author:qk
      E-mail:qizekai@aliyun.com

##<a name="index"/>目录
* [单例模式](#line)
* [工厂模式](#title)
* [注册树模式](#text)
* [观察者模式](#dot)
* [装饰器模式](#symbol)
* [各种模式](#link)
* [图片模式](#pic)

##<a name="line"/>`单例模式`
####介绍：
      简单的说，一个对象只负责一个特定的任务，一个接口，一个连接池
      单例类：
      1、构造函数需要标记为private，防止外部代码使用new操作符创建对象
      2、拥有一个保存类的实例的静态成员变量
      3、拥有一个访问这个实例的公共的静态方法，当前静态方法检测是否实例化
      另外，需要创建__clone()方法防止对象被复制（克隆）
      为什么要使用PHP单例模式？
      1、主要在于数据库应用, 一个应用中会存在大量的数据库操作, 使用单例模式,则可以避免大量的new 操作消耗的资源。
      2、如果系统中需要有一个类来全局控制某些配置信息, 那么使用单例模式可以很方便的实现。
      3、在一次页面请求中, 便于进行调试, 因为所有的代码(例如数据库操作类db)都集中在一个类中。
####代码（Singleton文件夹内）：
```php
<?php  
  
class DB    
{    
    private static $_instance;        
    private function __construct()            //防止被扩展和外部实例化
    {        
    }    
    private function __clone() {};            //覆盖__clone()方法，禁止克隆        
    public static function getInstance()      //只允许此处静态实例化对象
    {    
        if(! (self::$_instance instanceof self) ) {    
            self::$_instance = new self();    
        }    
        return self::$_instance;    
    } 
    
    $db=Db::getInstance();
```


##<a name="title"/>工厂模式

##<a name="text"/>注册树模式

##<a name="link"/>各种模式


##<a name="pic"/>图片模式


##<a name="dot"/>观察者模式

