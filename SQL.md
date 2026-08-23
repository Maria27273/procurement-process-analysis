# SQL Analysis

### Как отличаются типы закупок по количеству заявок, средней сумме, времени обработки, количеству возвратов и уточнений?

### SQL-запрос

```sql
SELECT
    purchase_type,
    COUNT(*) AS request_count,
    ROUND(AVG(amount), 2) AS avg_amount,
    ROUND(AVG(total_time_hours), 2) AS avg_processing_time,
    ROUND(AVG(return_count), 2) AS avg_returns,
    ROUND(AVG(clarification_count), 2) AS avg_clarifications
FROM procurement_requests
GROUP BY purchase_type
ORDER BY avg_processing_time DESC;
```

### Результат

| Тип закупки | Количество заявок | Средняя сумма, ₽ | Среднее время, ч | Возвраты | Уточнения |
| :---------- | ----------------: | ---------------: | ---------------: | -------: | --------: |
| Юридическая |               745 |       525 560,82 |            58,35 |     0,24 |      0,73 |
| IT          |             1 710 |       115 835,77 |            35,09 |     0,32 |      0,88 |
| Простая     |             2 545 |        18 270,79 |             9,96 |     0,07 |      0,43 |

Юридические закупки имеют самое высокое среднее время обработки - 58,35 ч, а простые - самое низкое (9,96 ч).


### Как распределяются заявки по итоговым статусам и сколько времени в среднем занимает их обработка?

### SQL-запрос

```sql
SELECT
    final_status,
    COUNT(*) AS request_count,
    ROUND(
        COUNT(*) * 100.0 / (SELECT COUNT(*) FROM procurement_requests),
        2
    ) AS share_percent,
    ROUND(AVG(total_time_hours), 2) AS avg_processing_time
FROM procurement_requests
GROUP BY final_status
ORDER BY request_count DESC;
```

### Результат

| Статус     | Количество заявок | Доля, % | Среднее время обработки, ч |
| :--------- | ----------------: | ------: | -------------------------: |
| Одобрена   |             4 239 |   84,78 |                      25,75 |
| В процессе |               386 |    7,72 |                      27,34 |
| Отклонена  |               375 |    7,50 |                      24,34 |


Большинство заявок (84,78%) получают статус «Одобрена». Доля отклонённых заявок составляет 7,5%, ещё 7,72% находятся в процессе обработки.


### Как количество возвратов заявки на доработку влияет на длительность её обработки?

SQL-запрос

```sql
SELECT
    return_count,
    COUNT(*) AS request_count,
    ROUND(AVG(total_time_hours), 2) AS avg_processing_time
FROM procurement_requests
GROUP BY return_count
ORDER BY return_count;
```

### Результат

| Количество возвратов | Количество заявок | Среднее время обработки, ч |
| :------------------- | ----------------: | -------------------------: |
| 0                    |             4 339 |                      22,37 |
| 1                    |               463 |                      43,15 |
| 2                    |               160 |                      57,70 |
| 3                    |                38 |                      67,20 |


С увеличением количества возвратов среднее время обработки возрастает: с 22,37 ч без возвратов до 67,20 ч при трёх возвратах.


### Как количество уточнений влияет на длительность обработки заявки?

### SQL-запрос

```sql
SELECT
    clarification_count,
    COUNT(*) AS request_count,
    ROUND(AVG(total_time_hours), 2) AS avg_processing_time
FROM procurement_requests
GROUP BY clarification_count
ORDER BY clarification_count;
```

### Результат

| Количество уточнений | Количество заявок | Среднее время обработки, ч |
| :------------------- | ----------------: | -------------------------: |
| 0                    |             3 039 |                      22,19 |
| 1                    |             1 447 |                      24,68 |
| 2                    |               136 |                      39,02 |
| 3                    |               224 |                      50,52 |
| 4                    |                47 |                      57,95 |
| 5                    |                64 |                      56,77 |
| 6                    |                43 |                      62,89 |


В целом с увеличением количества уточнений возрастает среднее время обработки: с 22,19 ч без уточнений до 62,89 ч при шести уточнениях.


### Какие подразделения создают больше всего заявок и отличаются ли они по средней сумме и времени обработки?

### SQL-запрос

```sql
SELECT
    department,
    COUNT(*) AS request_count,
    ROUND(AVG(amount), 2) AS avg_amount,
    ROUND(AVG(total_time_hours), 2) AS avg_processing_time
FROM procurement_requests
GROUP BY department
ORDER BY request_count DESC;
```

### Результат

| Подразделение  | Количество заявок | Средняя сумма, ₽ | Среднее время обработки, ч |
| :------------- | ----------------: | ---------------: | -------------------------: |
| IT             |             1 009 |       111 320,39 |                      24,48 |
| Sales          |               993 |       125 285,33 |                      26,05 |
| Marketing      |               729 |       127 051,19 |                      25,53 |
| Operations     |               710 |       134 505,22 |                      26,95 |
| Finance        |               538 |       135 461,62 |                      25,03 |
| HR             |               529 |       134 481,41 |                      27,40 |
| Administration |               492 |       136 692,06 |                      25,52 |


Наибольшее количество заявок создают IT и Sales. Наиболее длительное среднее время обработки наблюдается у HR - 27,4 ч.


### Как сложность маршрута согласования влияет на время обработки заявки и количество возвратов и уточнений?

### SQL-запрос

