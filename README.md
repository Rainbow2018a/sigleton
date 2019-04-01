#单例模式

###各种单例实现方式（5种）：懒汉模式，饿汉线程非安全模式，饿汉线程安全模式，内部类模式，枚举模式。现在最推荐的方式是枚举单例模式。以下是对这些模式的描述和介绍。
###懒汉模式

	package com.study.singleton;
	/**
	 * 饿汉式单例模式
	 * 特点：可以通过反射机制攻击；线程安全[多个类加载器除外]。
	 * @author Administrator
	 *
	 */
	public class HungryType {
	    public static final HungryType instance = new HungryType();
	    
	    private HungryType(){
	        
	    }
	    
	    public void splitAlipay() {
	        System.out.println("饿汉式单例模式");
	    }
	    
	    public static void main(String[] args) {
	        HungryType ht =  HungryType.instance;
	        ht.splitAlipay();
	        
	    }
	}

###饿汉线程非安全模式

	package com.study.singleton;
	/**
	 * 
	 * @author Administrator
	 * 懒汉单例模式
	 * 特点：延时加载；线程不安全，多线程下不能正常工作；
	 *
	 */
	public class SluggardType {
	    private static SluggardType instance = null;
	    
	    private SluggardType() {
	    
	    }
	    
	    public static SluggardType getInstance(){
	        if(instance == null){
	            instance = new SluggardType();
	        }
	        return instance;
	    }
	    
	    public void say(){
	        System.out.println("懒汉单例模式");
	    }
	    
	    public static void main(String[] args) {
	        SluggardType.getInstance().say();
	    }
	}

###饿汉线程安全模式

	package com.study.singleton;
	/**
	 * 
	 * @author Administrator
	 * 懒汉模式(双重校验锁[不推荐])单例
	 */
	public class SluggardType2 {
	    
	    //volatile 关键字可以禁止指令重排 ：可以确保instance = new SluggardType2()对应的指令不会重排序
	    //但是因为对volatile的操作都在Main Memory中，而Main Memory是被所有线程所共享的，这里的代价就是牺牲了性能，无法利用寄存器或Cache
	    private volatile static SluggardType2 instance = null;
	    
	    private SluggardType2(){
	        
	    }
	    
	    public static SluggardType2 getInstance(){
	        if(instance == null){
	            synchronized (SluggardType2.class) {
	                if(instance == null){
	                    instance = new SluggardType2();
	                }
	            }
	        }
	        
	        return instance;
	    }
	    
	    public void say(){
	        System.out.println(" 懒汉模式(双重校验锁[不推荐])单例");
	    }
	    
	    public static void main(String[] args) {
	        SluggardType2.getInstance().say();
	    }	 	    
	}

###内部类模式

	package com.study.singleton;
	/**
	 * 
	 * @author Administrator
	 * 借助内部类实现单利模式：
	 * 特点：既能实现延迟加载，又能实现线程安全
	 */
	public class InnerClassSingleton {
	    /**
	     * 类级的内部类，也就是静态的成员式内部类，该内部类的实例与外部类的实例没有绑定关系，而且只有被调用到时才会装载(装在过程是由jvm保证线程安全)
	     * ，从而实现了延迟加载
	     */
	    private static class SingletonHolder {
	        /**
	         * 静态初始化器，由JVM来保证线程安全
	         */
	        private static InnerClassSingleton instance = new InnerClassSingleton();
	    }
	
	    /**
	     * 私有化构造方法
	     */
	    private InnerClassSingleton() {
	    }
	
	    /**
	     * 这个模式的优势在于：getInstance方法并没有被同步，并且只是执行一个域的访问，因此延迟初始化并没有增加任何访问成本
	     */
	    public static InnerClassSingleton getInstance() {
	        return SingletonHolder.instance;
	    }   
	}

###枚举模式

	package com.study.singleton;
	/**
	 * 
	 * @author Administrator
	 * 枚举实现线程安全的单例模式:
	 * 特点：JVM会保证enum不能被反射并且构造器方法只执行一次
	 */
	public class EnumSingleton {
	    private EnumSingleton() {
	    }
	
	    public static EnumSingleton getInstance() {
	        return Singleton.INSTANCE.getInstance();
	    }
	
	    private static enum Singleton {
	        INSTANCE;
	
	        private EnumSingleton singleton;
	
	        // JVM会保证此方法绝对只调用一次
	        private Singleton() {
	            singleton = new EnumSingleton();
	        }
	
	        public EnumSingleton getInstance() {
	            return singleton;
	        }
	    }
	}
