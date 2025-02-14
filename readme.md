***

<p align="center">
🎉 Ультра-упрощенное объяснение шаблонов проектирования! 🎉
</p>
<p align="center">
Тема, которая может легко заставить любой разум колебаться. Здесь я пытаюсь сделать её более запоминающейся для вас<br> (и, возможно, для себя), объясняя её самым <i>простым</i> способом.
</p>

***

<br>

|[Порождающие шаблоны проектирования](#порождающие-шаблоны-проектирования)|[Структурные шаблоны проектирования](#структурные-шаблоны-проектирования)|[Поведенческие шаблоны проектирования](#поведенческие-шаблоны-проектирования)|
|:-|:-|:-|
|[Фабрика](#-фабрика)|[Адаптер](#-адаптер)|[Цепочка обязанностей](#-цепочка-обязанностей)|
|[Фабричный метод](#-фабричный-метод)|[Мост](#-мост)||
|[Абстрактная фабрика](#-абстрактная-фабрика)|[Компоновщик](#-компоновщик)||
|[Строитель](#-строитель)|[Декоратор](#-декоратор)||
|[Прототип](#-прототип)|[Фасад](#-фасад)||
|[Одиночка](#-одиночка)|[Приспособленец](#-приспособленец)||
||[Заместитель](#-заместитель)||

<br>

Введение
=================

Шаблоны проектирования — это решения повторяющихся проблем; **рекомендации по решению определенных проблем**. Это не классы, пакеты или библиотеки, которые вы можете подключить к своему приложению и ждать, пока произойдет волшебство. Это, скорее, рекомендации о том, как решать определенные проблемы в определенных ситуациях.

> Шаблоны проектирования — это решения повторяющихся проблем; рекомендации по решению определенных проблем

Wikipedia описывает их как

> В разработке программного обеспечения шаблон проектирования программного обеспечения — это общее многократно используемое решение часто встречающейся проблемы в данном контексте при проектировании программного обеспечения. Это не законченный дизайн, который можно преобразовать непосредственно в исходный или машинный код. Это описание или шаблон того, как решить проблему, который можно использовать во многих различных ситуациях.

⚠️ Будьте осторожны
-----------------

- Шаблоны проектирования не являются серебрянной пулей от всех ваших проблем.
- Не пытайтесь заставить их; если это сделать, то произойдет что-то плохое.
- Имейте в виду, что шаблоны проектирования — это **готовое** решение проблем, а не **поиск** решений; так что не думайте слишком много.
- Если использовать их в правильном месте и правильным образом, они могут оказаться спасителями; иначе они могут привести к ужасному беспорядку в коде.

> Также обратите внимание, что приведенные ниже примеры кода на PHP-7, однако это не должно вас останавливать, потому что концепции в любом случае те же.

Типы шаблонов проектирования
-----------------

* [Порождающие](#порождающие-шаблоны-проектирования)
* [Структурные](#структурные-шаблоны-проектирования)
* [Поведенческие](#поведенческие-шаблоны-проектирования)

Порождающие шаблоны проектирования
==========================

Простыми словами
> Порождающие шаблоны сосредоточены на том, как создать экземпляр объекта или группы связанных объектов.

Wikipedia говорит
> В программной инженерии порождающие шаблоны проектирования — это шаблоны проектирования, которые имеют дело с механизмами создания объектов, пытаясь создать объекты способом, подходящим для ситуации. Базовая форма создания объектов может привести к проблемам с дизайном или усложнить дизайн. Порождающие шаблоны проектирования решают эту проблему, тем или иным образом образом контролируя создание этого объекта.

 * [Фабрика](#-фабрика)
 * [Фабричный метод](#-фабричный-метод)
 * [Абстрактная фабрика](#-абстрактная-фабрика)
 * [Строитель](#-строитель)
 * [Прототип](#-прототип)
 * [Одиночка](#-одиночка)

🏠 Фабрика
--------------

Пример из жизни
> Предположим, вы строите дом и вам нужны двери. Вы можете либо надеть свою плотницкую одежду, принести немного дерева, клея, гвоздей и все необходимые инструменты, для изготовления двери и начать строить ее в своем доме, либо вы можете просто позвонить на фабрику и получить готовую дверь, чтобы вам не нужно было ничего узнавать о изготовлении дверей или разбираться с беспорядком, который возникает при ее изготовлении.

Простыми словами
> Фабрика просто генерирует экземпляр для клиента, не раскрывая ему никакой логики инстанцирования.

Wikipedia говорит
> В объектно-ориентированном программировании (ООП) фабрика — это объект для создания других объектов — формально фабрика — это функция или метод, который возвращает объекты изменяющегося прототипа или класса из некоторого вызова метода, который предполагается «новым».

**Пример кода**

В первую очередь у нас есть интерфейс Дверь(Door) и реализация
```php
interface Door
{
    public function getWidth(): float;
    public function getHeight(): float;
}

class WoodenDoor implements Door
{
    protected $width;
    protected $height;

    public function __construct(float $width, float $height)
    {
        $this->width = $width;
        $this->height = $height;
    }

    public function getWidth(): float
    {
        return $this->width;
    }

    public function getHeight(): float
    {
        return $this->height;
    }
}
```

Затем у нас есть фабрика по производству дверей(DoorFactory), которая изготавливает дверь(WoodenDoor) и возвращает ее
```php
class DoorFactory
{
    public static function makeDoor($width, $height): Door
    {
        return new WoodenDoor($width, $height);
    }
}
```

И тогда её можно использовать как
```php
// Сделай мне дверь 100x200
$door = DoorFactory::makeDoor(100, 200);

echo 'Width: ' . $door->getWidth();
echo 'Height: ' . $door->getHeight();

// Сделай мне дверь 50x100
$door2 = DoorFactory::makeDoor(50, 100);
```

**Когда использовать?**

Когда создание объекта — это не просто несколько действий, а включает в себя некоторую логику, тогда имеет смысл поместить его в выделенную фабрику, а не повторять везде один и тот же код.

🏭 Фабричный метод
--------------

Пример из жизни
> Рассмотрим случай с менеджером по найму. На каждую из должностей невозможно провести собеседование с одним человеком. В зависимости от вакансии она должна принять решение и делегировать этапы собеседования разным людям.

Простыми словами
> Фабричный метод предоставляет способ делегировать логику создания экземпляров дочерним классам.

Wikipedia говорит
> В программировании на основе классов фабричный метод — это шаблон создания, который использует фабричные методы для решения проблемы создания объектов без необходимости указывать точный класс создаваемого объекта. Это делается путем создания объектов путем вызова фабричного метода, либо указанного в интерфейсе и реализованного дочерними классами, либо реализованного в базовом классе и опционально переопределяемого производными классами, а не путем вызова конструктора.

**Пример кода**

Возьмем пример с нашим менеджером по найму выше. Прежде всего, у нас есть интерфейс Интервьюер(Interviewer) и несколько реализаций для него
```php
interface Interviewer
{
    public function askQuestions();
}

class Developer implements Interviewer
{
    public function askQuestions()
    {
        echo 'Вопрос о шаблонах дизайна';
    }
}

class CommunityExecutive implements Interviewer
{
    public function askQuestions()
    {
        echo 'Вопрос о построении сообщества';
    }
}
```

Теперь давайте создадим нашего менеджера по найму(HiringManager)
```php
abstract class HiringManager
{

    // Фабричный метод
    abstract protected function makeInterviewer(): Interviewer;

    public function takeInterview()
    {
        $interviewer = $this->makeInterviewer();
        $interviewer->askQuestions();
    }
}

```

Теперь любой ребенок может расширить его и предоставить необходимого интервьюера(Interviewer)
```php
class DevelopmentManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new Developer();
    }
}

class MarketingManager extends HiringManager
{
    protected function makeInterviewer(): Interviewer
    {
        return new CommunityExecutive();
    }
}
```

И тогда это можно использовать как
```php
$devManager = new DevelopmentManager();
$devManager->takeInterview(); // Вывод: Вопрос о шаблонах проектирования

$marketingManager = new MarketingManager();
$marketingManager->takeInterview(); // Вывод: Вопрос о построении сообщества
```

**Когда использовать?**

Полезно, когда в классе есть некоторая обобщенная обработка, но требуемый подкласс динамически определяется во время выполнения. Или, другими словами, когда клиент не знает какой именно подкласс ему может понадобиться.

🔨 Абстрактная фабрика
--------------

Пример из жизни
> Расширение нашего примера с дверью(Door) от Фабрики(Factory). В зависимости от ваших потребностей вы можете приобрести деревянную дверь в магазине деревянных дверей, железную дверь в магазине железных дверей или дверь из ПВХ в соответствующем магазине. Кроме того, вам может понадобиться парень с различными специальностями, чтобы установить дверь, например, плотник для деревянной двери, сварщик для железной двери и т.д. Как вы можете видеть, теперь существует зависимость между дверями, деревянная дверь нуждается в плотнике, железная дверь нуждается в сварщике и т.д.

Простыми словами
> Фабрика фабрик; фабрика, которая группирует отдельные, но связанные/зависимые фабрики вместе, не указывая их конкретные классы.

Wikipedia говорит
> Шаблон абстрактной фабрики предоставляет способ инкапсуляции группы отдельных фабрик, имеющих общую тему, без указания их конкретных классов.

**Пример кода**

Перевод примера с дверью выше. Во-первых, у нас есть интерфейс `Дверь(Door)` и некоторая реализация для него
```php
interface Door
{
    public function getDescription();
}

class WoodenDoor implements Door
{
    public function getDescription()
    {
        echo 'Я деревянная дверь';
    }
}

class IronDoor implements Door
{
    public function getDescription()
    {
        echo 'Я железная дверь';
    }
}
```

Кроме того, у нас есть несколько экспертов по установке для каждого типа дверей
```php
interface DoorFittingExpert
{
    public function getDescription();
}

class Carpenter implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'Я могу установить только деревянные двери';
    }
}

class Welder implements DoorFittingExpert
{
    public function getDescription()
    {
        echo 'Я могу установить только железные двери';
    }
}
```

Теперь у нас есть наша абстрактная фабрика, которая позволит нам создать семейство связанных объектов, т.е. фабрика деревянных дверей создаст деревянную дверь и эксперта по деревянным дверям, а фабрика по производству железных дверей создаст железную дверь и эксперта по железным дверям
```php
interface DoorFactory
{
    public function makeDoor(): Door;
    public function makeFittingExpert(): DoorFittingExpert;
}

// Фабрика деревянных дверей вернет деревянную дверь и плотника
class WoodenDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new WoodenDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Carpenter();
    }
}

// Фабрика железных дверей вернет железную дверь и сварщика
class IronDoorFactory implements DoorFactory
{
    public function makeDoor(): Door
    {
        return new IronDoor();
    }

    public function makeFittingExpert(): DoorFittingExpert
    {
        return new Welder();
    }
}
```

И тогда это можно использовать как
```php
$woodenFactory = new WoodenDoorFactory();

$door = $woodenFactory->makeDoor();
$expert = $woodenFactory->makeFittingExpert();

$door->getDescription();  // Вывод: Я деревянная дверь
$expert->getDescription(); // Вывод: Я могу установить только деревянные двери

// Same for Iron Factory
$ironFactory = new IronDoorFactory();

$door = $ironFactory->makeDoor();
$expert = $ironFactory->makeFittingExpert();

$door->getDescription();  // Вывод: Я железная дверь
$expert->getDescription(); // Вывод: Я могу установить только железные двери
```

Как вы можете видеть, фабрика деревянных дверей инкапсулировала `деревянную дверь`(wooden door) и `плотника`(carpenter), а фабрика железных дверей инкапсулировала `железную дверь`(iron door) и `сварщика`(welder). И это помогло нам убедиться, что для каждой из созданных дверей мы не получим неправильного специалиста.

**Когда использовать?**

Когда есть взаимосвязанные зависимости с непростой логикой создания.

👷 Строитель
--------------

Пример из жизни
> Представьте, что вы находитесь в Hardee's и делаете конкретный заказ, скажем, "Big Hardee" и вам передают его без *каких-либо вопросов*; это пример простой фабрики. Но бывают случаи, когда логика создания может включать в себя больше шагов. Например, вы хотите индивидуальный заказ как в Subway, у вас есть несколько вариантов приготовления бургера, например, какой хлеб вы хотите? Какие виды соусов вы бы хотели? Какой сыр вы бы хотели? и так далее. В таких случаях на помощь приходит шаблон проектирования Строитель.

Простыми словами
> Строитель позволяет создавать различные варианты объекта, избегая при этом загрязнения конструктором. Полезно, когда может быть несколько разновидностей объекта. Или когда для создания объекта требуется много шагов.

Wikipedia говорит
> Строитель — это шаблон проектирования программного обеспечения для создания объектов, предназначенный для поиска решения антишаблона телескопического конструктора.

Сказав это, позвольте мне добавить немного о том, что такое антишаблон телескопического конструктора. В тот или иной момент мы все видели конструктор, подобный приведенному ниже:
```php
public function __construct($size, $cheese = true, $pepperoni = true, $tomato = false, $lettuce = true)
{
}
```

Как можно видеть; количество параметров конструктора может быстро выйти из-под контроля и может стать трудно понять расположение параметров. Кроме того, этот список параметров может продолжать расти, если вы захотите добавить больше опций в будущем. Это называется антишаблоном телескопического конструктора.

**Пример кода**

Разумной альтернативой является использование шаблона Строитель. Прежде всего, у нас есть наш бургер(Burger), который мы хотим сделать
```php
class Burger
{
    protected $size;

    protected $cheese = false;
    protected $pepperoni = false;
    protected $lettuce = false;
    protected $tomato = false;

    public function __construct(BurgerBuilder $builder)
    {
        $this->size = $builder->size;
        $this->cheese = $builder->cheese;
        $this->pepperoni = $builder->pepperoni;
        $this->lettuce = $builder->lettuce;
        $this->tomato = $builder->tomato;
    }
}
```

А еще у нас есть строитель
```php
class BurgerBuilder
{
    public $size;

    public $cheese = false;
    public $pepperoni = false;
    public $lettuce = false;
    public $tomato = false;

    public function __construct(int $size)
    {
        $this->size = $size;
    }

    public function addPepperoni()
    {
        $this->pepperoni = true;
        return $this;
    }

    public function addLettuce()
    {
        $this->lettuce = true;
        return $this;
    }

    public function addCheese()
    {
        $this->cheese = true;
        return $this;
    }

    public function addTomato()
    {
        $this->tomato = true;
        return $this;
    }

    public function build(): Burger
    {
        return new Burger($this);
    }
}
```

И тогда его можно использовать как
```php
$burger = (new BurgerBuilder(14))
                    ->addPepperoni()
                    ->addLettuce()
                    ->addTomato()
                    ->build();
```

**Когда использовать?**

Когда может быть несколько разновидностей объекта и нужно избежать телескопирования конструктора. Ключевое отличие от шаблона Фабрика заключается в том, что шаблон Фабрика должен использоваться, когда создание является одноэтапным процессом, а шаблон Строитель - когда создание является многоэтапным процессом.

🐑 Прототип
--------------

Пример из жизни
> Помните Долли? Овца, которая была клонирована! Давайте не будем вдаваться в подробности, но ключевым моментом здесь является то, что все дело в клонировании.

Простыми словами
> Создание объекта на основе существующего объекта с помощью клонирования.

Wikipedia говорит
> Прототип — это порождающий шаблон проектирования в разработке программного обеспечения. Он используется, когда тип создаваемых объектов определяется прототипом экземпляра, который клонируется для создания новых объектов.

Короче говоря, он позволяет вам создать копию существующего объекта и изменить его под свои нужды, вместо того, чтобы создавать объект с нуля и настраивать его.

**Пример кода**

В PHP это можно легко сделать с помощью `clone`
```php
class Sheep
{
    protected $name;
    protected $category;

    public function __construct(string $name, string $category = 'Горная овца')
    {
        $this->name = $name;
        $this->category = $category;
    }

    public function setName(string $name)
    {
        $this->name = $name;
    }

    public function getName()
    {
        return $this->name;
    }

    public function setCategory(string $category)
    {
        $this->category = $category;
    }

    public function getCategory()
    {
        return $this->category;
    }
}
```

Затем её(Sheep) можно клонировать, как показано ниже
```php
$original = new Sheep('Jolly');
echo $original->getName(); // Jolly
echo $original->getCategory(); // Горная овца

// Клонируйте и изменяйте то, что требуется
$cloned = clone $original;
$cloned->setName('Dolly');
echo $cloned->getName(); // Dolly
echo $cloned->getCategory(); // Горная овца
```

Также вы можете использовать магический метод `__clone` для изменения поведения клонирования.

**Когда использовать?**

Когда требуется объект, похожий на существующий объект или когда создание будет дорогостоящим по сравнению с клонированием.

💍 Одиночка
--------------

Пример из жизни
> Одновременно может быть только один президент страны. Один и тот же президент должен быть привлечен к действию, когда этого требует долг. Президент здесь одиночка.

Простыми словами
> Одиночка гарантирует, что будет создан только один объект определенного класса.

Wikipedia говорит
> В разработке программного обеспечения шаблон Одиночка — это шаблон проектирования программного обеспечения, который ограничивает создание экземпляра класса одним объектом. Это полезно, когда для координации действий в системе требуется ровно один объект.

Шаблон Одиночка на самом деле считается антипаттерном, и его чрезмерного использования следует избегать. Это не означает, что он плохой и он может иметь некоторые допустимые варианты использования, но его следует использовать с осторожностью, потому что он вводит глобальное состояние в ваше приложение, и изменение его в одном месте может повлиять на другие области, а это довольно трудно отлаживать. Еще одна плохая вещь заключается в том, что он делает ваш код тесно связанным, плюс имитация(mocking) Одиночки может быть затруднена.

**Пример кода**

Чтобы создать одиночку сделайте конструктор приватным, отключите клонирование, отключите расширение и создайте статическую переменную для размещения экземпляра
```php
final class President
{
    private static $instance;

    private function __construct()
    {
        // Скрытие конструктора
    }

    public static function getInstance(): President
    {
        if (!self::$instance) {
            self::$instance = new self();
        }

        return self::$instance;
    }

    private function __clone()
    {
        // Отключение клонирования
    }

    private function __wakeup()
    {
        // Отключение unserialize
    }
}
```

И тогда его можно использовать как
```php
$president1 = President::getInstance();
$president2 = President::getInstance();

var_dump($president1 === $president2); // true
```

Структурные шаблоны проектирования
==========================

Простыми словами
> Структурные шаблоны в основном связаны с композицией объектов или, другими словами, с тем, как сущности могут использовать друг друга. Или еще одно объяснение: они помогают ответить на вопрос "Как создать программный компонент?"

Wikipedia говорит
> В разработке программного обеспечения структурные шаблоны проектирования — это шаблоны проектирования, которые упрощают проектирование, определяя простой способ реализации отношений между сущностями.

 * [Адаптер](#-адаптер)
 * [Мост](#-мост)
 * [Компоновщик](#-компоновщик)
 * [Декоратор](#-декоратор)
 * [Фасад](#-фасад)
 * [Приспособленец](#-приспособленец)
 * [Заместитель](#-заместитель)

🔌 Адаптер
--------------

Пример из жизни
> Предположим, что у вас есть несколько фотографий на карте памяти и вам нужно перенести их на компьютер. Для того, чтобы перенести их, вам нужен какой-то адаптер, совместимый с портами вашего компьютера, чтобы вы могли подключить карту памяти к компьютеру. В этом случае картридер является адаптером.
> Другим примером может быть знаменитый адаптер питания; вилка на трех ножках не может быть подключена к розетке с двумя контактами, она должна использовать адаптер питания, который делает ее совместимой с розеткой с двумя контактами.
> Еще одним примером может быть переводчик, переводящий слова, сказанные одним человеком другому.

Простыми словами
> Шаблон Адаптер позволяет обернуть несовместимый объект в адаптер, чтобы сделать его совместимым с другим классом.

Wikipedia говорит
> В разработке программного обеспечения шаблон Адаптер — это шаблон проектирования программного обеспечения, который позволяет использовать интерфейс существующего класса в качестве другого интерфейса. Он часто используется для того, чтобы существующие классы работали с другими без изменения их исходного кода.

**Пример кода**

Представьте игру, где есть охотник и он охотится на львов.

Во-первых, у нас есть интерфейс `Лев`(Lion), который должны реализовать все типы львов
```php
interface Lion
{
    public function roar();
}

class AfricanLion implements Lion
{
    public function roar()
    {
    }
}

class AsianLion implements Lion
{
    public function roar()
    {
    }
}
```

И охотник ожидает, что любая реализация интерфейса `Лев`(Lion) будет охотиться.
```php
class Hunter
{
    public function hunt(Lion $lion)
    {
        $lion->roar();
    }
}
```

Теперь предположим, что нам нужно добавить `Дикую собаку`(WildDog) в нашу игру, чтобы охотник мог охотиться и на нее. Но мы не можем сделать это напрямую, потому что у собаки другой интерфейс. Чтобы сделать его совместимым для нашего охотника, нам придется создать совместимый адаптер
```php
// Это нужно добавить в игру
class WildDog
{
    public function bark()
    {
    }
}

// Адаптер вокруг дикой собаки, чтобы сделать этот класс совместимым с нашей игрой
class WildDogAdapter implements Lion
{
    protected $dog;

    public function __construct(WildDog $dog)
    {
        $this->dog = $dog;
    }

    public function roar()
    {
        $this->dog->bark();
    }
}
```

И теперь `WildDog` можно использовать в нашей игре с помощью `WildDogAdapter`
```php
$wildDog = new WildDog();
$wildDogAdapter = new WildDogAdapter($wildDog);

$hunter = new Hunter();
$hunter->hunt($wildDogAdapter);
```

🚡 Мост
--------------

Пример из жизни
> Предположим, у вас есть веб-сайт с разными страницами, и вы должны позволить пользователю изменить тему. Что бы вы сделали? Создать несколько копий каждой из страниц для каждой из тем или просто создать отдельные темы и загрузить их в соответствии с предпочтениями пользователя? Шаблон Мост позволяет реализовать второй способ.

![С использование и без шаблона Мост](https://cloud.githubusercontent.com/assets/11269635/23065293/33b7aea0-f515-11e6-983f-98823c9845ee.png)

Простыми словами
> Шаблон Мост заключается в предпочтении композиции наследованию. Сведения о реализации передаются из иерархии в другой объект с отдельной иерархией.

Wikipedia говорит
> Шаблон Мост — это шаблон проектирования, используемый в разработке программного обеспечения, который предназначен для "отделения абстракции от ее реализации, чтобы они могли варьироваться независимо друг от друга".

**Пример кода**

Переведем наш пример с веб страницей выше. Здесь у нас есть иерархия `веб страниц`(WebPage)
```php
interface WebPage
{
    public function __construct(Theme $theme);
    public function getContent();
}

class About implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "About page in " . $this->theme->getColor();
    }
}

class Careers implements WebPage
{
    protected $theme;

    public function __construct(Theme $theme)
    {
        $this->theme = $theme;
    }

    public function getContent()
    {
        return "Careers page in " . $this->theme->getColor();
    }
}
```

И отдельная иерархия тем
```php
interface Theme
{
    public function getColor();
}

class DarkTheme implements Theme
{
    public function getColor()
    {
        return 'Dark Black';
    }
}
class LightTheme implements Theme
{
    public function getColor()
    {
        return 'Off white';
    }
}
class AquaTheme implements Theme
{
    public function getColor()
    {
        return 'Light blue';
    }
}
```

И обе иерархии
```php
$darkTheme = new DarkTheme();

$about = new About($darkTheme);
$careers = new Careers($darkTheme);

echo $about->getContent(); // "About page in Dark Black";
echo $careers->getContent(); // "Careers page in Dark Black";
```

🌿 Компоновщик
-----------------

Пример из жизни
> Каждая организация состоит из сотрудников. Каждый из сотрудников имеет одинаковые характеристики, т.е. имеет зарплату, имеет какие-то обязанности, может отчитываться или не отчитываться перед кем-то, может иметь или не иметь подчиненных и т.д.

Простыми словами
> Шаблон Компоновщик позволяет клиентам обрабатывать отдельные объекты единообразно.

Wikipedia говорит
> В разработке программного обеспечения шаблон Компоновщик — это шаблон проектирования с разбивкой. Составной шаблон описывает, что группа объектов должна обрабатываться так же, как и отдельный экземпляр объекта. Цель составного метода состоит в том, чтобы "компоновать" объекты в древовидные структуры, представляющие иерархии "часть-целое". Реализация составного шаблона позволяет клиентам обрабатывать отдельные объекты и композиции единообразно.

**Пример кода**

Берем в пример сотрудников(Employee) о которых шла речь выше. Здесь у нас есть разные типы сотрудников
```php
interface Employee
{
    public function __construct(string $name, float $salary);
    public function getName(): string;
    public function setSalary(float $salary);
    public function getSalary(): float;
    public function getRoles(): array;
}

class Developer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;
    
    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}

class Designer implements Employee
{
    protected $salary;
    protected $name;
    protected $roles;

    public function __construct(string $name, float $salary)
    {
        $this->name = $name;
        $this->salary = $salary;
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function setSalary(float $salary)
    {
        $this->salary = $salary;
    }

    public function getSalary(): float
    {
        return $this->salary;
    }

    public function getRoles(): array
    {
        return $this->roles;
    }
}
```

Затем у нас есть организация, которая состоит из нескольких различных типов сотрудников
```php
class Organization
{
    protected $employees;

    public function addEmployee(Employee $employee)
    {
        $this->employees[] = $employee;
    }

    public function getNetSalaries(): float
    {
        $netSalary = 0;

        foreach ($this->employees as $employee) {
            $netSalary += $employee->getSalary();
        }

        return $netSalary;
    }
}
```

И тогда её можно использовать как
```php
// Подготовка сотрудников
$john = new Developer('John Doe', 12000);
$jane = new Designer('Jane Doe', 15000);

// Добавление их в организацию
$organization = new Organization();
$organization->addEmployee($john);
$organization->addEmployee($jane);

echo "Общая заработная плата: " . $organization->getNetSalaries(); // Общая заработная плата: 27000
```

☕ Декоратор
-------------

Пример из жизни
> Представьте, что вы управляете автосервисом, предлагающим несколько услуг. Итак, как рассчитать счет, который будет списан? Вы выбираете одну услугу и динамично добавляете к ней цены на предоставляемые услуги до тех пор, пока не получите окончательную стоимость. Здесь каждый вид услуг – это декоратор.

Простыми словами
> Шаблон Декоратор позволяет динамически изменять поведение объекта во время выполнения, оборачивая его в объект класса декоратора.

Wikipedia говорит
> В объектно-ориентированном программировании шаблон Декоратор — это шаблон проектирования, который позволяет добавлять поведение к отдельному объекту статически или динамически, не влияя на поведение других объектов из того же класса. Шаблон декоратора часто полезен для соблюдения принципа единой ответственности, так как он позволяет разделить функциональность между классами с уникальными проблемными областями.

**Пример кода**

Возьмем, к примеру, кофе. Прежде всего, у нас есть простой кофе(SimpleCoffee), реализующий интерфейс кофе(Coffee)
```php
interface Coffee
{
    public function getCost();
    public function getDescription();
}

class SimpleCoffee implements Coffee
{
    public function getCost()
    {
        return 10;
    }

    public function getDescription()
    {
        return 'Simple coffee';
    }
}
```

Мы хотим сделать код расширяемым, чтобы при необходимости можно было его модифицировать. Давайте сделаем несколько дополнений (декораторов)
```php
class MilkCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 2;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', milk';
    }
}

class WhipCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 5;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', whip';
    }
}

class VanillaCoffee implements Coffee
{
    protected $coffee;

    public function __construct(Coffee $coffee)
    {
        $this->coffee = $coffee;
    }

    public function getCost()
    {
        return $this->coffee->getCost() + 3;
    }

    public function getDescription()
    {
        return $this->coffee->getDescription() . ', vanilla';
    }
}
```

Давайте приготовим кофе
```php
$someCoffee = new SimpleCoffee();
echo $someCoffee->getCost(); // 10
echo $someCoffee->getDescription(); // Simple Coffee

$someCoffee = new MilkCoffee($someCoffee);
echo $someCoffee->getCost(); // 12
echo $someCoffee->getDescription(); // Simple Coffee, milk

$someCoffee = new WhipCoffee($someCoffee);
echo $someCoffee->getCost(); // 17
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip

$someCoffee = new VanillaCoffee($someCoffee);
echo $someCoffee->getCost(); // 20
echo $someCoffee->getDescription(); // Simple Coffee, milk, whip, vanilla
```

📦 Фасад
-------------

Пример из жизни
> Как включить компьютер? "Нажать кнопку питания" — скажете вы! Это то, во что вы верите, потому что вы используете простой интерфейс, который компьютер предоставляет снаружи, внутри он должен сделать много вещей, чтобы это произошло. Этот простой интерфейс к сложной подсистеме является фасадом.

Простыми словами
> Шаблон Фасад обеспечивает упрощенный интерфейс к сложной подсистеме.

Wikipedia говорит
> Фасад — это объект, который предоставляет упрощенный интерфейс для более крупного массива кода, такого как библиотека классов.

**Пример кода**

Возьмем наш пример с компьютером выше. Здесь у нас есть класс Компьютер(Computer)
```php
class Computer
{
    public function getElectricShock()
    {
        echo "Ouch!";
    }

    public function makeSound()
    {
        echo "Beep beep!";
    }

    public function showLoadingScreen()
    {
        echo "Loading..";
    }

    public function bam()
    {
        echo "Ready to be used!";
    }

    public function closeEverything()
    {
        echo "Bup bup bup buzzzz!";
    }

    public function sooth()
    {
        echo "Zzzzz";
    }

    public function pullCurrent()
    {
        echo "Haaah!";
    }
}
```

А вот и фасад
```php
class ComputerFacade
{
    protected $computer;

    public function __construct(Computer $computer)
    {
        $this->computer = $computer;
    }

    public function turnOn()
    {
        $this->computer->getElectricShock();
        $this->computer->makeSound();
        $this->computer->showLoadingScreen();
        $this->computer->bam();
    }

    public function turnOff()
    {
        $this->computer->closeEverything();
        $this->computer->pullCurrent();
        $this->computer->sooth();
    }
}
```

Теперь фасад можно использовать как
```php
$computer = new ComputerFacade(new Computer());
$computer->turnOn(); // Ouch! Beep beep! Loading.. Ready to be used!
$computer->turnOff(); // Bup bup buzzz! Haah! Zzzzz
```

🍃 Приспособленец
---------

Пример из жизни
> Вы когда-нибудь пили свежий чай из какого-нибудь ларька? С одного чайника они разливают на несколько кружек, хотя вам нужна всего одна, остальное оставляют для любого другого клиента, чтобы сэкономить ресурсы, например, газ и т.д. Приспособленец - это всё, что связано с делением.

Простыми словами
> Шаблон используется для минимизации использования памяти или вычислительных затрат за счет максимально возможного совместного использования с аналогичными объектами.

Wikipedia говорит
> Приспособленец — это объект, который минимизирует использование памяти, обмениваясь как можно большим количеством данных с другими подобными объектами. Это способ использования объектов в большом количестве, когда простое повторяющееся представление потребовало бы неприемлемого объема памяти.

**Пример кода**

Переводим наш пример с чаем выше. В первую очередь у нас есть виды чая(KarakTea) и Чайник(TeaMaker)
```php
// Все, что будет кэшировано, является приспособленецем
// Поэтому виды чая здесь также будут приспособленецами
class KarakTea
{
}

// Действует как фабрика и сохраняет чай
class TeaMaker
{
    protected $availableTea = [];

    public function make($preference)
    {
        if (empty($this->availableTea[$preference])) {
            $this->availableTea[$preference] = new KarakTea();
        }

        return $this->availableTea[$preference];
    }
}
```

Затем у нас есть `Чайная лавка(TeaShop)`, которая принимает заказы и обслуживает их
```php
class TeaShop
{
    protected $orders;
    protected $teaMaker;

    public function __construct(TeaMaker $teaMaker)
    {
        $this->teaMaker = $teaMaker;
    }

    public function takeOrder(string $teaType, int $table)
    {
        $this->orders[$table] = $this->teaMaker->make($teaType);
    }

    public function serve()
    {
        foreach ($this->orders as $table => $tea) {
            echo "Serving tea to table# " . $table;
        }
    }
}
```

И это можно использовать как
```php
$teaMaker = new TeaMaker();
$shop = new TeaShop($teaMaker);

$shop->takeOrder('less sugar', 1);
$shop->takeOrder('more milk', 2);
$shop->takeOrder('without sugar', 5);

$shop->serve();
// Serving tea to table# 1
// Serving tea to table# 2
// Serving tea to table# 5
```

🎱 Заместитель
---------

Пример из жизни
> Вы когда-нибудь использовали карту доступа для прохода через дверь? Есть несколько вариантов открыть эту дверь, т.е. ее можно открыть либо с помощью карты доступа, либо нажатием кнопки, которая обходит охрану. Основная функциональность двери заключается в том, чтобы открываться, но поверх нее добавляется прокси для добавления некоторой функциональности. Позвольте мне лучше объяснить это на примере кода ниже.

Простыми словами
> Используя шаблон Заместитель, класс представляет функциональность другого класса.

Wikipedia говорит
> A proxy, in its most general form, is a class functioning as an interface to something else. A proxy is a wrapper or agent object that is being called by the client to access the real serving object behind the scenes. Use of the proxy can simply be forwarding to the real object, or can provide additional logic. In the proxy extra functionality can be provided, for example caching when operations on the real object are resource intensive, or checking preconditions before operations on the real object are invoked.
Заместитель в своей наиболее общей форме — это класс, функционирующий как интерфейс к чему-то еще. Заместитель — это обёртка или объект агента, который вызывается клиентом для доступа к реальному обслуживающему объекту за кулисами. Использование заместителя может быть просто переадресацией к реальному объекту или может предоставить дополнительную логику. В прокси может быть предусмотрена дополнительная функциональность, например, кэширование, когда операции над реальным объектом требуют больших ресурсов, или проверка предварительных условий перед вызовом операций над реальным объектом.

**Пример кода**

Возьмем наш пример с защитной дверью выше. Во-первых, у нас есть интерфейс дверь(Door) и реализация двери
```php
interface Door
{
    public function open();
    public function close();
}

class LabDoor implements Door
{
    public function open()
    {
        echo "Opening lab door";
    }

    public function close()
    {
        echo "Closing the lab door";
    }
}
```

Затем у нас есть Заместитель для защиты любых дверей, которые мы хотим
```php
class SecuredDoor implements Door
{
    protected $door;

    public function __construct(Door $door)
    {
        $this->door = $door;
    }

    public function open($password)
    {
        if ($this->authenticate($password)) {
            $this->door->open();
        } else {
            echo "Big no! It ain't possible.";
        }
    }

    public function authenticate($password)
    {
        return $password === '$ecr@t';
    }

    public function close()
    {
        $this->door->close();
    }
}
```

И это можно использовать как
```php
$door = new SecuredDoor(new LabDoor());
$door->open('invalid'); // Big no! It ain't possible.

$door->open('$ecr@t'); // Opening lab door
$door->close(); // Closing lab door
```

Еще одним примером может быть реализация некоего модуля отображения данных. Например, недавно я создал ODM (Object Data Mapper) для MongoDB, используя этот шаблон, где я написал заместителя для классов mongo, используя волшебный метод `__call()`. Все вызовы методов были проксированы в исходный класс mongo, и полученный результат был возвращен как есть, но в случае `find` или `findOne` данные были отображены на объекты требуемого класса, и объект был возвращен вместо `Cursor`.

Поведенческие шаблоны проектирования
==========================

Простыми словами
> Поведенческие шаблоны связаны с распределением обязанностей между объектами. Что отличает их от структурных шаблонов, так как они не только определяют структуру, но и намечают шаблоны для передачи сообщений/коммуникации между ними. Или, другими словами, они помогают ответить на вопрос "Как запустить поведение в программном компоненте?".

Wikipedia говорит
> В разработке программного обеспечения поведенческие шаблоны проектирования — это шаблоны проектирования, которые определяют общие шаблоны взаимодействия между объектами и реализуют эти шаблоны. Таким образом, эти шаблоны повышают гибкость в осуществлении этой коммуникации.

* [Цепочка обязанностей](#-цепочка-обязанностей)

🔗 Цепочка обязанностей
---------

Пример из жизни
> Например, в вашем аккаунте настроены три способа оплаты (`A`, `B` и `C`). Каждый из них имеет разное количество средств. `А` имеет 100 долларов, `В` имеет 300 долларов и `С` имеет 1000 долларов, и предпочтение для платежей выбирается как `А`, затем `В`, затем `С`. Вы пытаетесь купить что-то стоимостью 210 долларов. Используя цепочку обязанностей, в первую очередь будет проверен аккаунт `A`, может ли он совершить покупку, если да, то покупка будет совершена, и цепочка будет разорвана. Если нет, запрос будет перенаправлен на счет `B`, проверяя сумму, если да, цепочка будет разорвана, в противном случае запрос будет продолжать пересылать до тех пор, пока не найдет подходящего обработчика. Здесь `А`, `В` и `С` являются звеньями цепи, а весь феномен – Цепочка обязанностей.

Простыми словами
> Шаблон помогает выстроить цепочку объектов. Запрос поступает с одного конца и продолжает переходить от объекта к объекту, пока не найдет подходящего обработчика.

Wikipedia говорит
> В объектно-ориентированном проектировании шаблон цепочки обязанностей — это шаблон проектирования, состоящий из источника объектов команд и ряда объектов обработки. Каждый объект обработки содержит логику, определяющую типы объектов команд, с которыми он может работать; остальные передаются следующему объекту обработки в цепочке.

**Пример кода**

Переведем наш пример с аккаунтом выше. Прежде всего, у нас есть базовый счет, у которого есть логика для объединения счетов в цепочку и несколько счетов

```php
abstract class Account
{
    protected $successor;
    protected $balance;

    public function setNext(Account $account)
    {
        $this->successor = $account;
    }

    public function pay(float $amountToPay)
    {
        if ($this->canPay($amountToPay)) {
            echo sprintf('Оплачено %s через %s' . PHP_EOL, $amountToPay, get_called_class());
        } elseif ($this->successor) {
            echo sprintf('Невозможно оплатить через %s. Выбор другого способа оплаты ..' . PHP_EOL, get_called_class());
            $this->successor->pay($amountToPay);
        } else {
            throw new Exception('Ни на одном из счетов недостаточно средств');
        }
    }

    public function canPay($amount): bool
    {
        return $this->balance >= $amount;
    }
}

class Bank extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Paypal extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}

class Bitcoin extends Account
{
    protected $balance;

    public function __construct(float $balance)
    {
        $this->balance = $balance;
    }
}
```

Теперь давайте подготовим цепочку с использованием звеньев, определенных выше (т.е. Bank, PayPal, Bitcoin)

```php
// Подготовим цепочку, как показано ниже
//      $bank->$paypal->$bitcoin
//
// Первый приоритет у банка 
//      Если банк не может оплатить используем paypal
//      Если paypal не может оплатить используем bitcoin

$bank = new Bank(100);          // Bank с балансом 100
$paypal = new Paypal(200);      // Paypal с балансом 200
$bitcoin = new Bitcoin(300);    // Bitcoin с балансом 300

$bank->setNext($paypal);
$paypal->setNext($bitcoin);

// Попробуем произвести оплату используя банк
$bank->pay(259);

// Вывод будет следующим
// ==============
// Невозможно оплатить через bank. Выбор другого способа оплаты ..
// Невозможно оплатить через paypal. Выбор другого способа оплаты ..
// Оплачено 259 через Bitcoin!
```
