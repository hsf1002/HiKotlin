# kotlin
kotlin common grammer


## Kotlin的优点
1. 可读性更高，更简洁，使用Kotlin，可以更容易的避免创建模版型代码，因为大多数经典的情景都默认包含在Kotlin中
2. 空指针安全，当我们用java开发时，我们的大多数代码是要进行类型检查的，如果我们不想出现\*\*unexpected NullPointerException\*\*的话,我们就要在运行代码之前持续的检查是否有对象为null。Kotlin和其它语言一样，是空指针安全的，因为我们可以通过安全的调用操作来准确的声明一个object可以为null  
 ```
 //This won´t compile. Artist can´t be null
    var notNullArtist: Artist = null
    
 //Artist can be null
    var artist: Artist? = null
    artist.print()    // Will print only if artist != null
    artist?.print()    // Smart cast. We don´t need to use safe call operator if we previously checked  nullity
```
3. 扩展方法
可以给任何类添加新方法，这比我们在project中使用的工具类可读性更高。例如可以给Fragment添加一个新方法来显示Toast  
```
fun Fragment.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(getActivity(), message, duration).show()
}
```
可以这样使用：  
```
    fragment.toast("Hello world!")
```
4. 支持函数式编程
如果我们可以不用在我们需要的时候每一次都创建一个listener，就像创建一个click listener那样的操作， 
而是仅仅定义我们想要做什么？这种想法的确可以实现，它的实现得益于lambdad的使用：
```
    view.setOnClickListener({ toast("Hello world!") })
```
## Kotlin的缺点  
Kotlin 依旧在发展，虽然它相对稳定，并且final release版本就很快发布，但是Kotlin在进行android相关开发的时候还是有些限制的  
1. 交互性与自动代码生成：一些有名的android Libraries，例如Dagger 或 Butterknife，他们依赖于自动代码生成，这种情况下如果有名字冲突的话将无法进行代码生成。Kotlin正在解决这个问题，所以这个问题也许会很快解决。无论如何，我将在接下来的文章里阐明，Kotlin语言的表达能力会让我们觉得不再需要那么多的 Libraries
2. 声明自定义View比较困难：Kotlin类只能声明一个构造函数，然而自定义View通常需要三个。如果我们使用代码来创建View的话可以避免这个问题，但对于使用XML文件来声明View的话就不能满足需求了。最容易的变通方式是用java来声明这些自定义View的实现类，然后通过Kotlin来使用它们。Kotlin团队许诺将在M11 release解决这个问题

`package` 语句必须是文件中的第一行非注释的程序代码，如果没有指定包名， 那这个文件的内容就从属于没有名字的 default 包  
`import` 如果将两个含有相同名称的的类库以 * 同时导入，将会存在潜在的冲突，此时要么完全指明导入类的具体位置，要么用as关键字局部重命名，以解决冲突  

## 基本数据类型  
和Java一样，Kotlin也是基于JVM，不同的是，后者是静态类型语言，意味着所有变量和表达式类型在编译时已确定。在Java中，通过装箱和拆箱在基本数据类型和包装类型之间相互转换，而Kotlin中，所有变量的成员方法和属性都是对象。一些类型是Kotlin中内建，相当于创建的普通类，直接调用即可  
在Kotlin源代码中，不管是常量还是变量在声明是都必须具有类型注释或者初始化。如果在声明时，进行了初始化，会自行推导其数据类型，以为着常量或者变量注释类型。Kotlin中的数据类型包括数值类型、字符类型、布尔类型等  
字符类型不是基本数值类型，是一个独立的数据类型，字符是单引号包起来的 ‘1’ , ‘\n’ , ‘\uFF00’ ，可以通过显示转换将其转换为Int型，和数值类型一样，字符类型在空检查后会在需要的时候装箱，特性不会被装箱操作保留；布尔值只有 true 或者 false，看Boolean源码可知，其内置的操作包括not(非)、and(与)、or(或)、xor(异或)、compareTo()等  