```sql
SELECT
    approval_route,
    COUNT(*) AS request_count,
    ROUND(AVG(total_time_hours), 2) AS avg_processing_time,
    ROUND(AVG(return_count), 2) AS avg_returns,
    ROUND(AVG(clarification_count), 2) AS avg_clarifications
FROM procurement_requests
GROUP BY approval_route
ORDER BY avg_processing_time DESC;
```

### Результат

| Маршрут                                 | Заявок | Среднее время, ч | Возвраты | Уточнения |
| :-------------------------------------- | -----: | ---------------: | -------: | --------: |
| Manager → Legal → Finance → Procurement |     27 |            73,79 |     0,41 |      1,07 |
| Manager → Finance → Procurement → Legal |     19 |            67,74 |     0,21 |      0,37 |
| Manager → Finance → Legal → Procurement |    699 |            57,50 |     0,23 |      0,73 |
| Manager → IT → Procurement → Finance    |     40 |            45,49 |     0,23 |      0,63 |
| Manager → Finance → IT → Procurement    |     47 |            44,55 |     0,38 |      0,98 |
| Manager → IT → Finance → Procurement    |  1 623 |            34,56 |     0,32 |      0,88 |
| Manager → Finance → Procurement         |    122 |            22,18 |     0,08 |      0,53 |
| Manager → Procurement                   |  2 423 |             9,35 |     0,07 |      0,43 |


Наиболее простой маршрут Manager → Procurement имеет минимальное среднее время обработки - 9,35 ч. Более сложные маршруты занимают значительно больше времени - до 73,79 ч.


### Влияет ли размер закупки на длительность её обработки?

### SQL-запрос

```sql
SELECT
    CASE
        WHEN amount < 50000 THEN 'До 50 тыс.'
        WHEN amount < 100000 THEN '50–100 тыс.'
        WHEN amount < 250000 THEN '100–250 тыс.'
        WHEN amount < 500000 THEN '250–500 тыс.'
        ELSE '500 тыс. и более'
    END AS amount_range,
    COUNT(*) AS request_count,
    ROUND(AVG(amount), 2) AS avg_amount,
    ROUND(AVG(total_time_hours), 2) AS avg_processing_time
FROM procurement_requests
GROUP BY amount_range
ORDER BY
    CASE amount_range
        WHEN 'До 50 тыс.' THEN 1
        WHEN '50–100 тыс.' THEN 2
        WHEN '100–250 тыс.' THEN 3
        WHEN '250–500 тыс.' THEN 4
        WHEN '500 тыс. и более' THEN 5
    END;
 ```

### Результат

| Диапазон суммы   | Количество заявок | Средняя сумма, ₽ | Среднее время обработки, ч |
| :--------------- | ----------------: | ---------------: | -------------------------: |
| До 50 тыс.       |             2 620 |        18 472,61 |                      11,60 |
| 50–100 тыс.      |               779 |        72 124,23 |                      31,83 |
| 100–250 тыс.     |               966 |       154 673,84 |                      39,59 |
| 250–500 тыс.     |               355 |       343 360,36 |                      54,60 |
| 500 тыс. и более |               280 |       929 393,82 |                      57,24 |



С увеличением суммы заявки среднее время обработки возрастает: с 11,6 ч для заявок до 50 тыс. ₽ до 57,24 ч для заявок от 500 тыс. ₽.


### Какие причины чаще всего приводят к отклонению заявки и сколько времени занимает её обработка до отклонения?

### SQL-запрос

```sql
SELECT
    rejection_reason,
    COUNT(*) AS rejected_count,
    ROUND(
        COUNT(*) * 100.0 /
        (SELECT COUNT(*)
         FROM procurement_requests
         WHERE final_status = 'Отклонена'),
        2
    ) AS share_percent,
    ROUND(AVG(total_time_hours), 2) AS avg_processing_time
FROM procurement_requests
WHERE final_status = 'Отклонена'
GROUP BY rejection_reason
ORDER BY rejected_count DESC;
```

### Результат
| Причина отклонения                  | Количество | Доля, % | Среднее время обработки, ч |
| :---------------------------------- | ---------: | ------: | -------------------------: |
| Недостаточно бюджета                |        140 |   37,33 |                      23,74 |
| Некорректные данные                 |        125 |   33,33 |                      24,13 |
| Не согласовано руководителем        |         53 |   14,13 |                       9,38 |
| Технические требования не соблюдены |         39 |   10,40 |                      34,96 |
| Юридические риски                   |         18 |    4,80 |                      51,55 |


Основные причины отклонения — недостаточный бюджет (37,33%) и некорректные данные (33,33%).


### Какова доля заявок, возвращаемых на доработку, и насколько возвраты увеличивают время обработки?

### SQL-запрос

```sql
SELECT
    CASE
        WHEN return_count = 0 THEN 'Без возвратов'
        ELSE 'С возвратами'
    END AS return_group,
    COUNT(*) AS request_count,
    ROUND(
        COUNT(*) * 100.0 /
        (SELECT COUNT(*) FROM procurement_requests),
        2
    ) AS share_percent,
    ROUND(AVG(total_time_hours), 2) AS avg_processing_time
FROM procurement_requests
GROUP BY return_group
ORDER BY request_count DESC;
```

### Результат

| Группа        | Количество заявок | Доля, % | Среднее время обработки, ч |
| :------------ | ----------------: | ------: | -------------------------: |
| Без возвратов |             4 339 |   86,78 |                      22,37 |
| С возвратами  |               661 |   13,22 |                      48,06 |


Заявки с возвратами составляют 13,22% от общего количества и обрабатываются в среднем более чем в 2 раза дольше, чем заявки без возвратов.



