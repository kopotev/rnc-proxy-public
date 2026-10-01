# Метапараметры ГИКРЯ

Корпус: `GICR`.

Метапараметры передаются в:
`subcorpus.sectionValues[].conditionValues`.

## Тип публикации

`fieldName = "type"`
значение: `text.v`

## Дата создания

`fieldName = "created"`

Используй:

dateRange:
  begin: {year, month, day}
  end: {year, month, day}
  matching: INT_RANGE_INTERSECT

## Год рождения автора

`fieldName = "birthday"`

Используй:

intRange:
  begin: YEAR
  end: YEAR
  matching: INT_RANGE_INTERSECT

## Пол автора

`fieldName = "sex"`
`text.v = "жен"` или `"муж"`

## ВКонтакте

`fieldName = "pri_cat:ВКонтакте"`
`check.v = true`

## География ВКонтакте

Город:
`fieldName = "city:ВКонтакте"`

Регион:
`fieldName = "region:ВКонтакте"`

Страна:
`fieldName = "country:ВКонтакте"`

Значения передаются через `text.v`.

## Проверка актуальной схемы

Если имя метаполя неочевидно или статическая справка расходится с поведением Worker, сначала используй `getAttributes` для корпуса `GICR`.

Если результат `getAttributes` расходится со статической справкой, приоритет имеет актуальная схема Worker.

Для географии ВКонтакте в актуальной схеме используются:

- `city:ВКонтакте`
- `region:ВКонтакте`
- `country:ВКонтакте`

Не используй `country` для страны ВКонтакте, если актуальная схема возвращает `country:ВКонтакте`.

Не используй устаревшие поля `created2` и `pri_data:ВКонтакте`.
