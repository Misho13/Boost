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
#include <boost/optional.hpp> #include <iostream> 
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

<a name="example232"></a>
`Пример 23.2. Хранение string в boost::any`
```c++
#include <boost/any.hpp> 
#include <string>int main() 
{  
  boost::any a = std::string{"Boost"}; 
} 
```

Для доступа к значению **`boost::any`** переменных, используйте **`boost::any_cast`** оператор cast (см. [Пример 23.3](#example233))

<a name="example233"></a>
`Пример 23.3. Доступ к значениям при помощи boost::any_cast`
```c++ `#include <boost/any.hpp> 
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
<a name="Boost.Variant"></a>
##Глава №24 Boost.Variant
<a name="Boost.PropertyTree"></a>
##Глава №25 Boost.PropertyTree
<a name="Boost.Tribool"></a>
##Глава №27 Boost.Tribool

