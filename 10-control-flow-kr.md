---
layout: page
title: 재현가능한 과학적 분석을 위한 R
subtitle: 제어 흐름
minutes: 35
---



> ## 학습 목표 {.objectives}
>
> * `if` 와 `else` 를 갖는 조건문을 작성한다.
> * `for` 루프를 이해하고 작성한다.
>

종종 코딩할 때, 동작 흐름을 제어하고 싶을 때가 있다.
한 조건 혹은 조건 집합이 만족될 때만 동작이 일어나게 설정함으로써
이런 작업을 수행한다.
또 다른 방식으로, 특정 횟수만큼 동작이 일어나도록 설정할 수도 있다.

R에서 흐름을 제어하는 방식이 몇가지 있다.
조건문에 대해서, 가장 흔히 사용되는 접근법이 루프 구성체(loop construct)다:


~~~{.r}
# if
if (condition is true) {
  perform action
}

# if ... else
if (condition is true) {
  perform action
} else {  # that is, if the condition is false,
  perform alternative action
}
~~~

예를 들어, 변수 `x`가 특정값을 갖게 되면, 메시지를 출력하게 R에게 지시할 수도 있다:


~~~{.r}
# sample a random number from a Poisson distribution
# with a mean (lambda) of 8

x <- rpois(1, lambda=8)

if (x >= 10) {
  print("x is greater than or equal to 10")
}

x
~~~



~~~{.output}
[1] 8

~~~

옆사람과 동일하지 않는 출력결과를 같지 않음에 주목한다.
왜냐하면, 동일한 분포에서 다른 난수가 표집되어서 그렇다.

초기 숫자를 설정해서 모두 동일한 '의사 난수'를 생성하고 나서,
더 많은 정보를 출력하자:


~~~{.r}
x <- rpois(1, lambda=8)

if (x >= 10) {
  print("x is greater than or equal to 10")
} else if (x > 5) {
  print("x is greater than 5")
} else {
  print("x is less than 5")
}
~~~



~~~{.output}
[1] "x is greater than 5"

~~~

