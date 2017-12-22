the demo application redirects data from stdin to a file **log.txt** using a package **print**.
## Laboratory work XI

Данная лабораторная работа посвещена изучению компонентов **Boost** на примере `program_options`

```ShellSession
$ open http://www.boost.org/doc/libs/1_65_0/doc/html/program_options.html
```

## Tasks

- [X] 1. Создать публичный репозиторий с названием **lab11** на сервисе **GitHub**
- [X] 2. Выполнить инструкцию учебного материала
- [X] 3. Ознакомиться со ссылками учебного материала
- [X] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
$ alias edit=<nano|vi|vim|subl>
$ alias gsed=sed # for *-nix system
```
Активация переменных окружения
```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .
$ source scripts/activate
```
Клонируем репозиторий предыдущей лабораторной
```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab10 projects/lab11
$ cd projects/lab11
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab11
```
Настройка boost в CMakeLists.txt 
```ShellSession
# boost::program_options
$ edit CMakeLists.txt
$ edit sources/demo.cpp # записываем код с program_options
```
Сборка проекта
```ShellSession
$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
$ cmake --build _build --target install
$ mkdir artifacts && cd artifacts
```
Тест
```ShellSession
$ echo "text1 text2 text3" | ../_install/bin/demo
$ test -f default.log
$ echo $?
0
```
Тест с конфиг-файлом
```ShellSession
$ mkdir ${HOME}/.config
$ echo "name=config.log" > ${HOME}/.config/demo.cfg
$ echo "text1 text2 text3" | ../_install/bin/demo
$ test -f config.log
$ echo $?
0
```
Тест на чтение из переменных окружения
```ShellSession
$ export DEMO_OUTPUT=env.log
$ echo "text1 text2 text3" | ../_install/bin/demo
$ test -f env.log
$ echo $?
0
```
Тест с передачей аргумента в командной строке
```ShellSession
$ echo "text1 text2 text3" | ../_install/bin/demo --output arg.log
$ test -f arg.log
$ echo $?
0
```
Написание инструкций в travis
```ShellSession
$ cat >> .travis.yml <<EOF
- cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install
- cmake --build _build --target install
- mkdir artifacts && cd artifacts
- echo "text1 text2 text3" | ../_install/bin/demo
- test -f default.log
- mkdir ${HOME}/.config
- echo "output=config.log" > ${HOME}/.config/demo.cfg
- echo "text1 text2 text3" | ../_install/bin/demo
- test -f config.log
- export DEMO_OUTPUT=env.log
- echo "text1 text2 text3" | ../_install/bin/demo
- test -f env.log
- echo "text1 text2 text3" | ../_install/bin/demo --output arg.log
- test -f arg.log
EOF
```
Смена названия
```ShellSession
$ gsed -i 's/lab10/lab11/g' README.md
```
Коммитим и пуш изменений
```ShellSession
$ git add .
$ git commit -m"changed format output"
$ git push origin master
```
Подключение travis
```ShellSession
$ travis login --auto
$ travis enable
```

## Report

```ShellSession
$ popd
$ export LAB_NUMBER=11
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}
$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Links

- [String Algorithms](http://www.boost.org/doc/libs/1_65_0/doc/html/string_algo.html)
- [Date Time](http://www.boost.org/doc/libs/1_65_0/doc/html/date_time.html)
- [DLL](http://www.boost.org/doc/libs/1_65_0/doc/html/boost_dll.html)
- [Heap](http://www.boost.org/doc/libs/1_65_0/doc/html/heap.html)
- [Interprocess](http://www.boost.org/doc/libs/1_65_0/doc/html/interprocess.html)
- [Lockfree](http://www.boost.org/doc/libs/1_65_0/doc/html/lockfree.html)
- [Lexicalcast](http://www.boost.org/doc/libs/1_65_0/doc/html/boost_lexical_cast.html)
- [Property Tree](http://www.boost.org/doc/libs/1_65_0/doc/html/property_tree.html)

```
Copyright (c) 2017 Братья Вершинины
```
