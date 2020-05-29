<div align="center">
  <img width="494" height="244" src="https://github.com/goavengers/go-principles/blob/master/img/solid_2x.png">
  <h1>Философия архитектуры ООП, SOLID-принципы, Dry, KISS и YAGNI</h1>
  <h5>Вместе мы разберемся!</h5>
</div>

## Содержание

1. OOP (Object Oriented Programming)
2. SOLID
3. DRY - Don’t repeat yourself
4. KISS (Keep it simple, stupid!)
5. Avoid Creating a YAGNI (You aren’t going to need it)
6. LOD (Law of Demeter)

## SOLID

За этой аббревиатурой скрываются 5 базовых принципов ООП, предложенные __Робертом Мартином__ в его статье [«Принципы проектирования и шаблоны проектирования»](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf). Следование их духу сделает код легко тестируемым, расширяемым, читаемым и поддерживаемым.

Вот шпаргалка по этим принципам:

- (__S__) Single Responsibility Principle (принцип единственности ответственности)
- (__O__) Open/Closed Principle (принцип открытости/закрытости)
- (__L__) Liskov Substitution Principle (принцип подстановки Барбары Лисков)
- (__I__) Interface Segregation Principle (принцип разделения интерфейса) 
- (__D__) Dependency Inversion Principle (принцип инверсии зависимостей) 

По словам __Роберта С. Мартина__, симптомы гниющего дизайна или плохого кода:

- __Жесткость__ (трудно менять код, так как простое изменение затрагивает много мест);
- __Неподвижность__ (сложно разделить код на модули, которые можно использовать в других программах);
- __Вязкость__ (разрабатывать и/или тестировать код довольно тяжело);
- __Ненужная Сложность__ (в коде есть неиспользуемый функционал);
- __Ненужная Повторяемость__ (Copy/Paste);
- __Плохая Читабельность__ (трудно понять что код делает, трудно его поддерживать);
- __Хрупкость__ (легко поломать функционал даже небольшими изменениями);

Но это улучшение, теперь мы можем сказать что то вроде "мне не нравится это потому, что слишком сложно модифицировать", или "мне не нравится это потому, что я не могу сказать, что этот код пытается сделать", но что насчет того, чтобы вести обсуждение позитивно?

Разве это не было бы здорово, если бы существовал способ описать свойства хорошего дизайна, а не только плохого и иметь возможность рассуждать объективными терминами? Для этого нам на помощь спешат принципы проектирования архитектуры и написания программного кода.

Сейчас мы рассмотрим эти принципы на схематичных примерах. Обратите внимание на то, что главная цель примеров заключается в том, чтобы помочь читателю понять принципы __SOLID__, узнать, как их применять и как следовать им, проектируя приложения. Автор материала не стремился к тому, чтобы выйти на работающий код, который можно было бы использовать в реальных проектах.

В golang в качестве отдельных частей у нас есть - пакаджи, структуры, методы и интерфейсы. Давайте расссмотрим SOLID в этих терминах.

### Single Responsibility Principle (принцип единственности ответственности)

> «Одно поручение. Всего одно.» — Локи говорит Скурджу в фильме «Тор: Рагнарёк».

На самом верхнем уровне мы декомпозируем систему на пакаджи. В соответствии с этим принципом каждый пакадж должен заниматься какой-то отдельной вещью.

Дальше пакадж мы делим на структуры с набором методов. Каждая структура и связанные с ней методы несут отвественность за какую-то более специфическую задачу внутри пакаджа.

Каждый метод структуры выполняет какую-то одну единственную задачу.

Представим, что у нас есть стукртура реализующая интерфейс `IAnimal`

```go
type IAnimal interface {
	GetAnimal() string
	SaveAnimal()
}
```

```go
type Animal struct {
	name string
}

func (animal *Animal) GetAnimal() string {
	return animal.name
}

func (animal *Animal) SaveAnimal() {
	// impl
}
```

Стуктура `Animal`, представленная здесь, описывает какое-то животное. 
Эта стуктура нарушает принцип единственной ответственности. 
Как именно нарушается этот принцип?

В соответствии с принципом единственной ответственности структура должена решать лишь какую-то одну задачу. 
Она же решает две, занимаясь работой с хранилищем данных в методе `SaveAnimal` и манипулируя свойствами объекта в методе `GetAnimal`.

Как такая структура класса может привести к проблемам?

Если изменится порядок работы с хранилищем данных, используемым приложением, то придётся вносить изменения во все структуры, работающие с хранилищем. 
Такая архитектура не отличается гибкостью, изменения одних подсистем затрагивают другие, что напоминает эффект домино.

Для того чтобы привести вышеприведённый код в соответствие с принципом единственной ответственности, создадим ещё одну стуктуру, единственной задачей которой является работа с хранилищем, в частности — сохранение в нём объектов структуры.

