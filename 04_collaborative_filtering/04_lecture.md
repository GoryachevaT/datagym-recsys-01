## Коллаборативная фильтрация

### 1. Def RecSys

- **Академический** - помочь пользователю найти объекты, которые будут ему интересны и которые он __не смог бы найти без помощт RS__
- **Бизнесовый** - показать пользователю такие объекты + __оптимизировать бизнес метрику__

### 2. User Preferences

- Explicit feedback (явные отзывы) - рейтинги, лайки, дизлайки
    - Pros
        - хорошо коррелирует с пользовательским предпочтением
    - Cons
        - Актуален лишь на момент отзыва
        - Требует доп. действий от пользователя
        - Явных отзывов обычно мало
        - Доступен лишь в некоторых областях
- Implicit feedback (неявные отзывы) - пользователь совершает некоторые действия, а система их интерпертирует
    - Pros
        - Доступен везде
        - Никаких усилий для пользователя
        - Любое взаимодействие с ситемой - потенциальный фидбек
    - Cons
        - Только косвенные сигналы
        - Один и тот же сигнал в разных ситуациях может обозначать разные вещи

### 3. Statement of problem

- Есть информация о том, какие объекты нравились пользователю в прошлом
- Хоти создать некоторую функцию F, которая предсказывает для неизвестной пары user-item "уровень предпочтения":

$$ F: User * Item -> Utility$$


### 4. Conten-based (CBRS)

- Зависит от качества описания объектов
- Чрезмерная специализация
    - Рекомендации основаны на знании о предпочтениях только этого пользователя
    - В базовой комплектации CB RS не может рекомендоваьт сопутствующие товары

Чего хотим?
- Сохранить преимущества
    - Пользователь предпочитает то, что видел раньше
- Учести недостатки
    - Чрезмерная специализация (и отсутствие новизны) из-за привязывания к свойствам объекта
- Добавить нового
    - Пользователи с похожим вкусом предпочитают похожие объекты

=> хотим рекомендательную модель на основе паттернов рейтингов или действий (просмотров, например), без необходимости использования дополнительной информации о свойствах товаров или пользователях.

### 5. Basic techniques

- Memory-based - the observations stored in the system are directly used to predict unknown observTIONS
    - User-based
    - Item-based
- Model-based
    - Matrix Factorization
    - Weighted Regularization

### 6. Memory-based

#### 6.1 Matrix completion
[photo 1]

#### 6.2 User-based CF

Рекомендуем контент, похожий на то, что понравилось людям, похожим на нас.

- Найти пользователей, похожих на меня
- Найти, что они смотрели
- Порекомендовать мне то же самое

[photo 2, 3]

**Объяснение рекомендаций**
Объект нравится пользователям [X и Y], чьи вкусы совпадают с вашими

#### 6.3 Сontent-based CF

Рекомендуем контент, похожий на то, что понравилось нам.

Соседей ищем не для пользователей, а для items
[photo 4]

**Объяснение рекомендаций**
Объект похож на понравившиеся вам ранее объекты X и Y.

#### 6.4 Как найти соседей?

- kNN
    - выбор k значительно влияет на качество
    - оптимальное значение k зависит от данных (обычно от 25 до 50)
    - функция близости: cosine similarity, коэф. корреляции Пирсона итд
- Approximate kNN (tools)
    - Annoy - https://github.com/spotify/annoy
    - faiss - https://github.com/facebooksearch/faiss

#### 6.5 User-based VS Item-based

[photo 6]

#### 6.6 Pros & Cons memory-based methods

**Преимущества**
- Простая реализация
- Рекомендации возможно объяснить
- Редко требуют большого количесвта ресурсов
- Для работы с новыми оценками не требуется перестраивать модель

**Недостатки**
- Частичное покрытие каталога
- Уязвимы к cold-start и data sparsity problems
- Для моделирования сложных зависимостей между пользователями и объектам требуются дополнительные усилия [извращения]

### 7. Model-based Techniques

The observation stored in the system are used to learn a set of model  parameters (e.g. latent feats of users and items)

#### 7.1 Matrix Factorization (MF)

[формулы с фото 7]

#### 7.2 Stochastic Gradient Descent

[формулы с фото 8]

### 8. Implicit Feedback

- Явный фидбек - об __интересах__ пользователей
- Неявный фидбек - о __предпочтениях__ пользователей


#### 8.1 Basic Assumption

- There are user behavioral patterns that highly correlate with explicit user

#### 8.2 Typical Implicit Feddback Model

- Исследуем действия пользователей
- 
- Считаем, что это хорошие рекомендации
- Добавляем бизнес-правила

10

#### 8.3 Predicting Action

11

#### 8.4 WR-MF (implicit/ALS/implicit ALS)

12
В имплисит подходе есть цель предсказать - совершит ли пользователь некоторое целевое действие 

=> воодим индикаторную функцию (было взаимодействие или нет) - pui = вероятность предсказания целевого действия
pui = 1 - уверенность, что элемент был/будет интересен пользователю
pui = 0 - уверены, что элемент не будет интересен

=> нужно предсказать pui, если в исходных данных pui=0 (раньше не взаимодействовал, а теперь будет)

вводим cui - функция уверенности, 1 - минимум уверенности.

arui - 

Говорим, что хотим смоделировать pui через матричную факторизацию (перемножение двух векторов) 
=> минимизировать значение между историическим значением и предсказанным
=> но если действия не было, то не значит, что оно не произойдет
=> взвешиваем ошибку на конфиденс 
=> не обучаемся

+ слайд с формулами
+ посмотреть еще раз трансляцию

#### 8.5 ALS

Инициализация рандомно.

+ слайд с формулами
+ посмотреть еще раз трансляцию

#### 8.6 Pros & Cons

- Преимущества
    - Промышленный стандарт
    - Более точный, чем memory-base (что не всегда хорошо, склонны к переобучению)
- Недостатки
    - Сложно интерпретировать (перемножаем какие-то вектора)
    - Уязвимы к cold-start и data sparsity problem
    - Линейная модель
    - Требуется обучение модели

### 9. Fold IN

- По умолчанию MF модели не умеют работать с новыми пользователями/наблюдениями.
- Чтобы их добавить, нужно обучать модель заново.
- Не хотим перестраивать матрицу, когда появляются новые items and users.

Идея Fold In: к нам пришли некоторые новые наблюдения и  налету надо перевычислить 1 вектор, остальные вещи оставляем константными.

- Позволяет создавать/обновлять вектора скрытых фич на лету
- Применима ко всем моделям на базе матричной факторизации (MF)
- Идейно похожа на обновлениие вектора пользователя в WR-MF (итеративное обновление)

+ формула со слайда

### 10. Prediction vs Ranking

- Prediction - минимизируем ошибку между предсказанными и ground truth значениями для объекта
- Ranking - пытаемся предсказать правильный порядок объектов

#### 10.1 Bayesian Personalized Ranking (BPR)

### 11. Heteroge
+ Multi-action MF