# Введение в pixi.js


Pixi js — это чудесная библиотека, реализующая быстрый и простой механизм
визуализации. Она может работать в сочетании с рядом других игровых библиотек на
JavaScript и выполнять визуализацию на основе `canvas` и WebGL.

В этой главе мы рассмотрим очень простое введение в работу pixi.js. Мы
выполним визуализацию изображения с детёнышем зомби, чтобы продемонстрировать
основы работы с рендерером и функциональные возможности pixi.js по созданию
сцены, спрайтов и текстуры.

## Создание каталога для проекта

Создайте для проекта новый каталог и сделайте его текущей рабочей директорией. 

    mkdir pixi-intro
    cd pixi-intro

### Создание файла package.json

    npm init

Введите данные, следуя подсказкам `npm init` и в результате получите файл 
package.json.

## Установка pixi.js с помощью пакетного менеджера

    npm install --save GoodBoyDigital/pixi.js

Версия, опубликованная на данный момент в репозитории npm,
является устаревшей, поэтому установим пакет прямо с GitGub, используя
команду, приведённую выше. Параметр ` --save` позволяет сохранить pixi.js как
зависимость файла package.json.

## Подготовьте изображение, которое будете использовать

Если хотите, можете использовать эту картинку с детёнышем зомби:

    https://raw.githubusercontent.com/sethvincent/hogjam4/gh-pages/images/menu-image/05.png

Или же выберите любую другую картинку на своё усмотрение.

Только убедитесь, что изображение имеет название zombie.png и помещено в
корневую папку проекта.

## Создание файла index.js

    touch index.js

## Редактирование файла index.js

Давайте создадим простой пример, в котором изображение будет выводиться  на
экран.

### Запрос на подключение pixi.js к вашей программе

    var PIXI = require('pixi.js');

Мы запрашиваем модуль pixi.js как `PIXI`,  потому что именно так, заглавными,
объект pixi.js прописан в документации.

### Создаём рендерер для игры

    var renderer = new PIXI.CanvasRenderer(window.innerWidth, window.innerHeight);

Здесь мы используем рендерер pixi на основе canvas. Точно так же просто можно
использовать WebGL. Для этого вместо `PIXI.CanvasRenderer` пропишите
`PIXI.WebGLRenderer`. С помощью аргументов `window.innerWidth` и
`window.innerHeight` мы указываем, что рендерер должен заполнить всю ширину и
высоту экрана.

### Присоединение рендерера к телу html-файла

    document.body.appendChild(renderer.view);

### Создаём сцену, на которой будет происходить отрисовка

    var stage = new PIXI.Stage;

Мы добавим наше изображение в качестве спрайта, который будет отрисовываться
на этой сцене.

### Создаём из изображения текстуру и спрайт

    var zombieTexture = PIXI.Texture.fromImage('zombie.png'); 
    var zombie = new PIXI.Sprite(zombieTexture);

Сначала создаём текстуру, используя метод `PIXI.Texture.fromImage()` и
передаём изображению относительный url-адрес в качестве аргумента.

Затем создаём спрайт с помощью метода `PIXI.Sprite()` и передаём ему текстуру
в качестве аргумента.

### Назначаем расположение спрайта с зомби

    zombie.position.x = window.innerWidth / 2 — 150;
    zombie.position.y = window.innerHeight / 2 — 150;

У изображения с зомби размеры 300 на 300 пикселей, так что код, приведенный
выше,  отцентрирует спрайт по середине окна.

### Добавляем на сцену спрайт с зомби

    stage.addChild(zombie);

Этот код добавляет наш спрайт на сцену, так что он будет отрисован 
при выполнении метода `renderer.render()`.

### Создание функции отрисовки для запуска рендерера

    function draw() { 
        renderer.render(stage); 
        requestAnimationFrame(draw); 
    }

Эта функция отрисовки запускает метод `renderer.render()` с аргументом `stage`.
Также она запускает `requestAnimationFrame` с самой функцией `draw()` в качестве
аргумента, что гарантирует циклично повторяющийся вызов `draw()`.

### Запуск приложения

    draw();

Мы выполняем функцию `draw()` в первый раз, чтобы запустить игру и,  так как
внутри мы вызываем функцию отрисовки,  она будет рекурсивно выполняться для
каждого фрейма `requestAnimationFrame`.

## Полный код примера

Вот весь код файла index.js:

    var PIXI = require('pixi.js');

    var renderer = new PIXI.CanvasRenderer(window.innerWidth, window.innerHeight);

    document.body.appendChild(renderer.view);

    var stage = new PIXI.Stage;

    var zombieTexture = PIXI.Texture.fromImage('zombie.png'); 
    var zombie = new PIXI.Sprite(zombieTexture);

    zombie.position.x = window.innerWidth / 2 — 150;
    zombie.position.y = window.innerHeight / 2 — 150;

    stage.addChild(zombie);

    function draw() { renderer.render(stage); requestAnimationFrame(draw); }

    draw(); 

## Запуск сервера разработки

Эту программу можно запустить на вашей локальной машине, используя модуль
`beefy`,  чтобы объединить зависимости с помощью Browserify и подать файлы с
помощью сервера разработки со встроенной возможностью автообновления.

### Установите глобально beefy и browserify

Если вы еще этого не сделали, установите beefy и browserify:

    npm install -g beefy browserify

### Запустите сервер

Выполните эту команду, чтобы запустить сервер разработки:

    beefy index.js --live

На выходе вы увидите следующее:

    listening on http://localhost:9966/

Теперь вы можете открыть [http://localhost:9966/][1] в своём браузере и 
увидеть, как работает игра.

### Хотите научиться писать игры на JavaScript?

Это отрывок из нашей книги «Создание двумерных игр на JavaScript». Её можно 
купить отдельно или вместе с другими нашими книгами и скринкастами.

## Хотите узнать больше о pixi.js?

Зайдите на сайт проекта: [pixijs.com][2]

Взгляните на репозиторий pixi.js на GitHub: [github.com/GoodBoyDigital/pixi.js][3]

Документацию по проекту можно почитать здесь: [goodboydigital.com/pixijs/docs][4]

[1]: http://localhost:9966/
[2]: http://www.pixijs.com/
[3]: https://github.com/GoodBoyDigital/pixi.js
[4]: http://www.goodboydigital.com/pixijs/docs/
