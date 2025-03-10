---
editable: false
sourcePath: ru/_api-ref/datalens/function-ref/CONCAT.md
---

# CONCAT



#### Синтаксис {#syntax}


```
CONCAT( arg_1, arg_2, arg_3 [ , ... ] )
```

#### Описание {#description}
Объединяет произвольное количество строк. При использовании нестроковых типов происходит преобразование в строку и объединение.

**Типы аргументов:**
- `arg_1` — `Логический | Дата | Дата и время | Дата и время (устаревший) | Дробное число | Геоточка | Геополигон | Целое число | Строка | UUID`
- `arg_2` — `Логический | Дата | Дата и время | Дата и время (устаревший) | Дробное число | Геоточка | Геополигон | Целое число | Строка | UUID`
- `arg_3` — `Логический | Дата | Дата и время | Дата и время (устаревший) | Дробное число | Геоточка | Геополигон | Целое число | Строка | UUID`


**Возвращаемый тип**: `Строка`

#### Примеры {#examples}

```
CONCAT("Дата рождения ", #2019-01-23#) = "Дата рождения 2019-01-23"
```

```
CONCAT(2019, 01, 23) = "20190123"
```


#### Поддержка источников данных {#data-source-support}

`Материализованный датасет`, `ClickHouse 19.13`, `Yandex.Metrica`, `Microsoft SQL Server 2017 (14.0)`, `MySQL 5.6`, `Oracle Database 12c (12.1)`, `PostgreSQL 9.3`, `YDB`.
