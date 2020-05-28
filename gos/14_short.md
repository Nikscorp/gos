Арчиловское: https://drive.google.com/drive/folders/0B3XUBeyj27tAaHBQSm56LU1zMVE

# 14. Cредние и эмпирические операционные характеристики стратегий распознавания (классификаторов, регрессий). Проблема переобучения. Проблема устойчивости решений. Роль обучающей, валидационной и контрольной выборок при построении распознающей системы. Скользящий контроль (кросс-валидация). Регуляризация на примере линейной регрессии

#### Cредние и эмпирические операционные характеристики стратегий распознавания (классификаторов, регрессий)

Среди задач обучения с учителем можно выделить 2 основные:

+ классификация
+ регрессия

**Классификатором** называется следующее отображение 
$$
\hat{c}: X \rightarrow C, C = \{C_1, C_2, ..., C_k\}
$$
—  конечное множество классов

Проблема обучения для классификации -- обучить такую аппроксимацию $ \hat{c} $ истинной помечающей функции $c$. 

> Примеры классификации: определить кошка, собака или какое-либо еще животное на фотографии (многоклассовая); определить есть ли кошка на фотографии или нету (бинарная).

Для классификаторов оценить качество можно построив **матрицу ошибок**. Предположим, что некоторые данные можно разделить на 2 класса (бинарная классификация).  Таким образом, матрица будет выглядеть следующим образом

|               | $y = 1$                        | $y = 0$                        |
| ------------- | ------------------------------ | ------------------------------ |
| $\hat{y} = 1$ | True Positive                  | False Positive (ошибка 1 рода) |
| $\hat{y} = 0$ | False Negative (ошибка 2 рода) | True Negative                  |

$y$ -- истинный класс объекта

$\hat{y}$ -- предсказанный класс классификатором

Соответственно, на диагонали подобной матрицы будет находиться количество объектов распознанных правильно. В остальных местах -- ошибки. Отмечу также, что подобную матрицу можно построить и для многоклассовой классификации ($|C| > 2$).

По такой матрице можно рассчитать различные **показатели качества классификатора** на размеченной выборке:

+ accuracy (доля правильных ответов алгоритма) = $\frac{TP + TN}{TP + FP + TN + FN}$ -- почти не используется из-за проблем с несбалансированными классами.

+ precision (точность)  = $\frac{TP}{TP + FP}$

+ recall (полнота) = $\frac{TP}{TP + FN}$

Рассмотрим задачу регрессии. **Оценочной функцией** называется отображение:
$$
\hat{f} : X \rightarrow \R
$$
Проблема обучения регрессии заключается в построении оценочной функции по примерам $(x_i, f(x_i))$. 

Регрессионные модели рассчитываются путем применения функции потерь к **невязкам** $f(x) - \hat{f}(x)$ (для упрощения $y - \hat{y}$) -- отличие предсказания $\hat{y}$ от истинного значения $y$

Несколько популярных **функций потерь** $\mathcal{L}(\hat{y}, y)$:

MAE -- Mean Absolute Error = $\frac{1}{m}|y - \hat{y}|$

MSE -- Mean Squared Error = $\frac{1}{m}(y - \hat{y})^2$

где m -- количество элементов 

#### Проблема переобучения

Основным способом поиска закономерностей (процесса обучения) является поиск в некотором априори заданном семействе алгоритмов прогнозирования $M'= {A : X' → Y'}$ алгоритма, наилучшим образом аппроксимирующего связь переменных из набора $X_1 , ... , X_n$ с переменной $Y$ на обучающей выборке, где $X'$ – область возможных значений векторов переменных $X_1 , ... , Xn$(известные переменные); Y' – область возможных значений переменной Y (прогнозируемая величина).

Расширение модели $M' = {A: X' → Y'}$ всегда приводит к повышению точности аппроксимации на обучающей выборке. Однако повышение точности на обучающей выборке, связанное с увеличением сложности модели, часто не ведет к увеличению обобщающей способности. Более того, обобщающая способность может даже снижаться. Различие между точностью на обучающей выборке и обобщающей способностью при этом возрастает. Данный эффект называется **эффектом переобучения**.

#### Проблема устойчивости решений

Под **устойчивыми** обучающими алгоритмами понимаются такие, которые дают решение, незначительно изменяющееся при малом изменении обучающей выборки.

Для многомерной линейной регрессии:

 Задача восстановления линейной регрессии - задача обучения по прецедентам при $Y \rightarrow \R$, связь задается в виде $Y = w_0 + w_1X_1 + . . . + w_nX_n$.  Поиск параметров $w$ ищется при помощи МНК. В итоге получается система вида : $-2X^T y^T + 2X^T Xβ^T = 0$. Решение этой системы существует, если $det(X^TX)$ не равен 0. При сильной коррелированности одной из переменных ${ X_1 , . . . , X_n }$ на выборке с какой-либо линейной комбинацией других переменных значение $det(X^{T}X)$ оказывается близким к 0. При этом вычисленный вектор оценок $β^T$ может сильно изменяться при относительно небольших чисто случайных изменениях вектора $y = (y_1 , . . . , y_m )$ . 

