---
layout: page
title: 재현가능한 과학적 분석을 위한 R
subtitle: 논문 품질 그래프 생성하기
minutes: 70
---



> ## 학습 목표 {.objectives}
>
> * `ggplot2` 팩키지를 사용해서 논문품질 그래프를 생성할 수 있다.
> * 그래픽 문법(Grammar of Graphics)의 기초를 이해한다:
>   - 미학 계층
>   - 기하 계층
>   - 통계량 추가
>   - 척도 변환
>   - 집단별 색깔입히기 혹은 창으로 구분 
>

데이터를 도식화하는 것이 변수 다양한 관계를 재빨리 탐색하는 최상의 방식 중 하나다.

R에는 세가지 주류 도식화 시스템이 존재한다: [기본 도식화 시스템][base],[lattice][lattice]
팩키지, [ggplot2][ggplot2] 팩키지.

[base]: http://www.statmethods.net/graphs/
[lattice]: http://www.statmethods.net/advgraphs/trellis.html
[ggplot2]: http://www.statmethods.net/advgraphs/ggplot2.html

`ggplot2` 팩키지를 학습하는데 이유는 논문품질 그래프 생성에 있어 가장 효과적이기 때문이다.

`ggplot2`는 그래픽 문법에 기반했다. 즉, 어떤 그래프도 동일한 구성요소 집합으로 표현된다: 
**데이터** 집합, **좌표 시스템**, **geoms** 집합 -- 데이터 점에 대한 시각적 표현.

`ggplot2`를 이해하는 핵심은 그림을 계층으로 사고하는 것이다: 
포토샵, 일러스트레이터, 잉크스케이프 같은 이미지 편집 프로그램으로 작업하는 것과 같다.

예제를 가지고 시작해본다:


~~~{.r}
library(ggplot2)
ggplot(data = gapminder, aes(x = lifeExp, y = gdpPercap)) +
  geom_point()
~~~

<img src="fig/08-plot-ggplot2-lifeExp-vs-gdpPercap-scatter-1.png" title="plot of chunk lifeExp-vs-gdpPercap-scatter" alt="plot of chunk lifeExp-vs-gdpPercap-scatter" style="display: block; margin: auto;" />

그래서, 처음으로 수행하는 작업은 `ggplot` 함수를 호출하는 것이다. 
이 함수가 R에게 새로운 그림을 생성하고, 
`ggplot` 함수에 전달하는 어떤 인자도 해당 그림에 *전역* 선택옵션(그림에 있는 모든 계층에 적용)임을 전달한다.

