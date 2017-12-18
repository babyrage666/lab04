## Laboratory work IV

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab04** на сервисе **GitHub**
- [X] 2. Ознакомиться со ссылками учебного материала
- [X] 3. Выполнить инструкцию учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=DespiteDeath # Экспортируем пользователя
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab03 lab04 # Клонируем репозиторий
$ cd lab04 # Перемещаемся в папку lab04
$ git remote remove origin # Удаляем ссылку
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab04 # Добавляем ссылку
```

```ShellSession
$ g++ -I./include -std=c++11 -c sources/print.cpp # Компилируем с помощью g++
$ ls print.o # Проверяем существование
$ ar rvs print.a print.o # Архивируем r-Replace v-Provide verbose output s-Write an object-file index into the archive 
$ file print.a # Определяем тип файла
$ g++ -I./include -std=c++11 -c examples/example1.cpp # Компилируем с помощью g++
$ ls example1.o # Проверяем существование
$ g++ example1.o print.a -o example1 # Компилируем с помощью g++
$ ./example1 && echo # Выводим
```

```ShellSession
$ g++ -I./include -std=c++11 -c examples/example2.cpp # Компилируем с помощью g++
$ ls example2.o # Проверяем существование
$ g++ example2.o print.a -o example2 # Компилируем с помощью g++
$ ./example2 # Запускаем скомпилированный файл
$ cat log.txt && echo # Связываем файлы и выводим
```

Удаляем рекурсивно 

```ShellSession
$ rm -rf example1.o example2.o print.o 
$ rm -rf print.a 
$ rm -rf example1 example2 
$ rm -rf log.txt 
```

Запись в файл CMakeLists.txt (добавляем cmake_minimum_required VERSION)

```ShellSession
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.0)
project(print)
EOF
```

Запись в файл CMakeLists.txt (добавляем cmake standart)

```ShellSession
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

Запись в файл CMakeLists.txt (add library)

```ShellSession
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)
EOF
```

Запись в файл CMakeLists.txt

```ShellSession
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

Этап генерации проекта (-H. == path; . == pwd)  (-B выходной путь (куда поместит все промеж. файлы))

```ShellSession
$ cmake -H. -B_build
$ cmake --build _build
```

Запись в файл CMakeLists.txt (добавляем add executable)

```ShellSession
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

Запись в файл CMakeLists.txt (добавляем target link libraries)

```ShellSession
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

Этап построения  

```ShellSession
$ cmake --build _build
$ cmake --build _build --target print
$ cmake --build _build --target example1
$ cmake --build _build --target example2
```

```ShellSession
$ ls -la _build/libprint.a # Проверка существования
$ _build/example1 && echo # Построение и вывод example1
hello
$ _build/example2 # Построение example2
$ cat log.txt && echo # Связывание файлов и стандартный вывод 
hello
```

```ShellSession
$ git clone https://github.com/tp-labs/lab04 tmp # Клонируем директорию
$ mv -f tmp/CMakeLists.txt . # Перемещение кроме CMakeLists.txt
$ rm -rf tmp # Удалить рекурсивно 
```

```ShellSession
$ cat CMakeLists.txt # Создаем CMakeLists.txt
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install # Этап построения (h == path; . == pwd)(-B выходной путь поместит промеж. файлы)) 
$ cmake --build _build --target install # Этап построения, --target (опиция (что то конкретное))
$ tree _install # Устанавливаем дерево
```

```ShellSession
$ git add CMakeLists.txt # Добавляем CMakeLists.txt
$ git commit -m"added CMakeLists.txt" # Коммитим
$ git push origin master # Пушим
```

## Report

```ShellSession
$ cd ~/workspace/labs/
$ export LAB_NUMBER=04
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2017 Братья Вершинины
```