```go
type IAnimal interface {
	GetAnimal() string
}

type IAnimalStorage interface {
	Save(animal Animal)
	Get(animal Animal)
}

type AnimalStorage struct {}
func (storage *AnimalStorage) Save(animal Animal) {
	// impl
}

func (storage *AnimalStorage) Get(animal Animal) {
	// impl
}

type Animal struct {
	name string
}

func (animal *Animal) GetName() string {
	return animal.name
}
```

Вот что по этому поводу говорит __Стив Фентон__: 

> Проектируя классы, мы должны стремиться к тому, чтобы объединять родственные компоненты, то есть такие, изменения в которых происходят по одним и тем же причинам. 
> Нам следует стараться разделять компоненты, изменения в которых вызывают различные причины.

Правильное применение принципа единственной ответственности приводит к высокой степени связности элементов внутри модуля, то есть к тому, что задачи, решаемые внутри него, хорошо соответствуют его главной цели.

Код: [Принцип единственности ответственности](./code/solid/single-responsibility/single-responsibility.go)

### Open/Closed Principle (принцип открытости/закрытости)

> Система должна быть открыта для расширения и закрыта для модификации.

Не будем уходить далеко и рассмотрим пример с животными, структура `Animal`:

```go
type Animal struct {
	name string
}
```

Мы хотим перебрать список животных, каждое из которых представлено объектом класса Animal, и узнать о том, какие звуки они издают. 
Представим, что мы решаем эту задачу с помощью функции `AnimalSounds`:

```go
func AnimalSounds() {
	animals := []Animal{
		Animal{name: "lion"},
		Animal{name: "mouse"},
	}

	for _, animal := range animals {
		if animal.name == "lion" {
			fmt.Println("roar")
		} else if animal.name == "mouse" {
			fmt.Println("squeak")
		}
	}
}
```

Самая главная проблема такой архитектуры заключается в том, что функция определяет то, какой звук издаёт то или иное животное, анализируя конкретные объекты. 
Функция `AnimalSounds` не соответствует принципу открытости-закрытости, так как, например, при появлении новых видов животных, нам, для того, чтобы с её помощью можно было бы узнавать звуки, издаваемые ими, придётся её изменить.


Добавим в массив новый элемент:

```go
func AnimalSounds() {
	animals := []Animal{
		Animal{name: "lion"},
		Animal{name: "mouse"},

        // Новый
		Animal{name: "snake"},
	}
    
    ...
}
```

После этого нам придётся поменять код функции `AnimalSounds`:

```go
func AnimalSounds() {
	...

	for _, animal := range animals {
		if animal.name == "lion" {
			fmt.Println("roar")
		} else if animal.name == "mouse" {
			fmt.Println("squeak")
		} else if animal.name == "snake" {
			fmt.Println("hiss")
		}
	}
}
```

Как видите, при добавлении в массив нового животного придётся дополнять код функции. 
Пример это очень простой, но если подобная архитектура используется в реальном проекте, функцию придётся постоянно расширять, добавляя в неё новые выражения `if`.

Как привести функцию `AnimalSounds` в соответствие с принципом открытости-закрытости? Например — так:

```go
type Animal interface {
	MakeSound() string
}

type Lion struct {}
func (lion *Lion) MakeSound() string {
	return "roar"
}

type Squirrel struct {}
func (squirrel *Squirrel) MakeSound() string {
	return "squeak"
}

type Snake struct {}
func (snake *Snake) MakeSound() string {
	return "hiss"
}

func AnimalSounds() {
	animals := []Animal{
		&Lion{},
		&Squirrel{},
		&Snake{},
	}

	for _, animal := range animals {
		fmt.Println(animal.MakeSound())
	}
}
```

Можно заметить, что у кадой структуры реализующий интерфейс `Animal` теперь есть метод `MakeSound`. 
При таком подходе нужно, чтобы структуры, предназначенные для описания конкретных животных, реализовывали бы интерфейс `Animal`.

В результате у каждой стуктуры, описывающего животного, будет собственный метод `MakeSound`, а при переборе массива с животными в функции `AnimalSounds` достаточно будет вызвать этот метод для каждого элемента массива.

Если теперь добавить в массив объект, описывающий новое животное, функцию `AnimalSounds` менять не придётся. 
Мы привели её в соответствие с принципом открытости-закрытости.

Код: [Принцип Открытости/Закрытости](./code/solid/open-closed/open-closed.go)

### Liskov Substitution Principle (принцип подстановки Барбары Лисков)


### Interface Segregation Principle (принцип разделения интерфейса) 


### Dependency Inversion Principle (принцип инверсии зависимостей) 


