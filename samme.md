## AdaBoost для задач многоклассовой классификации

Обобщение алгоритма AdaBoost на многоклассовый случай было предложено в [SII-2-3-A8-Zhu](http://ww.web.stanford.edu/~hastie/Papers/SII-2-3-A8-Zhu.pdf). В данной работе авторы предлагают два алгоритма SAMME и SAMME.R, которые представляют собой многоклассовую модификацию AdaBoost для дискретного (когда базовые алгоритмы возвращают только метки классов) и непрерывного (алгоритмы вычисляют вероятности) случаев.

### Алгоритм SAMME (Stagewise Additive Modeling)

#### Дано:
  1. Выборка <img src="https://tex.s2cms.ru/svg/L%20%3D%20%7B%5C%7B(x_i%2C%20y_i)%7D%5C%7D_%7Bi%3D1%7D%5EN%20%3D%20(X%2C%20y)" alt="L = {\{(x_i, y_i)}\}_{i=1}^N = (X, y)" />
  1. Количество базовых моделей <img src="https://tex.s2cms.ru/svg/L" alt="L" />

#### Найти:
> <img src="https://tex.s2cms.ru/svg/%5Chat%7By%7D%20%3A%20R%5ED%20%E2%86%92%20%20%7B%5C%7B(1%2C2%2C...%2CK%7D%5C%7D" alt="\hat{y} : R^D →  {\{(1,2,...,K}\}" />

#### Алгоритм
1. Проинициализируем веса для каждого объекта <img src="https://tex.s2cms.ru/svg/w_1%5Ei%20%3D%20%5Cfrac%7B1%7D%7BN%7D%2C%20i%20%3D%201%2C%20.%20.%20.%20%2C%20N" alt="w_1^i = \frac{1}{N}, i = 1, . . . , N" />
1. Для всех <img src="https://tex.s2cms.ru/svg/l%20%3D%201%2C...%2CL" alt="l = 1,...,L" />
1. Обучим базовый алгоритм <img src="https://tex.s2cms.ru/svg/%5Chat%7By%7D_l" alt="\hat{y}_l" /> при помощи весов <img src="https://tex.s2cms.ru/svg/w_n%5El" alt="w_n^l" />
1. Вычислим взвешенную ошибку <img src="https://tex.s2cms.ru/svg/%5Cvarepsilon_l%20" alt="\varepsilon_l " /> алгоритма <img src="https://tex.s2cms.ru/svg/%5Chat%7By%7D_l" alt="\hat{y}_l" /> :
> <img src="https://tex.s2cms.ru/svg/%CE%B5_l%3D%20%5Csum_%7Bi%3D0%7D%5E%7Bn%7D%20w_i%5El%5B%5Chat%7By%7D_l(X_i)%5Cne%20y_i%5D" alt="ε_l= \sum_{i=0}^{n} w_i^l[\hat{y}_l(X_i)\ne y_i]" />                        

   5. Вычислим вес нового классификатора: 

> <img src="https://tex.s2cms.ru/svg/%CE%B1_l%20%3Dln%5Cfrac%7B1-%5Cvarepsilon_l%7D%7B%5Cvarepsilon_l%7D%2Bln(K-1)" alt="α_l =ln\frac{1-\varepsilon_l}{\varepsilon_l}+ln(K-1)" />

6. Пересчитаем и нормализуем веса: 
> <img src="https://tex.s2cms.ru/svg/%5Cbar%7Bw%7D_i%5E%7Bl%2B1%7D%3Dw_i%5El%5Ccdot%20exp(%5Calpha_l%5Ccdot%5B%5Chat%7By%7D_l(X_i)%5Cne%20y_i%5D%2Ci%3D1%2C..%2CN%2C" alt="\bar{w}_i^{l+1}=w_i^l\cdot exp(\alpha_l\cdot[\hat{y}_l(X_i)\ne y_i],i=1,..,N," />
<img src="https://tex.s2cms.ru/svg/w_i%5E%7Bl%2B1%7D%3D%5Cfrac%7B%5Cbar%7Bw%7D_i%5E%7Bl%2B1%7D%7D%7B%5Csum_%7Bl%3D1%7D%5E%7BL%7D%20%5Cbar%7Bw%7D_i%5E%7Bl%2B1%7D%7D%2Ci%3D1%2C..%2CN" alt="w_i^{l+1}=\frac{\bar{w}_i^{l+1}}{\sum_{l=1}^{L} \bar{w}_i^{l+1}},i=1,..,N" />

7. Конец итерации
8. Финальный прогоном классификатора: 
> <img src="https://tex.s2cms.ru/svg/%5Chat%7By%7D%20(X)%3Darg%5Cmax_%7Bk%7D%5Csum_%7Bl%3D1%7D%5E%7BL%7D%20%5Calpha_l%5B%5Chat%7By%7D_l(X)%3D%20y_i%5D" alt="\hat{y} (X)=arg\max_{k}\sum_{l=1}^{L} \alpha_l[\hat{y}_l(X)= y_i]" />

SAMME является прямым обобщением AdaBoost, так как при K = 2, SAMME становится эквивалентен двухклассовому AdaBoost.

Схематично SAMME изображен на Рис. 1


![Paper written in LaTeX](samme.jpg)

***

Кулакова Виолетта, 895a