`==`  
1. 如果作用于基本数据类型的变量，则直接比较其存储的 “值”是否相等
2. 如果作用于引用类型的变量，则比较的是所指向的对象的地址  

`equals`  
1. equals方法不能作用于基本数据类型的变量
2. 如果没有对equals方法进行重写，则比较的是引用类型的变量所指向的对象的地址  
3.诸如String、Date等类对equals方法进行了重写的话，比较的是所指向的对象的内容  

`===`    
1. 对于基本数据类型，如果类型不同，其结果就是不等。如果同类型相比，与“==”一致，直接比较其存储的 “值”是否相等；
2. 对于引用类型，与“==”一致，比较的是所指向的对象的地址  

## 流程控制语句  
```
if (a > b) {
    max_0 = a 
} else {
    max_0 = b
}

var max_1 = if (a > b) a else b

val max_2 = if (a > b) {
    println("Choose a")
    a
} else {
    println("Choose b")
    b
}

val x : Int = 10
when (x) {
    0,1 -> print("x == 0 or x == 1")
    else -> print("otherwise")
}

when (x) {
    in 1..10 -> print("x is in the range")
    in validNumbers -> print("x is valid")
    !in 10..20 -> print("x is outside the range")
    else -> print("none of the above")
}

val hasPrefix = when (x) {
    is String -> x.startsWith("prefix")
    else -> false
}

for (item in collection)
    print(item)

for (i in array.indices)
    print(array[i])
```  
return、break、continue等三种返回跳转操作符与Java功能一样

## 函数  
一个函数不返回任何有意义的结果值，那么它的返回类型为Unit，如果一个函数体由多行语句组成的代码段，那么必须明确指定返回值类型，除非函数的的返回值为Unit，函数参数不能添加var或val关键字  

## 局部函数  
```
fun local_fun(name:String = "hello"){
    fun local_fun_test()    {
        println("local function in a function. my name: $name")
    }
    local_fun_test()
}
```  

## 单表达式函数  
如果一个函数的函数体只有一个表达式，函数体可以直接写在 “=”之后
```
fun double(x: Int): Int = x * 2
```  
成员函数  
扩展函数  
匿名函数  
内联函数  
尾递归函数  
基于函数表达式和递归函数，来实现某些基本循环的的算法，采用这种方式可以有效的避免栈溢出的危险，当函数被关键字tailrec修饰，同时满足尾递归(tail recursion)的函数式编程方式的形式时，编译器就会对代码进行优化，消除函数的递归调用，产生一段基于循环实现的，快速而且高效的代码  
```
tailrec  fun sumSom(start:Int, end:Int, result:Int): Int{
    var res = result
    var sta = start
    while (sta <= end)   {
        res += sta
        sta++
    }
    return  res
}
```  

## 高阶函数  
```
fun gaojie_fun(a:Int, b:Int, sumSom:(Int, Int, Int) -> Int): Int{
    if (a > b)    {
        return sumSom(0, a, 0)
    }
    else {
        return sumSom(0, b, 0)
    }
}
```  

## 类  
所有的类默认都是不可继承的，即默认被final修饰符修饰。在创建一个类并作为父类时，需手动添加open或者abstract关键字，以声明此类可被继承；声明的属性值必须得初始化，属性必须初始化，否则编译报错，或者将此属性用abstract修饰符修饰。在abstract修饰的属性值，即使不用初始化，必须声明其数据类型，并在其子类初始化；默认为被public修饰符修饰，也就意味着其他类可以通过其类任意调用。声明属性时，编辑器会自动生成getter和setter方法，var是允许有getter 和 setter方法，如果变量是val声明的，它类似于Java中的final，所以如果以val声明就不允许有setter方法，如果属性值的数据类型可以通过编译器自动推断，或者在getter和setter方法中并没有对属性做特殊处理，这些方法都可以省略；如果你想改变访问的可见性或者是对其进行注解，但是又不想改变它的默认实现，那么你就可以定义set和get但不进行实现  
```
var setterVisibility: String = "abc"
private set // 设值方法的可见度为 private, 并使用默认实现
var setterWithAnnotation: Any? = null
@Inject set // 对设值方法添加 Inject 注解

class Person { 
    var lastName: String = "zhang"
    get() = field.toUpperCase()
    set
 
    var no: Int = 100
    get() = field
    set(value) {
        if (value < 10) {
            field = value
        } else {
            field = -1
```  
getter方法的可见性与属性的可见性一致。假如声明一个public变量，将其的getter方法用其他修饰符修饰，会报错，setter方法可以自定义修饰符，其修饰符不一定与属性的修饰符一致，其使用范围由修饰符决定，并不一定与属性的适用范围一致  

