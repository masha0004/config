### Задача 1
Исследование виртуальной стековой машины CPython.
Изучите возможности просмотра байткода ВМ CPython.

```bash
import dis

def foo(x):
    while x:
        x -= 1
    return x + 1

print(dis.dis(foo))
```
Опишите по шагам, что делает каждая из следующих команд (приведите эквивалентное выражение на Python):

```bash
11   0 LOAD_FAST                0 (x)
     2 LOAD_CONST               1 (10)
     4 BINARY_MULTIPLY
     6 LOAD_CONST               2 (42)
     8 BINARY_ADD
    10 RETURN_VALUE
```

0 LOAD_FAST 0 (x)

    Действие: Загружает значение локальной переменной x в стек.
    Стек после операции: [x]
    Эквивалент на Python: x

2 LOAD_CONST 1 (10)

    Действие: Загружает константу 10 в стек.
    Стек после операции: [x, 10]
    Эквивалент на Python: 10

4 BINARY_MULTIPLY

    Действие: Умножает два верхних значения на стеке и кладет результат обратно в стек.
    Операция: x * 10
    Стек после операции: [x * 10]
    Эквивалент на Python: x * 10

6 LOAD_CONST 2 (42)

    Действие: Загружает константу 42 в стек.
    Стек после операции: [x * 10, 42]
    Эквивалент на Python: 42

8 BINARY_ADD

    Действие: Складывает два верхних значения на стеке и кладет результат обратно в стек.
    Операция: (x * 10) + 42
    Стек после операции: [x * 10 + 42]
    Эквивалент на Python: x * 10 + 42

10 RETURN_VALUE

    Действие: Возвращает верхнее значение из стека как результат функции.
    Эквивалент на Python: return x * 10 + 42

```bash
def foo(x):
    return x * 10 + 42
```

### Задача 2
Что делает следующий байткод (опишите шаги его работы)? Это известная функция, назовите ее.
```bash
  5           0 LOAD_CONST               1 (1)
              2 STORE_FAST               1 (r)

  6     >>    4 LOAD_FAST                0 (n)
              6 LOAD_CONST               1 (1)
              8 COMPARE_OP               4 (>)
             10 POP_JUMP_IF_FALSE       30

  7          12 LOAD_FAST                1 (r)
             14 LOAD_FAST                0 (n)
             16 INPLACE_MULTIPLY
             18 STORE_FAST               1 (r)

  8          20 LOAD_FAST                0 (n)
             22 LOAD_CONST               1 (1)
             24 INPLACE_SUBTRACT
             26 STORE_FAST               0 (n)
             28 JUMP_ABSOLUTE            4

  9     >>   30 LOAD_FAST                1 (r)
             32 RETURN_VALUE
```

Пошаговое описание:

0 LOAD_CONST 1 (1)

    Действие: Загружает константу 1 в стек.
    Стек: [1]
    Эквивалент: 1

2 STORE_FAST 1 (r)

    Действие: Сохраняет верхнее значение из стека в переменную r.
    Стек: []
    Эквивалент: r = 1

4 LOAD_FAST 0 (n)

    Действие: Загружает значение переменной n в стек.
    Стек: [n]

6 LOAD_CONST 1 (1)

    Действие: Загружает константу 1 в стек.
    Стек: [n, 1]

8 COMPARE_OP 4 (>)

    Действие: Сравнивает, больше ли n чем 1.
    Стек: [n > 1]
    Эквивалент: n > 1

10 POP_JUMP_IF_FALSE 30

    Действие: Если условие False, переходит к индексу 30.
    Стек: []
    Эквивалент: if not (n > 1): goto line 30

12 LOAD_FAST 1 (r)

    Действие: Загружает r в стек.
    Стек: [r]

14 LOAD_FAST 0 (n)

    Действие: Загружает n в стек.
    Стек: [r, n]

16 INPLACE_MULTIPLY

    Действие: Умножает r на n и сохраняет результат на стеке.
    Стек: [r * n]
    Эквивалент: r *= n

18 STORE_FAST 1 (r)

    Действие: Сохраняет результат в переменную r.
    Стек: []

