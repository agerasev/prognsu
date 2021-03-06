---
title: Контейнеры стандартной библиотеки C++
menu: textbook-cpp
---

При написании почти любой программы возникает необходимость работы с набором объектов. К набору объектов можно добавлять объекты, удалять из него объекты, искать определенный объект и т.д. В любом высокоуровневом языке программирования реализованы классические структуры данных (контейнеры), позволяющие решать часто возникающие задачи по работе с наборами объектов. Умение использовать подходящие структуры данных является базовым навыком разработчика.

Стандартная библиотека C++ содержит контейнеры двух типов:

* **Последовательные** контейнеры обеспечивают эффективный последовательный доступ к элементам. Примерами последовательных структур данных являются двусвязный список и массив, которые в C++ представлены типами `list` и `vector`, соответственно. Вероятно, читатель уже знаком с этими структурами.
* **Ассоциативные** контейнеры обеспечивают быстрый поиск элементов. Примером ассоциативных структур данных являются множество и словарь, которым соответствуют типы `set` и `map`. Тип `set` позволяет хранить *уникальные* объекты различных типов, эффективно добавлять, удалять объекты и выполнять поиск. Тип `map` позволяет хранить пары ключ-значение, причем ключи должны быть уникальными.

Разные типы контейнеров C++ разработаны таким образом, чтобы работа с ними происходила единообразно. Во многих случаях можно изменить тип контейнера, не изменяя часть кода, в которой происходит работа с контейнером. Сейчас мы рассмотрим примеры работы с основными контейнерами и кратко обсудим их относительные преимущества и недостатки. В дальнейшем в ходе курса мы будем уточнять знания о контейнерах.

## Контейнер vector

Контейнер `vector` используется чаще всего. Чтобы создать объект типа `vector`, необходимо в угловых скобках указать тип объектов, которые мы собираемся хранить. Вот несколько примеров создания объектов типа `vector`:

```cpp
#include <vector>
#include <string>

using namespace std;

vector<int> v1;  // пустой вектор для объектов типа int
vector<int> v2 = {1,2,3};  // создание вектора с помощью списока инициализации
vector<double> v3(10);  // вектор размера 10 для хранения объектов типа double
vector<string> v4(3, "vec");  // вектор строк, который содержит три строки "vec"
vector<vector<double>> v5;  // вектор векторов для типа double
```

Тип `vector` может работать с любым типом данных, в том числе с самим собой, как показано в последнем примере. Это относится ко всем контейнерам C++ с некоторыми уточнениями.

Обращаться к элементу вектора можно по индексу:

```cpp
vector<int> v{1,2,3};
int a = v[0];  // a = 1
int b = v.at(1);  // b = 2
v[2] = 5;  // v = [1,2,5]
v.at(0) = 0; // v = [0,2,5]
```

При вызове оператора `[]` не происходит проверка того, что индекс не превышает размер вектора, в то время как метод `at()` выполняет такую проверку и в случае неудачи генерирует исключение.

Чтобы добавить один элемент в конец вектора, можно воспользоваться методом `push_back()`. Метод `insert()` возволяет вставить объект в прозвольное место в векторе:

```cpp
vector<int> v{1,2,4};
v.push_back(5);  // v = [1,2,4,5]
v.insert(v.begin()+2, 3);  // v = [1,2,3,4,5]
vector<int> w{6,7,8};
v.insert(v.end(), w.begin(), w.end());  // v = [1,2,3,4,5,6,7,8]
```

Методы `begin()` и `end()` позволяют получить объекты, которые указывают на начало и конец вектора, соответственно. Позже мы уточним более детально, что это за объекты.

Вставка объекта в конец вектора с помощью метода `push_back()` происходит максимально эффективно, поскольку объекты хранятся последовательно в памяти. Вставка объекта в начало вектора методом `insert()` приводит к необходимости перемещать все остальные объекты. Если в вашей программе происходит вставка в начало вектора в цикле, то скорее всего вы выбрали не самую подходящую структуру данных.

Метод `size()` возвращает текущий размер вектора, метод `empty()` возвращает `true`, если размер равен нулю и `false` в противном случае. Эти методы определены для всех стандартных контейнеров C++.

Прочитать последовательно все элементы вектора можно способом, который работает для всех стандартных контейнеров:

```cpp
vector<int> v{1,2,3,4};
for (int item : v) {
    cout << item << ' ';
}
```

Внимательный читатель заметил, что работа с векторами во многом похожа на работу с типом `string`. Это не удивительно, ведь `string` можно рассматривать в качестве специального последовательного контейнера. Также это сходство иллюстрирует стремление к единообразию в работе с различными контейнерами в C++.