lateinit修饰符只能用于 var 属性, 而且只能是声明在类主体部分之内的属性(不可以是主构造器中声明的属性)，此类属性不能有自定义的取值方法和设值方法，属性类型必须是非 null 的, 而且不能是基本类型，被声明延迟初始化的属性，必须在使用以前调用其初始化方法，否则会报异常  
```
class MyInfo {
    lateinit var person: Person
 
    fun initData() {
        person = Person()
    }
}
 
// Test
fun main(args: Array<String>) {
    val myInfo: MyInfo = MyInfo()
 
    myInfo.initData()
    myInfo.person.doSwim()
}
```  

## 数据类  
创建一些数据里，没有任何逻辑功能，仅仅来用来保存数据，在Kolin中，将这些类统一称为数据类，用关键字data标记，数据类的声明条件  
* 主构造器至少要有一个参数
* 主构造器的所有参数必须标记为 val 或 var 
* 数据类不能是抽象类、open 类、封闭(sealed)类、或内部(inner)类
* 数据类不能继承自任何其他类(但可以实现接口)  

如果数据类有无参构造函数，需在主构造函数中将成员属性初始化，编译器会根据主构造器中声明的全部属性, 自动推断产生以下成员函数:  
* equals()/hashCode()函数对
* toString() 函数, 输出格式为 “User(name=John, age=42)” 
* componentN() 函数群, 这些函数与类的属性对应, 函数名中的数字 1 到 N, 与属性的声明顺序一致
* copy() 函数（功能是复制一个对象，然后修改其一部分属性，但保持其他属性保持不变。copy并不是简单的复制，它会创建新的对象，对于基本类型的属性，其值会被拷贝并赋值，但是对于引用类型，其只是将其地址传递过去，还是进行浅拷贝，并没有创建新的对象）  

如果在该数据类或者基类中重写了以上某个成员函数，将不会再自动推断，以重写的为准
```
data class Person(var name: String, var age: Int) {
} 
data class MyInfo(val no: Int, var person: Person) { 
    override fun hashCode(): Int {
        return super.hashCode()
    }
}
val jane = Person("Jane", 35)
val (n, a) = jane
println("$n, $a years of age") // 打印结果将是 "Jane, 35 years of age"

b?.length  如果b不是null，这个表达式将会返回b.length
val len = b?.length ?: -1  如果 ?: 左侧的表达式值不是null, 就会返回表达式的的值，否则，返回右侧表达式的值
val len = b!!.length  如果b不为null, 这个表达式将会返回这个非null值，否则如果b是 null, 这个表达式就会抛出一个 NPE
```  

## 嵌套类  
```
class nested {       
    val nest_i = 88
    fun nest_fun()    {
        //println("$name")      // cannot access outer var
        println("nest_fun:  $nest_i")
    }
}
val nested = Student.nested()
nested.nest_fun()
```  

## 内部类  
```
inner  class innerted {     
    val inner_i = 99
    fun inner_fun()    {
        print("inner_fun:  $inner_i")
        println("       $name")      // can access outer var
    }
}
s.innerted().inner_fun()        // only called by specific object
```  

