# Homework
Представьте, что вы стажер в компании "Formatter Inc.".

# Задание 1
Вам поручили перейти на систему автоматизированной сборки CMake. Исходные файлы находятся в директории formatter_lib. В этой директории находятся файлы для статической библиотеки formatter. Создайте CMakeList.txt в директории formatter_lib, с помощью которого можно будет собирать статическую библиотеку formatter.
```
$ cmake --version
cmake version 3.20

CMake suite maintained and supported by Kitware (kitware.com/cmake).
$ cd ./formatter_lib/
$ cmake -H. -B_build
$ cmake --build ./_build
```
Вывод:
```
-- The C compiler identification is GNU 12.2.0
-- The CXX compiler identification is GNU 12.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/debain/polyndrap/workspace/projects/lab03/formatter_lib/_build
```

```
$ cd ../formatter_ex_lib/
$ cmake -H. -B_build
$ cmake --build ./_build
$  g++ -c formatter_ex.cpp -o formatter_ex.o
$  ar rcs libformatter_ex.a formatter_ex.o
```
# Содержимое CMakeLists.txt:

```
cmake_minimum_required(VERSION 3.25)

project(formatter_ex)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Установим путь к директории formatter_ex_lib
set(CMAKE_CURRENT_SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/formatter_ex_lib)

# Явно перечислим исходные файлы
set(SOURCES
    formatter_ex.cpp
)

# Создадим библиотеку formatter_ex
add_library(formatter_ex STATIC ${SOURCES})

# Установим директорию для поиска заголовочных файлов
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/formatter_lib)

# Добавим зависимость от библиотеки formatter
target_link_libraries(formatter_ex formatter)
```
# Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек. Чтобы продемонстрировать как работать с библиотекой formatter_ex, вам необходимо создать два CMakeList.txt для двух простых приложений:

hello_world, которое использует библиотеку formatter_ex; solver, приложение которое испольует статические библиотеки formatter_ex и solver_lib.

1. Hello_world_application
```
$ cd ../hello_world_application
$ cmake -H. -B_build
$ cmake --build _build
```
Содержимое CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.25)
									 # Если версия установленой программы
									 # старее указаной, произайдёт аварийный выход.

project(hello_world)				 # Название проекта

set(SOURCE_EXE hello_world.cpp)			 # Установка переменной со списком исходников

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/)

add_executable(main ${SOURCE_EXE})	 # Создает исполняемый файл с именем main

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/ formatter_ex_lib)

target_link_libraries(main formatter_ex_lib)		 # Линковка программы с библиотекой
```
Вывод:
-- The C compiler identification is GNU 12.2.0
-- The CXX compiler identification is GNU 12.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/debain/polyndrap/workspace/projects/lab03/hello_world_application/_build
2. Solver_application
```
$ cd ../solver_lib/
$ cmake -H. -B_build
$ cmake --build _build
```
CMakeLists.txt
```
cmake_minimum_required(VERSION 3.25)

project(solver)				# Название проекта

set(SOURCE_EXE equation.cpp)			# Установка переменной со списком исходников

include_directories("${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/"
					"${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/")

add_executable(main ${SOURCE_EXE})	# Создает исполняемый файл с именем main

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../formatter_ex_lib/ formatter_ex_lib)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/../solver_lib/ solver_lib)

target_link_libraries(main formatter_ex_lib solver_lib)		# Линковка программы с библиотекой
```
Вывод
-- The C compiler identification is GNU 12.2.0
-- The CXX compiler identification is GNU 12.2.0
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/debain/polyndrap/workspace/projects/lab03/hello_world_application/_build

