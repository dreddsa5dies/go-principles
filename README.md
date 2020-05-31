<div align="center">
  <img width="494" height="244" src="https://github.com/goavengers/go-principles/blob/master/img/solid_2x.png">
  <h1>Философия архитектуры ООП, SOLID-принципы, Dry, KISS и YAGNI</h1>
  <h5>Вместе мы разберемся!</h5>
</div>

## Содержание

- [ ] OOP (Object Oriented Programming)
- [x] [SOLID](#solid)
- [ ] DRY - Don’t repeat yourself
- [x] [KISS (Keep it simple, stupid!)](#kiss)
- [x] [Avoid Creating a YAGNI (You aren’t going to need it)](#yagni)
- [ ] LOD (Law of Demeter)

## <a name="solid"></a> SOLID

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

- [x] [Single Responsibility Principle (принцип единственности ответственности)](./docs/solid/Single%20Responsibility%20Principle.md)
- [x] [Open/Closed Principle (принцип открытости/закрытости)](./docs/solid/Open%20Closed%20Principle.md)
- [ ] [Liskov Substitution Principle (принцип подстановки Барбары Лисков)](./docs/solid/Liskov%20Substitution%20Principle.md)
- [ ] [Interface Segregation Principle (принцип разделения интерфейса)](./docs/solid/Interface%20Segregation%20Principle.md)
- [ ] [Dependency Inversion Principle (принцип инверсии зависимостей)](./docs/solid/Dependency%20Inversion%20Principle.md)

## <a name="solid"></a> Object Oriented Programming
## <a name="dry"></a> DRY - Don’t repeat yourself
## <a name="kiss"></a> KISS (Keep it simple, stupid! — Делайте вещи проще!)

> Принцип программирования KISS — делайте вещи проще

Большая часть программных систем необосновано перегружена практически ненужными функциями, что ухудшает удобство их использование конечными пользователями, а также усложняет их поддержку и развитие разработчиками. Следование принципу __«KISS»__ позволяет разрабатывать решения, которые просты в использовании и в сопровождении.

__KISS__ — это принцип проектирования и программирования, при котором простота системы декларируется в качестве основной цели или ценности. Есть два варианта расшифровки аббревиатуры: __«keep it simple, stupid»__ и более корректный __«keep it short and simple»__.

**В проектировании следование принципу __KISS__ выражается в том, что:**

- не имеет смысла реализовывать дополнительные функции, которые не будут использоваться вовсе или их использование крайне маловероятно, как правило, большинству пользователей достаточно базового функционала, а усложнение только вредит удобству приложения;

- не стоит перегружать интерфейс теми опциями, которые не будут нужны большинству пользователей, гораздо проще предусмотреть для них отдельный «расширенный» интерфейс (или вовсе отказаться от данного функционала);

- бессмысленно делать реализацию сложной бизнес-логики, которая учитывает абсолютно все возможные варианты поведения системы, пользователя и окружающей среды, — во-первых, это просто невозможно, а во-вторых, такая фанатичность заставляет собирать «звездолёт», что чаще всего иррационально с коммерческой точки зрения.

**В программировании следование принципу __KISS__ можно описать так:**

- не имеет смысла беспредельно увеличивать уровень абстракции, надо уметь вовремя остановиться;
- бессмысленно закладывать в проект избыточные функции «про запас», которые может быть когда-нибудь кому-либо понадобятся (тут скорее правильнее [подход согласно принципу __YAGNI__](#yagni), который рассмотрим чуть ниже);
- не стоит подключать огромную библиотеку, если вам от неё нужна лишь пара функций;
- декомпозиция чего-то сложного на простые составляющие — это архитектурно верный подход (тут __KISS__ перекликается с [DRY](#dry));
- абсолютная математическая точность или предельная детализация нужны не всегда — большинство систем создаются не для запуска космических шаттлов, данные можно и нужно обрабатывать с той точностью, которая достаточна для качественного решения задачи, а детализацию выдавать в нужном пользователю объёме, а не в максимально возможном объёме.

Также __KISS__ имеет много общего c принципом разделения интерфейса из пяти принципов [__SOLID__](#solid), сформулированных __Робертом Мартином__.

## <a name="yagni"></a> Avoid Creating a YAGNI (You aren’t going to need it — Вам это не понадобится)

> Лучший код — это ненаписанный код.

Если упрощенно, то следование данному принципу заключается в том, что возможности, которые не описаны в требованиях к системе, просто не должны реализовываться. Это позволяет вести разработку, руководствуясь экономическими критериями — Заказчик не должен оплачивать ненужные ему функции, а разработчики не должны тратить своё оплачиваемое время на реализацию того, что не требуется.

Основная проблема, которую решает принцип __YAGNI__ — это устранение тяги программистов к излишней абстракции, к экспериментам _«из интереса»_ и к реализации функционала, который сейчас не нужен, но, по мнению разработчика, может либо вскоре понадобиться, либо просто будет полезен, хотя в реальности такого очень часто не происходит.

«Бесплатных» функций в программных продуктах просто не бывает. Если рассматривать материальную сторону, то любые ненужные, но фактически реализованные «фичи» оплачиваются либо Заказчиком (в бюджет закладываются расходы на те функции, которые не нужны), либо Исполнителем из прибыли по проекту. И тот, и другой варианты с точки зрения бизнеса неверны. Если же говорить о нематериальных затратах, то любые «бонусные» возможности усложняют сопровождение, увеличивают вероятность ошибок и усложняют взаимодействие с продуктом, — между объёмом кодовой базы и описанными характеристиками есть прямая зависимость. Больше написанного кода — труднее сопровождать и выше вероятность появления «багов».

Принципы __YAGNI__ и [__KISS__](#kiss) очень похожи, если __KISS__ нацелен на упрощение и полезен в плане работы с теми требованиями, которые имеют место быть, то __YAGNI__ более категоричен и применяется для ограждения проектов по разработке ПО от «размывания» их рамок.

Подход к реализации проектов строго по ТЗ верен с нескольких ракурсов. Заказчик не должен платить за то, что ему не надо, а продукт должен быть максимально сопровождаем и его качество не должно страдать от интеграции ненужных функций.

## <a name="solid"></a> LOD (Law of Demeter)