Мы обсудили далеко не все возможности векторов, но этого достаточно для начала работы с ними. Более подробную информацию о типе `vector` можно найти в [документации](https://en.cppreference.com/w/cpp/container/vector).

## Контейнер set

Тип `set` реализует ассоциативный контейнер "множество". Объект типа `set` хранит *упорядоченное* множество уникальных объектов. Операции добавления, удаления и поиска элемента в `set` требуют количество вычислений, пропорциональное логарифму текущего размера множества. Эти характеристики достигаются специальным способом хранения объектов - в виде [двоичного дерева поиска](https://ru.wikipedia.org/wiki/%D0%9A%D1%80%D0%B0%D1%81%D0%BD%D0%BE-%D1%87%D1%91%D1%80%D0%BD%D0%BE%D0%B5_%D0%B4%D0%B5%D1%80%D0%B5%D0%B2%D0%BE).

Создать объект `set` можно различными способами:

```cpp
#include <set>
#include <string>
#include <vector>
#include <iostream>

using namespace std;

set<int> s1;  // пустое множество для int
set<int> s2{1,3,5,2};  // инициализация из список объектов
vector<string> v{"frog", "dog", "cat", "dog"};
set<string> s3(v.begin(), v.end());
for (string item : s3) {
    cout << item << ", ";
}  // Выведет cat, dog, frog
```

Добавление элементов в `set` выполняется с помощью метода `insert`

```cpp
set<int> s{1,3,1,1};  // s = {1, 3}
s.insert(2);  // s = {1,2,3}
s.insert({1,2,3,4,5});  // s = {1,2,3,4,5}
vector<int> v{1,80,82};
s.insert(v.begin(), v.end());  // s = {1,2,3,4,5,80,82}
```

Рассмотрим несколько методов для поиска элементов в `set`:

* `find` позволяет найти элемент, если он есть в `set`. Метод возвращает *итератор* на найденный элемент. Если элемент не найден, возвращается специальный итератор, указывающий в никуда (возвращается методом `end()`);
* `lower_bound` возвращает итератор на первый элемент, который не меньше данного;
* `upper_bound` возвращает итератор на первый элемент, который больше данного.

Про итераторы мы будем говорить отдельно. Сейчас достаточно знать, что это некоторый объект, который *указывает* на элемент в контейнере. Незнание некоторых деталей не мешает нам начать их использовать:

```cpp
set<int> s{1,3,4,5};
set<int>::iterator it = s.find(2);
if (it == s.end()) {
    cout << "Not found\n";
} else {
    int val = *it;
    cout << "Found " << val << endl;
}

if (auto it2 = s.lower_bound(3); it2 != s.end()) {
    cout << "Lower bound is " << *it2 << endl;  // Lower bound is 3
}

if (auto it3 = s.upper_bound(2); it3 != s.end()) {
    cout << "Upper bound is " << *it3 << endl;  // Lower bound is 3
}
```

Из этого примера мы видим, что итератор для типа `set<int>` имеет тип `set<int>::iterator`. Ключевое слово `auto` позволяет не набирать длинные названия типов каждый раз, когда мы имеем дело с итераторами. Также мы использовали операцию *разыменовывания* итератора `int val = *it;`, которая по форме очень похожа на разыменовывание указателей в C.

Наконец, в этом примере использован прием, который стал доступен, начиная со стандарта  C++ 2017 года: мы инициализировали итераторы `it2` и `it3` в теле условного оператора `if`. Конструкция с инициализированием переменной в теле оператора `if` позволяет явно указать, что переменная не будет использоваться за пределами условного оператора. Этот прием позволяет писать более выразительный код и не держать в области видимости лишние объекты.

Объекты типа `set` хранят упорядоченные наборы объектов. Это накладывает некоторые требования на тип этих объектов: для типа должен быть определен оператор меньше `<`. Именно он используется для поддержания правильной структуры.

## Контейнер map

Еще один очень полезный ассоциативный контейнер - тип `map`. Он позволяет хранить пары ключ-значение и эффективно находить объекты по ключу. Все очень похоже на тип `set`, только с каждым уникальным ключем связано некоторые значение. Большинство методов типа `map` аналогичны методам типа `set`. Следующий пример показывает основные приемы работы с контейнером `map`:

```cpp
map<string, int> m1;  // ключ типа string, значение типа int
m1.insert({"NSU", 257});

map<string, int> m2 {
    {"NSU", 257},
    {"MSU", 150},
    {"MIPT", 230},
    {"Stanford U", 1}
};

if (auto it = m2.find("NSU"); it != m2.end()) {
    string university = it->first;
    int rank = it->second;
    cout << "University " << university << " has rank " << rank << endl;
}

if (auto it2 = m2.find("Stanford U"); it2 != m2.end()) {
    m2.erase(it2);  // удаляем пару ключ-значение из словаря
}

for (auto& [key, val] : m2) {
    cout << key << ": " << val << endl;
}
// Вывод будет упорядочен по ключам
// MIPT: 230
// MSU: 150
// NSU: 257
```

## Неупорядоченные ассоциативные контейнеры

В стандартной библиотеке реализованы неупорядоченные версии контейнеров `unordered_set` и `unordered_map`, которые гарантируют сложность добавления, удаления и поиска элементов за константное время. Это означает, что количество выполняемых операций не зависит от количества элементов в контейнере. Эти гарантии достигаются благодаря хранению объектов в виде [хэш-таблиц](https://ru.wikipedia.org/wiki/%D0%A5%D0%B5%D1%88-%D1%82%D0%B0%D0%B1%D0%BB%D0%B8%D1%86%D0%B0).

## Резюме

Мы обсудили два типа контейнеров из стандартной библиотеки C++: последовательные и ассоциативные. Рассмотрели логику работы с наиболее часто используемыми контейнерами `vector`, `set` и `map`. Выбор подходящих структур данных является одним из ключевых умений грамотного разработчика программного обеспечения. Мы призываем вас продолжать самостоятельно изучать различные структуры данных.

## Документация

* [https://en.cppreference.com/w/cpp/container](https://en.cppreference.com/w/cpp/container)
