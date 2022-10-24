# Лабораторная работа 8. Узагальнене програмування

## Цілі лабораторної роботи:

- Вивчити принципи та мету механізму узагальненого програмування;
- Навчитися створювати узагальнені класи та інтерфейси;
- Розібратися з процесом створення узагальнених методів.

## Завдання на лабораторну роботу

### Завдання 1

Дан узагальнений інтерфейс

```java
interface Stack<T> {
    void push(T element);
    T pop();
    T peek();
    boolean isEmpty();
    void clear();
}
```

Щоб створити масив узагальненого типу, використовуйте наступну конструкцію

```java
T[] array = (T[])new Object[INITIAL_ARRAY_LENGTH];
```

1. Реалізуйте інтерфейс за допомогою узагальненого класу. 

2. Реалізуйте інтерфейс за допомогою конкретного класу, вказуючи клас `String` у якості типізованої змінної.

### Завдання 2

Дан клас `BookData`, який моделює інформацію про книгу в інтернет-магазині. Поле `reviews` зберігає загальну кількість оцінок користувачів, а `total` зберігає загальну суму оцінок. Рейтинг книги вираховується як `total / reviews`.

```java
class BookData {

    private String title;
    private String author;
    private int reviews;
    private double total;
}
```

Необхідно зробити так, щоб клас підтримував узагальнений інтерфейс `Comparable<>`. Книга з більш високим рейтингом повинна бути "менше", ніж книга з меншим рейтингом. У разі рівності рейтингів, книги порівнюються за назвою у звичайному порядку.

### Завдання 3

Даний наступний код

```java
import java.lang.reflect.Method;
public class Main {
    public static void main(String[] args) {
        Printer myPrinter = new Printer();
        Integer[] intArray = {1, 2, 3};
        String[] stringArray = {"Hello", "World"};
        myPrinter.printArray(intArray);
        myPrinter.printArray(stringArray);
    }
}

class Printer {}
```

Згідно коду, є масив цілих чисел та масив рядків. Напишіть в класі `Printer` узагальнений метод `printArray()`, який може роздруковувати всі елементи як масиву цілих чисел, так і масиву рядків (дивіться код методу `main()`).

Якщо задача вирішена вірно, код в методі `main()` повинен працювати коректно та роздруковувати елементи масивів. **Код методу `main()` змінювати не можна!**

### Задание 4

Даний приклад методу `filter()` з лабораторной роботи 7.

```java
public int[] filter(int[] input, Predicate p) {
    int[] result = new int[input.length];

    int counter = 0;
    for (int i : input) {
        if (p.test(i)) {
            result[counter] = i;
            counter++;
        }
    }

    return Arrays.copyOfRange(result, 0, counter);
}
```

Напишіть узагальнену версію методу `filter()`, який міг би приймати на вхід масив об'єктів типу `T` та узагальнений предикат типу `T`.

Для створення масиву узагальненого типу використовуйте наступну конструкцію

```java
T[] array = (T[])new Object[INITIAL_ARRAY_LENGTH];
```

### Завдання 5

Даний метод `contains()`, що перевіряє входження рядку у масив рядків.

```java
boolean contains(String[] array, String element) {

    for (String str : array)
        if (str.equals(element))
            return true;

    return false;
}
```

Напишіть узагальнену версію методу `contains()`. Узагальнена версія методу приймає на вхід масив типу `T` (тип `T` повинен реалізовувати узагальнений інтерфейс `Comparable`) та об'єкт типу `V` (який повинен бути чи типом `T` чи його підкласом). 

### Завдання 6

Одним ж потужних механізмів, який відсутній в мові Java - це можливість повертати множину об'єктів в результаті роботи методу. Оператор `return` дозволяє вказати тільки один об'єкт, але ми можемо створити об'єкт, який зберігає в собі декілька об'єктів, які ви бажаєте повернути.

Така концепція називається **кортежем** (**tuple**) та представляє собою групу об'єктів, які "загорнуті" в один об'єкт. Отримувач об'єкту може читати елементи, а не може додавати чи змінювати присутні елементи.

Кортежи можуть бути різної довжини, але кожен об'єкт кортежу може мати будь-який тип. Щоб забезпечити типобезпеку такого кортежу, можно використати механізм узагальненого програмування.

Нижче наведений приклад кортежу для двох конкретних типів.

```java
class ConcreteTwoTuple {

    public final String first;
    public final Integer second;

    public ConcreteTwoTuple(String first, Integer second) {
        this.first = first;
        this.second = second;
    }
    
    @Override
    public String toString() {
        return "(" + first + "," + second + ')';
    }
}
```

Нижче представлений приклад використання такого кортежу

```java
StudentRatingTuple tuple = getStudentWithRating("Іванов Петр Петрович");
System.out.println("Студент: " + tuple.first + ", рейтинг: " + tuple.second);

...

public StudentRatingTuple getStudentWithRating(String fullName) {
    Student student = new Student(fullName);
    int rating = student.calculateRating();

    return new StudentRatingTuple(student, rating);
}

...

class StudentRatingTuple {

    public final Student first;
    public final Integer second;

    public StudentRatingTuple(Student first, Integer second) {
        this.first = first;
        this.second = second;
    }

    @Override
    public String toString() {
        return "(" + first + "," + second + ')';
    }
}
```

Такий підхід не дає достатньої гнучкості, так як для кожного типу `first` чи `second` ми повинні створювати свій окремий клас.

Ми можемо використати тип `Object`

```java
class ObjectTwoTuple {

    public final Object first;
    public final Object second;

    public ObjectTwoTuple(Object first, Object second) {
        this.first = first;
        this.second = second;
    }

    @Override
    public String toString() {
        return "(" + first + "," + second + ')';
    }
}
```

але такий підхід не зберігає інформацію щодо типу полів `first` та `second`.

Ваше завдання полягає в наступному - вирішити цю проблему за допомогою механізму узагальненого програмування.

1. Напишіть узагальнену версію класу `ConcreteTwoTuple` під назвою `GenericTwoTuple`, в якій поле `first` буде мати тип `T`, а поле `second` - тип `V`.
2. Створіть клас під назвою `GenericThreeTuple` та використовуючи механізм композиції, використайте функціонал класу `GenericTwoTuple` та додайте ще одне поле поле `three` типу `S`.
3. Продемонструйте роботу класу `GenericTwoTuple` та `GenericThreeTuple` на прикладах, які ви придумаєте самостійно.
