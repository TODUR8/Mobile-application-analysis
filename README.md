### Анализ мобильного приложения.

Описание проекта:

- Необходимо создать для продуктового менеджера, работающего в студии мобильных игр, функцию расчёта retention.
- Необходимо проанализировать результаты A/B-теста различных промо-наборов для выявления оптимального варианта и определения релевантных показателей принятия решения. 


Стек технологий:
- Python(Pandas, numpy, scipy, seaborn, matplotlib). 



Описание данных:

Структура auth_df (активность пользователей)
* auth_ts - время в формате timestamp
* uid - id пользователя

Структура reg_df (регистрация пользователей)
* reg_ts - время в формате timestamp
* uid - id пользователя



**Функция retention**

Напишем функцию retention которая на вход получает 4 аргумента:
* дату начала периода когортного анализа в формате yyyy-mm-dd.
* дату окончания периода когортного анализа в формате yyyy-mm-dd.
* retention для какого типа даты необходим (день, неделя, месяц).
* число дней для оценки retention.

В качестве примера введем следующие данные: 2020-09-01, 2020-10-01, день, 30. И получим следующий график retention:
![график retention](https://github.com/TODUR8/-/blob/main/retention.png)

Вывод:

График наглядно показывает, что удержание игроков со временем снижается, при этом более свежие когорты (поздние даты) не демонстрируют заметного улучшения метрики. Это свидетельствует о том, что текущая стратегия работы с игроками после их привлечения нуждается в усилении. Дополнительные стимулирующие механики или улучшение игрового опыта на ранних стадиях могут помочь повысить retention на более длительный период.


**А-В тест**
Итоги А-В теста:
* произвели анализ данных в обеих группах на нормальность распределения, с помощью теста Шапиро-Уилка выявили ненормальность распределения в обеих группах.
* так как распределения ненормальные, использовали для сравнения групп непараметрический тест Манна-Уитни (ранговый), получили p-уровень значимости > 0.05, что говорит о невозможности отклонения нулевой гипотезы о равенстве выборок, и, следовательно, о статистически незначимом различии средних значений в данных группах.
* взяли из групп только платящих пользователей и также провели тест на нормальность распределения, который приводит к выводу о ненормальном распределении в обеих группах.
* Для дальнейшего сравнения групп выполнили тест Манна-Уитни и получили p-уровень значимости = 0, следовательно, средние значения в ГС двух выборок не равны и выборки отличаются статистически значимо.
* Определили, что  разница в ARPPU двух групп(средний чек), составляет 11,31%
* с помощью бутстрапа выяснили, на каких децилях распределения revenue различия статистически значимы, получили стат значимые различия на всех децилях распределения в 89%.
* Так как получили, что чеки всех платящих пользователей по децилям в тестовой группе выше чем в контрольной, имеет смысл проверять перед А/Б тестом с помощью А/А тестирования группы пользователей (могут быть различия в сегментах).
* Таким образом, пользователи тестовой группы увеличили свои чеки во всех децилях платящих пользователей, поэтому делаем вывод, что акции в тестовой группе привели к улучшенным показателям метрики ARPPU и данный набор акций можно применять на всех пользователях (выкатывать в продакшн).
