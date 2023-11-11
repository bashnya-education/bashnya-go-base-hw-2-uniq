# БАШНЯ | Backend на Go | Домашнее задание №2

## Как работать с данным репозиторием

1. Создать репозиторий на основе данного шаблона (Нажать зелёную кнопку **`Use this template`** -> **Create a new repository**. Имя репозитория можно задать произвольно)
2. Создать ветку `develop`
3. Выполнить задание. Всё решение необходимо расположить в корне репозитория
4. Создать Pull Request (PR) на слияние ветки `develop` в ветку `main`
5. **Внимание!** Это ДЗ имеет систему [автоматического тестирования](#автоматическое-тестирование). Отправлять решене на проверку необходимо **только после успешного прохождения автоматического тестирования!**
7. Мерджить ветки можно только после одобрения ревьюера!

## Задание

Реализовать утилиту для поиска уникальных строк. Сама утилита имеет набор параметров, которые необходимо поддержать.

### Параметры

`-с` - подсчитать количество встречаний строки во входных данных.
Вывести это число перед строкой отделив пробелом.

`-d` - вывести только те строки, которые повторились во входных данных.

`-u` - вывести только те строки, которые не повторились во входных данных.

`-f num_fields` - не учитывать первые `num_fields` полей в строке.
Полем в строке является непустой набор символов отделённый пробелом.

`-s num_chars` - не учитывать первые `num_chars` символов в строке.
При использовании вместе с параметром `-f` учитываются первые символы
после `num_fields` полей (не учитывая пробел-разделитель после
последнего поля).

`-i` - не учитывать регистр букв.

### Использование

`uniq [-c | -d | -u] [-i] [-f num] [-s chars] [input_file [output_file]]`

1. Все параметры опциональны. Поведения утилиты без параметров --
   простой вывод уникальных строк из входных данных.

2. Параметры c, d, u взаимозаменяемы. Необходимо учитывать,
   что параллельно эти параметры не имеют никакого смысла. При
   передаче одного вместе с другим нужно отобразить пользователю
   правильное использование утилиты

3. Если не передан input_file, то входным потоком считать stdin

4. Если не передан output_file, то выходным потоком считать stdout

### Пример работы

<details>
    <summary>Без параметров</summary>

```bash
$cat input.txt
I love music.
I love music.
I love music.

I love music of Kartik.
I love music of Kartik.
Thanks.
$cat input.txt | go run uniq.go
I love music.

I love music of Kartik.
Thanks.
```

</details>

<details>
    <summary>С параметром input_file</summary>

```bash
$cat input.txt
I love music.
I love music.
I love music.

I love music of Kartik.
I love music of Kartik.
Thanks.
$go run uniq.go input.txt
I love music.

I love music of Kartik.
Thanks.
```

</details>

<details>
    <summary>С параметрами input_file и output_file</summary>

```bash
$cat input.txt
I love music.
I love music.
I love music.

I love music of Kartik.
I love music of Kartik.
Thanks.
$go run uniq.go input.txt output.txt
$cat output.txt
I love music.

I love music of Kartik.
Thanks.
```

</details>

<details>
    <summary>С параметром -c</summary>

```bash
$cat input.txt
I love music.
I love music.
I love music.

I love music of Kartik.
I love music of Kartik.
Thanks.
$cat input.txt | go run uniq.go -c
3 I love music.
1 
2 I love music of Kartik.
1 Thanks.
```

</details>

<details>
    <summary>С параметром -d</summary>

```bash
$cat input.txt
I love music.
I love music.
I love music.

I love music of Kartik.
I love music of Kartik.
Thanks.
$cat input.txt | go run uniq.go -d
I love music.
I love music of Kartik.
```

</details>

<details>
    <summary>С параметром -u</summary>

```bash
$cat input.txt
I love music.
I love music.
I love music.

I love music of Kartik.
I love music of Kartik.
Thanks.
$cat input.txt | go run uniq.go -d

Thanks.
```

</details>

<details>
    <summary>С параметром -i</summary>

```bash
$cat input.txt
I LOVE MUSIC.
I love music.
I LoVe MuSiC.

I love MuSIC of Kartik.
I love music of kartik.
Thanks.
$cat input.txt | go run uniq.go -i
I LOVE MUSIC.

I love MuSIC of Kartik.
Thanks.
```

</details>

<details>
    <summary>С параметром -f num</summary>

```bash
$cat input.txt
We love music.
I love music.
They love music.

I love music of Kartik.
We love music of Kartik.
Thanks.
$cat input.txt | go run uniq.go -f 1
We love music.

I love music of Kartik.
Thanks.
```

</details>

<details>
    <summary>С параметром -s num</summary>

```bash
$cat input.txt
I love music.
A love music.
C love music.

I love music of Kartik.
We love music of Kartik.
Thanks.
$cat input.txt | go run uniq.go -s 1
I love music.

I love music of Kartik.
We love music of Kartik.
Thanks.
```

</details>

### Unit-тесты

Необходимо протестировать поведение написанной функциональности
с различными параметрами. Для тестирования нужно написать unit-тесты
на эту функциональность. Тесты нужны как для успешных случаев,
так и для неуспешных. Примеры тестов можно посмотреть [здесь](https://github.com/go-park-mail-ru/lectures/blob/master/1-basics/6_is_sorted/sorted/sorted_test.go).

## Автоматическое тестирование

Для автоматической проверки написанного решения используется сервис GitHub Actions.

После создания Pull Request-а в ветку `main` запускается автоматическое тестирование, включающее в себя следующие этипы:
1) Сборка проекта
2) Запуск линтера
3) Запуск unit-тестов, написанных в рамках решения
4) Запуск [приёмочных тестов](./acceptance-tests)