## 修饰符  
在Kotlin中，存在private、protected、internal以及 public等四种修饰符，它们可用于修饰类、对象、接口、构造器、函数、属性、以及属性的设值方法等，局部变量, 局部函数, 以及局部类, 都不能指定可见度修饰符  
`包-Kotlin中，包级别的定义称为top-level，即直接定义在包内，可定义函数、属性、类、对象及接口`  
* public：默认修饰符，被其修饰的在任何位置都能访问
* private：只能在当前源文件内使用
* internal：在同一模块内使用（是指一起编译的一组 Kotlin 源代码文件）
* protected：在当前类及其子类内访问  

`类和接口`  
* public：默认修饰符，被其修饰的在任何位置都能访问
* private：表示只在这个类(以及它的所有成员)之内可以访问
* internal：在同一模块内使用
* protected：在当前类及其子类内访问  

`访问强度：public>internal>protected>private`  

一般在类的主构造器都不使用修饰符修饰，即默认使用public修饰符。但在实际开发时，主构造器可能被其他修饰符修饰。比如在创建单例模式时，为了防止在外部调用主构造器，主构造器就是使用private修饰符修饰。当使用其他修饰符修饰构造器时，需要明确添加一个constructor关键字  

## 构造函数  
主构造函数（primary constructor）不能包含任意代码，只能声明构造函数的参数，参数若不设置为var或val，默认为val，其值不可修改。若想初始化代码，放在以init做前缀的初始化块内。如果主构造函数没有任何注解或者可见性声明，constructor关键字可以被省略，主构造函数可以定义一个，如果定义了，则二级构造函数必须用this调用主构造函数，主构造函数的函数体是init块，每个构造函数必须初始化所有成员变量  
```
class Person(var name : String, var age : Int) { 
    init {
        name = "zhang"
        println("Person init")
    }
```    
每次Person创建对象时，都会调用init初始化代码块  
```
calss Customer public inject constructor (name: String) {...}
```
如果类有主构造函数，每个二级构造函数都要或直接或间接通过使用 this 关键字初始化主构造函数  
```
class Person(var name : String, var age : Int) {
     constructor( name: String,  age: Int, addr: String) : this(name, age) {
     }
     constructor(name: String, age: Int, addr: String, sex: Int) :  this(name, age, addr){
```       
二级构造函数的参数不能设定为var或val，但默认为val类型，主构造函数里面的参数即为该类的属性，而二级构造函数中的参数，只是参数，并不是该类的属性；在JVM虚拟机中，如果主构造函数的所有参数都有默认值，编译器会生成一个无参的构造函数；如果一个非抽象类没有声明构造函数(主构造函数或二级构造函数)，它会产生一个没有参数的构造函数；构造函数的权限修饰符为public，如果想的类有公共的构造函数私有化，你就得声明一个空的主构造函数，并用private修饰  

在Kotlin中如果函数体只有一句的话，可以省略大括号，=后面就是返回值，这种简化函数的方法类似于lambda表达式  

## 伴随  
在类的内部声明，使用companion关键字标记声明，其名字可省略，可代替Java中的static，但更像Java中成员内部类  
```
abstract class Person(val name:String, val age:Int, val gender:Char){
    abstract val DNA:String
    companion object DesPerson {
        val des:String = "I am a companin val"
        fun des()  {
            println("I am a companin fun")
        }

class Student(name:String, age:Int, gender:Char, val major:String):Person(name, age, gender), iTalk, iWalk{
    override val DNA: String
        get() = "SSSSSSSSSSSSSSSSSSUUUUUUUUUUUUUUUEEEEEEEEEEEEEEEEEE"
    init {
        total++
    }
    companion object  {
        var total = 0
        fun getTotal() {
            println("total student: $total")
        }
    }
```  

