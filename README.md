# [ES6 деструктуризація. Детальна інструкція](https://codeburst.io/es6-destructuring-the-complete-guide-7f842d08b98f)

За останні роки JS не сильно вдосконалився у порівнянні з іншими мовами. Але все ж таки є зміни, які додають потужні можливості, і які варто відзначити. В першу чергу це: шаблонні літерали, деструктуризація, оператор розпакування, стрілкові функції, класи тощо.

У цій статті ми зосередимось на вивченні того, як використовувати **деструктуризацію** для спрощення наших програм. Якщо ви використовуєте [TypeScript](https://www.typescriptlang.org/) або JS фреймворки: [React](https://reactjs.org/), [Vue](https://vuejs.org/), [Preact](https://preactjs.com/),[ Angular](https://angular.io/) тощо, то ви, можливо, вже знайомі з деструктуризацією. Однак існує ймовірність, що, читаючи цю статтю, ви дізнаєтесь кілька нових можливостей деструктуризації, про які ви раніше не здогадувались.

В цьому матеріалі передбачається, що ви вже знайомі з JS. Існує ймовірність того, що деякі фрагменти коду можуть не працювати у вашому веб-браузері, оскільки не всі функції ES6 та ES7 повністю підтримуються всіма веб-переглядачами. Ви можете перевірити тестові фрагменти коду на [Codepen](https://codepen.io/pen/) або іншому онлайн-редакторі. Або ж можете скористатися [Babel](https://babeljs.io/).

## **Чому деструктуризація?**

Щоб пояснити причину використання деструктуризації, ми розглянемо сценарій, з яким стикався майже кожен при написанні коду на JavaScript. Уявіть собі, що у нас є дані студента, включаючи оцінки з трьох предметів (*математика, хімія, англійська мова*), представлені в об'єкті, і нам потрібно показати певну інформацію на основі цих даних. В результаті ми б отримали наступне: 

```
const student = {
	name: 'John Doe',
	age: 16,
	scores: {
		maths: 74,
		english: 63,
		science: 85
		}
};

function displaySummary(student) {
    console.log('Hello, ' + student.name);
    console.log('Your Maths score is ' + (student.scores.maths || 0));
    console.log('Your English score is ' + (student.scores.english || 0));
    console.log('Your Science score is ' + (student.scores.science || 0));
}

displaySummary(student);

// Hello, John Doe
// Your Maths score is 74
// Your English score is 63
// Your Science score is 85
```

Завдяки наведеному вище фрагменту коду ми досягнемо бажаного результату. Проте, є кілька застережень для написання коду таким чином. Одне з них — ви можете легко зробити помилку і замість ```scores.english``` наприклад,  ви пишете ```score.english```, що призведе до помилки.  Знову ж таки, якщо об'єкт оцінки, який ми отримуємо, глибоко вкладено в інший об'єкт, ланцюжок доступу до об'єкта стає довшим, що може означати більше коду. Це може здатися невеликою проблемою, але за допомогою деструктуризації, ми можемо зробити те ж саме в більш зрозумілому та компактному синтаксисі.

## **Що таке деструктуризація?**

***Деструктуризація** — поділ складної структури на прості частини.*
У JavaScript ця складна структура, як правило, є об'єктом або масивом. Використовуючи синтаксис деструктуризації ви можете отримати менші фрагменти з масивів і об'єктів. Синтаксис деструктуризації можна використовувати для оголошення або присвоєння змінної. Ви також можете обробляти вкладені структури за допомогою вкладеного синтаксису деструктуризації. 

Використовуючи деструктуризацію, наша функція з попереднього фрагмента коду матиме наступний вигляд:

```
function displaySummary({ name, scores: { maths = 0, english = 0, science = 0 } }) {
    console.log('Hello, ' + name);
    console.log('Your Maths score is ' + maths);
    console.log('Your English score is ' + english);
    console.log('Your Science score is ' + science);
}
```

Є ймовірність того, що код в останньому фрагменті не збігається з вашим. Якщо це так, то продовжуйте вивчати статтю, поки не дійдете до кінця — запевняю вас, це має сенс. Продовжимо вивчати можливості деструктуризації.

## **Деструктуризація об’єкта**

Те, що ми бачили в останньому фрагменті, — форма деструктуризації об'єктів,  яка використовується як присвоєння для функції. Розглянемо, що ми можемо зробити з деструктуризацією об'єктів, починаючи з нуля. Для деструктуризації функції у ролі виразу присвоєння в основному використовується літерал об’єкту. Ось базовий фрагмент:

```
const student = {
    firstname: 'Glad',
    lastname: 'Chinda',
    country: 'Nigeria'
};

// Деструктуризація об'єкта
const { firstname, lastname, country } = student;

console.log(firstname, lastname, country); // Glad Chinda Nigeria
```

Тут ми використовуємо синтаксис деструктуризації об’єкту для присвоєння значень трьом змінним: ```firstname```, ```lastname``` і ```country``` , використовуючи їх відповідні ключі в об’єкті ```student```. Це найбільш базова форма деструктуризації об’єкта. Цей самий синтаксис може бути використаний для присвоєння змінних наступним чином:

```
// Інііціалізація локальних змінних
let country = 'Canada';
let firstname = 'John';
let lastname = 'Doe';

const student = {
    firstname: 'Glad',
    lastname: 'Chinda',
    country: 'Nigeria'
};

// Перевизначення ім'я та прізвища за допомогою деструктуризації
// Додаємо пару круглих дужок, оскільки це вираз призначення
({ firstname, lastname } = student);

// країна залишається незмінною (Canada)
console.log(firstname, lastname, country); // Glad Chinda Canada
```

Наведений фрагмент показує як, використовуючи синтаксис деструктуризації, присвоїти нові значення локальним змінним. **Зверніть увагу, що нам довелося використовувати пару округлених дужок `(())` у виразі присвоєння.** Якщо прибрати їх, то літерал об'єкту деструктуризації буде прийнято як блок коду, що призведе до помилки, тому що блок не може бути в лівій частині виразу присвоєння.

## **Значення за замовчуванням**

Спроба присвоїти змінну, яка відповідає ключу, що не існує в об'єкті деструктуризації, призведе до присвоєння змінній значення ```undefined```. Ви можете передати значення за замовчуванням, які будуть призначені для таких змінних замість ```undefined```.  Ось простий приклад.

```
const person = {
    name: 'John Doe',
    country: 'Canada'
};

// Присвоїти значення 25 для віку за замовчуванням, якщо undefined
const { name, country, age = 25 } = person;

// Шаблонні літерали ES6
console.log(`I am ${name} from ${country} and I am ${age} years old.`);

// I am John Doe from Canada and I am 25 years old.'
```

Тут ми присвоюємо значення за замовчуванням ```25``` змінній ```age```. Оскільки ключ ```age``` не існує в об'єкті ```person```, ```25``` присвоюється змінній ```age``` замість ```undefined```. 

### **Використання інших імен змінних**

Досі ми присвоювали змінні, які мають таке саме ім'я як і відповідний ключ об'єкта. Ви можете присвоїти інше ім’я змінної, використовуючи цей синтаксис: ```[object_key]:[variable_name] = [default_value]```. Ось простий приклад.

```
const person = {
    name: 'John Doe',
    country: 'Canada'
};

// Присвоїти значення 25 для віку за замовчуванням, якщо ключ віку undefined
const { name: fullname, country: place, age: years = 25 } = person;

// Шаблонні літерали ES6
console.log(`I am ${fullname} from ${place} and I am ${years} years old.`);

// I am John Doe from Canada and I am 25 years old.'
```

Ми створюємо три локальні змінні з іменами: ```fullname```, ```place``` і ```years``` що відповідають ключам ```name```, ```country``` і ```age``` відповідного об’єкту ```person```.

## **Деструктуризація вкладеного об’єкта**

Повернувшись до нашого початкового прикладу деструктуризації, ми нагадуємо, що об'єкт ```scores``` вкладений в об'єкт ```student```. Припустимо, що ми хочемо присвоїти локальним змінним значення оцінок з математики й хімії. В наступному фрагменті коду показано як ми можемо зробити це використовуючи деструктуризацію вкладеного об’єкта.

```
const student = {
    name: 'John Doe',
    age: 16,
    scores: {
        maths: 74,
        english: 63
    }
};

// Ми визначаємо три локальні змінні: name, maths, science
const { name, scores: {maths, science = 50} } = student;

console.log(`${name} scored ${maths} in Maths and ${science} in Elementary Science.`);

// John Doe scored 74 in Maths and 50 in Elementary Science.
```

Тут ми визначили три локальні змінні: ```name```, ```maths``` і ```science```. Також ми встановили значення за замовчуванням ```50``` для ```science``` на випадок, якщо такий ключ буде відсутнім у вкладеному об’єкті ```scores```. Зверніть увагу, що ```scores``` не визначена як змінна. Замість цього ми використовуємо вкладену деструктуризацію, щоб отримати значення ```maths``` і ```science``` з вкладеного об’єкта ```scores```.  

**Використовуючи деструктуризацію вкладеного об'єкта, будьте обережні, щоб уникнути використання пустого вкладеного літерала об'єкта.** Незважаючи на те, що це валідний синтаксис, він насправді не здійснює жодних присвоєнь. Наприклад, наступна деструктуризація нічого не присвоює.

```
// Жодних присвоєнь не здійснюється
// Оскільки вкладений літерал об'єкта ({}) нічого не містить
const { scores: {} } = student;
```

## **Деструктуризація масиву**

На даний час ви вже відчуваєте себе ніндзя деструктуризації, пройшовши через перешкоди в розумінні деструктуризації об'єкта. 

Деструктуризація масиву дуже подібна, тож розглянемо її прямо зараз. Для деструктуризації масиву ви використовуєте масив літералів у лівій частині виразу присвоєння. Кожне ім’я змінної в масиві літералів відображає відповідний елемент з тим самим індексом у масиві, що деструктуризується.
Ось короткий приклад.

```
const rgb = [255, 200, 0];

// Array Destructuring
const [red, green, blue] = rgb;

console.log(`R: ${red}, G: ${green}, B: ${blue}`); // R: 255, G: 200, B: 0
```

В цьому прикладі ми присвоюємо елементи масиву ```rgb``` трьом локальним змінним ```red```, ```green``` і ```blue```, використовуючи деструктуризацію масиву. Зверніть увагу, що кожній змінній присвоюється відповідний елемент з тим самим індексом в масиві  ```rgb```.

## **Значення за замовчуванням**

Якщо число елементів масиву більше, ніж кількість локальних змінних, що передається до деструктуризованого літерального масиву, тоді решта елементів не зіставляються. Але якщо кількість локальних змінних, переданих в літеральний масив деструктуризації перевищує число елементів в масиві, то кожній зайвій локальній змінній, окрім тих яким ви задасте значення, присвоюється значення за замовчуванням. 

Як і в разі деструктуризації об'єкта, ви можете встановити значення за замовчуванням для локальних змінних, використовуючи деструктуризацію масивів. У наступному прикладі ми будемо встановлювати значення за замовчуванням для деяких змінних у випадку, якщо відповідний елемент не знайдено.

```
const rgb = [200];

// Встановлюємо стандартне значення 255 для red та blue
const [red = 255, green, blue = 255] = rgb;

console.log(`R: ${red}, G: ${green}, B: ${blue}`); // R: 200, G: undefined, B: 255
```

Ви також можете здійснити присвоєння використовуючи деструктуризацію. На відміну від деструктуризації об'єкта, вам **не потрібно брати в дужки вираз, що присвоюється**. Ось приклад.

```
let red = 100;
let green = 200;
let blue = 50;

const rgb = [200, 255, 100];

// Присвоєння через деструктуризацію для red та green
[red, green] = rgb;

console.log(`R: ${red}, G: ${green}, B: ${blue}`); // R: 200, G: 255, B: 50
```

## **Пропуск елементів**

Існує можливість пропускати елементи, значення яких ти не хочеш присвоювати локальним змінним і присвоювати лише ті, які тобі потрібні. Ось приклад того, як ми можемо присвоїти тільки значення ```blue``` в локальну зміну.

```
const rgb = [200, 255, 100];

// Пропустити перші два елементи
// Встановити значення лише для третього елемента blue
const [,, blue] = rgb;

console.log(`Blue: ${blue}`); // Blue: 100
```

Ми використали розділювач кому (,), щоб опустити перші два елементи масиву ```rgb```, оскільки нам потрібен лише третій елемент.

## **Перестановка змінних**

Одне дуже приємне застосування деструктуризації масиву полягає в зміні локальних змінних. Уявіть, що ви створюєте застосунок для роботи з фотографіями, і ви хочете змінити розмір висоти та ширини фотографії, коли перемикається орієнтація фотографії між пейзажем та портретом. Старомодний спосіб зробити це буде виглядати наступним чином:

```
let width = 300;
let height = 400;
const landscape = true;

console.log(`${width} x ${height}`); // 300 x 400

if (landscape) {
    // Необхідна додаткова змінна для копіювання початкової ширини
    let initialWidth = width;

    // Переставляємо змінні
    width = height;
  
    // height призначається до скопійованого значення width
    height = initialWidth;

    console.log(`${width} x ${height}`); // 400 x 300
}
```

Наведений вище фрагмент коду виконує завдання, хоч нам довелося використовувати додаткову змінну для копіювання значення однієї зі змінних в іншу. За допомогою деструктуризації масиву ми можемо виконати обмін за допомогою одного присвоєння. Наступний фрагмент показує, як для цього можна використати деструктуризацію масиву.

```
let width = 300;
let height = 400;
const landscape = true;

console.log(`${width} x ${height}`); // 300 x 400

if (landscape) {
    // Змінюємо змінні
    [width, height] = [height, width];
  
    console.log(`${width} x ${height}`); // 400 x 300
}
```

## **Деструктуризація вкладеного масиву**

Як і у випадку з об'єктами, ви також можете здійснювати вкладену деструктуризацію масивів. Щоб використовувати вкладену деструктуризацію масиву, **порядок відповідності елементів та елементів-масивів повинна зберігатись**. Ось короткий приклад, щоб проілюструвати це.

```
const color = ['#FF00FF', [255, 0, 255], 'rgb(255, 0, 255)'];

// Використовуйте вкладене деструктування, щоб призначити red, green та blue
const [hex, [red, green, blue]] = color;

console.log(hex, red, green, blue); // #FF00FF 255 0 255
```

У наведеному вище коді ми присвоїли  змінній ```hex``` значення і використали деструктуризацію вкладеного масиву, щоб присвоїти значення змінним ```red```, ```green``` і ```blue```.

## **Присвоєння решти масиву змінній**

Іноді виникає бажання присвоїти деякі значення змінним, одночасно забезпечуючи отримання решти значень (присвоїти їх іншій змінній). Новий оператор розпакування (...), який було додано в ES6 в поєднанні з деструктуризацією можна використати для вирішення цієї задачі. Тут маленький приклад:

```
const rainbow = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];

// Призначаємо перший і третій елементи red і yellow
// Призначаємо решту елементів змінній anotherColors за допомогою оператора розпакування (...)
const [red,, yellow, ...otherColors] = rainbow;

console.log(otherColors); // ['green', 'blue', 'indigo', 'violet']
```

Тут ви можете побачити, що після присвоєння першого і третього значення масиву ```rainbow```  ```red``` і ```yellow``` відповідно, ми використали оператор розпакування для отримання і присвоєння решти значень масиву змінній ```otherColors```.  Саме такий процес має назву присвоєння решти масиву змінній. Однак зауважте, що **змінна, в яку записується решта елементів масиву, у випадку коли вона використовується, завжди повинна бути останнім елементом деструктурованого масиву**, інакше виникне помилка. 

Є кілька приємних бонусів процесу присвоєння решти масиву змінній, які варто розглянути. Одним з них є клонування масивів.

## **Клонування масивів**

У JS **масив належить до непримітивного типу даних ```Object```, тому під час присвоєння змінній, не копіюється власне масив, а записуються  посилання на нього.** Тобто в наступному прикладі обидві змінні ```rainbow``` і ```rainbowClone``` будуть посиланнями на один масив: зміни, здійснені з ```rainbow``` відображаться в ```rainbowClone```.

```
const rainbow = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];
const rainbowClone = rainbow;

// rainbow та rainbowClone посилаються на один й той же масив
console.log(rainbow === rainbowClone); // true

// Зберігати тільки перші 3 елементи й відкинути все інше
rainbowClone.splice(3);

// Зміни в rainbowClone також впливають на rainbow
console.log(rainbow); // ['red', 'orange', 'yellow']
console.log(rainbowClone); // ['red', 'orange', 'yellow']
```

Наступний фрагмент коду показує, як ми можемо клонувати масив використовуючи спосіб ES5 - операції ```Array.prototype.slice``` та ```Array.prototype.concat```.

```
const rainbow = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];

//Копіюємо за допомогою Array.prototype.slice
const rainbowClone1 = rainbow.slice();

console.log(rainbow === rainbowClone1); // false
console.log(rainbowClone1); // ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']

// Копіюємо за допомогою Array.prototype.concat
const rainbowClone2 = rainbow.concat();

console.log(rainbow === rainbowClone2); // false
console.log(rainbowClone2); // ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']
```

В наступному фрагменті можна побачити клонування масиву використовуючи **деструктуризацію та оператор розпакування**.

```
const rainbow = ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet'];

// Копіюємо за допомогою деструктуризації масива і оператора розпакування
const [...rainbowClone] = rainbow;

console.log(rainbow === rainbowClone); // false
console.log(rainbowClone); // ['red', 'orange', 'yellow', 'green', 'blue', 'indigo', 'violet']
```

## **Вкладена деструктуризація об’єктів та масивів**

Тут ми розглянемо випадок, коли ми працюємо з досить складними об’єктами чи масивами і нам необхідно присвоїти деякі значення локальним змінним. Хорошим прикладом буде об’єкт з кількома вкладеними об’єктами чи багатовимірними масивами. У такому випадку ми можемо використовувати поєднання деструктуризацій масиву та об’єкту, щоб отримати окремі необхідні нам частини складної структури.
Наступний блок коду ілюструє простий приклад.

```
const person = {
    name: 'John Doe',
    age: 25,
    location: {
        country: 'Canada',
        city: 'Vancouver',
        coordinates: [49.2827, -123.1207]
    }
}

// Погляньте, як тут використовується комбінація деструктуризації об'єкта та масиву
//Ми визначаємо 5 змінних: name, country, city, lat, lng
const {name, location: {country, city, coordinates: [lat, lng]}} = person;

console.log(`I am ${name} from ${city}, ${country}. Latitude(${lat}), Longitude(${lng})`);

// I am John Doe from Vancouver, Canada. Latitude(49.2827), Longitude(-123.1207)
```

## **Деструктуризація параметрів функції**

Деструктуризація також може бути застосована до параметрів функції для отримання та присвоєння значень локальним змінам. Зауважте, однак, що **деструктуризований параметр не може бути опущений** (це обов’язково), інакше ви отримаєте помилку. Хорошим варіантом  використання є функція ```displaySummary()``` з нашого початкового прикладу, яка очікує в ролі параметра об’єкт ```student```. Ми можемо деструктуризувати об’єкт ```student``` і присвоїти окремі значення локальним змінним функції. Тут приклад:

```
const student = {
    name: 'John Doe',
    age: 16,
    scores: {
        maths: 74,
        english: 63,
        science: 85
    }
};

// Без деструктуризації
function displaySummary(student) {
    console.log('Hello, ' + student.name);
    console.log('Your Maths score is ' + (student.scores.maths || 0));
    console.log('Your English score is ' + (student.scores.english || 0));
    console.log('Your Science score is ' + (student.scores.science || 0));
}

// З деструктуризацією
function displaySummary({name, scores: { maths = 0, english = 0, science = 0 }}) {
    console.log('Hello, ' + name);
    console.log('Your Maths score is ' + maths);
    console.log('Your English score is ' + english);
    console.log('Your Science score is ' + science);
}

displaySummary(student);
```

Тут ми витягуємо необхідні нам значення з об’єкта ```student``` і присвоюємо їх локальним змінним ```name```, ```maths```, ```english``` і ```science```. Зверніть увагу, що хоча ми вказали значення за замовчуванням для деяких змінних, якщо ви викликаєте функцію без аргументів, ви отримаєте помилку, оскільки завжди потрібні параметри деструктуризації. Ви можете призначити резервний об'єкт, як значення за замовчуванням для об'єкта ```student``` та вкладеного об’єкта ```scores```, якщо вони не будуть надані, щоб уникнути помилки, як показано у наступному фрагменті.

```
function displaySummary({ name, scores: { maths = 0, english = 0, science = 0 } = {} } = {}) {
    console.log('Hello, ' + name);
    console.log('Your Maths score is ' + maths);
    console.log('Your English score is ' + english);
    console.log('Your Science score is ' + science);
}

// Викликаємо без аргумента student
displaySummary();

// Hello, undefined
// Your Maths score is 0
// Your English score is 0
// Your Science score is 0
```