Если все проверки проходят успешно, на странице PR-а должна появиться зелёная галочка и надпись **"All checks have passed"**. 

## Материалы в помощь

* https://habr.ru/post/306914/ - пакет io

* https://golang.org/pkg/sort/

* https://golang.org/pkg/io/

* https://golang.org/pkg/io/ioutil/

* https://godoc.org/flag - пакет для флагов

* https://godoc.org/github.com/stretchr/testify - удобный набор
  пакетов для тестирования

* https://golang.org/pkg/bufio/#Scanner - удобный способ прочитать
  линии из потока данных

## Best practices

1. Уникализация может понадобиться не только как утилита, но
   и как часть более крупной логики. Для этого саму функцию
   уникализации можно вынести в отдельный пакет. Поскольку
   более крупная логика не всегда связана с чтением аргументов
   и данных из файла или `stdin`, то на вход этой функции нужно
   передавать слайс строк и аргументы.

2. Как файл, так и stdin удовлетворяет интерфейсу `io.Reader`.
   Поэтому логику по чтению можно сделать универсальной. Аналогично
   и с записью - `io.Writer`

3. Множество параметров, которые вдобавок и опциональны, лучше
   передавать структурой (например Options). Так проще расширять
   функциональность, а внешнему пользователю функции (не всей утилиты)
   будет проще передать правильные аргументы внутрь.

4. Для написания однотипных тестовых случаев используется
   [табличное тестирование](https://github.com/golang/go/wiki/TableDrivenTests).
   Получается, что можно написать две функции тестов: успешные тестовые случаи
   и неуспешные тестовые случаи.

5. Для сравнения ожидаемого и действительного можно использовать
   пакет [require](https://godoc.org/github.com/stretchr/testify/require).
   Кроме простых сравнений на равенство пакет предоставляет много
   других ассертов.

6. Тесты не должны зависеть от внешних ресурсов. Не нужно читать
   файлы внутри теста. Так же не нужно тестировать передачу параметров
   при вызове утилиты. Никакого внешнего взаимодействия. Тестирование
   функции должно быть построено на том, что мы передаём некоторые
   входные данные в функцию и сравниваем ответ функции с ожидаемыми
   выходными данными.
