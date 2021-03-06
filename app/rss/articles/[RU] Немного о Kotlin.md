---
title: '(RU) Немного о Kotlin.'
url: https://habrahabr.ru/post/277479/
categories:
    - Kotlin
    - Review
author: '@fogone'
date: Feb 20, 2016  08:25
---
![Kotlin](https://habrastorage.org/getpro/habr/post_images/b30/fc2/317/b30fc231752f0d1d270b4c80538a28dc.jpg)


На днях JetBrains после пятилетней работы выпустила первый релиз языка Kotlin. Давайте посмотрим, что же это за язык, попробуем разобраться зачем и для кого он, какие имеет функциональные особенности. Скорее всего в статью затесались и личные впечатления от языка, но я старался, чтобы они не влияли на изложение полезной информации. Если вы еще ничего или почти ничего не знаете о Kotlin, то я завидую вам, ибо по моему ощущению почитать про инструмент, который ты долго ждал, сродни распаковке новогоднего подарка. Впрочем судите сами.

## Что такое Котлин

Котлин — это небольшой остров в Финском заливе недалеко от Санкт-Петербурга. Видимо, тем самым создатели дают отсылку к тому, что новый язык, как остров Котлин — младший русский брат далекого острова [Ява](https://ru.wikipedia.org/wiki/%D0%AF%D0%B2%D0%B0).

## Для кого этот язык

Действительно, новый язык компилируется в JVM байт-код (есть еще и компиляция в JavaScript, но так как релиз компилятора именно в JVM, эту тему придется опять отложить). А это значит, что он может заинтересовать всех, кто имеет дело с Java-машиной и в целом с языками со сборщиком мусора (а с выходом релиза компиляции в JavaScript покрытие и возможности будут еще шире).

## Простой и совместимый

Две главных особенности котлина, на мой взгляд, это его простота и полная совместимость с Java. Котлин создавался компанией, которая делает очень много продуктов на Java и которая хорошо разбирается в современных инструментах разработки. Запрос на новый язык витает в воздухе давно, но сделать такой язык, который бы позволил взять (огромную) готовую кодовую базу Java, обычных Java-разработчиков, дать им новый инструмент и бесшовно (но более эффективно) продолжать разработку — такого инструмента до появления котлина не существовало. Создатели нового языка, на мой взгляд, очень хорошо почувствовали потребности бизнеса и разработчиков: бизнесу дали возможность увеличить эффективность разработчиков, а разработчикам дать современный инструмент для разработки. И когда я говорю о «современном инструменте», я, конечно, имею ввиду не только компилятор, но и поддержку в IDE, без которой лично моя деятельность разработчика мне видится совсем немыслимой.
В итоге: простота позволяет использовать язык почти любому Java-разработчику, который готов потратить полчаса на то, чтобы посмотреть туториал или спецификацию языка, обратная совместимость же позволяет использовать язык в уже существующем проекте.

## Production-ready

Конечно, в первую очередь, запрос на этот язык был у самой JetBrains, отсюда и некоторое понимание, каким он должен быть. Более того, JetBrains же внутри компании его и тестировала: к моменту выхода этого релиза у компании уже есть как минимум один [крупный продукт сделанный чисто на котлине](https://blog.jetbrains.com/dotnet/2016/01/13/project-rider-a-csharp-ide/) (да, я понимаю, что платформа по прежнему написана на Java). Отсюда можно предположить, что заявления о готовности языка к продакшену не голословны. А из своего же опыта использования котлина с 2012 года могу сказать, что из тех проблем, с которыми я сталкивался в дорелизные времена, до релиза ни одна не дожила. Есть еще небольшие проблемы с плагином для IDEA, но сам компилятор работает как часы.

## Совместим с Java 1.6

Это очень важный момент, ведь именно эта версия Java используется во всех современных версиях Android, и, не смотря на [запланированный переход на OpenJDK](http://venturebeat.com/2015/12/29/google-confirms-next-android-version-wont-use-oracles-proprietary-java-apis/), восьмая версия попадет в руки разработчикам под мобильные устройства не так скоро как хотелось бы. Конечно, есть всякие ретролямбды и другие ухищрения, но котлин — это не только лямбды для андроид, но и современный язык, позволяющий сделать разработку под андроид [проще](https://kotlinlang.org/docs/tutorials/android-plugin.html) и приятней без особых затрат. А увеличение размера apk совсем не значительно по нынешним временам: 823KB (для версии 1.0.0)

## Особенности

Полный список возможностей, конечно, лучше искать в [документации](https://kotlinlang.org/docs/reference/), я же постараюсь отразить наиболее важные на мой взгляд моменты в общих словах:

## Null safety

Почему-то исторически так сложилось, что [эта](https://kotlinlang.org/docs/reference/null-safety.html) особенность котлина вспоминается первой. И хотя она безусловно важна, на мой взгляд не является важнейшей. Здесь язык позволяет, определяя переменные, поля, константы и тд, указать, может ли в них храниться ссылка на null. Поднимает на новый уровень идею аннотаций вроде @Nullable и NotNull, позволяет умно приводить к не-nullable типу после проверки её на null. Надо отметить, что бывают случаи, когда эта особенность расходится с моим закостенелым разработкой на Java представлением о том как должны быть сделаны некоторые вещи, но после некоторого раздумья хорошее решение всегда находится.

```kotlin
/* вопросом помечаем, что может прийти null */
fun someFunction(someNullableParam:SomeType?) {
    if(someNullableParam != null) {
         // smart cast. Компилятор видит, что передаваемое
         // значение не null и разрешает его передать в функцию
        anotherFunction(someNullableParam)
    }
}

/* здесь же уже null не пройдет, в попытке передать
 * null или nullable значение компилятор выдаст ошибку */
fun anotherFunction(someParam:SomeType) {
    // делаем что-то без опаски, что переданное значение null
}
```

## Выведение типов

Да, котлин почти везде где возможно, умеет вывести тип, однако тип всё же придется определить для публичных методов и свойств, что очень разумно (мне подсказывают, что это неправда — с какой-то версии это убрали):

```kotlin
// Kotlin в курсе, что здесь List<Char>
val result = sequenceOf(10).map { it.toString() }.flatMap { it.toCharArray().asSequence() }.toList()
```


## Extension methods

[Возможность](https://kotlinlang.org/docs/reference/extensions.html), которой мне остро не хватает в Java для увеличения гибкости языка и решений. Заключается в возможности определить метод для типа отдельно от его (типа) объявления. Такая функция, конечно, не будет виртуальной и никак не меняет класса, которому мы добавляем метод, однако позволяет добавить как утилитарную функциональность для уже существующего кода, так и разгрузить интерфейс от этих же утилитарных методов.

```kotlin
interface Vector2 {
    val x:Float // это не поле, а ридонли свойство (property)
    val y:Float // в Java были бы методы getX() и getY()
}

/* Extension property. Без поля в классе, просто getLength() */
val Vector2.length:Float
    get() = (x * x + y  * y).sqrt() // притворимся, что такая extension-функция для Float уже существует

/* переопределяем оператор + */
operator fun Vector2.plus(other:Vector2):Vector2 = createVector(x+this.x, y+this.y) // какой-то способ создать новый вектор

/* без тела, после знака = пишем одно выражение */
fun Vector2.dot(x: Float, y: Float): Float = x * this.x + y * this.y

/* Помечая функцию с одним параметром как infix,
 * мы позволяем вызывать её через пробел: v1 dot v2 */
infix fun Vector2.dot(vec2: Vector2): Float = dot(vec2.x, vec2.y)

fun usage(vec1:Vector2, vec2:Vector2) {
    val dotProduct = vec1 dot vec2
    val sum = vec1 + vec2 // на выходе новый вектор
    val length = sum.length // обращаемся просто как к свойству

}
```

## Лямбды

Конечно, как любой современный язык с претензией на возможности функцинального программирования, у котлина функция — это сущность первого класса, если переводить дословно. Т.е. функции можно не только объявлять прямо в пакете (из джавы они видны всё равно в классах — по имени файла), но и передавать в качестве параметров, возвращать из других функций и тд. И сейчас, конечно, никого этим не удивишь, но например в сравнении с Java, где синтаксически функций как таковых нет (а только функциональные интерфейсы), в котлине полноценный синтаксис для объявления функции:

```kotlin
/* передаем в одну функцию другую -- принимает в параметр Int
 * и Int же возвращает. Возвращаем её же, только с фиксированным
 * значением в качестве параметра */
fun passTen(func: (Int)->Int ): ()->Int {
    return { func(10) }
}
```

## Extension-лямбды

Наряду с extension-методами, это еще одна моя любимая фича. Позволяет определить лямбду, которая будет еще и extension-методом. Звучит не очень, да. Посмотрим на примере:

```kotlin
class World(val name:String = "world")

val printName:World.()->Unit = {
     // интерполяцией в синтаксисе языка сейчас уже
     // никого не привлечь на темную сторону
    println("Hello $name")
}

val world = World()

 // вызываем нашу функцию как будто это метод нашего класса!
world.printName()
```

Эта возможность особенно интересно смотрится в [билдерах](https://kotlinlang.org/docs/reference/type-safe-builders.html), посмотреть на которые я предлагаю вам самостоятельно — на случай, если вам интересно, как получаются вот такие конструкции:

```kotlin
html {
    head {
      title {+"XML encoding with Kotlin"}
    }
    body {
      h1 {+"XML encoding with Kotlin"}
      a(href = "http://kotlinlang.org") {+"Kotlin"}
    }
}
```

## Inline-фукнции

Помечая функцию как inline мы просим компилятор поместить её по месту использования. Чаще всего такими вещами занимается рантайм, но есть кейзы, когда мы точно знаем, что фукнция это просто шорткат для какого-то действия — особенно эффективно это работает с передаваемыми лямбдами:

```kotlin
/* передаваемой лямбдой block сейчас уже никого не удивишь.
 * Главно, что лишних затрат на вызов этой функции не будет вообще,
 * иногда мне кажется, что это что-то вроде макросов */
inline fun lock(lock:Lock, block:()->Unit) {
    lock.lock()
    try {
        block()
    } finally {
        lock.unlock()
    }
}

fun usage() {
    lock(Lock()) {
        // делаем что-то внутри блокировки
    }
}
```

Конечно, на такие функции накладывается серия ограничений, подробнее см. [документацию](https://kotlinlang.org/docs/reference/inline-functions.html).

## Делегирование

В котлине есть два типа делегирования. [Первый](https://kotlinlang.org/docs/reference/delegation.html), который позволяет делегировать все методы реализуемого интерфейса к какому-то инстансу этого типа:

```kotlin
interface Connection {
    fun connect()
}

/* здесь мы видим стандартный для котлина синтаксис определения
 * класса вместе с параметрами конструктора и свойствами --
 * в данном случае connection будет и в конструкторе и в поле.
 * Есть возможность определить и множественный конструктор
 * см https://kotlinlang.org/docs/reference/classes.html#constructors
 * И, наконец, мы видим что класс реализует интерфейс Connection, все методы
 * которого делегируются к переданному в конструктор экземпляру Connection-а.
 * При желании их конечно можно переопределить в теле класса */
class ConnectionWrapper(val connection:Connection) : Connection by connection
```


У этого синтаксиса есть ряд ограничений. Например, инстанс для делегирования должен быть известен до вызова конструктора.

Второй тип делегирования — это [delegated properties](https://kotlinlang.org/docs/reference/delegated-properties.html). Позволяет определить объект с методами get (и set для var), к которым будет осуществляться делегирование доступа при обращении к свойству объекта.

```kotlin
class Foo {
    /* это делегат из стандартной библиотеки,
       позволяет отложить инициализацию поля
       до первого обращения к нему */
    private val someProeprty by lazy { HavyType() }
}
```

## Generics

Создатели котлина несколько [улучшили](https://kotlinlang.org/docs/reference/generics.html) Java-дженерики. Из-за совместимости с джавой не всё получилось как хотелось бы, но им удалось исправить много неприятных моментов, которые не учли их предшественники при работе над Java 5.

## Деструктуризация

```kotlin
val (first, second) = someFunc()
```

Чтобы такой код заработал, возвращаемое значение из someFunc() должно быть типа, у которого есть (можно extension) методы component1(), component2():

```kotlin
class Foo {
    fun component1():String = "test"
    fun component2():Int = 10
}
fun someFunc():Foo = Foo()

// или так, to -- в это такой infix extension-метод определенный
// для Any, который создает экземпляр класса Pair, метод hashMapOf
// в свою очередь принимает vararg параметр таких пар
val map = hashMapOf(1 to "test")
for ((id, name) in map) {
    //  такой синтаксис возможен, потому что для Map-а определен метод iterator()
    // возвращающий набор Map.Entry, а для него в свою очередь определены два
    // extension-метода component1() и component2()
}
```

## Data-классы

Сахар компилятора для создания бинов:

```kotlin
data class Bean(val a:String, val b:Int)
```

Создает бин с полями + автогенерирует equals+hashCode+toString()+componentN из раздела выше, что позволяет писать такой код:

```kotlin
fun someFunc():Bean = Bean("test", 10)
val (a, b) = someFunc()
```

Полезная вещь, но о нюансах см. пункт «О грустном».

## Стандартная библиотека

Конечно, нельзя не упомянуть и [стандартную библиотеку](https://kotlinlang.org/api/latest/jvm/stdlib/index.html). Так как котлин нацелен в первую очередь на работу вместе с Java, то и целиком своей стандартной библиотеки у него нет. Большая часть стандартной библиотеки Kotlin нацелена на улучшение и исправление библиотеки старшего брата — Java. Однако, это тема для другой большой статьи.

## О грустном

Вы могли подумать, что это идеальный продукт, но нет, есть и неприятные моменты:

## IDE

Над плагином еще работать и работать, периодически выдает эксепшены, плохо умеет в toString() в дебаге, а так же любит промахиваться по ссылке на исходник, иногда (видимо из за особенностей инлайна) путает где поставлен брэкпоинт и тому подобные проблемы. Это всё конечно со временем наверняка поправят, но сейчас мы имеем именно это.

## Data-классы

Надо признать, что идея была хорошая, но в данный момент есть масса [ограничений](https://kotlinlang.org/docs/reference/data-classes.html), наложенных на этот тип классов, что позволяет их использовать в сильно более ограниченном числе кейзов, нежели хотелось бы. Создатели языка обещают поработать над решением этой проблемы, но пока так.

## Некоторая неряшливость

Конечно, неряшливость в первую очередь в головах, но краткость синтаксиса иногда играет злую шутку, и местами код выглядит неважно. Возможно, наличие стайл-гайда несколько эту проблему исправило бы, но пока иногда приходится постараться, чтобы не только хорошо работало, но и красиво выглядело. Особенно на мой субъективный взгляд страшно выглядят get, set для свойств.

## В заключение

Одной статьёй невозможно охватить все особенности и аспекты языка, но я и не пытался. Моей задачей было познакомить с языком, может быть обратить на него внимание. Тот, кто заинтересовался, сможет найти больше в [документации](https://kotlinlang.org/docs/reference/), посмотреть [исходники](https://github.com/JetBrains/kotlin), [попробовать](http://try.kotlinlang.org/), [задать вопрос](https://habrahabr.ru/company/JetBrains/blog/277573/). Сложно предсказать популярность этого языка, но уже сейчас видно, что такого продукта многие ждали, проекты на котлине появляются как грибы, а после релиза частота их появления увеличится еще. По моему впечатлению, языка хорошо сбалансирован и продуман — во время написания кода, складывается ощущение, что всё на своем месте. Если вы используете jvm или любой другой язык со сборкой мусора, есть смысл обратить внимание на котлин. Лично для меня, котлин — это тот инструмент, которого я долго ждал и теперь не представляю, как мог бы обходиться без него.
