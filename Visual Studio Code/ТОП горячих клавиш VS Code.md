
> Все сочетания описаны для VS Code на Windows  
> ↓ / ↑ / ← / → — стрелки вниз, вниз и т.д.  
> ЛКМ / ПКМ / СКМ — левая, правая, средняя кнопки мышки соответственно.

1. **Shift + Tab** — сместить табуляцию на один шаг влево. Если вы пишете на Python, то табуляция или четыре пробела — ваш неизменный спутник. Но мало кто знает, что достаточно поставить курсор в любое место строки, нажать Shift + Tab и вуаля, вся строка смещается влево на «один таб».
    
    ![Shift + Tab](https://habrastorage.org/getpro/habr/upload_files/ff7/159/b4e/ff7159b4e42e3e0077ae4dce96a46180.gif "Shift + Tab")
    
2. **Ctrl + /** — закомментировать или раскомментировать строку. VS Code сам разберется, какой язык программирования вы используете, и в начале строки установит или удалит необходимый символ для комментария. Место, где находится курсор на строке неважно.  
    
    ![Ctrl + /](https://habrastorage.org/getpro/habr/upload_files/378/2b8/b19/3782b8b19839beb9b839c52c02324047.gif "Ctrl + /")
    
3. **Shift + Del** — удалить строку целиком. Теперь не нужно выделять мышкой всю строку и потом нажимать Backspace. Не нужно выделять всю строку. Правда!  
    
    ![Shift + Del](https://habrastorage.org/getpro/habr/upload_files/79b/348/98c/79b34898c8baf5e0495e5ec88970c5e5.gif "Shift + Del")
        
4. **Alt + ↑ / ↓** — перемещение строки с курсором вверх или вниз. Просто попробуйте и ощутите, насколько это удобно. Знаете шутку «стоит всего один раз зимой надеть подштанники, и ты уже не можешь остановиться»? Так вот стоит только один раз переместить так строку, и вы уже не сможете по-другому!
    
    ![Alt + ↑ / ↓](https://habrastorage.org/getpro/habr/upload_files/4de/bd6/b4e/4debd6b4ec2c01524edbdc4cf83744c0.gif "Alt + ↑ / ↓")
    
5. **Shift + Alt + ↓ / ↑** — дублирование строки с курсором вниз. В зависимости от ↓ или ↑ курсор останется на текущей или новой строке. Теперь можно обойтись без Ctrl + C, хотя нет, нельзя =)
    
    ![Alt + ↑ / ↓](https://habrastorage.org/getpro/habr/upload_files/d37/6df/40f/d376df40f2b7d799267c2fd32a75c10e.gif "Alt + ↑ / ↓")
    
6. **F2** — переименовать переменную. Прошу заметить, что переименовываются все переменные с таким названием только внутри блока, не внутри всего открытого файла. Часто нужно переименовать переменную, которая уже используется в нескольких местах функции, и тут либо вручную расставлять курсор в нужное место, либо поставить курсор на переменную и нажать F2.
    
    ![F2](https://habrastorage.org/getpro/habr/upload_files/a49/9b1/737/a499b1737bbbbbb870857077e151dec9.gif "F2")

    
7. **F12** или **Alt + ЛКМ** на переменной — перейти к переменной или родительскому классу. Часто рассказывают про PyCharm, будто только он умеет проваливаться в родительские классы, чтобы посмотреть, какие его атрибуты мы можем переопределять, наследуясь от него; но так умеет и VS Code.
    
    ![F12 или Alt + ЛКМ](https://habrastorage.org/getpro/habr/upload_files/c44/625/9d5/c446259d57475d8b3ad832045007e7a8.gif "F12 или Alt + ЛКМ")
    
8. **Ctrl + D** — выделяет слово, на котором находится курсор. Следующее нажатие на D (удерживая Ctrl) выделить следующее по порядку вниз идентичное значение. Вот пишете вы функцию, и вам нужно выделить ближайшие значения ‘name’. Легко! Выделить все вхождения слова можно вот так — Ctrl + F2. Радует то, что курсор оказывается в конце каждого выделенного значения и сразу можно редактировать!
    
    ![Ctrl + D](https://habrastorage.org/getpro/habr/upload_files/75a/074/56d/75a07456def328e86361306438c0af05.gif "Ctrl + D")
    
9. **Ctrl + L** — выделяет всю строку. Целиком. Теперь копипастить еще проще, не правда ли? =)
    
    ![Ctrl + L](https://habrastorage.org/getpro/habr/upload_files/4df/3e3/99f/4df3e399fbb96aa3f47a9218268f5c8c.gif "Ctrl + L")
    
10. **Ctrl + Alt + →** — разделить рабочую область и переместить актуальную вкладку вправо. **Ctrl + Alt + ←** возвращает вкладку назад. Вы не поверите, насколько удобно видеть, например, [_models.py_](http://models.py/) и [_views.py_](http://views.py/) рядом.
    
    ![Ctrl + Alt + →](https://habrastorage.org/getpro/habr/upload_files/231/545/4a7/2315454a7de74313a687a5b8da47a746.gif "Ctrl + Alt + →")
А теперь неочевидные, но потрясающие возможности. Меню → Файл → Настройки → Сочетания клавиш (Ctrl + K + Ctrl + S), в строке поиска вводим необходимый параметр и кликаем по результату мышкой, после нажимаем нужные клавиши для установки пользовательской настройки и наслаждаемся. Команды, которые точно стоит попробовать:

`editor.action.jumpToBracket` — переход к парной скобке, у меня установлено на Ctrl + Q. Сначала переход к ближайшей скобке, а следующее нажатие перемещает вас к парной скобке и так далее. Часто нам нужно оказаться либо в начале скобок, либо в конце. А кликать мышкой или стрелками не всегда удобно. Теперь достаточно одного нажатия и вы у нужной скобки.

![Ctrl + Alt + →](https://habrastorage.org/getpro/habr/upload_files/dda/839/8c2/dda8398c28730ecdc15dbd98c06128f2.gif "Ctrl + Alt + →")

Ctrl + Alt + →

`editor.action.selectToBracket` — выделить все внутри ближайших скобок и сами скобки, у меня это Ctrl + Shift + Q. Сколько кликов мышкой, сколько ошибок, выделяя внутри скобок мышкой или Shift + стрелки. А теперь можно просто одним нажатием выделить все точно и быстро.

![editor.action.selectToBracket](https://habrastorage.org/getpro/habr/upload_files/cd3/ebc/05a/cd3ebc05a81487127c47e0ac7de0fcbe.gif "editor.action.selectToBracket")

editor.action.selectToBracket

Буду благодарен за любые интересные и полезные хоткеи, пишите в комментариях, что понравилось из моих, и что вы используете сами?