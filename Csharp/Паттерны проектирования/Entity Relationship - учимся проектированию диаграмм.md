
Главный плюс этой модели проектирования, что она универсальна. Ее можно применять для построения диаграмм, баз данных, программ, принципов взаимодействия..
На  старте нужно знать, что осн. работа проводится над взаимоотношением сущности и связи. Для более легкого восприятия стоит запомнить, что сущность - существительное, которое находится в прямоугольнике и действие - глагол, которое находится в ромбе.

Пример:
![[Pasted image 20231121212907.png]]

На рисунке все понятно - программист учит питон, но что за цифры (1-единички) на рисунке. **Это показатель типа связи!**. В данном примере используется показатель - Один к одному (1:1)

Диаграмма должна читаться в обе стороны. И в нашем примере если слева направо все понятно, то справа налево уже пропадает логика. Потому на диаграммы можно добавлять указатели в виде стрелок:
![[Pasted image 20231121213233.png]]

**АТРИБУТЫ**

Что мы знаем о нашем программисте? Допустим, имя и фамилию. Отметим это на рисунке. Это будут его атрибуты! В общем представлении атрибут - это столбец (Column) в базе данных.
![[Pasted image 20231121213512.png]]

*Атрибуты бывают и пустыми*

Если в таблице вашей БД необязательно указывать имя и фамилию, то тогда атрибут должен состоять из двух овалов: внешнего и внутреннего (внутри которого название атрибуты). В коде это обозначается символом  [[(?) (Nullable)]]

*Идентифицирующие атрибуты*

Вы можете встретить подчеркивание названия атрибута в диаграме - это обозначает идентифицирующий атрибут, т.е. атрибут, который должен быть заполнен всегда, т.к. он является обязательным (первичный ключ). Например, всем известный - id.

*Составные атрибуты*

Как быть если нам нужно дать программисту знания? Неудобно же перечислять отдельным атрибутом каждое из них?
Для этого используются составные атрибуты (атрибут, который состоит из атрибутов-составляющих). Важно отметить, что атрибуты-составляющие - также могут быть составными (вопрос лишь в сложности реализации).
![[Pasted image 20231121221001.png]]

**Типы связи**

Теперь рассмотрим оставшиеся типы связи.
Продолжим с типа связи - Один ко многому - 1:N
Покажем на примере:
![[Pasted image 20231121221104.png]]
Теперь, как видно из рисунка, программист изучает еще и Perl. На практике же ответвлений может быть сотни и тысячи. 
*Тут же хочется отметить, что у атрибуты бывают и у связей.*
Попробуем объяснить на словах - допустим у нас есть связь "Транзакции". Допустим, что в вашем проекте нужно сохранить всю информацию о проведенных транзакциях, будь то сохранение в файле или бд, суммы и прочее. Вам нужно сохранять время, исключения (ошибки) и что-то еще. В нашем случае, все вышеперечисленное - атрибуты, которые будут принадлежать связи. Такие атрибуты также могут быть составными, идентифицирующими, необязательными.

**Многое ко многому**

Последний тип связи называется *Многое ко многому* - M:N
Здесь можно показать на примере взаимосвязи Фильма и Зрителя, например, на каком либо сервисе по просмотру фильмов.
![[Pasted image 20231121221828.png]]

Но тут есть два спорных момента:

1) Почему связь больше смахивает на сущность?
	Ответ: Для упрощения связи типа "Многое ко многому" используются промежуточные сущности.
	
2) Почему здесь нет ответвлений?
	 Ответ: Зритель может подписаться на множество фильмов
		  У фильмов может быть много зрителей, которые подписаны на них.

А теперь давайте рассмотрим другой способ реализации типа связи "Многое ко многому", который будет чуть сложнее в записи, но понятнее тем, кто не знает о промежуточных сущностях.
![[Pasted image 20231121222225.png]]
Как можно заметить, в данном примере есть тип связи "Один ко многому" и даже несколько. **Это легко объяснить!** Дело в том, что тип свзяи "Многое ко многому" равняется двум "Один ко многому"
								M : N = 1 : N + N : 1
Наверное, Вы заинтересованы в том, почему у нас, между связью и сущностью, два ребра.  
Это уже чуть сложнее объяснить. Читайте внимательно.  
Дело в том, что бывают _опциональные_ и _обязательные связи_. Запомните тождество:  

> Опциональные связи создают частичное участие, в то время как обязательные — полное.

— Что такое частичное и полное участия?  
  
Частичное участие — тоже одно из исключений, похожее чем-то на необязательный атрибут, вот только зависит от сущности. Представьте картину. Есть две сущности:  
Покупатель и Продукты. Тип связи — Один ко многому.  
У них общая связь — Покупает. Но нам нужно понять другое. Без чего покупатель — не покупатель?  
— Без хотя бы одной покупки!  
Данный случай — представитель частичной связи, тк мы даём выбор «Покупать и стать покупателем или отказаться». В таком случае, у нас, будет одно ребро между связью «Покупает» и сущностью «Продукты». Теперь рассмотрим полное участие.  
  
Полное участие представляет из себя тот случай, когда выбора нет. Наш программист останется программистом, даже если ничего не выучит, благодаря тому, что мы фиксируем на диаграмме то, что он должен что-то учить, а исключений быть не может. Фиксируем мы это дело двумя рёбрами. Тип участия зависит от того, как вы проектируете, нужна ли выборка на этапе связи.  
С этим закончили. Продолжаем.

Вспомните пример «Один ко многому», где после связи «Учит» были названия ЯП(Языков программирования), что приводило к большому количеству разветвлений, потому как было не правильно в плане записи. Только подумайте, ведь нам не обязательно делать ответвления к каждому ЯП. Мы можем просто создать сущность «Язык программирования», в которой мы разместим атрибуты, которые будут отвечать за его название, возраст, мощность и многое другое. Думаю, Вы поняли. Советую использовать сокращенную запись «Многое ко многому».

### Слабые сущности
  
Рассмотрим заключительное понятие.  
  
Представьте, что у Вас в существует таблица «Родитель» и «Ребенок», соответственно такие-же сущности в диаграмме. Может ли одно существовать без другого? Я думаю — нет. Как в биологическом, так и в целом логическом.  

> Слабая сущность: яблока без яблони быть не может
  
В этом примере сущность «Ребенок» — слабая сущность.  

> Слабые сущности — это те сущности, которые не могут существовать без другой сущности.

Мы создаём сущность «Ребёнок», в надежде на то, что у Родителя/Родителей нет детей с одинаковыми именами, тк иначе — нашу сущность, которая может являться таблицей в БД, будет сложно назвать _Нормализованной_(таблица, в которой соблюдаются правила Атомарности данных и существует Первичный ключ-идентификатор), ведь мы банально не сможем отличить детей.

Однако, такие случаи имеют место быть, но исправить это можно добавлением дополнительного атрибута. В таком случае, атрибут «Имя» — то, что и создает подобную ситуацию, а называется он _компонентом ключа слабой сущности_. Название подобных атрибутов, в овалах, подчеркиваются пунктиром, а сущность и связь помещаются в дополнительные фигуры, в которых они состоят.  
  
Представлю вам это на примере:  
  
![[Pasted image 20231121222738.png]]

#Relationship #Паттерны_Проектирования #Атрибуты #Диаграмы