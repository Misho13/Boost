#Boost 
##Содержание 
- [Глава №21](#Boost.Optional)
- [Глава №23](#Boost.Any)
- [Глава №24](#Boost.Variant)
- [Глава №25](#Boost.PropertyTree)
- [Глава №27](#Boost.Tribool)

<a name="Boost.Optional"></a>
##Глава №21 Boost.Optional
Библиотека [Boost.Optional](http://www.boost.org/doc/libs/1_62_0/libs/optional/doc/html/index.html)  предоставляет класc **`boost::optional`**, который может быть использован для дополнительных возвращаемых значений. Эти возвращаемые значения функций могут не всегда возвращать результат. [Пример 21.1](#example211) показывает, как необязательные возвращаемые значения обычно реализуются без Boost.Optional. 

<a name="example211"></a>
`Пример 21.1. Специальные значения для указания необязательных возвращаемых значений.`
```c++
#include <iostream> 
#include <cstdlib> 
#include <ctime> 
#include <cmath> 

int get_even_random_number() 
{  
  int i = std::rand();  
  return (i % 2 == 0) ? i : -1; 
} 
int main() 
{  
  std::srand(static_cast<unsigned int>(std::time(0)));  
  int i = get_even_random_number();  
  if (i != -1)    std::cout << std::sqrt(static_cast<float>(i)) << '\n'; 
}
```
[Пример 21.1](#example211) использует функцию **`get_even_random_number()`**, которая должна возвращать четное случайное число. Она делает это довольно примитивным образом, вызывая функцию **`std::rand()`** из стандартной библиотеки. Если **`std::rand()`** генерирует четное случайное число, это число возвращается функцией **`get_even_random_number()`**. Если сгенерированное случайное число нечетное, то возвращается -1.

В этом примере, -1 означает, что ни одно четное случайное число не может быть сгенерированно. Таким образом, функция **`get_even_random_number()`** не может гарантировать, что даже случайное число возвращается. Возвращаемое значение не является обязательным. 

Многие функции используют специальные значения как -1, чтобы обозначить, что результат не может быть возвращен. Например, метод **`find()`** класса **`std::string`** возвращает специальное значение **`std::string::npos`**, если подстрока не найдена. Функции с возвращаемыми указателями обычно возвращают 0, чтобы показать, что никакого результата не существует. 
**`boost::optional`** обеспечивает Boost Optional, позволяющая четко определить необязательные возвращаемые значения. 

<a name="example212"></a>
`Пример 21.2. Необязательные возвращаемые значения при помощи boost::optional`
```c++
#include <boost/optional.hpp> 
#include <iostream>
#include <cstdlib> 
#include <ctime> 
#include <cmath> 

using boost::optional; 

optional<int> get_even_random_number() 
{  
  int i = std::rand();  
  return (i % 2 == 0) ? i : optional<int>{}; 
} 

int main() 
{  
  std::srand(static_cast<unsigned int>(std::time(0)));  
  optional<int> i = get_even_random_number();  
  if (i)    std::cout << std::sqrt(static_cast<float>(*i)) << '\n'; 
}
```

[Пример 21.2](#example212) возвращаемое значение функции **`get_even_random_number()`** имеет новый тип, **`boost::optional<int>`**. **`boost::optional`** -  шаблон, который должен быть инициализирован с фактическим типом возвращаемого значения. ***boost/optional.hpp*** должна быть включена в **`boost::optional`**. 

Если **`get_even_random_number()`** генерирует четное случайное число, значение возвращается напрямую, автоматически переведенное в объект типа **`boost::optional <int>`**, потому что **`boost::optional`** предоставляет неисключительнsq конструктор. Если **`get_even_random_number()`** не создает четное случайное число, возвращается пустой объект типа **`boost::optional <int>`**. Возвращаемое значение создается с помощью вызова конструктора по умолчанию.

**`main()`** проверяет, пуст ли он. Если не пуст, то номер, хранящийся в ***i*** осуществляется оператором **`boost::optional`**, который работает как указатель. Однако, вы не должны думать, что **`boost::optional`** является указателем, потому что, например, значения в **`boost::optional`** копируются конструктором копирования в то время, как указатель не копирует значение, на которое указывает.

<a name="example213"></a>
`Пример 21.3. Другие полезные функции-члены boost::optional`
```c++
#include <boost/optional.hpp> 
#include <iostream> 
#include <cstdlib> 
#include <ctime> 
#include <cmath>

using boost::optional;

optional<int> get_even_random_number() 
{
  int i = std::rand();  
  return optional<int>{i % 2 == 0, i}; 
}

int main() 
{  
  std::srand(static_cast<unsigned int>(std::time(0)));  
  optional<int> i = get_even_random_number();  
  if (i.is_initialized())    std::cout << std::sqrt(static_cast<float>(i.get())) << '\n'; 
}
```

[Пример 21.3](#example213) представляет другие полезные члены функции **`boost::optional`**. Этот класс предоставляет специальный конструктор, который принимает условие как первый параметр. Если у словие имеет значение ***true***, объект типа **`boost::optional`** инициализируется со вторым параметром. Если условие имеет значение ***false***, создается пустой объект типа **`boost::optional`**.  [Пример 21.3](#example213) использует этот конструктор в функции **`get_even_random_number()`**.

С **`is_initialized()`** вы можете проверить, является ли объект типа **`boost::optional`** пустым. Boost.Optional говорит о том, инициализированн или неинициализированн ли объект имени функции-члена **`is_initialized()`**. Функция **`get()`** член является эквивалентом **`operator*`**.

<a name="example214"></a>
`Пример 21.4. Различные вспомогательные функции Boost.Optional`
```c++
#include <boost/optional.hpp> 
#include <iostream> 
#include <cstdlib> 
#include <ctime> 
#include <cmath>

using namespace boost;

optional<int> get_even_random_number() 
{  
  int i = std::rand();    return make_optional(i % 2 == 0, i); 
}

int main()
{  
  std::srand(static_cast<unsigned int>(std::time(0)));  
o  optional<int> i = get_even_random_number(); 
  double d = get_optional_value_or(i, 0);  
  std::cout << std::sqrt(d) << '\n'; 
}
```
Boost.Optional предоставляет бесплатные постоянные вспомогательные функции, такие как **`boost:: make_optional()`** и **`boost:: get_optional_value_or()`** (см. [Пример 21.4](#example214)). **`boost:: make_optional()`** может быть вызван для создания объекта типа **`boost::optional`**. По умолчанию значение, возвращаемое при **`boost::optional`** пустое, но можно вызвать **`boost::get_optional_value_or()`**.

Функция **`boost:: get_optional_value_or()`** также предоставляется в качестве функциичлена **`boost::optional`**. Ee название - **`get_value_or()`**.

Наряду с ***boost/optional/optional_io.hpp*** Boost.Optional предоставляет файл заголовка с перегруженными потоками операторов, которые позволяют записывать объекты типа **`boost::optional`**, например, в стандартный вывод.

<a name="Boost.Any"></a>
##Глава №23 Boost.Any

Строго типизированные языки, например, ***C++***, требуют, чтобы каждая переменная имела конкретный тип, который определяет, какую информацию он может хранить. Другие языки, такие как ***JavaScript***, позволяют разработчикам хранить любую информацию в переменной. Например, в ***JavaScript*** одна переменная может содержать строку, число, а потом логическое значение. 
[Boost.Any](http://www.boost.org/doc/libs/1_62_0/doc/html/any.html) предоставляет класс **`boost::any`**, который, как и переменные ***JavaScript***, может хранить произвольные типы информации. 

<a name="example231"></a>
`Пример 23.1. Использование boost::any` 
 ```c++
#include <boost/any.hpp>

int main() 
{  
  boost::any a = 1;  
  a = 3.14;    a = true;
} 
```

Для использования **`boost::any`**, включите заголовочный файл ***boost/any.hpp***. Затем объекты типа **`boost.any`** могут быть созданы для хранения произвольных данных. В [примере 23.1](#example231) переменная является сначала ***int***, потом ***double***, а затем ***bool***.

Переменные типа **`boost::any`** являются ограниченными в том, чтоони могут хранить; Есть некоторые предварительные условия, не смотря на минимальные из них. Любое значение, хранящееся в переменной типа **`boost::any`** должно быть копируемо-конструированным. Таким образом невозможно хранить массив C, так как массивы C не копируемо-конструированные.

Чтобы сохранить строку, а не только указатель на строку C, используйте **`std::string`** (см. [Пример 23.2](#example232)).

<a name="example232"></a>
`Пример 23.2. Хранение string в boost::any`
```c++
#include <boost/any.hpp> 
#include <string>int 

main() 
{  
  boost::any a = std::string{"Boost"}; 
} 
```

Для доступа к значению **`boost::any`** переменных, используйте **`boost::any_cast`** оператор cast (см. [Пример 23.3](#example233))

<a name="example233"></a>
`Пример 23.3. Доступ к значениям при помощи boost::any_cast`
```c++ 
#include <boost/any.hpp> 
#include <iostream>

int main() 
{  
	boost::any a = 1;  
	std::cout << boost::any_cast<int>(a) << '\n';  
	a = 3.14;  
	std::cout << boost::any_cast<double>(a) << '\n';  
	a = true;  
	std::cout << std::boolalpha << boost::any_cast<bool>(a) << '\n'; 
} 
```

Путем передачи соответствующего типа в качестве параметра шаблона в **`boost::any_cast`**, преобразуется значение переменной. Если указан недопустимый тип, создается исключение типа **`boost::bad_any_cast`**.

<a name="example234"></a>
`Пример 23.4. boost::bad_any_cast в случае ошибки `	
```c++
#include <boost/any.hpp> 
#include <iostream>

int main() 
{ 
  try  
  {   
    boost::any a = 1;    
    std::cout << boost::any_cast<float>(a) << '\n';  
  }
  catch (boost::bad_any_cast &e)  
  {   
    std::cerr << e.what() << '\n';  
  } 
} 
```

[Пример 23.4](#example234) создает исключение так, как параметр шаблона типа ***float*** не соответствует типу ***int***. Программа также будет вызывать исключение, если ***short*** или ***long*** были использованы в качестве параметра шаблона.

Поскольку **`boost::bad_any_cast`** является производным от **`std::bad_cast`**, ***catch*** обработчики могут перехватывать исключения этого типа. 

Чтобы проверить, содержит ли переменная типа **`boost::any`** информацию, используйте функцию **`empty()`**. Чтобы проверить тип хранимой информации, используйте функцию **`type()`**.

<a name="example235"></a>
`Пример 23.5. Проверка типа хранимого значения`
```c++
#include <boost/any.hpp> 
#include <typeinfo> 
#include <iostream>

int main() 
{  
boost::any a = 1;  
if (!a.empty())  
  {    
    const std::type_info &ti = a.type();    
    std::cout << ti.name() << '\n';  
  } 
}
```

[Пример 23.5](#example235) использует **`empty()`** и **`type()`**. В то время как **`empty()`** возвращает логическое значение, возвращаемое значение **`type()`** имеет тип **`std::type_info`**, который определяется в заголовочном файле ***typeinfo***.

[Пример 23.6](#example236) показывает, как получить указатель на значение, хранящееся в переменной **`boost::any`** с помощью **`boost::any_cast`**.

<a name="example236"></a>
`Пример 23.6. Доступ к значениям через указатель`
```c++
#include <boost/any.hpp> 
#include <iostream>

int main() 
{
  boost::any a = 1;  
  int *i = boost::any_cast<int>(&a);  
  std::cout << *i << '\n'; 
} 
```

Вы просто передаете указатель на переменную **`boost::any`** для **`boost::any_cast`**; параметр шаблона останется неизменным.

<a name="Boost.Variant"></a>
##Глава №24 Boost.Variant

[Boost.Variant](http://www.boost.org/doc/libs/1_62_0/doc/html/variant.html) предоставляет класс с именем **`boost::variant`**, который напоминает союз. Значения различных типов можно хранить в переменной **`boost::variant`**.В один момент можно хранить только одно значение. При создании нового значения, старое значение перезаписывается. Но новое значение может иметь другой тип отличного от прежнего. Единственное требование заключается в том, чтобы типы значений были переданы в качестве параметров шаблона **`boost::variant`**, также известны, как **`boost::variant variable`**.

**`bost::variant`** поддерживает любой тип значения. Например, можно хранить **`std::string`** в **`boost::variant variable`**, хотя это было не возможно до ***C++ 11***. В ***C++ 11*** были смягчены требования к слиянию. В настоящее время слияние может содержать **`std::string`**. Поскольку **`std::string`** должен быть инициализирован с новым размещением и должен быть уничтожен в результате явного вызова деструктора, все еще может иметь смысл использовать **`boost::variant`**, даже в среде разработки ***C++ 11***. 

<a name="example241"></a>
`Пример 24.1. Использование boost::variant `
```c++
#include <boost/variant.hpp> 
#include <string>

int main() 
{  
  boost::variant<double, char, std::string> v;  
  v = 3.14;  
  v = 'A';  
  v = "Boost"; 
}
```

**`boost::variant`** определяется в ***boost/variant.hpp***. Поскольку **`boost::variant`** является шаблоном, необходимо указать хотя бы один параметр. Один или несколько параметров шаблона указывают на поддерживаемые типы. В [примере 24.1](#example241) ***v*** может хранить значения типа ***double***, ***char*** или ***string***. Однако, если вы пытались присвоить ***v*** значение типа ***int***, результирующий код не будет компилироваться.

<a name="example242"></a>
`Пример  24.2.`
```c++
#include <boost/variant.hpp>
#include <string>
#include <iostream>

int main()
{
  boost::variant<double, char, std::string> v;
  v = 3.14;
  std::cout << boost::get<double>(v) << '\n';
  v = 'A';
  std::cout << boost::get<char>(v) << '\n';
  v = "Boost";
  std::cout << boost::get<std::string>(v) << '\n';
}
```
Чтобы отобразить сохраненные значения v, используйте свободно стоящую функцию **`boost:: get()`** (см. [Пример 24.2](#example242)).

**`boost::get()`** ожидает один из типов переменной, допустимой для соответствующей переменной как параметр шаблона. Указание недопустимого типа приведет к ошибке во время выполнения, так как не происходит проверка типов во время компиляции. 

Переменные типа **`boost::variant`** могут быть написаны для потоков, таких, как стандартный выходной поток, минуя опасность ошибки времени выполнения (см. [Примем 24.3](#example243)).

<a name="example243"></a>
`Пример 24.3. Прямой выход из boost::variant в потоке `
```c++
include <boost/variant.hpp>
#include <string>
#include <iostream>

int main()
{
  boost::variant<double, char, std::string> v;
  v = 3.14;
  std::cout << v << '\n';
  v = 'A';
  std::cout << v << '\n';
  v = "Boost";
  std::cout << v << '\n';
}
```

Для безопасных типов доступа Boost.Variant предоставляет функцию с именем **`boost::apply_visitor()`**.

<a name="example244"></a>
`Example 24.4. Использование visitor для boost::variant`
```c++
#include <boost/variant.hpp>
#include <string>
#include <iostream>

struct output : public boost::static_visitor<>
{
  void operator()(double d) const { std::cout << d << '\n'; }
  void operator()(char c) const { std::cout << c << '\n'; }
  void operator()(std::string s) const { std::cout << s << '\n'; }
};

int main()
{
  boost::variant<double, char, std::string> v;
  v = 3.14;
  boost::apply_visitor(output{}, v);
  v = 'A';
  boost::apply_visitor(output{}, v);
  v = "Boost";
  boost::apply_visitor(output{}, v);
}
```

Как первый параметр, **`boost:: apply_visitor()`**  ожидает объект класса, производного от **`boost::static_visitor`**. Этот класс должен перегружать **`operator()`** для каждого типа переменной, используемой **`boost::variant`**, на которую она действует. Следовательно, оператор перегружен три раза в [Примере 24.4](#example244),  поскольку ***v*** поддерживает типы ***double***, ***char*** и ***string***.

**`boost::static_visitor`** является шаблоном. Тип возвращаемого значения **`operator()`** должен быть указан в качестве параметра шаблона. Если оператор не имеет возвращаемого значения, параметр шаблона не является обязательным, как показано в примере. 

Второй параметр, передаваемый для **`boost:: apply_visitor()`** является переменной **`boost::variant`**. 

**`boost::apply_visitor()`** автоматически вызывает **`operator()`** для первого параметра, который сравнивает тип значения, хранящегося во втором параметре. Это означает, что демонстрационная программа использует различные перегруженные операторы каждый раз, когда вызывается **`boost:: apply_visitor()`** – сначала ***double***,затем ***char*** и, наконец для ***string***. 

Преимуществом **`boost::apply_visitor()`** является не только правильный оператор, вызывающийся автоматически. Еще  **`boost::apply_visitor()`** гарантирует,что перегруженные операторы были предоставлены для каждого типа, поддерживаемого **`boost::variant`** переменных. Если один из трех перегруженных операторов не определен, код не может быть скомпилирован.

Если перегруженные операторы являются эквивалентными в функциональности, код можно упростить с помощью шаблона (см. [Пример 24.5](#example245)).

<a name="example245"></a>
`Пример 24.5Использование visitor с шаблонной функцией для boost::variant`
```c++
#include <boost/variant.hpp>
#include <string>
#include <iostream>

struct output : public boost::static_visitor<>
{
  template <typename T>
  void operator()(T t) const { std::cout << t << '\n'; }
};

int main()
{
  boost::variant<double, char, std::string> v;
  v = 3.14;
  boost::apply_visitor(output{}, v);
  v = 'A';
  boost::apply_visitor(output{}, v);
  v = "Boost";
  boost::apply_visitor(output{}, v);
}
```

Так как **`boost::apply_visitor()`** гарантирует правильность кода во время компиляции, он предпочтительней **`boost::get()`**. 


<a name="Boost.PropertyTree"></a>
##Глава №25 Boost.PropertyTree

**`boost::property_tree::ptree`** класса [Boost.PropertyTree](http://www.boost.org/doc/libs/1_62_0/doc/html/property_tree.html) предоставляет древовидную структуру для хранения пар ключ/значение. Древовидная структура означает, что ствол существует с многочисленными брэнчами, которые имеют много веток. Файловая система является хорошим примером древовидной структуры. Файловые системы имеют корневой каталог с подкаталогами, которые сами могут иметь подкаталоги и так далее.

Для использования **`boost::property_tree::ptree`**, включите ***boost/property_tree/ptree.hpp***. Это главный заголовочный файл, поэтому другие заголовочные файлы не нужны для Boost.PropertyTree

<a name="example251"></a>
`Пример 25.1. Доступ к данным в boost::property_tree::ptree `
```c++
#include <boost/property_tree/ptree.hpp>
#include <iostream>

using boost::property_tree::ptree;

int main()
{
  ptree pt;
  pt.put("C:.Windows.System", "20 files");

  ptree &c = pt.get_child("C:");
  ptree &windows = c.get_child("Windows");
  ptree &system = windows.get_child("System");
  std::cout << system.get_value<std::string>() << '\n';
}
```

[Пример 25.1](#example251) использует **`boost::property_tree::ptree`** для хранения пути к каталогу.Это делается с помощью вызова **`put()`**. Этот челен функции ожидает двух параметров, поскольку **`boost::property_tree::ptree`** является структурой дерева, которое сохраняет пары ключ/значение . Дерево не просто состоят из ветвей и бренчей, значение должно быть присвоено к каждой ветке и бренчу. В [примере 25,1](#example251) значение - «20 файлов».

Первый параметр, передаваемый **`put()`**, является наиболее интересным. Это путь к каталогу. Однако, он не использует ***backlash***, являющимся обычным разделителем пути на Windows. Он использует точку. 

Вам нужно использовать точку, потому что это Boost.PropertyTree предпологает разделитель для ключей. Параметр «C:. Windows.System» говорит ***pt*** создать ветвь под названием C: с бренчем под названием Windows, который имеет другую ветвь под названием System. Точка создает вложенную структуру ветвей. Если «C:\Windows\System» передан в качестве параметра, ***pt*** будет иметь только одна ветвь под названием C:\Windows\System. 

После вызова **`put()`** ***pt*** доступен для чтения сохраненного значения «20 файлов» и записать его в стандартный вывод. Это делается путем перепрыжки с ветки на ветку или в каталог. 

Для доступа к ***subbranch***, вы вызываете **`get_child()`** - возвращает ссылку на объект же типа **`get_child()`**, который был вызван. В [примере 25.1](#example251) - это ссылка на **`boost::property_tree::ptree`**. Так как каждая ветвь может иметь несколько ответвлений, и так как нет структурной разницы между высшей и низшей ветви, используется тот же тип. 

Третий вызов **`get_child()`** получает **`boost::property_tree::ptree`**, который представляет каталог System. **`get_value()`** вызывается для чтения значения, которое было сохранено в начале примера с **`put()`**.

Пожалуйста, обратите внимание, что **`get_value()`** является шаблон функции. Передайте тип возвращаемого значения в качестве параметра шаблона. Этот способ **`get_value()`** может сделать автоматическое преобразование типов.

<a name="example252"></a>
`Пример 25.2. Доступ к данным в basic_ptree < std::string, int >`
```c+
#include <boost/property_tree/ptree.hpp>
#include <utility>
#include <iostream>

int main()
{
  typedef boost::property_tree::basic_ptree<std::string, int> ptree;
  ptree pt;
  pt.put(ptree::path_type{"C:\\Windows\\System", '\\'}, 20);
  pt.put(ptree::path_type{"C:\\Windows\\Cursors", '\\'}, 50);

  ptree &windows = pt.get_child(ptree::path_type{"C:\\Windows", '\\'});
  int files = 0;
  for (const std::pair<std::string, ptree> &p : windows)
    files += p.second.get_value<int>();
  std::cout << files << '\n';
}
```

Есть два изменения в [примере 25.2](#example252), по сравнению с [примером 25.1](#example251). Это изменения созданы, чтобы сохранить пути к каталогам и количество файлов в них более легко. Во-первых, пути используют backslash в качестве разделителя при передаче **`put()`**. Во-вторых, число файлов хранится в виде целого числа ***int***.


По умолчанию, Boost.PropertyTree использует точку в качестве разделителя дляключей. Если вам нужно использовать другой символ, например backslash, как разделитель, не передать ключ как строка **`put()`**. Вместо этого вы оберните его в объект типа **`boost::property_tree::ptree::path_type`**. Конструктор этого класса, который зависит от **`boost::property_tree::ptree`**, принимает ключ в качестве первого параметра и знак разделителя в качестве второго параметра. Таким образом, можно использовать путь например C:\Windows\System, как показано в [примере 25.2](#example252), без необходимости замены backslash.

**`boost::property_tree::ptree`** основан на **`boost::property_tree::basic_ptree`** шаблона класса. Поскольку ключи и значения часто являются строками, **`boost::property_tree::ptree`** переопределен. Однако, можно использовать **`boost::property_tree::basic_ptree`** с различными типами для ключей и значений. Дерево в [примере 25.2](#example252) использует ***int*** для хранения числа файлов в каталоге, а не строку.

**`boost::property_tree::ptree`** предоставляет член функции **`begin()`** и **`end()`**. Однако **`boost::property_tree::ptree`** только позволяет выполнять итерацию над ветвью в одном уровне. [Пример 25.2](#example252) итерацию по подкаталогам C:\Windows. Вы не можете получить итератор для перебора всех ветвей на всех уровнях.

Цикл в [примере 25.2](#example252) считывает число файлов во всех подкаталогах C:\Windows для расчета в общей сложности. В результате в примере выводится ***70***. Пример не имеет прямой доступ к объектам типа ***ptree***. Вместо этого он выполняет итерацию элементов типа **`std::pair < std::string`**, **`ptree >`**. ***first*** содержит ключ текущей ветви. Это система и курсоры в [примере 25.2](#example252). ***second*** обеспечивает доступ к объекту ***ptree*** типа, который представляет возможные подкаталоги. В этом примере считываются только значения, назначенные для системы и курсоры. Как и в [примере 25.1](#example251) вызывается функция **`get_value()`**.

**`boost::property_tree::ptree`** хранит только значение текущей ветви,а не его ключ. Вы можете получить значение с **`get_value()`**, но нет никакой функции для получения ключа. Ключ хранится в **`boost::property_tree::ptree`** на один уровень вверх. Это также объясняет, почему цикл выполняет итерацию элементов типа **`std::pair < std::string`**, **`ptree >`**.

<a name="example253"></a>
`Пример 25.3. Доступ к данным с переводчиком`
```c++
#include <boost/property_tree/ptree.hpp>
#include <boost/optional.hpp>
#include <iostream>
#include <cstdlib>

struct string_to_int_translator
{
  typedef std::string internal_type;
  typedef int external_type;

  boost::optional<int> get_value(const std::string &s)
  {
    char *c;
    long l = std::strtol(s.c_str(), &c, 10);
    return boost::make_optional(c != s.c_str(), static_cast<int>(l));
  }
};

int main()
{
  typedef boost::property_tree::iptree ptree;
  ptree pt;
  pt.put(ptree::path_type{"C:\\Windows\\System", '\\'}, "20 files");
  pt.put(ptree::path_type{"C:\\Windows\\Cursors", '\\'}, "50 files");

  string_to_int_translator tr;
  int files =
    pt.get<int>(ptree::path_type{"c:\\windows\\system", '\\'}, tr) +
    pt.get<int>(ptree::path_type{"c:\\windows\\cursors", '\\'}, tr);
  std::cout << files << '\n';
}
```

[Пример 25.3](#example253) использует **`boost::property_tree::iptree`** другого предопределенного дерева из Boost.PropertyTree. В общем этот тип ведет себя как **`boost::property_tree::ptree`**. Единственное отличие состоит в том что **`boost::property_tree::iptree`** не различает нижний и верхний регистр. Например значение, хранящееся с ключом C:\Windows\System можно читать с c:\windows\system.

В отличие от [примера 25.1](#example251) **`get_child()`** не вызывается несколько раз для доступа отделений. Так же, как **`put()`** может использоваться для хранения значения непосредственно в подветвях, значение из подветви можно читать с **`get()`**. Ключ определяется так же, как – например, с помощью **`boost::property_tree::iptree::path_type`**.

Как и  **`get_value()`** **`get()`**, является шаблоном функции. Вы должны передать тип возвращаемого значения в качестве параметра шаблона. Boost.PropertyTree выполняет автоматическое преобразование типов.

[Пример 25.3](#example253) определяет переводчик **`string_to_int_translator`**, который преобразует значение типа ***std::string*** в ***int***. Переводчик передается как дополнительный параметр **`get()`**. Потому что переводчик используется только для чтения, он определяет только одну функцию-член, **`get_value()`**. Если вы хотите использовать переводчик для записи, то будет необходимо определить член функции **`put_value()`** и затем передать переводчик в качестве дополнительного параметра **`put()`**.

**`get_value()`** возвращает значение типа, который используется в ***pt***. Однако поскольку преобразование типов не всегда справляются, используется **`boost::optional`**. Если значение хранится в [примере 25.3](#example253), который нельзя преобразовать в ***int*** с **`std::strtol()`**,возвращается пустой объект типа **`boost::optional`**.

Пожалуйста, обратите внимание на то, что переводчик должен также определить два типа ***internal_type*** и ***external_type***. Если вам нужно конвертировать типы при хранении данных, определите **`put_value()`**, похожий на **`get_value()`**.

Если изменить [пример 25.3](#example253) для хранения значения «20» вместо значения «20 файлов», **`get_value()`** может быть вызван без прохождения переводчика. Переводчики, предоставляемые Boost.PropertyTree могут конвертировать из ***std::string*** в ***int***. Однако, преобразование типа завершается успешно, только если вся строка может быть преобразованна. Строка не должна содержать буквы. Так как **`std::strtol()`** может сделать преобразование типов до тех пор, пока строка начинается с цифры, более обильный переводчик string_to_int_translator используется в [примере 25.3](#example253).

<a name="example254"></a>
`Пример 25.4. Различные члены функций boost::property_tree::ptree`
```c++
#include <boost/property_tree/ptree.hpp>
#include <utility>
#include <iostream>

using boost::property_tree::ptree;

int main()
{
  ptree pt;
  pt.put("C:.Windows.System", "20 files");

  boost::optional<std::string> c = pt.get_optional<std::string>("C:");
  std::cout << std::boolalpha << c.is_initialized() << '\n';

  pt.put_child("D:.Program Files", ptree{"50 files"});
  pt.add_child("D:.Program Files", ptree{"60 files"});

  ptree d = pt.get_child("D:");
  for (const std::pair<std::string, ptree> &p : d)
    std::cout << p.second.get_value<std::string>() << '\n';

  boost::optional<ptree&> e = pt.get_child_optional("E:");
  std::cout << e.is_initialized() << '\n';
}
```

Можно вызвать функцию **`get_optional()`**, если вы хотите прочитать значение ключа, но вы не уверены, cуществует ли ключ. **`get_optional()`** возвращает значение в объект типа **`boost::optional`**. Если объект является пустым, значит ключ не был найден. В противном случае **`get_optional()`** работает так же, как **`get()`**.

Может показаться, что **`put_child()`** и **`add_child()`** такие же, как **`put()`**. Разница состоит в том что **`put()`** создает только пару ключ/значение, в то время как **`put_child()`** и **`add_child()`** вставляют все поддерево. Обратите внимание, что объект типа **`boost::property_tree::ptree`** передается как второй параметр **`put_child()`** и **`add_child()`**.

Разница между **`put_child()`** и **`add_child()`** в том, что **`put_child()`** обращается к ключу, если ключ уже существует, в то время как **`add_child()`** всегда вставляет новый ключ в дерево. Вот почему дерево в [примере 25.4](#example254) имеет два ключа, под названием «D:. Program Files». В зависимости от варианта использования это может ввести в заблуждение. Если дерево представляет файловую систему, не должно быть двух одинаковых путей. Вы должны избегать вставки одинаковых ключей,если вы не хотите дубликаты в дереве.

[Пример 25.4](#example254) отображает значение ключей ниже «D:» в цикле ***for***. Пример записи файлов: 50 и 60 файлов на стандартный вывод, что доказывает, есть два одинаковых ключей под названием «D:. Program Files».

Последняя функция-член, в [примере 25.4](#example254) является **`get_child_optional()`**. Эта функция используется как **`get_child()`**. **`get_child_optional()`** возвращает объект типа **`boost::optional`**. Вы вызываете **`boost::optional`**, если вы не уверены в том, существует ли ключ.

<a name="example255"></a>
`Пример 25.5. Сериализация boost::property_tree::ptree в формате JSON`
```c++
#include <boost/property_tree/ptree.hpp>
#include <boost/property_tree/json_parser.hpp>
#include <iostream>

using namespace boost::property_tree;

int main()
{
  ptree pt;
  pt.put("C:.Windows.System", "20 files");
  pt.put("C:.Windows.Cursors", "50 files");

  json_parser::write_json("file.json", pt);

  ptree pt2;
  json_parser::read_json("file.json", pt2);

  std::cout << std::boolalpha << (pt == pt2) << '\n';
}
```

Boost.PropertyTree больше, чем просто обеспечивают структуры для управления данными в памяти. Как видно в [примере 25.5](#example255), библиотека также предоставляет функции для сохранения **`boost::property_tree::ptree`** в файл и загрузить его из файла.

***Boost/property_tree/json_parser.hpp*** обеспечивает доступ к функции **`boost::property_tree::json_parser::write_json()`** и **`boost::property_tree::json_parser::read_json()`**. Эти функции позволяют сохранять и загружать **`boost::property_tree::ptree`** сериализации в JSON формате. Таким образом, вы можете поддержатьфайлы конфигурации в формате JSON.

Если вы хотите вызвать функции, которые хранят **`boost::property_tree::ptree`** в файле или загрузить его из файла, необходимо включить файлы заголовков, такие как ***boost/property_tree/json_parser.hpp***. Не достаточно только включить ***boost/property_tree/ptree.hpp***.

В дополнение к функции **`boost::property_tree::json_parser::write_json()`** и **`boost::property_tree::json_parser::read_json()`** Boost.PropertyTree предоставляет функции для дополнительных форматов данных. Вы используете **`boost::property_tree::ini_parser::write_ini()`** и **`boost::property_tree::ini_parser::read_ini()`** от ***boost/property_tree/ini_parser.hpp*** для поддержки INI-файлов. С **`boost::property_tree::xml_parser::write_xml()`** и **`boost::property_tree::xml_parser::read_xml()`** из ***boost/property_tree/xml_parser.hpp*** данные можно загрузить, и они хранятся в формате XML. С **`boost::property_tree::info_parser::write_info()`** и **`boost::property_tree::info_parser::read_info()`** из ***boost/property_tree/info_parser.hpp*** можно получить доступ к другому формату, который был разработан и оптимизирован для реализации деревьев из Boost.PropertyTree.

Ни один из поддерживаемых форматов не гарантирует, что **`boost::property_tree::ptree`** будет выглядеть так же после сохранения и перезагрузки. Например формат JSON может потерять сведения о типе, потому что **`boost::property_tree::ptree`** не может различать  ***true*** и ***«true»***. Тип всегда остается тем же. Основное внимание библиотеки находится на структуре ***boost::property_tree::ptree***, а не на поддержку различных форматов данных.

<a name="Boost.Tribool"></a>
##Глава №27 Boost.Tribool

Библиотека [Boost.Tribool]() предоставляет класс **`boost::logic::tribool`**, который похож на ***bool***. Однако в то время как ***bool*** может обработать два оператора, **`boost::logic::tribool`** обрабатывает три.
Для использования **`boost::logic::tribool`**, включите ***boost/logic/tribool.hpp***. 

<a name="example271"></a>
`Пример 27.1. Три утверждения  boost::logic::tribool`
```c++
#include <boost/logic/tribool.hpp>
#include <iostream>

using namespace boost::logic;

int main()
{
  tribool b;
  std::cout << std::boolalpha << b << '\n';

  b = true;
  b = false;
  b = indeterminate;
  if (b)
    ;
  else if (!b)
    ;
  else
    std::cout << "indeterminate\n";
}
```

Переменной типа **`boost::logic::tribool`** может быть присвоено значение ***true***, ***false*** или ***indeterminate***. Конструктор по умолчанию инициализирует переменную в значение ***false***. Вот почему [пример 27.1](#example271) сначала записывает значение ***false***. 

Оператор ***if*** в [примере 27.1](#example271) показывает, значение ***b***. Вы должны проверить его для ***true*** и ***false***. Если переменная имеет значение ***indeterminate***, как показано в примере, будет выполнен блок ***else***.

Boost.Tribool также предоставляет функцию **`boost::logic::indeterminate()`**. При передаче переменной типа **`boost::logic::tribool`**, имеющее значение ***indeterminate***, эта функция возвращает ***true***. Если переменная имеет значение ***true*** или ***false***, он будет возвращать значение ***false***. 

<a name="example272"></a>
`Пример 27.2. Логические операции с boost::logic::tribool`
```c++
#include <boost/logic/tribool.hpp>
#include <boost/logic/tribool_io.hpp>
#include <iostream>

using namespace boost::logic;

int main()
{
  std::cout.setf(std::ios::boolalpha);

  tribool b1 = true;
  std::cout << (b1 || indeterminate) << '\n';
  std::cout << (b1 && indeterminate) << '\n';

  tribool b2 = false;
  std::cout << (b2 || indeterminate) << '\n';
  std::cout << (b2 && indeterminate) << '\n';

  tribool b3 = indeterminate;
  std::cout << (b3 || b3) << '\n';
  std::cout << (b3 && b3) << '\n';
}
```

Логические операторы можно использовать с переменными типа **`boost::logic::tribool`**, так же, как и с переменными типа ***bool***. На самом деле это единственный способ для переменных типа **`boost::logic::tribool`** потому, что класс не предоставляет каких-либо членов функций. 

[Пример 27.2](#example272) возвращает ***true*** для ***b1 || indeterminate***, значение ***false*** для ***b2 && неопределенный*** и ***indeterminate*** во всех остальных случаях. Если вы посмотрите на операции и их результаты, вы заметите, что **`boost::logic::tribool`** ведет себя, как можно было бы ожидать интуитивно. Документация на Boost.Tribool также содержит таблицы, показывающие, какие операции приводят к каким результатам. 

[Пример 27.2](#example272) также показывает, как значения ***true***, ***false*** и ***indeterminate*** записываются в стандартный вывод с переменными типа **`boost::logic::tribool`**. ***boost/logic/tribool_io.hpp***  должен быть включен и установлен флаг ***std::ios::boolalpha*** для стандартного вывода.

Boost.Tribool также предоставляет макрос ***BOOST_TRIBOOL_THIRD_STATE***, который позволяет вам заменить другое значение ***indeterminate***. Например можно использовать ***dontknow*** вместо ***indeterminate***.