## 扩展  
扩展函数并没有对原类做修改，而是为被扩展类的对象添加新的函数，扩展函数是静态解析的，它并不是接收者类型的虚拟成员，意味着调用扩展函数时，具体被调用的是哪一个函数，由调用函数的的对象表达式来决定，而不是动态的类型决定，扩展属性实际上不会向类添加新的成员，因此无法让一个扩展属性拥有一个后端域变量；对于扩展属性不允许存在初始化器，扩展属性的行为只能通过明确给定的取值方法与设值方法来定义，意味着扩展属性只能被声明为val而不能被声明为var，如果强制声明为var，即使进行了初始化，在运行也会报异常错误，提示该属性没有后端域变量；在类的内部，也可以为另外一个类定义扩展  
```
val Student.last_idx:Int
    get() = Student.total - 1
fun Student.run() {
    println("Expand function: I am a student running.")
}
```  

## 伴随对象的扩展  
```
val Student.Companion.number:Int
    get() = 94
fun Student.Companion.printNum(){
    println("Expand Companion function: my num is ${Student.Companion.number}")
}
this@MyInfo.doFly() // 调用本类被隐藏额函数
```  

## 异常  
在Kotlin中，所有的异常都继承于Throwable。对于每一个异常而言，它不仅仅包括异常的信息，还可以选择性包括异常的原因；try、catch、finally三个语句块均不能单独使用，三者可以组成 try…catch…finally、try…catch、try…finally三种结构，catch语句可以有一个或多个，finally语句最多一个；try、catch、finally三个代码块中变量的作用域为代码块内部，分别独立而不能相互访问。如果要在三个块中都可以访问，则需要将变量定义到这些块的外面；多个catch块时候，只会匹配其中一个异常类并执行catch块代码，而不会再执行别的catch块，并且匹配catch语句的顺序是由上到下；try表达式中可以有0个或多个catch代码段，finally 代码段可以省略，但是catch或 finally代码段至少要出现一个与try配对出现  

## 抽象  
抽象方法是一种特殊的方法：它只有声明，而没有具体的实现，抽象方法必须用abstract关键字进行修饰，抽象方法不用手动添加open，默认被open修饰，含有抽象方法的类成为抽象类，必须由abtract关键字修饰，在抽象类中，不仅可以有抽象方法，同时可以有具体实现的方法；抽象属性就是在var或val前被abstract修饰，抽象属性在抽象类中不能被初始化 ，如果在子类没有主构造函数，要对抽象属性，手动初始化。如果子类中有主构造函数，抽象属性可以在主构造函数中声明  

## 抽象类和普通类的区别  
抽象方法必须为public或者protected（因为如果为private，则不能被子类继承，子类便无法实现该方法），缺省情况下默认为public；抽象类不能用来创建对象；如果一个类继承于一个抽象类，则子类必须实现父类的抽象方法，如果子类没有实现父类的抽象方法，则必须将子类也定义为abstract类，如果抽象类中含有抽象属性，再实现子类中必须将抽象属性初始化，除非子类也为抽象类  

## 接口  
接口和抽象类不同的是，接口不能保存状态，可以有属性但必须是抽象的，接口是通过关键字 interface 来定义，接口中允许定义属性，但不允许直接初始化，默认是 abstract的，接口不会保存属性值，实现接口时，必须重载属性；接口声明与类一致，接口是特殊的抽象类，接口可以继承类或接口，java中是不允许接口继承类的  
```
public interface SomeInterface{
    var value:String //默认abstract
    fun reading() // 未实现
    fun writing(){  //已实现
        // do nothing
    }
}

open class Box constructor(var value:String){
    open var name:String = "jason"
}
interface IBox :Box{
    fun beat(){
        println("beat $name")
    }
}
```  
一个类只能继承一个抽象类，却可以实现多个接口；抽象类是对事物的抽象，即对类抽象，而接口是对行为的抽象，抽象类是对整个类整体进行抽象，包括属性、行为，但接口却是对类局部（行为）进行抽象。飞机和鸟是不同类的事物，但是它们都有一个共性会飞；在设计的时候，将飞机设计为一个类Airplane，将鸟设计为一个类Bird，但是不能将 飞行 这个特性也设计为类，因此它只是一个行为特性，并不是对一类事物的抽象描述。此时可以将 飞行 设计为一个接口Fly，包含方法fly( )，然后Airplane和Bird分别根据自己的需要实现Fly这个接口  

