## AdaBoost для задач многоклассовой классификации

Впервые AdaBoost (от Adaptive Boosting) был предложен в [Journal of Computer and System Sciences](http://www.face-rec.org/algorithms/Boosting-Ensemble/decision-theoretic_generalization.pdf) (1997) и стал настоящим прорывом в области построения точных алгоритмов на базе «слабых» моделей. Основная идея AdaBoost заключается в перевзвешивании выборки на каждой итерации таким образом, чтобы наиболее сложные объекты получали больший вес. Это позволяет обучить новый алгоритм, который сможет исправить ошибки предыдущих, уделяя усиленное внимание объектам с большим весом. Как и для всех видов бустинга, решающее правило представляет собой взвешенную сумму.

В изначальном виде AdaBoost был способен решать задачи классификации только на два класса. Обобщение алгоритма AdaBoost на многоклассовый случай было предложено в [SII-2-3-A8-Zhu](http://ww.web.stanford.edu/~hastie/Papers/SII-2-3-A8-Zhu.pdf). В данной работе авторы предлагают два алгоритма SAMME и SAMME.R, которые представляют собой многоклассовую модификацию AdaBoost для дискретного (когда базовые алгоритмы возвращают только метки классов) и непрерывного (алгоритмы вычисляют вероятности) случаев.

***
### Алгоритм SAMME (Stagewise Additive Modeling)


#### Дано
  1. К - количество классов, D - количество признаков
  1. Обучающая выборка <img src="https://tex.s2cms.ru/svg/S%20%3D%20%7B%5C%7B(x_i%2C%20y_i)%7D%5C%7D_%7Bi%3D1%7D%5EN%20%3D%20(X%2C%20y)" alt="S = {\{(x_i, y_i)}\}_{i=1}^N = (X, y)" />, где <img src="https://tex.s2cms.ru/svg/x_i%20%E2%88%88%20R%5ED" alt="x_i ∈ R^D" /> — вектор признаков, а <img src="https://tex.s2cms.ru/svg/y_i" alt="y_i" /> принимает значения из конечного множества <img src="https://tex.s2cms.ru/svg/%7B%5C%7B1%2C2%2C...%2CK%7D%5C%7D" alt="{\{1,2,...,K}\}" />
>Выборку S удобно записать в виде матрицы <img src="https://tex.s2cms.ru/svg/X%20%E2%88%88%20R%5E%7BN%5Ctimes%20D%7D" alt="X ∈ R^{N\times D}" />, строками которой являются <img src="https://tex.s2cms.ru/svg/x%5E%E2%8A%A4_i" alt="x^⊤_i" /> , и вектора <img src="https://tex.s2cms.ru/svg/y%20%3D%20(y_1%2C%20.%20.%20.%20%2C%20y_N%20)%5E%E2%8A%A4" alt="y = (y_1, . . . , y_N )^⊤" />:

>   <img src="https://tex.s2cms.ru/svg/S%3D%20(X%2C%20y)" alt="S= (X, y)" />

  3. Количество базовых моделей: <img src="https://tex.s2cms.ru/svg/L" alt="L" />

#### Найти
Hа основе данной выборки построить решающее правило
<img src="https://tex.s2cms.ru/svg/%5Chat%7By%7D%20%3A%20R%5ED%20%E2%86%92%20%20%7B%5C%7B1%2C2%2C...%2CK%7D%5C%7D" alt="\hat{y} : R^D →  {\{1,2,...,K}\}" />, которое для произвольного x предсказывает <img src="https://tex.s2cms.ru/svg/%5Chat%7By%7D(x)" alt="\hat{y}(x)" /> — принадлежность x к одному из K классов. 

#### Алгоритм
1. Проинициализируем веса для каждого объекта <img src="https://tex.s2cms.ru/svg/w_i%5E1%20%3D%20%5Cfrac%7B1%7D%7BN%7D%2C%20i%20%3D%201%2C%20.%20.%20.%20%2C%20N" alt="w_i^1 = \frac{1}{N}, i = 1, . . . , N" />
1. Для всех <img src="https://tex.s2cms.ru/svg/l%20%3D%201%2C...%2CL" alt="l = 1,...,L" />:
> a. Обучим базовый алгоритм <img src="https://tex.s2cms.ru/svg/%5Chat%7By%7D_l" alt="\hat{y}_l" /> при помощи весов <img src="https://tex.s2cms.ru/svg/w_n%5El" alt="w_n^l" />

> b. Вычислим взвешенную ошибку <img src="https://tex.s2cms.ru/svg/%5Cvarepsilon_l%20" alt="\varepsilon_l " /> алгоритма <img src="https://tex.s2cms.ru/svg/%5Chat%7By%7D_l" alt="\hat{y}_l" /> :

> <img src="https://tex.s2cms.ru/svg/%5Cvarepsilon_l%3D%20%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%20w_i%5El%5B%5Chat%7By%7D_l(X_i)%5Cne%20y_i%5D" alt="\varepsilon_l= \sum_{i=1}^{n} w_i^l[\hat{y}_l(X_i)\ne y_i]" />                        

   >c. Вычислим вес нового классификатора: 

> <img src="https://tex.s2cms.ru/svg/(1)%5Calpha_l%20%3Dln%5Cfrac%7B1-%5Cvarepsilon_l%7D%7B%5Cvarepsilon_l%7D%2Bln(K-1)" alt="(1)\alpha_l =ln\frac{1-\varepsilon_l}{\varepsilon_l}+ln(K-1)" /> 

> d. Пересчитаем и нормализуем веса: 

> <img src="https://tex.s2cms.ru/svg/%5Cbar%7Bw%7D_i%5E%7Bl%2B1%7D%3Dw_i%5El%5Ccdot%20exp(%5Calpha_l%5Ccdot%5B%5Chat%7By%7D_l(X_i)%5Cne%20y_i%5D%2Ci%3D1%2C..%2CN%2C" alt="\bar{w}_i^{l+1}=w_i^l\cdot exp(\alpha_l\cdot[\hat{y}_l(X_i)\ne y_i],i=1,..,N," />

  > <img src="https://tex.s2cms.ru/svg/w_i%5E%7Bl%2B1%7D%3D%5Cfrac%7B%5Cbar%7Bw%7D_i%5E%7Bl%2B1%7D%7D%7B%5Csum_%7Bl%3D1%7D%5E%7BL%7D%20%5Cbar%7Bw%7D_i%5E%7Bl%2B1%7D%7D%2Ci%3D1%2C..%2CN" alt="w_i^{l+1}=\frac{\bar{w}_i^{l+1}}{\sum_{l=1}^{L} \bar{w}_i^{l+1}},i=1,..,N" />


3. Финальный прогон классификатора: 
> <img src="https://tex.s2cms.ru/svg/%5Chat%7By%7D%20(X)%3Darg%5Cmax_%7Bk%7D%5Csum_%7Bl%3D1%7D%5E%7BL%7D%20%5Calpha_l%5B%5Chat%7By%7D_l(X)%3D%20y%5D" alt="\hat{y} (X)=arg\max_{k}\sum_{l=1}^{L} \alpha_l[\hat{y}_l(X)= y]" />

***

Алгоритм SAMME имеет одну и ту же структуру что и AdaBoost, с отличием в вычислении веса классификатора <img src="https://tex.s2cms.ru/svg/(1)" alt="(1)" />. 

Обобщение функции потерь на многоклассовый случай следует естественным образом: 

<img src="https://tex.s2cms.ru/svg/%20L(y%2Cf)%20%3D%20exp%20(-%5Cfrac%7B1%7D%7BK%7D(y_1f_1%20%2B...%2By_Kf_K))%20%3D%20exp(-%5Cfrac%7B1%7D%7BK%7Dy%5ETf)%20" alt=" L(y,f) = exp (-\frac{1}{K}(y_1f_1 +...+y_Kf_K)) = exp(-\frac{1}{K}y^Tf) " />

где <img src="https://tex.s2cms.ru/svg/f_1%20%2B%20...%20%2B%20f_K%20%3D%200" alt="f_1 + ... + f_K = 0" /> (симметричное ограничение);
   

Для двухклассовых задач классификации частота ошибок случайного предположения равна <img src="https://tex.s2cms.ru/svg/1%2F2" alt="1/2" />. В многоклассовом случае гораздо труднее добиться того, чтобы частота ошибок случайного угадывания была <img src="https://tex.s2cms.ru/svg/(K%20-%201)%20%2F%20K" alt="(K - 1) / K" />. Как отмечают создатели AdaBoost, основным недостатком AdaBoost является то, что он не способен обрабатывать слабых учащихся с частотой ошибок больше <img src="https://tex.s2cms.ru/svg/1%2F2" alt="1/2" />. AdaBoost может легко выйти из строя в случае с несколькими классами. 

Очевидно, что при K = 2 SAMME сводится к AdaBoost. 

Непосредственным следствием является то, что теперь для того, чтобы <img src="https://tex.s2cms.ru/svg/%5Calpha_l" alt="\alpha_l" /> была положительной, необходимо только <img src="https://tex.s2cms.ru/svg/(1-%5Cvarepsilon_l)%3E%201%20%2F%20K" alt="(1-\varepsilon_l)&gt; 1 / K" /> , то есть чтобы точность каждого «слабого» классификатора была лучше, чем раннее угадывание (= <img src="https://tex.s2cms.ru/svg/1%2F2" alt="1/2" />).

![Paper written in LaTeX](samme.jpg)

Как видно из рисунка, тестовая ошибка SAMME быстро уменьшается до низкого значения и продолжает уменьшаться даже после 600 итераций, что  и должно ожидаться от успешного алгоритма ускорения.
 


***
Кулакова Виолетта, 895a
