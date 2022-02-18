Dicas de Python
=========================

### Manipulação de data e tempo

Site oficial do DateOffset:

* Sobre DataOffet:

[https://pandas.pydata.org/docs/reference/api/pandas.tseries.offsets.DateOffset.html](https://pandas.pydata.org/docs/reference/api/pandas.tseries.offsets.DateOffset.html)

* Sobre Timedelta:

[https://pandas.pydata.org/docs/reference/api/pandas.Timedelta.html](https://pandas.pydata.org/docs/reference/api/pandas.Timedelta.html)

As dicas abaixo são do link:

[https://towardsdatascience.com/5-examples-to-learn-date-and-time-manipulation-with-python-pandas-9ab9cdeb032e](https://towardsdatascience.com/5-examples-to-learn-date-and-time-manipulation-with-python-pandas-9ab9cdeb032e)

> **Dica 1:** Para decrementar ou diminuir, basta adicionar o sinal de menos (-).
>
> Exemplo: `months=-6`.

> **Dica 2:** Para incrementar em anos, basta alterar o nome `months` para `years`.
>
> Exemplo:
>
> `df["Date2"] = df["Date"] + pd.DateOffset(years=1) `

> **Dica 3**: As possibilidades de `DateOffset` são:
> years, month, week, day, hour, minute, second, microsecon e nanosecond.

### Manipulação de data e hora com DateOffset

The `DateOffset` function can be used for adding a specific length of duration to dates.

A função `DataOffset` pode ser utilizada para adicionar um comprimento temporal específico ao conjunto de datas.

```python
import pandas as pd

df = pd.DataFrame({
    "Date": pd.date_range(start="2021-12-28", periods=5, freq="D"),
    "Measurement": [1, 10, 25, 7, 12]
})

print(df)
```

O código inicia com a data em `2021-12-28`, serão criados 5 tempos (`period`) ou dias de acordo com `freq`.

Ao executar o código acima, o resultado será:

```python
        Date  Measurement
0 2021-12-28            1
1 2021-12-29           10
2 2021-12-30           25
3 2021-12-31            7
4 2022-01-01           12
```

### Criação de uma nova coluna no DataFrame adicionando 6 meses a coluna já existente de datas.

Neste exemplo, será criada uma nova coluna chamada ´Date2´.

```python
df["Date2"] = df["Date"] + pd.DateOffset(months=6)

print(df)
```

O resultado será:

```python
        Date  Measurement      Date2
0 2021-12-28            1 2022-06-28
1 2021-12-29           10 2022-06-29
2 2021-12-30           25 2022-06-30
3 2021-12-31            7 2022-06-30
4 2022-01-01           12 2022-07-01
```

Note que houve um incremento de 6 meses na variável `Date`.

### Adicionando incremento

Os valores que estão sendo trabalhados na coluna `Date` são do tipo `datetime`, portanto é possível adicionar incrementos ou intervalos.

```python
df["Date2"] = df["Date"] + pd.DateOffset(hours=2)`

print(df)
```

Neste caso, foi adicionado o intervalo a cada 2 horas.

### Manipulação do Datetime utilizando Timedelta

O `Timedelta` é útil para somar datas e tempos. A sua declaração é um pouco diferente da função `DateOffset`.

O parâmetro `unit` pode receber os seguintes valores:

* W’, ‘D’, ‘T’, ‘S’, ‘L’, ‘U’, ou ‘N’
* days’ ou ‘day’
* hours’, ‘hour’, ‘hr’, ou ‘h’
* minutes’, ‘minute’, ‘min’, ou ‘m’
* seconds’, ‘second’, ou ‘sec’
* milliseconds’, ‘millisecond’, ‘millis’, ou ‘milli’
* microseconds’, ‘microsecond’, ‘micros’, ou ‘micro’
* nanoseconds’, ‘nanosecond’, ‘nanos’, ‘nano’, ou ‘ns’.

```python
df["Date2"] = df["Date"] + pd.Timedelta(3, unit="day")

print(df)
```
O resultado será:

```python
        Date  Measurement      Date2
0 2021-12-28            1 2021-12-31
1 2021-12-29           10 2022-01-01
2 2021-12-30           25 2022-01-02
3 2021-12-31            7 2022-01-03
4 2022-01-01           12 2022-01-04
```

Neste exemplo, os dias são incrementados a cada três dias, por isso, o valor 3 e a unidade (`unit`) está em dias (`day`).

O `Timedelta` aceita strings como argumento `("3 days")`. Exemplo:

```python
df["Date2"] = df["Date"] + pd.Timedelta("3 days")

print(df)
```

### Extraindo informação do objeto datetime

O objeto datetime possui vários pedaços de informações que podem ser utilizadas, por exemplo: year, month, day, week, hour, microsecond, detre outras possibilidades.

Para ver as informações, basta digitar:

`print(dir(df["Date"].dt))`

Algumas possibilidades:

* 'day'
* 'day_name'
* 'day_of_week'
* 'day_of_year'
* 'dayofweek'
* 'dayofyear'
* 'days_in_month'
* 'daysinmonth'
* 'freq'
* 'hour'
* 'is_leap_year'
* 'is_month_end'
* 'is_month_start'
* 'is_quarter_end'
* 'is_quarter_start'
* 'is_year_end'
* 'is_year_start'
* 'minute'
* 'month'
* 'month_name'
* 'normalize'
* 'quarter'
* 'round'
* 'second'
* 'time'
* 'timetz'
* 'to_period'
* 'to_pydatetime'
* 'week'
* 'weekday'
* 'weekofyear'
* 'year'

Exemplo 1: Ao digitar o comando abaixo:

`print(df["Date"].dt.month)`

O resultado será:

```python
0    12
1    12
2    12
3    12
4     1
Name: Date, dtype: int64
```

### Função isocalendar

Retorna de uma vez as informações de year, week e day.

Exemplo:

`print(df["Date"].dt.isocalendar())`

O resultado será:

```python
   year  week  day
0  2021    52    2
1  2021    52    3
2  2021    52    4
3  2021    52    5
4  2021    52    6
```

### Diferença entre datas

Neste exemplo, será mostrado como calcular a diferença entre duas datas. Essa tarefa pode ser feita por meio do `datetime`.

```python
# Cria uma nova coluna chamada Date2.
df["Date2"] = df["Date"] + pd.DateOffset(months=6)

# Cria uma nova coluna que armazenará a diferença entre as duas datas, "Date2" e "Date".
df["diff"] = df["Date2"] - df["Date"]

print(df)
```

O resultado será:

```python
        Date  Measurement      Date2     diff
0 2021-12-28            1 2022-06-28 182 days
1 2021-12-29           10 2022-06-29 182 days
2 2021-12-30           25 2022-06-30 182 days
3 2021-12-31            7 2022-06-30 181 days
4 2022-01-01           12 2022-07-01 181 days
```

A coluna `diff` é do tipo `timedelta`, dessa forma é possível extrair o número de dias usando o método `day`.

`print(df["diff"].dt.days)`

O resultado do comando acima será:

```python
0    182
1    182
2    182
3    181
4    181
Name: diff, dtype: int64
```

E como saber o resultado em meses?

Para converter a diferença em meses ou anos, utiliza-se o `timedelta` do `NumPy` porque o `Pandas` não pode construir um `Timedelta` de meses ou anos.

```python
import numpy as np

numero_meses = df["diff"] / np.timedelta64(1, 'M')

print(numero_meses)
```
O resultado será:

```python
0    5.979589
1    5.979589
2    5.979589
3    5.946734
4    5.946734
Name: diff, dtype: float64
```