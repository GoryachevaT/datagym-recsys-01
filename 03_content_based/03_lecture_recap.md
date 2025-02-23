# 03 Content-based RecSys

## 1. General
### Задачи поиска информации:
- что-то конкретное и известное, пример - ищем авто конретной марки
- не знаю что это конкретно, но если увижу то пойму - хорошие наушники
- серфинг без определенной цели, пример - блошиный рынок, netflix, steam

### Что такое CB RS (Content-based recsys)?
- Неперсонализованные CB - item-item recs (похожие по разным критериям: примеры - товары того же продавца, фильмы с этим актером)
- Персонализованные CB - user-item recs (похожие на то, что пользователь смотрел. действия других пользователей не учитываются), можно матчить айтемы и пользователя по некоторым атрибутам профиля

## 2. Pros & cons
### Плюсы CB
- Не нужна история других пользователей, чтобы сформировать рекомендации. Достаточно лишь профиля пользователя.
- Более прозрачный ящик, чем коллаборативные системы (но не всегда)
- item cold start - можно рекомендовать новый айтем сразу после его появления, не дожидаясь, пока наберется статистика
- можно даже не обучать
- cold start - проблема "холодного" старта, когда нет истории действий, бывает item-cold start / user-cold start
### Минусы CB
- не всегда достаточно информации для того, чтобы понять, почему пользователю нравится тот или иной контент (примеры - видео/шутки/странички из интернета, где удалили все кроме текста)
- отсутствует discovery - контентные рекомендательные системы будут рекомендовать то, что максимально похоже на профиль пользователя
- user cold start - никуда не девается, хотя справляется лучше, чем CF

## 3. Representation 
### Как можно представить объект
- one hot encoding
- tf/idf
- word2vec-like embeddings
- metric learning
- dssm
### Как учить эмбеддинги
- как word2vec, контекст - сессия / корзина
### User Representation
Как можно представить профиль пользователя, если нам известна его история взаимодействия с айтемами? Варианты:
- просто среднее всех айтемов
- агрегаты по различным категориям
- средневзвешенное (например по времени)
- модель (temporal RNN)
### Получили профиль пользователя, профиль айтема, как можно обучить модель?
- можно ML
- можно просто поискать по одной из similarity metric (dot/cosine/euclidean)

## 4. Метрики 

### Serendipity / Novelty / Diversity
- serendipity [интуитивная прозорливость] - когда мы делаем крутую рекоменацию, которую пользователь ну никак не ожидал, но ему зашло
- novelty - не показывать то, что пользователь уже видел (айтемы/категории)
- diversity - показывать разные типы айтемов, очень хорошо для онлайн метрик

### Метрики близости на разных фичах
- https://scikit-learn.org/stable/modules/generated/sklearn.neighbors.DistanceMetric.html - быстрый подсчет euclidean distance

### Metrics
http://queirozf.com/entries/evaluation-metrics-for-ranking-problems-introduction-and-examples#precision-k
- precision [точность] - основная метрика, на которую стоит смотреть (К выбираем опираясь на продукт, если пользователь видит 8 айтемов в топе, то лучше мерить precision@8)
- recall [покрытие] - сколько реальных айтемов пользователя содержится в рекомендациях, на него стоит смотреть скорее при отборе кандидатов для модели 2 уровня
- NDCG / AP - метрики, которые показывают релевантность топа пользователю в целом
- MRR - mean reciprocal rank - обратный ранк первого релевантного айтема, имеет смысл, когда пользователю нужно сделать только одно целевое действие

## Рекомендации в Авито
- как все начиналось - https://www.youtube.com/watch?v=3MEe5IzBJk4
- старые "похожие" и бандиты - https://habr.com/ru/company/avito/blog/417571/
- как было год назад - https://www.youtube.com/watch?v=Kk9Cgom9wv8
- основные места с рекомендациями - главная и похожие
- на главной - смесь 2х моделей, CF и CB, ранжируем моделью 2 уровня, категории смешивает согласно интересам
- CB модель - похожие на то, что пользователь смотрел, взвешенные по времени
- CF - матричная факторизация на основе LightFM, чанки по категориям + локациям
- Похожие - item2vec (https://habr.com/ru/company/avito/blog/491942/)