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


Страна:
`fieldName = "country"`

Значения передаются через `text.v`.

Не используй устаревшие поля `created2` и `pri_data:ВКонтакте`.