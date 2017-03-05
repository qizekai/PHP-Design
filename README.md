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
* [适配器模式](#dot)
* [装饰器模式](#symbol)
* [原型模式](#link)
* [观察者模式](#gcz)
* [数据对象映射模式](#sjdxys)
* [策略模式](#cl)
* [迭代器模式](#ddq)


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
      4、在众多的数据库连接接口中，只会创建一次，而不会多次创建.
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
	    这个静态还可以接受任意数据的参数，必须返回一个对象。当我们对象所对应的类的类名发生变化的时候，我们只需要改一下工  
	    厂类类里面的实例化方法即可。不需要外部改所有的地方，如果是更改参数，那么又得另外一种说法了。   
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
####介绍:
	单例模式解决的是如何在整个项目中创建唯一对象实例的问题，工厂模式解决的是如何不通过new建立实例对象的方法。 那么注册树模式想解决什么问题呢？ 在考虑这个问题前，我们还是有必要考虑下前两种模式目前面临的局限。  首先，单例模式创建唯一对象的过程本身还有一种判断，即判断对象是否存在。存在则返回对象，不存在则创建对象并返回。 每次创建实例对象都要存在这么一层判断。 工厂模式更多考虑的是扩展维护的问题。 总的来说，单例模式和工厂模式可以产生更加合理的对象。怎么方便调用这些对象呢？而且在项目内如此建立的对象好像散兵游勇一样，不便统筹管理安排啊。因 而，注册树模式应运而生。不管你是通过单例模式还是工厂模式还是二者结合生成的对象，都统统给我“插到”注册树上。我用某个对象的时候，直接从注册树上取 一下就好。这和我们使用全局变量一样的方便实用。 而且注册树模式还为其他模式提供了一种非常好的想法。
	
####代码:
<?php
namespace IMooc;

class Register
{
    protected static $objects;

    static function set($alias, $object)
    {
        self::$objects[$alias] = $object;
    }

    static function get($key)
    {
        if (!isset(self::$objects[$key]))
        {
            return false;
        }
        return self::$objects[$key];
    }

    function _unset($alias)
    {
        unset(self::$objects[$alias]);
    }
}
##<a name="link"/>各种模式


##<a name="pic"/>图片模式


##<a name="dot"/>观察者模式

##<a name="symbol">装饰器模式


##<a name="cl">策略模式


##<a name="ddq">迭代器模式




##<a name="gcz">观察者模式
