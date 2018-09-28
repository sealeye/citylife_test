Перед началом работы нужно установить зависимости:
```bash
npm install
```
## Режимы
Одноразовая сборка:
```bash
npm start
```
Запуск живой сборки на локальном сервере:
```bash
npm run live
```

Сборка без автоматической перезагрузки страниц:
```bash
npm run no-server
```

## Шаблонизация
Шаблоны собираются из папки `templates` с помощью [twig](https://github.com/twigjs/twig.js/wiki). Универсальные блоки в `blocks`. Боевые файлы автоматически собираются в корне проекта. Основной шаблон лежит в `core/layout.twig`. Страницы включают в себя основной шаблон. Код пишется внутри блока `body`:
 ```twig
 {% extends "core/layout.twig" %}
 
 {% block body %}
   ...
 {% endblock %}
 ```
## Кастомные данные при шаблонизации
Чтобы добавить возможность передавать в шаблоны данные (например, расставлять контент в рандомном порядке) необходимо создать файл `templates/data.json` и в `gulpfile.js` заменить таск шаблонизации на следующий: 
```javascript
gulp.task('twig', function() {
  gulp.src(paths.templates + '*.twig')
    .pipe(plumber({errorHandler: onError}))
    .pipe(data(function() {
      if (fs.existsSync('templates/data.json')) {
        return JSON.parse(fs.readFileSync(paths.json));
      }
    }))
    .pipe(twig())
    .pipe(gulp.dest(paths.html))
    .pipe(reload({stream: true}));
});
```
Далее — добавить в таск `watch`:
```javascript
gulp.watch('templates/data.json', ['twig']);
```

## Стили
Верстаются в `assets/styles/styles.scss`, компилируются в `css/style.css`. Разделение на смысловые слои. Сначала core — фундамент: переменные, сетка, дефолтные стили. 

### SCSS 
Переменные :
```scss
$helvetica: "Helvetica Neue", Arial, sans-serif;
```
Вложенность для элементов и модификаторов в БЭМе:
```scss
.block {
  ...
  
  &__element {
    ...
  }
  
  &--modifier {
    ...
  }
}
```
См. [документацию SASS](http://sass-lang.com/guide)

## Сжатие картинок
Картинки кладутся в `assets/img/` и с помощью [gulp-imagemin](https://www.npmjs.com/package/gulp-imagemin) минифицируются в папку `img/`.

## Скрипты
Можно писать на es2015 — подключен и работает Babel. Включен jQuery 3.