继承是一个 “是不是”的关系，而 接口 实现则是 “有没有”的关系；抽象类作为很多子类的父类，它是一种模板式设计，而接口是一种行为规范，它是一种辐射式设计；对于抽象类，如果需要添加新的方法，可以直接在抽象类中添加具体的实现，子类可以不进行变更；而对于接口则不行，如果接口进行了变更，则所有实现这个接口的类都必须进行相应的改动  

## 继承  
以“:”操作符，完成子类继承父类；在官方文档上Any类是所有Kotlin文件的根，所有的类均继承于Any类；创建一个类时，若未声明父类，会隐式的继承于Any；Any类并不是Java.lang.Object类，其仅有equals(other: Any?)、 hashCode()、toString()等三个方法，并没有任何其他成员属性；在kotlin中， 实现继承通常遵循如下规则：如果一个类从它的直接父类继承了同一个函数的多个实现，那么它必须重写这个函数并且提供自己的实现(或许只是直接用了继承来的实现) 为表示使用父类中提供的方法我们用 super 表示，子类构造函数要么单独初始化父类成员变量，要么调用super  
```
open class A {
    open fun f () { print("A") }
    fun a() { print("a") }
}  
interface B {
    fun f() { print("B") } //接口的成员变量默认是 open 的
    fun b() { print("b") }
}  
class C() : A() , B{
    override fun f() {
        super<A>.f()//调用 A.f()
        super<B>.f()//调用 B.f()
```  

当子类继承了某个类之后，便可以使用父类中的成员变量，但是并不是完全继承父类的所有成员变量。具体的原则如下：
能够继承父类的public和protected成员变量；不能够继承父类的private成员变量；对于父类的包访问权限成员变量，如果子类和父类在同一个包下，则子类能够继承；否则，子类不能够继承；对于子类可以继承的父类成员变量，如果在子类中出现了同名称的成员变量，则会发生隐藏现象，即子类的成员变量会屏蔽掉父类的同名成员变量。如果要在子类中访问父类中同名成员变量，需要使用super关键字来进行引用  

## 解构声明  
就是将一个对象解构(destructure)为多个变量，意味着一个解构声明会一次性创建多个变量。有两个动作：声明了多个变量；将对象的属性值赋值给相应的变量。在Kotlin-数据类中，编译器会根据主构造器中声明的全部属性, 自动推断产生componentN() 函数群, 这些函数与类的属性对应，函数名中的数字1到N，与属性的声明顺序一致，但对于普通类的成员属性，编译器并不会自动推断产生componentN() ，此时componentN()，需要我们自己定义了。对于自定义的componentN()，需要注意以下几点：  
1. componentN()函数需要标记为 operator , 才可以在解构声明中使用 
2. componentN()函数的返回值类型必须与属性类型一致  
```
fun main(args: Array<String>) {
    val p1 = Person("sky", 20)   
    val p2 = Person("lotus", 21)
    val p3 = Person("cherry", 22)
    var p = listOf(p1, p2, p3)
    for ( (Name, Age) in p )
        println("Name: $Name, Age: $Age") // destructuring Person, auto generlizes p1.component1(), p1.component2()...
    val p6 = Person("audiorn", 26)
    p6.mobile = "13410241111"
    var (Name, Age, Moblie) = p6        // destructuring Person, auto identifying p1.component3()
    println("Name: $Name, Age: $Age, Mobile: $Moblie")

    var m = HashMap<String, Person>()
    m.put("s", p1)
    m.put("l", p2)
    m.put("c", p3)
    for ((s, p) in m)
        println("s: $s, person: $p")        // print order s->c->l
}
data class Person(var name: String, var age: Int) {   // must be data class
    var mobile:String? = null           // ? can be empty
    operator fun component3():String?    {      // must be component3()
        return this.mobile!!        // !! NotEmpty judgement
    }
}
```  