> ## Tip: 의사 난수(pseudo-random numbers) {.callout}
>
> 상기 경우에, `rpois` 함수는 평균(즉 람다) 8을 갖는 포아송 분포를 따르는 
> 난수를 생성한다.
> `set.seed` 함수는 모든 컴퓨터가 정확하게 동일한 
> '의사 난수'([more about pseudo-random numbers](http://en.wikibooks.org/wiki/R_Programming/Random_Number_Generation))를 생성하도록 보장한다. 
> 그래서, `set.seed(10)`으로 설정하면, `x` 에는 값 8 이 대입된다.
> 정확하게 동일한 숫자를 얻어야만 된다.
>

**중요:** R이 `if` 문 내부 조건을 평가할 때,
논리적 요소를 찾는다, 즉 `TRUE` 혹은 `FALSE`. 
이것이 초보자에게 두통을 유발할 수 있다. 예를 들어:


~~~{.r}
x  <-  4 == 3
if (x) {
  "4 equals 3"
}
~~~

실행하면, 벡터 `x` 가 `FALSE` 라서 메시지가 출력되지 않는다.


~~~{.r}
x <- 4 == 3
x
~~~



~~~{.output}
[1] FALSE

~~~

> ## 도전과제 1 {.challenge}
>
> `if` 문을 사용해서 `gapminder` 데이터셋에서
> 2002 년부터 어떤 레코드가 있는지 보고하도록 메시지를 
> 출력하게 한다.
> 2012 년에 대해서도 동일한 작업을 수행한다.
>

다음과 같은 경고 메시지가 나오는 사람이 있나요?


~~~{.error}
Warning in if (gapminder$year == 2012) {: length > 1 이라는 조건이 있고, 첫
번째 요소만이 사용될 것입니다

~~~

작성한 조건문이 하나 이상 논리 요소를 갖는 벡터를 평가하게 되면,
`if` 함수는 쭉 실행되지만, 첫번째 요소에 대한 조건만 평가한다.
따라서, 조건문이 길이 1 이 되도록 확실히 할 필요가 있다.

> ## Tip: `any` 와 `all` 함수 {.callout}
>
> `any` 함수는 벡터에 적어도 값 하나가 `TRUE` 가 되어야만 `TRUE` 값을 반환한다.
> 그렇지 않은 경우 `FALSE` 값을 반환한다.
> 이런 점은 `%in%` 연산자에 유사한 방식으로 적용된다.
> `all` 함수는 명칭에서 나타나듯이, 벡터에 모든 값이 `TRUE` 인 경우에만 
> `TRUE` 값을 반환한다.
>

## 반복 연산자

값 집합에 반복 작업을 수행하고, 반복 순서가 중요하고, 각각에 대해
동일한 연산을 수행하는 경우, `for` 루프가 해당 작업을 실행한다.
앞선 쉘 수업에서 `for` 루프를 살펴봤다.
`for` 루프는 가장 유연한 루프를 도는 연산이지만,
유연성으로 인해 올바르게 사용하기 가장 어렵기도 하다.
반복 순서가 중요하지 않는 경우, `for` 루프 사용을 회피한다:
즉, 반복할 때마다 연산작업은 이전 반복작업 결과에 의존하게 된다.

`for` 루프에 대한 기본 구조는 다음과 같다:


~~~{.r}
for(iterator in set of values){
  do a thing
}
~~~

예를 들어:


~~~{.r}
for(i in 1:10){
  print(i)
}
~~~



~~~{.output}
[1] 1
[1] 2
[1] 3
[1] 4
[1] 5
[1] 6
[1] 7
[1] 8
[1] 9
[1] 10

~~~

`1:10` 은 즉석에서 벡터를 생성한다;
또한, 다른 벡터도 반복할 수 있다.

두개 이상을 반복하는데 또다른 `for` 루프를 중첩할 수도 있다.


~~~{.r}
for (i in 1:5){
  for(j in c('a', 'b', 'c', 'd', 'e')){
    print(paste(i,j))
  }
}
~~~



~~~{.output}
[1] "1 a"
[1] "1 b"
[1] "1 c"
[1] "1 d"
[1] "1 e"
[1] "2 a"
[1] "2 b"
[1] "2 c"
[1] "2 d"
[1] "2 e"
[1] "3 a"
[1] "3 b"
[1] "3 c"
[1] "3 d"
[1] "3 e"
[1] "4 a"
[1] "4 b"
[1] "4 c"
[1] "4 d"
[1] "4 e"
[1] "5 a"
[1] "5 b"
[1] "5 c"
[1] "5 d"
[1] "5 e"

~~~

결과를 바로 출력하는 대신에, 
루프 출력결과를 새로운 객체에 대입할 수도 있다.


~~~{.r}
output_vector <- c()
for (i in 1:5){
  for(j in c('a', 'b', 'c', 'd', 'e')){
    temp_output <- paste(i, j)
    output_vector <- c(output_vector, temp_output)
  }
}
output_vector
~~~



~~~{.output}
 [1] "1 a" "1 b" "1 c" "1 d" "1 e" "2 a" "2 b" "2 c" "2 d" "2 e" "3 a"
[12] "3 b" "3 c" "3 d" "3 e" "4 a" "4 b" "4 c" "4 d" "4 e" "5 a" "5 b"
[23] "5 c" "5 d" "5 e"

~~~

이러한 접근법은 유용할 수도 있지만, '실행결과를 키워나감'
(실행결과 객체를 점진적으로 키워나감) 이런 전략은 컴퓨터 계산 측면으로 보면 비효율적이다.
그래서, 많은 값을 반복할 때는 회피한다.


> ## Tip: 실행결과를 키워나가지 말라! {.callout}
>
> 초보자나 경험많은 R 사용자 모두 저지르는 가장 큰 실수 중 하나가
> 실행결과 객체(벡터, 리스트, 행렬, 데이터프레임)를 루프를 돌리면서
> 키워나가는 것이다.
> 컴퓨터는 이런 것을 처리하는데 매우 좋지 못하다.
> 그래서 연산 속도가 급격히 늦어질 수 있다.
> 미리 적절한 차원을 갖는 빈 연산결과 저장 객체를 정의하는 것이 
> 훨씬 낫다.
> 그래서, 상기처럼 실행결과를 저장할 행렬을 미리 알고 있다면,
> 5 칼럼과 5 열로 구성된 빈 행렬를 생성하고 나서,
> 매번 반복을 돌릴 때마다 적절한 위치에 실행결과를 저장한다.
>

더 좋은 방식은 값을 채워넣기 전에 (빈) 출력결과를 저장할 객체를 정의하는 것이다.
이번 예제의 경우, 더 많은 관련성이 보이지만, 여전히 더 효율적이다.


~~~{.r}
output_matrix <- matrix(nrow=5, ncol=5)
j_vector <- c('a', 'b', 'c', 'd', 'e')
for (i in 1:5){
  for(j in 1:5){
    temp_j_value <- j_vector[j]
    temp_output <- paste(i, temp_j_value)
    output_matrix[i, j] <- temp_output
  }
}
output_vector2 <- as.vector(output_matrix)
output_vector2
~~~



~~~{.output}
 [1] "1 a" "2 a" "3 a" "4 a" "5 a" "1 b" "2 b" "3 b" "4 b" "5 b" "1 c"
[12] "2 c" "3 c" "4 c" "5 c" "1 d" "2 d" "3 d" "4 d" "5 d" "1 e" "2 e"
[23] "3 e" "4 e" "5 e"

~~~

> ## Tip: While 루프 {.callout}
>
> 때로는 특정 조건이 만족될 때까지 연산작업을 반복할 필요도 있다.
> 이런 작업은 `while` 루프로 수행한다.
> 
> 
> ~~~{.r}
> while(this condition is true){
>   do a thing
> }
> ~~~
> 
> 예제로, 0.1 보다 작은 난수가 나올 때까지 0 과 1 사이 균등 분포(`runif` 함수)에서 난수를 뽑아내는
> `while` 루프가 다음에 나와 있다.
> 
> ~~~ {.r}
> z <- 1
> while(z > 0.1){
>   z <- runif(1)
>   print(z)
> }
> ~~~
> 
> `while` 루프가 항상 적절한 것은 아니다.
> 조건이 절대로 만족되지 않는 경우,
> 무한 루프에 빠지지 않도록 특별히 유의해야 된다.
>

> ## 도전과제 2 {.challenge}
>
> `output_vector` 와 `output_vector2` 객체를 비교하라.
> 두 객체는 동일한가?
> 만약 동일하지 않나면, 왜 동일하지 않을까?
> 코드 마지막 블록을 변경해서 어떻게 `output_vector2` 를
> `output_vector` 와 같도록 만들 수 있을까?
>

> ## 도전과제 3 {.challenge}
>
> `gapminder` 데이터 루프를 대륙별로 돌리는 스크립트를 작성한다.
> 그리고 나서 평균 기대수명이 50년 보다 길거나 짧은지 결과를 출력한다.
>

> ## 도전과제 4 {.challenge}
>
> 도전과제 3 에서 나온 스크립트를 변경해서 각 국각별로 루프를 돌린다.
> 이번에는 예상수명이 50 년보다 짧은지, 50 년에서 70 년 사이, 70 년 이상인지 
> 결과를 출력한다.
>

> ## 도전과제 5 - Advanced {.challenge}
>
> `gapminder` 데이터셋에서 각 국가별로 루프를 돌리는 스크립트를 작성한다.
> 국가명이 'B' 로 시작하는지 테스트하고,
> 만약 평균 기대수명이 50 년보다 작은 경우 선그래프로 시간에 대해 기대수명을
> 그래프를 그린다.
>
