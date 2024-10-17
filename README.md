## Отчет по лабораторной работе № 1

#### № группы: `ПМ-2403`

#### Выполнил: `Карпенко Вадим Вадимович`

#### Вариант: `10`

### Cодержание:

- [Постановка задачи](#1-постановка-задачи)
- [Входные и выходные данные](#2-входные-и-выходные-данные)
- [Выбор структуры данных](#3-выбор-структуры-данных)
- [Алгоритм](#4-алгоритм)
- [Программа](#5-программа)
- [Анализ правильности решения](#6-анализ-правильности-решения)

### 1. Постановка задачи
> На числовой прямой находится два отрезка: [A, B] и [C, D]. Известно,
что все 4 точки (A, B, C, D) различны. Определить, как расположен отрезок [C,
D] относительно отрезка [A, B]. Варианты: строго левее (<), строго правее (>),
внутри (in), снаружи (out), слева и внутри (<=), справа и внутри (>=). На вход
программы подаются целые числа A, B, C, D.

Возможные варианты расположения включают:

- Строго левее (отрезок C, D полностью находится слева от A, B)
- Строго правее (отрезок C, D полностью находится справа от A, B)
- Внутри (отрезок C, D полностью находится в пределах A, B)
- Снаружи (отрезок C, D пересекает отрезок A, B и по обе стороны от него)
- Слева и внутри (отрезок C, D начинается слева и полностью входит в A, B)
- Справа и внутри (отрезок C, D заканчивается справа и полностью входит в A, B)

### 2. Входные и выходные данные
#### Данные на вход
- Четыре различных целых числа A, B, C и D. 

     Выходные данные:
- Строка, указывающая положение отрезка C, D относительно отрезка A, B
### 3. Выбор структуры данных
Для решения данной задачи можно использовать обычные переменные для хранения целых чисел, а также простые условные конструкции для определения положения отрезков.

### 4. Алгоритм
#### Алгоритм выполнения программы:
1. Прочитать входные данные: A, B, C, D.
2. Перепроверить: если A > B, поменять значения A и B местами. Аналогично для C и D.
3. Применить следующие условия для определения положения отрезка C, D относительно A, B:
   - Если D < A, то C, D строго левее A, B.
   - Если C > B, то C, D строго правее A, B.
   - Если C ≥ A и D ≤ B, то C, D полностью находится внутри A, B.
   - Если C < A и D > B, то C, D полностью снаружи A, B.
   - Если D ≤ B и C ≤ A, то C, D слева и внутри A, B.
   - Если C ≥ A и D > B, то C, D справа и внутри A, B.
   - Проверка на пересечения (необязательно).
 	
#### Блок-схема
```mermaid
graph TD;
    A([Начало]) --> B[/Ввод: A, B, C, D/]
    B --> C{A == B || A == C || A == D || B == C || B == D || C == D}
    C --|Да|--> D[Ошибка: "Все точки A, B, C и D должны быть различны!"]
    D --> Z([Конец])
    C --|Нет|--> E[Убедиться: A < B и C < D]
    E --> F{D < A}
    F --|Да|--> G[Возврат: "<"] --> H[Вывод: "Строго левее"]
    F --|Нет|--> I{C > B}
    I --|Да|--> J[Возврат: ">"] --> K[Вывод: "Строго правее"]
    I --|Нет|--> L{C >= A && D <= B}
    L --|Да|--> M[Возврат: "in"] --> N[Вывод: "Внутри"]
    L --|Нет|--> O{C < A && D > B}
    O --|Да|--> P[Возврат: "out"] --> Q[Вывод: "Снаружи"]
    O --|Нет|--> R{D <= B && C <= A}
    R --|Да|--> S[Возврат: "<= (left and in)"] --> T[Вывод: "Слева и внутри"]
    R --|Нет|--> U{C >= A && D > B}
    U --|Да|--> V[Возврат: ">= (right and in)"] --> W[Вывод: "Справа и внутри"]
    U --|Нет|--> X[Возврат: "overlap"] --> Y[Вывод: "Пересечение"]
    H --> Z
    K --> Z
    N --> Z
    Q --> Z
    T --> Z
    W --> Z
    Y --> Z

```
### 5. Программа
```java
import java.util.Scanner;

public class SegmentPosition {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // Чтение входных данных
        System.out.print("Введите A: ");
        int A = scanner.nextInt();
        System.out.print("Введите B: ");
        int B = scanner.nextInt();
        System.out.print("Введите C: ");
        int C = scanner.nextInt();
        System.out.print("Введите D: ");
        int D = scanner.nextInt();

        // Проверка на уникальность значений
        if (A == B || A == C || A == D || B == C || B == D || C == D) {
            System.out.println("Ошибка: Все точки A, B, C и D должны быть различны!");
            return;
        }

        // Определение положения отрезков
        String result = determineSegmentPosition(A, B, C, D);
        System.out.println("Положение отрезка [C, D] относительно отрезка [A, B]: " + result);
    }

    private static String determineSegmentPosition(int A, int B, int C, int D) {
        // Убедимся, что A < B и C < D
        if (A > B) {
            int temp = A;
            A = B;
            B = temp;
        }
        if (C > D) {
            int temp = C;
            C = D;
            D = temp;
        }

        // Определяем положение отрезка [C, D] относительно [A, B]
        if (D < A) {
            return "<";  // Строго левее
        } else if (C > B) {
            return ">";  // Строго правее
        } else if (C >= A && D <= B) {
            return "in";  // Внутри
        } else if (C < A && D > B) {
            return "out";  // Снаружи
        } else if (D <= B && C <= A) {
            return "<= (left and in)";  // Слева и внутри
        } else if (C >= A && D > B) {
            return ">= (right and in)";  // Справа и внутри
        } else {
            return "overlap";  // Пересечение (если нужно)
        }
    }
}
```
### 6. Анализ правильности решения
Программа работает корректно на всем множестве решений с учетом ограничений.
1. Считывание данных: Программа запрашивает у пользователя ввод четырёх различных целых чисел \(A\), \(B\), \(C\) и \(D\).

2. Проверка уникальности: Программа проверяет, чтобы все введенные числа были уникальны. Если хотя бы одно число совпадает, выводится сообщение об ошибке.

3. Определение положения отрезков: Функция determineSegmentPosition определяет, как расположен отрезок \(C, D\) относительно отрезка \(A, B\) по заданным условиям.

4. Вывод результата: Результат положения отрезка выводится в консоль.
   
