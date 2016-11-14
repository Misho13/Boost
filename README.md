#Boost 
##Содержание 
- [Глава №21](##Глава-№21-Boost.Optional)
- [Глава №23](##Глава-№23-Boost.Any)
- [Глава №24](##Глава-№24-Boost.Variant)
- [Глава №25](##Глава-№25-Boost.PropertyTree)
- [Глава №27](##Глава-№27-Boost.Tribool)


##Глава №21 Boost.Optional
Библиотека [Boost.Optional](http://www.boost.org/doc/libs/1_62_0/libs/optional/doc/html/index.html)  предоставляет класc **`boost::optional`**, который может быть использован для дополнительных возвращаемых значений. Эти возвращаемые значения функций могут не всегда возвращать результат. [Пример 21.1](#example211) показывает, как необязательные возвращаемые значения обычно реализуются без Boost.Optional. 

<a name="example211"></a>
`Пример 21.1 Специальные значения для указания необязательных возвращаемых значений.`
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
if (i != -1)    
std::cout << std::sqrt(static_cast<float>(i)) << '\n'; 
}
```
[Пример 21.1](#example211) использует функцию **`get_even_random_number()`**, которая должна возвращать четное случайное число. Она делает это довольно примитивным образом, вызывая функцию **`std::rand()`** из стандартной библиотеки. Если **`std::rand()`** генерирует четное случайное число, это число возвращается функцией **`get_even_random_number()`**. Если сгенерированное случайное число нечетное, то возвращается -1.

В этом примере, -1 означает, что ни одно четное случайное число не может быть сгенерированно. Таким образом, функция **`get_even_random_number()`** не может гарантировать, что даже случайное число возвращается. Возвращаемое значение не является обязательным. 

Многие функции используют специальные значения как -1, чтобы обозначить, что результат не может быть возвращен. Например, метод **`find()`** класса **`std::string`** возвращает специальное значение **`std::string::npos`**, если подстрока не найдена. Функции с возвращаемыми указателями обычно возвращают 0, чтобы показать, что никакого результата не существует. 
**`boost::optional`** обеспечивает Boost Optional, позволяющая четко определить необязательные возвращаемые значения. 

<a name="example212"></a>
`Пример 21.2 Необязательные возвращаемые значения при помощи **`boost::optional `**`
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
##Глава №23 Boost.Any
##Глава №24 Boost.Variant
##Глава №25 Boost.PropertyTree
##Глава №27 Boost.Tribool