## 集合-set  
其对象不排序、不重复；Kotlin没有专门的语法用来创建set，可以使用标准库中的方法比如setOf()-只读mutableSetOf()-可写；
Kotlin中，标准的数据类型，比如基本数据类型、String等都有了相应的判定方式，对于自定义的类，判断两个对象的是否重复标准是hashCode()和equals()两个参考值，只有两个对象的hashCode值一样与equals()为真时，才认为是相同的对象，所以自定义的类必须要要重写hashCode()和equals()两个函数  
```
fun main(args: Array<String>) {
    val b1 = Book("Walden", 444, "Lusou")
    val b2 = Book("Gone with the Wind", 888, "Feiwenli")
    val b3 = Book("Tales of two towns", 555, "Dikens")
    val b4 = Book("Walden", 666, "Lucas")
    val s1 = setOf<Book>(b1, b2, b3, b4)
    for (item in s1) {
        println("b: $item,  hashcode: ${item.hashCode()}")
    }
    var s2 = mutableSetOf<Book>(b1, b2, b3, b4)
    s2.add(Book("GodFather", 777, "Al, Pacino"))
    s2.remove(b4)
    for (item in s2) {
        println("b: $item,  hashcode: ${item.hashCode()}")
    }
}
data class Book(var name:String, var page:Int, var anthor:String){
    override fun equals(other: Any?): Boolean {
        if (other is Book) {
            return other.page == this.page
        }
        return super.equals(other)
    }
    override fun hashCode(): Int {
        return name.hashCode()
    }
    override fun toString(): String {
        return this.name
    }
}
```

## 集合-List  
其对象以线性方式存储，有序，可重复；与set一样，Kotlin并没有提供创建List的函数，如果想创建一个List，可以调用标准库中的方法，listOf() , mutableListOf()。listOf()是使用ArrayList实现的，返回的list是只读的，其内存效率更高。在开发过程中，可以尽可能的多用只读List，可以在一定程度上提高内存效率  

## 集合-Map  
是一种把键对象和值对象映射的集合，它的每一个元素都包含一对键对象和值对象。 Map没有继承于Collection接口，从Map集合中检索元素时，只要给出键对象，就会返回对应的值对象。Kitlin中，与list、set一样的是，Map也分为只读map和可变两种。Kotlin中，创建Map时，需调用标准库中的系列函数，如mapOf()，mutableMapOf()，他们所创建的Map是基于Java的LinkedHashMap，Kotlin并不支持Java的TreeMap  
`HashMap`：Map基于散列表的实现，插入和查询“键值对”的开销是固定的。可以通过构造器设置容量capacity和负载因子load factor，以调整容器的性能  
`LinkedHashMap`： 类似于HashMap，但是迭代遍历它时，取得“键值对”的顺序是其插入次序，或者是最近最少使用(LRU)的次序，只比HashMap慢一点，而在迭代访问时发而更快，因为它使用链表维护内部次序  
```
var hashMap = hashMapOf<Int, Book>(1 to b1, 2 to b2, 3 to b3)
var linkedMap = linkedMapOf<String, Book>("sky" to b1, "lotus" to b4) 
```  

## 函数重载  
```
fun  add():Int
fun  add(x:Int):Int
fun  add(x:Int, y:Int):Int
fun  add(x:Double, y:Double):Double
fun  add(x:Int, y:Int, z:Int):Int
```  

## 运算符重载  
```
class override_operator(var x:Int, var y:Int){
constructor():this(0, 0)
operator fun plus(z:override_operator):override_operator
operator fun minus(z:override_operator):override_operator
operator fun times(z:override_operator):override_operator
operator fun div(z:override_operator):override_operator
operator fun mod(z:override_operator):override_operator
operator fun compareTo(z:override_operator):Int
override fun equals(z:Any?):Boolean
其他运算符：rangeTo，unaryPlus，unaryMinus，not，inc，des
```