Данное явление называется **мультиколлинеарностью**. Оценивание регрессионных коэффициентов с использованием МНК при наличии мультиколлинеарности оказывается неустойчивым. Требуется число объектов >> число признаков.

#### Роль обучающей, валидационной и контрольной выборок при построении распознающей системы

Поиск алгоритма осуществляется по выборке прецедентов, которая обычно является случайной выборкой объектов из Ω с известными значениями $Y, X_1 , . . . , X_n$ . Выборку прецедентов также принято называть **обучающей выборкой**.

Обобщающая способность может оцениваться по случайной выборке объектов из одной и той же генеральной совокупности, соответствующей исследуемому процессу, которую принято называть **контрольной выборкой**. Контрольная выборка не должна содержать объекты из обучающей выборки.

Валидационная выборка -- выборка из той же ген. совокупности, используется для сравнения обученных моделей между собой и выбор наилучшей.

#### Скользящий контроль (кросс-валидация)

Фиксируется некоторое множество разбиений исходной выборки на две подвыборки: [*обучающую*](http://www.machinelearning.ru/wiki/index.php?title=Обучающая_выборка) и [*контрольную*](http://www.machinelearning.ru/wiki/index.php?title=Контрольная_выборка). Для каждого разбиения выполняется настройка [алгоритма](http://www.machinelearning.ru/wiki/index.php?title=Алгоритм) по обучающей подвыборке, затем оценивается его средняя ошибка на объектах контрольной подвыборки. *Оценкой скользящего контроля* называется средняя по всем разбиениям величина ошибки на контрольных подвыборках.

Если выборка независима, то средняя ошибка *скользящего контроля* даёт несмещённую оценку вероятности ошибки.  

##### Сама процедура:

Выборка ![X^L](http://www.machinelearning.ru/mimetex/?X^L) разбивается ![N](http://www.machinelearning.ru/mimetex/?N) различными способами на две непересекающиеся подвыборки: ![X^L = X^m_n \cup X^k_n](http://www.machinelearning.ru/mimetex/?X^L = X^m_n \cup X^k_n),  где  ![X^m_n](http://www.machinelearning.ru/mimetex/?X^m_n) — обучающая подвыборка длины *m*, ![X^k_n](http://www.machinelearning.ru/mimetex/?X^k_n) — контрольная подвыборка длины ![k=L-m](http://www.machinelearning.ru/mimetex/?k=L-m), ![n=1,\ldots,N](http://www.machinelearning.ru/mimetex/?n=1,\ldots,N) — номер разбиения.

Для каждого разбиения *n* строится алгоритм  ![a_n = \mu(X^m_n)](http://www.machinelearning.ru/mimetex/?a_n = \mu(X^m_n)) и вычисляется значение функционала качества  ![Q_n = Q (a_n, X^k_n)](http://www.machinelearning.ru/mimetex/?Q_n = Q (a_n, X^k_n)). Среднее арифметическое значений ![Q_n](http://www.machinelearning.ru/mimetex/?Q_n) по всем разбиениям называется *оценкой скользящего контроля*:
$$
CV(\mu, X^L) = \frac1{N}\sum_{n=1}^{N}Q(\mu(X^m_n), X^k_n)
$$

Регуляризация на примере линейной регрессии

Задача линейной регрессии: https://habr.com/en/company/ods/blog/322076/

Давайте ограничим пространство гипотез только линейными функциями от m + 1 аргумента, будем считать, что нулевой признак для всех объектов равен единице $x_0 = 1$.

$$
\hat{y}(\overline{x}) = w_0 x_0 + w_1 x_1 + w_2 x_2 + \dots + w_m x_m = \sum_{i=0}^m w_i x_i = \overline{x}^T \overline{w}
$$

Эмпирический риск (функция стоимости) принимает форму среднеквадратичной ошибки:

$$
\mathcal{L}(X, \overline{y}, \overline{w}) = \frac{1}{2n} \sum_{i=1}^n \left(y_i - \overline{x}_i^T \overline{w}_i\right)^2
$$

К этому риску добавляется слагаемое  $λ F(w)$, где $F(w)$ — — регуляризационная функция, $λ$ — параметр, задающий степень влияния регуляризации.
Регуляризация предназначена для регулирования сложности модели и ее  целью является упрощение модели. Это, в частности, помогает бороться с  переобучением и позволяет увеличить обобщающую способность модели.

 Типичные примеры регуляризационных функций:

1. $L_1 = ∑ |w|$
    Известная как LASSO-регуляризация (Least Absolute Shrinkage and  Selection Operator), и, как несложно догадаться из названия, она  позволяет снижать размерность коэффициентов, обращая некоторые из них в  нули.

2. $L_2 = ∑ w^2$
    Иногда ее называют ridge-регуляризацией (гребневая), и она позволяет минимизировать  значения коэффициентов модели, а заодно сделать ее устойчивой к  незначительным изменениям исходных данных.

3. $L_{EN} = \alpha L_1 + (1 - \alpha) L_2$
    Совмещая LASSO и ridge, получаем ElasticNet, которая объединяет два мира со всеми их плюсами и минусами.