20 LOAD_FAST 0 (n)

    Действие: Загружает n в стек.
    Стек: [n]

22 LOAD_CONST 1 (1)

    Действие: Загружает 1 в стек.
    Стек: [n, 1]

24 INPLACE_SUBTRACT

    Действие: Вычитает 1 из n и сохраняет результат на стеке.
    Стек: [n - 1]
    Эквивалент: n -= 1

26 STORE_FAST 0 (n)

    Действие: Сохраняет результат в переменную n.
    Стек: []

28 JUMP_ABSOLUTE 4

    Действие: Переходит обратно к инструкции с индексом 4.
    Эквивалент: goto line 4

30 LOAD_FAST 1 (r)

    Действие: Загружает r в стек.
    Стек: [r]

32 RETURN_VALUE

    Действие: Возвращает значение r из функции.
    Эквивалент: return r

```bash
def factorial(n):
    r = 1
    while n > 1:
        r *= n
        n -= 1
    return r
```

### Задача 3
Приведите результаты из задач 1 и 2 для виртуальной машины JVM (Java) или .Net (C#).

Java
```bash
public int foo(int x) {
    return x * 10 + 42;
}
```

Байткод Java
```bash
public int foo(int);
  Code:
     0: iload_1         // Загружает x
     1: bipush        10  // Константа 10
     3: imul             // Умножение
     4: bipush        42  // Константа 42
     6: iadd             // Сложение
     7: ireturn          // Возврат результата
```

Java
```bash
public int factorial(int n) {
    int r = 1;
    while (n > 1) {
        r *= n;
        n -= 1;
    }
    return r;
}
```

Байткод Java
```bash
public int factorial(int);
  Code:
     0: iconst_1        // r = 1
     1: istore_2
     2: iload_1         // Загружаем n
     3: iconst_1
     4: if_icmple 17    // Если n <= 1, перейти к 17
     7: iload_2         // Загружаем r
     8: iload_1         // Загружаем n
     9: imul            // r * n
    10: istore_2        // r = r * n
    11: iload_1         // Загружаем n
    12: iconst_1
    13: isub            // n - 1
    14: istore_1        // n = n - 1
    15: goto 2          // Переход к началу цикла
    17: iload_2         // Загружаем r
    18: ireturn         // Возврат r
```

C#
```bash
public int Foo(int x)
{
    return x * 10 + 42;
}
```

IL-код C#
```bash
.method public hidebysig instance int32 Foo(int32 x) cil managed
{
    .maxstack 2
    IL_0000: ldarg.1      // Загружаем x
    IL_0001: ldc.i4.s 10  // Константа 10
    IL_0003: mul          // Умножение
    IL_0004: ldc.i4.s 42  // Константа 42
    IL_0006: add          // Сложение
    IL_0007: ret          // Возврат результата
}
```

C#
```bash
public int Factorial(int n)
{
    int r = 1;
    while (n > 1)
    {
        r *= n;
        n -= 1;
    }
    return r;
}
```

IL-код C#
```bash
.method public hidebysig instance int32 Factorial(int32 n) cil managed
{
    .maxstack 2
    .locals init ([0] int32 r)
    IL_0000: ldc.i4.1      // r = 1
    IL_0001: stloc.0
    IL_0002: br.s IL_0009  // Переход к проверке условия

    IL_0004: ldloc.0       // Загружаем r
    IL_0005: ldarg.1       // Загружаем n
    IL_0006: mul           // r * n
    IL_0007: stloc.0       // r = r * n
    IL_0008: ldarg.1       // Загружаем n

    IL_0009: ldc.i4.1      // Константа 1
    IL_000a: sub           // n - 1
    IL_000b: starg.s n     // n = n - 1

    IL_000d: ldarg.1       // Загружаем n
    IL_000e: ldc.i4.1      // Константа 1
    IL_000f: bgt.s IL_0004 // Если n > 1, перейти к началу цикла

    IL_0011: ldloc.0       // Загружаем r
    IL_0012: ret           // Возврат r
}
```
