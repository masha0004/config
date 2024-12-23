### Задача 1
Написать программу на Питоне, которая транслирует граф зависимостей civgraph в makefile в духе примера выше. Для мало знакомых с Питоном используется упрощенный вариант civgraph: civgraph.json.

Пример:

> make mathematics
mining
bronze_working
sailing
astrology
celestial_navigation
pottery
writing
code_of_laws
foreign_trade
currency
irrigation
masonry
early_empire
mysticism
drama_poetry
mathematics

```bash
import json

# Загрузка графа из JSON файла
def load_civgraph(file):
    with open(file, 'r') as f:
        return json.load(f)

# Функция для генерации Makefile
def generate_makefile(civgraph, target):
    visited = set()  # Для отслеживания посещенных задач
    result = []

    # Рекурсивная функция обхода зависимостей
    def visit(tech):
        if tech in visited:
            return
        visited.add(tech)
        for dep in civgraph.get(tech, []):
            visit(dep)
        result.append(tech)

    # Посещаем целевую технологию
    visit(target)

    # Печатаем задачи в порядке их выполнения
    for task in result:
        print(task)

if __name__ == '__main__':
    civgraph = load_civgraph('civgraph.json')
    target = input('Enter the target technology: ')  # Например, mathematics
    generate_makefile(civgraph, target)
```

Вывод
```bash
> python3 civ_to_make.py
Enter the target technology: mathematics
mining
bronze_working
sailing
astrology
celestial_navigation
pottery
writing
code_of_laws
currency
irrigation
masonry
early_empire
mysticism
drama_poetry
mathematics
```

### Задача 2
Реализовать вариант трансляции, при котором повторный запуск make не выводит для civgraph на экран уже выполненные "задачи".

```bash
import json
import os

# Файл для сохранения выполненных задач
COMPLETED_TASKS_FILE = "completed_tasks.txt"

# Загрузка списка выполненных задач из файла
def load_completed_tasks():
    if os.path.exists(COMPLETED_TASKS_FILE):
        with open(COMPLETED_TASKS_FILE, 'r') as f:
            return set(f.read().splitlines())
    return set()

# Сохранение выполненных задач в файл
def save_completed_tasks(completed_tasks):
    with open(COMPLETED_TASKS_FILE, 'w') as f:
        f.write('\n'.join(completed_tasks))

# Функция генерации Makefile с проверкой на уже выполненные задачи
def generate_makefile(civgraph, target):
    visited = set()
    result = []
    completed_tasks = load_completed_tasks()

    def visit(tech):
        if tech in visited or tech in completed_tasks:
            return
        visited.add(tech)
        for dep in civgraph.get(tech, []):
            visit(dep)
        result.append(tech)

    visit(target)

    for task in result:
        if task not in completed_tasks:
            print(task)
            completed_tasks.add(task)

    save_completed_tasks(completed_tasks)

if __name__ == '__main__':
    civgraph = load_civgraph('civgraph.json')
    target = input('Enter the target technology: ')
    generate_makefile(civgraph, target)
```


### Задача 3
Добавить цель clean, не забыв и про "животное".

```bash 
import json
import os

COMPLETED_TASKS_FILE = "completed_tasks.txt"

def load_completed_tasks():
    if os.path.exists(COMPLETED_TASKS_FILE):
        with open(COMPLETED_TASKS_FILE, 'r') as f:
            return set(f.read().splitlines())
    return set()

def save_completed_tasks(completed_tasks):
    with open(COMPLETED_TASKS_FILE, 'w') as f:
        f.write('\n'.join(completed_tasks))

def clean():
    if os.path.exists(COMPLETED_TASKS_FILE):
        os.remove(COMPLETED_TASKS_FILE)
        print("Cleaned completed tasks.")

def generate_makefile(civgraph, target):
    visited = set()
    result = []
    completed_tasks = load_completed_tasks()

    def visit(tech):
        if tech in visited or tech in completed_tasks:
            return
        visited.add(tech)
        for dep in civgraph.get(tech, []):
            visit(dep)
        result.append(tech)

    visit(target)

    for task in result:
        if task not in completed_tasks:
            print(task)
            completed_tasks.add(task)

    save_completed_tasks(completed_tasks)

if __name__ == '__main__':
    civgraph = load_civgraph('civgraph.json')
    action = input('Enter action (make/clean): ')

    if action == 'clean':
        clean()
    else:
        target = input('Enter the target technology: ')
        generate_makefile(civgraph, target)
```

### Задача 4
Написать makefile для следующего скрипта сборки:

gcc prog.c data.c -o prog
dir /B > files.lst
7z a distr.zip *.*

Вместо gcc можно использовать другой компилятор командной строки, но на вход ему должны подаваться два модуля: prog и data. Если используете не Windows, то исправьте вызовы команд на их эквиваленты из вашей ОС. В makefile должны быть, как минимум, следующие задачи: all, clean, archive. Обязательно покажите на примере, что уже сделанные подзадачи у вас не перестраиваются.

```bash
CC=gcc  # Или другой компилятор, например, clang
CFLAGS=-o prog
SRC=prog.c data.c
ARCHIVE=distr.zip

# Цель по умолчанию
all: prog files.lst archive

# Компиляция программы
prog: $(SRC)
	$(CC) $(SRC) $(CFLAGS)

# Создание списка файлов
files.lst:
	dir /B > files.lst  # В Windows используется команда dir. Для Linux: ls > files.lst

# Архивация проекта
archive: prog files.lst
	7z a $(ARCHIVE) *.*

# Очистка проекта
clean:
	del prog.exe files.lst $(ARCHIVE)  # В Windows используется команда del. Для Linux: rm -f prog files.lst $(ARCHIVE)
```