`ggplot`에 인자를 두개 전달했다.
먼저, `ggplot`에 그림에 사용할 데이터가 무엇인지 전달한다; 
번 예제에서 앞에서 불러온 `gapminder`데이터.
두번째 인자를 `aes`함수에 전달했는데, 
`ggplot`에게 **데이터**에 나온 변수를 도식화하는 그림의 *미학적인* 속성에 매핑하는 방법을 전달한다; 
이번 경우에는 **x**와 **y** 위치. 여기서 `ggplot`에 `gapminder`데이터프레임 "lifeExp" 칼럼을 **x**-축에, 
"gdpPercap" 칼럼을 **y**-축에 도식화한다. 
명시적으로 `aes`에 칼럼명을 전달(예를 들어, x = gapminder[, "lifeExp"]`)하지 않은 것에 주목한다. 
이것이 가능한 이유는 `ggplot` 함수가 **데이터**에 존재하는 칼럼을 식별할만큼 똑똑하기 때문이다!

그 자체로, `ggplot`함수를 호출한다고 도식화가 바로 되는 것은 아니다:


~~~{.r}
ggplot(data = gapminder, aes(x = lifeExp, y = gdpPercap))
~~~

<img src="fig/08-plot-ggplot2-unnamed-chunk-2-1.png" title="plot of chunk unnamed-chunk-2" alt="plot of chunk unnamed-chunk-2" style="display: block; margin: auto;" />

`ggplot` 함수에 데이터를 시각적으로 표현하는 방법을 전달할 필요가 있다. 
**geom** 계층을 추가해서 작업이 수행된다. 본 사례에서, `geom_point`를 사용했다; 
**x**와 **y** 사이 관계를 시각적으로 산점도 형태로 표현하도록 `ggplot`에게 전달한다:


~~~{.r}
ggplot(data = gapminder, aes(x = lifeExp, y = gdpPercap)) +
  geom_point()
~~~

<img src="fig/08-plot-ggplot2-lifeExp-vs-gdpPercap-scatter2-1.png" title="plot of chunk lifeExp-vs-gdpPercap-scatter2" alt="plot of chunk lifeExp-vs-gdpPercap-scatter2" style="display: block; margin: auto;" />

> ## 도전과제 1 {.challenge}
>
> 상기예제를 변경해서, 기대수명이 시간에 따라 어떻게 
> 변해왔는지 시각화하는 그림을 생성한다:
>
> 
> ~~~{.r}
> ggplot(data = gapminder, aes(x = lifeExp, y = gdpPercap)) + geom_point()
> ~~~
>
> **힌트:** `gapminder` 데이터셋에 "year"라는 칼럼이 있는데, **x**-축에 나타나야 된다.
>

> ## 도전과제 2 {.challenge}
>
> 이전 예제와 도전과제에서, `aes` 함수를 사용해서 **geom** 산점도로 **x** 와 **y** 지점을 
> 각 점에 대해 표현했다.
> 변경할 수 있는 또다른 *미학적* 속성은 각 점에 대한 *색깔*이다.
> 앞선 도전과제 코드를 변경해서 "continent" 대륙별로 각 점에 **색**을 입힌다.
> 데이터에서 어떤 경향성을 볼 수 있는가? 예상했던 경향성인가?
>

## 계층

산점도가 아마도 시간에 따라 변하는 정보를 시각화하는데 최선은 아니다.
대신에, `ggplot`에 선그림(line plot)으로 데이터를 시각화한다:


~~~{.r}
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country, color=continent)) +
  geom_line()
~~~

<img src="fig/08-plot-ggplot2-lifeExp-line-1.png" title="plot of chunk lifeExp-line" alt="plot of chunk lifeExp-line" style="display: block; margin: auto;" />

`geom_point` 계층을 추가하는 대신에, `geom_line` 계층을 추가했다.
*미학적*으로 **by**를 추가해서, `ggplot`이 각 국가를 직선으로 연결해서 도식화한다.

하지만, 직선과 점을 함께 시각화하려고 하면 어떨까?
단순히, 또다른 계층을 그림에 추가하면 된다:


~~~{.r}
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country, color=continent)) +
  geom_line() + geom_point()
~~~

<img src="fig/08-plot-ggplot2-lifeExp-line-point-1.png" title="plot of chunk lifeExp-line-point" alt="plot of chunk lifeExp-line-point" style="display: block; margin: auto;" />

각 계층은 이전 계층 위에 도식화됨에 주목한다.
이번 예제에서, 점이 직선 *위에* 도식화되었다.
다음에 도식화한 시연이 나와있다:



~~~{.r}
ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country)) +
  geom_line(aes(color=continent)) + geom_point()
~~~

<img src="fig/08-plot-ggplot2-lifeExp-layer-example-1-1.png" title="plot of chunk lifeExp-layer-example-1" alt="plot of chunk lifeExp-layer-example-1" style="display: block; margin: auto;" />

이번 예제에서, *미학적*인 **색상** 매핑이 `ggplot`에 전역으로 설정된 점 선택옵션에서 
`geom_line` 계층으로 이동했다. 그래서, 해당 점에는 더이상 적용되지 않는다.
이제 분명하게 직선 위에 점이 도식화된 것을 확인할 수 있다.

> ## 도전과제 3 {.challenge}
>
> 앞선 예제에서 점과 직선 계층을 뒤바꾼다. 어떻게 될까요?
>

## 변환과 통계량

`ggplot`으로 데이터 위에 통계적 모형을 쉽게 겹치게 할 수 있다.
이를 시연하기 위해서, 첫번째 예제로 되돌아간다:


~~~{.r}
ggplot(data = gapminder, aes(x = lifeExp, y = gdpPercap, color=continent)) +
  geom_point()
~~~

<img src="fig/08-plot-ggplot2-lifeExp-vs-gdpPercap-scatter3-1.png" title="plot of chunk lifeExp-vs-gdpPercap-scatter3" alt="plot of chunk lifeExp-vs-gdpPercap-scatter3" style="display: block; margin: auto;" />

현재, 일인당 GDP에 일부 심각한 이상점이 있어 점사이 내재된 관계를 보기 힘들다.
*scale* 척도함수를 사용해서 y-축 척도를 변경한다.
이러한 제어를 통해 데이터 값과 *미학적인* 시각값 사이 매핑을 제어한다.


~~~{.r}
ggplot(data = gapminder, aes(x = lifeExp, y = gdpPercap)) +
  geom_point() + scale_y_log10()
~~~

<img src="fig/08-plot-ggplot2-axis-scale-1.png" title="plot of chunk axis-scale" alt="plot of chunk axis-scale" style="display: block; margin: auto;" />

그림에 렌더링하기 전에 `log10` 함수가 `gdpPercap` 칼럼값에 변환을 시켰다.
그래서, 각 자리수 10은 변환된 척도에 1씩 증가에 대응된다. 
예를 들어, 1인당 GDP 1,000은 y-축에 3, 10,000은 y-축에 4에 대응된다.
로그 변환은 y-축에 흩어진 데이터 시각화를 쉽게 도와준다.

또다른 계층(`geom_smooth`)을 추가해서 관계를 단순히 적합시킬 수 있다:


~~~{.r}
ggplot(data = gapminder, aes(x = lifeExp, y = gdpPercap)) +
  geom_point() + scale_y_log10() + geom_smooth(method="lm")
~~~

<img src="fig/08-plot-ggplot2-lm-fit-1.png" title="plot of chunk lm-fit" alt="plot of chunk lm-fit" style="display: block; margin: auto;" />

굵은 선은 `geom_smooth` 계층에 *미학적인* **크기**를 설정해서 조정할 수 있다:


~~~{.r}
ggplot(data = gapminder, aes(x = lifeExp, y = gdpPercap)) +
  geom_point() + scale_y_log10() + geom_smooth(method="lm", size=1.5)
~~~

<img src="fig/08-plot-ggplot2-lm-fit2-1.png" title="plot of chunk lm-fit2" alt="plot of chunk lm-fit2" style="display: block; margin: auto;" />

*미학적인* 항목을 명세할 수 있는 방식이 두개 있다.
바로 앞에서 `geom_smooth` 함수에 인자로 전달해서 **크기**에 대한 미학적인 설정을 했다.
앞에서는 `aes` 함수를 사용해서 데이터 변수와 시각적 표현 사이 *매핑*으로 정의했다.

> ## 도전과제 4 {.challenge}
>
> 바로 앞 예제에서 점 계층에 나온 점 크기와 색상을 변경하라.
>
> **힌트:** `aes` 함수를 사용하지 않는다.
>

## 다중-창 그림

앞에서 그림 하나에 모든 국가에 대해 시간의 변화에 따른 기대수명 변화를 시각화했다.
대안으로, **패싯(facet)** 창 계층을 추가해서, 그림을 여러개 창으로 쪼갤 수도 있다:


~~~{.r}
ggplot(data = gapminder, aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country)
~~~

<img src="fig/08-plot-ggplot2-facet-1.png" title="plot of chunk facet" alt="plot of chunk facet" style="display: block; margin: auto;" />

`facet_wrap` 계층은 "공식"을 인자로 받는데, (~) 틸드로 표기한다.
`gapminder` 데이터셋 국가별 칼럼에 유일한 값 각각에 대해 별도 창에 도식화하게 한다.

## 텍스트 변경하기

논문 출판으로 그림을 깔끔하게 만들려면, 텍스트 요소를 일부 변경할 필요가 있다.
x-축이 너무 난잡하고, y-축은 데이터프레임 칼럼명이 아닌 "Life expectancy"으로 적혀야 된다.

몇개 다른 계층을 추가함으로써 텍스트를 변경할 수 있다.
**theme** 계층이 각 축에 대한 텍스트, 전반적인 텍스트 크기를  제어한다.
축 라벨을 변경하는 특수 계층도 있다.
범례 제목을 변경하려면, **scales** 계층을 사용한다.


~~~{.r}
ggplot(data = gapminder, aes(x = year, y = lifeExp, color=continent)) +
  geom_line() + facet_wrap( ~ country) +
  xlab("Year") + ylab("Life expectancy") + ggtitle("Figure 1") +
  scale_fill_discrete(name="Continent") +
  theme(axis.text.x=element_blank(), axis.ticks.x=element_blank())
~~~

<img src="fig/08-plot-ggplot2-theme-1.png" title="plot of chunk theme" alt="plot of chunk theme" style="display: block; margin: auto;" />

지금까지 `ggplot2`로 작업할 수 있는 것을 살짝 맛봤다.
RStudio는 다양한 계층에 대한 정말 유용한 [cheat sheet][cheat]를 제공한다.
더 자세한 문서는 [ggplot2 웹사이트][ggplot-doc]에 공개되어 있다.
마지막으로, 무언가 변경하는 방법에 대해 잘 모르겠으면,
구글 검색창에 연관된 질문을 올리면, 변경해서 바로 사용할 수 있는
재사용가능한 코드가 StackOverflow 사이트에 올라와 있을 것이다!

[cheat]: http://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf
[ggplot-doc]: http://docs.ggplot2.org/current/


> ## 도전과제 5 {.challenge}
>
> 대륙별로 채워진 1인당 GPD 밀도 그래프를 생성하라.
>
> 고급:
> - x-축을 변경해서 쭉 펼쳐진 데이터를 좀더 보기좋게 시각화하라.
> - 패싯 계층을 추가해서 연도별로 밀도 그래프를 창에 도식화하라.
>

## 도전과제 해답

> ## 도전과제 1에 대한 해답 {.challenge}
>
> 상기예제를 변경해서, 기대수명이 시간에 따라 어떻게 
> 변해왔는지 시각화하는 그림을 생성한다:
>
> 
> ~~~{.r}
> ggplot(data = gapminder, aes(x = year, y = lifeExp)) + geom_point()
> ~~~
> 
> <img src="fig/08-plot-ggplot2-ch1-sol-1.png" title="plot of chunk ch1-sol" alt="plot of chunk ch1-sol" style="display: block; margin: auto;" />
>

> ## 도전과제 2에 대한 해답 {.challenge}
>
> 이전 예제와 도전과제에서, `aes` 함수를 사용해서 **geom** 산점도로 **x** 와 **y** 지점을 
> 각 점에 대해 표현했다.
> 변경할 수 있는 또다른 *미학적* 속성은 각 점에 대한 *색깔*이다.
> 앞선 도전과제 코드를 변경해서 "continent" 대륙별로 각 점에 **색**을 입힌다.
> 데이터에서 어떤 경향성을 볼 수 있는가? 예상했던 경향성인가?
>
> 
> ~~~{.r}
> ggplot(data = gapminder, aes(x = year, y = lifeExp, color=continent)) +
>   geom_point()
> ~~~
> 
> <img src="fig/08-plot-ggplot2-ch2-sol-1.png" title="plot of chunk ch2-sol" alt="plot of chunk ch2-sol" style="display: block; margin: auto;" />
>

> ## 도전과제 3에 대한 해답 {.challenge}
>
> 앞선 예제에서 점과 직선 계층을 뒤바꾼다. 어떻게 될까요?
>
> 
> ~~~{.r}
> ggplot(data = gapminder, aes(x=year, y=lifeExp, by=country)) +
>  geom_point() + geom_line(aes(color=continent))
> ~~~
> 
> <img src="fig/08-plot-ggplot2-ch3-sol-1.png" title="plot of chunk ch3-sol" alt="plot of chunk ch3-sol" style="display: block; margin: auto;" />
> 
> 이제야 비로소 직선이 점위에 그려진다!
>


> ## 도전과제 4에 대한 해답 {.challenge}
>
> 바로 앞 예제에서 점 계층에 나온 점 크기와 색상을 변경하라.
>
> **힌트:** `aes` 함수를 사용하지 않는다.
>
> 
> ~~~{.r}
> ggplot(data = gapminder, aes(x = lifeExp, y = gdpPercap)) +
>  geom_point(size=3, color="orange") + scale_y_log10() +
>  geom_smooth(method="lm", size=1.5)
> ~~~
> 
> <img src="fig/08-plot-ggplot2-ch4-sol-1.png" title="plot of chunk ch4-sol" alt="plot of chunk ch4-sol" style="display: block; margin: auto;" />
>

> ## 도전과제 5에 대한 해답 {.challenge}
>
> 대륙별로 채워진 1인당 GPD 밀도 그래프를 생성하라.
>
> 고급:
> - x-축을 변경해서 쭉 펼쳐진 데이터를 좀더 보기좋게 시각화하라.
> - 패싯 계층을 추가해서 연도별로 밀도 그래프를 창에 도식화하라.
>
> 
> ~~~{.r}
> ggplot(data = gapminder, aes(x = gdpPercap, fill=continent)) +
>  geom_density(alpha=0.6) + facet_wrap( ~ year) + scale_x_log10()
> ~~~
> 
> <img src="fig/08-plot-ggplot2-ch5-sol-1.png" title="plot of chunk ch5-sol" alt="plot of chunk ch5-sol" style="display: block; margin: auto;" />
>
