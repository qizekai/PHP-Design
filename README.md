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
####介绍：
            工厂模式：说白了就是使用统一方法（工厂方法）来实例化对象，我们定义一个专门用来创建其它对象的类。这样在需要调用   
	    某个类的时候，我们就不需要去使用new关键字实例化这个类，而是通过我们的工厂类调用某个方法得到类的实例。
            工厂模式通常用来返回符合类似接口的不同的类，工厂的一种常见用法就是创建多态的提供者，从而允许我们基于应用程序逻  
	    辑或者配置设置来决定应实例化哪一个类，可以使用这样的提供者来扩展一个类，而不需要重构应用程序的其他部分，从而使  
	    用新的扩展后的名称 。
            通常，工厂模式有一个关键的构造，根据一般原则命名为Factory的静态方法，然而这只是一种原则，工厂方法可以任意命名，  
	    这个静态还可以接受任意数据的参数，必须返回一个对象。当我们对象所对应的类的类名发生变化的时候，我们只需要改一下工厂  
	    类类里面的实例化方法即可。不需要外部改所有的地方，如果是更改参数，那么又得另外一种说法了。   
####代码（Factory文件夹内）：
```php
index.php                       //单一入口
<?php
$a=Database::getInstance();    //直接调用数据库静态化方法
$db=Factory::createDatabase();  //使用工厂模式，来实例化数据库
Database.php                    //数据库类
<?php
class Database
{
	static protected $db;
	private function __construct()
	{
	}
	static function getInstance()
	{
		if(self::$db){
			return slef::$db;
		}else{
			self::$db=new self();
			return self::$db;
		}
	}
}
Factory.php              /工厂模式类
<?php
namespace IMooc;
class Factory
{
	static function createDatabase()
	{
		$db=Database::getinstance();
		return $db;
	}
}
```
##<a name="text"/>注册树模式

##<a name="link"/>各种模式


##<a name="pic"/>图片模式


##<a name="dot"/>观察者模式

