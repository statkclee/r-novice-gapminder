---
layout: page
title: 재현가능한 과학적 분석을 위한 R
subtitle: 자료구조
minutes: 45
---




> ## 학습 목표 {.objectives}
>
> - 다양한 자료형에 대해 인지한다.
> - R에서 흔히 마주하는 다양한 기본 자료구조에 대해 인지한다.
> - 자료형, 클래스, 객체 구조에 관해 파악하고자 R에 질문을 던진다.
>

## 자료형

어떤 자료를 분석하기 전에, 기본 자료형과 자료구조에 관해 매우 잘 이해할 필요가 있다.
기본 자료형과 자료구조를 이해하는 것은 **매우 중요**하다.
이유는 R로 매일 솜씨있게 조작하는 것이고, 초보자에게 있어 가장 혼란을 많이 주는 원천이기 때문이다.

R에는 5가지 원자 자료형(더이상 작은 것으로 쪼갤 수 없다는 의미)이 있다:


* 논리형 (예를 들어, `TRUE`, `FALSE`)
* 숫자형
    * 정수형 (예를 들어, `2L`, `as.integer(3)`)
    * 실수형 (즉, 소수점) (예를 들어, `-24.57`, `2.0`, `pi`)
* 복소수형 (즉. 복소수) (예를 들어, `1 + 0i`, `1 + 4i`)
* 텍스트 (R에서는 "문자(character"라고 부름) (예를 들어, `"a"`, `"swc"`, `'This is a cat'`)

R에는 데이터를 취조하여 자료형을 파악하는데 사용되는 함수가 몇가지 있다:


~~~{.r}
typeof() # 해당 데이터의 원자 자료형이 무엇인가?
is.logical() # TRUE/FALSE 논리형 데이터인가?
is.numeric() # 숫자형 데이터인가?
is.integer() # 정수형 데이터인가?
is.complex() # 복소수형 데이터인가?
is.character() # 문자형 데이터인가?
str()  # 데이터가 뭔지 모르겠다?
~~~

> ## 도전과제 1: 자료형 {.challenge}
>
> 지금까지 지식을 총동원해서 변수에 값을 대입한다.
> 다음 특징을 갖는 데이터 예제를 생성한다:
>
> 1) 변수명: 'answer', 자료형: 논리형
> 2) 변수명: 'height', 자료형: 숫자형
> 3) 변수명: 'dog_name', 자료형: 문자형
>
> 생성한 각 변수에 대해서, 의도한 데이터가 생성되었는지 테스트한다.
> 예상한지 못한 것을 발견했는가?
>

## 자료구조

R에서 흔히 마주치는 5가지 자료구조가 있다: 

* 벡터(vector)
* 요인(factor)
* 리스트(list)
* 행렬(matrix)
* 데이터프레임(data.frame)

지금은 좀더 구체적으로 벡터만 집중한다. 이유는 자료형에 대해 더 많이 파악할 수 있기 때문이다.

## 벡터

벡터는 R에서 가장 흔한 기본 자료구조이며 R의 주된 동력원이기도 하다.
원자 벡터로도 종종 불리는데, 이유는 중요하게 **단지 한가지 자료형만 포함할** 수 있기 때문이다.
벡터는 다른 자료구조의 중요 구성요소다.

젝터는 앞에서 소개한 5가지 어떤 자료형도 포함할 수 있다:

* 논리형 (예를 들어, `TRUE`, `FALSE`)
* 정수형 (예를 들어, `2L`, `as.integer(3)`)
* 숫자형 (실수 혹은 소수점) (예를 들어, `2`, `2.0`, `pi`)
* 복소수 (예를 들어, `1 + 0i`, `1 + 4i`)
* 문자형 (예를 들어, `"a"`, `"swc"`)

> ## Tip: "문자 벡터" {.callout}
>
> "문자 벡터" 용어를 경고나 오류 메시지에서 들어봤을 수도 있다. 
> 다소 혼동스럽고, 불운한 명칭이다.
> "문자" 형은 정말 텍스트를 인용부호로 감싼 것을 의미한다는 것을 기억한다.
>

`vector()` 으로 혹은 연결(concatenate) 함수, `c()`로 공벡터를 생성한다.


~~~{.r}
x <- vector()
x
~~~



~~~{.output}
logical(0)

~~~

그래서, 기본디폴트 설정으로, "논리" 자료형을 갖는 공벡터(즉, 길이 0)를 생성한다.


~~~{.r}
x <- vector(length = 10) # 사전에 정의된 길이
x
~~~



~~~{.output}
 [1] FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE

~~~

`FALSE` 갯수를 세면, 10 이 된다.


~~~{.r}
x <- vector("character", length = 10)  # 서전에 정의된 길이와 자료형
x
~~~



~~~{.output}
 [1] "" "" "" "" "" "" "" "" "" ""

~~~

혹은, 연결함수를 사용해서 벡터에 원하는 어떤 값도 결합할 수 있다
(동일한 원자 자료형이기만 하면 된다!).


~~~{.r}
x <- c(10, 12, 45, 33)
x
~~~



~~~{.output}
[1] 10 12 45 33

~~~

연속된 숫자로 벡터를 생성할 수도 있다.


~~~{.r}
series <- 1:10
series
~~~



~~~{.output}
 [1]  1  2  3  4  5  6  7  8  9 10

~~~


~~~{.r}
seq(10)
~~~



~~~{.output}
 [1]  1  2  3  4  5  6  7  8  9 10

~~~


~~~{.r}
seq(1, 10, by = 0.1)
~~~



~~~{.output}
 [1]  1.0  1.1  1.2  1.3  1.4  1.5  1.6  1.7  1.8  1.9  2.0  2.1  2.2  2.3
[15]  2.4  2.5  2.6  2.7  2.8  2.9  3.0  3.1  3.2  3.3  3.4  3.5  3.6  3.7
[29]  3.8  3.9  4.0  4.1  4.2  4.3  4.4  4.5  4.6  4.7  4.8  4.9  5.0  5.1
[43]  5.2  5.3  5.4  5.5  5.6  5.7  5.8  5.9  6.0  6.1  6.2  6.3  6.4  6.5
[57]  6.6  6.7  6.8  6.9  7.0  7.1  7.2  7.3  7.4  7.5  7.6  7.7  7.8  7.9
[71]  8.0  8.1  8.2  8.3  8.4  8.5  8.6  8.7  8.8  8.9  9.0  9.1  9.2  9.3
[85]  9.4  9.5  9.6  9.7  9.8  9.9 10.0

~~~

> ## Tip: 정수 생성하기 {.callout}
>
> 연결함수, `c()`를 사용해서 숫자를 결합할 때,
> 자료형은 자동적으로 "숫자형", 즉 실수/소수점 숫자다.
> 명확하게, 정수형(자연수만) 벡터를 생성하려면, 각 숫자에 L을 추가한다.
> 즉, `c(10L, 12L, 45L, 33L)`.
>

연결함수를 사용해서 벡터에 요소를 추가할 수도 있다:


~~~{.r}
x <- c(x, 57)
x
~~~



~~~{.output}
[1] 10 12 45 33 57

~~~

> ## 도전과제 2 {.challenge}
>
> 벡터에는 한가지 원자 자료형만 담길 수 있다.
> 만약 다른 자료형을 조합하려하면, 
> R은 최소공배수(가장 쉽게 강제해서 우겨넣을 수 있는 자료형) 벡터를 생성한다.
>
> **우선 실행하지 말고, 다음 명령어가 어떤 작업을 수행할지 추측해 보라:**
>
> 
> ~~~{.r}
> xx <- c(1.7, "a")
> xx <- c(TRUE, 2)
> xx <- c("a", TRUE)
> ~~~
>

이것을 묵시적 강제(implicit coerction)라고 부른다.

강제규칙은 다음과 같이 적용된다: `논리형` -> `정수형` -> `숫자형` -> `복소수형` ->
`문자형`.

명시적으로 `as.<class_name>`을 사용해서 벡터를 강제할 수도 있다. 예를 들어,


~~~{.r}
as.numeric()
~~~



~~~{.output}
numeric(0)

~~~



~~~{.r}
as.character()
~~~



~~~{.output}
character(0)

~~~

R은 자동으로 해당 값에 대해 가장 유의미한 어떤 작업도 수행하려 한다:


~~~{.r}
as.character(x)
~~~



~~~{.output}
[1] "10" "12" "45" "33" "57"

~~~


~~~{.r}
as.complex(x)
~~~



~~~{.output}
[1] 10+0i 12+0i 45+0i 33+0i 57+0i

~~~


~~~{.r}
x <- 0:6
as.logical(x)
~~~



~~~{.output}
[1] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE

~~~

이런 점은 많은 프로그래밍 언어에서도 발견되는 특징이다.
0 은 `FALSE`가 되고 다른 숫자는 `TRUE`로 처리된다.
종종 강제조차도 터무니 없는 것에는 동작하지 않는다.

일부 경우에, R은 유의미한 어떤 작업을 수행할 수도 없다:


~~~{.r}
x <- c("a", "b", "c")
as.numeric(x)
~~~



~~~{.error}
Warning: 강제형변환에 의해 생성된 NA 입니다

~~~



~~~{.output}
[1] NA NA NA

~~~



~~~{.r}
as.logical(x)
~~~



~~~{.output}
[1] NA NA NA

~~~

양쪽 모든 경우에, "NA" 벡터가 반환된다. 
첫번째 경우는 경고도 출력된다.

> ## Tip: 특수 객체 {.callout}
>
> "NA"는 R에 있어 특수 객체로 결측값을 표기한다.
> NA 는 벡터 어떤 자료형에도 생겨난다.
> 다른 특수 객체 자료형도 있다:
> `Inf` 는 무한을 표기한다(양수 혹은 음수가 될 수 있다).
> 반면에 `NaN`은 "Not a Number", 정의되지 않는 값(즉, `0/0`)을 의미한다.
> `NULL`은 자료구조가 존재하지 않음(하지만, 리스트 요소로 사용될 수 있음)을 표기한다.
>

벡터 자료구조에 관해서 질문을 던질 수 있다:


~~~{.r}
x <- 0:10
tail(x, n=2) # 마지막 'n' 번째 요소를 뽑아낸다.
~~~



~~~{.output}
[1]  9 10

~~~


~~~{.r}
head(x, n=1) # 첫 'n' 번째 요소를 뽑아낸다.
~~~



~~~{.output}
[1] 0

~~~


~~~{.r}
length(x)
~~~



~~~{.output}
[1] 11

~~~


~~~{.r}
str(x)
~~~



~~~{.output}
 int [1:11] 0 1 2 3 4 5 6 7 8 9 ...

~~~

벡터에 명칭을 붙일 수 있다:


~~~{.r}
x <- 1:4
names(x) <- c("a", "b", "c", "d")
x
~~~



~~~{.output}
a b c d 
1 2 3 4 

~~~

> ## 프로그래머를 위한 고급 Tip {.callout}
>
> 다른 프로그래밍 언어를 경험했다면, 
> 딕셔너리와 해쉬 테이블과 유사한 유용한 도구로 인식할 수도 있다.
> 작은 벡터에 대해서는 사실이지만, 정말 해쉬 테이블 기능을 사용하려면,
> 환경 객체를 사용해야만 된다. 
> `?new.env`을 참조한다.
>

## 행렬

마주칠 것 같은 또다른 자료구조가 행렬이다.
행렬을 까면, 차원 속성이 추가된 정말 원자벡터만 나온다. 

`matrix` 함수로 행렬을 생성한다.
임의 난수 데이터를 생성해보자:


~~~{.r}
set.seed(1) # 난수 생성이 매번 실행될 때마다 같도록 고정시키는 역할을 한다.
x <- matrix(rnorm(18), ncol=6, nrow=3)
x
~~~



~~~{.output}
           [,1]       [,2]      [,3]       [,4]       [,5]        [,6]
[1,] -0.6264538  1.5952808 0.4874291 -0.3053884 -0.6212406 -0.04493361
[2,]  0.1836433  0.3295078 0.7383247  1.5117812 -2.2146999 -0.01619026
[3,] -0.8356286 -0.8204684 0.5757814  0.3898432  1.1249309  0.94383621

~~~


~~~{.r}
str(x)
~~~



~~~{.output}
 num [1:3, 1:6] -0.626 0.184 -0.836 1.595 0.33 ...

~~~

 `rownames`, `colnames`, `dimnames`을 사용해서
 행렬 행과 칼럼 명칭을 설정하거나 불러온다.
 `nrow`, `ncol` 함수(데이터프레임에도 적용됨!)는 행과 열의 갯수를 일러준다.
 반면에, `length` 함수는 요소 갯수를 일러준다.

>
> ## 도전과제 3 {.challenge}
>
> `length(x)` 결과는 무엇이라고 생각합니까?
> 직접 실행해 보세요. 맞췄나요? 왜 그런지/ 왜 그렇지 않나요?
>

>
> ## 도전과제 4 {.challenge}
>
> 또다른 행렬을 생성한다.
> 이번에는 1:50 까지 숫자를 포함하는 10 개 행과 5 개 칼럼을 갖춘다.
> `matrix` 함수는 기본디폴트로 처음 생성시 행과 열로 행렬을 채울 수 있나요?
> 행렬 변경에 대한 방법을 해결할 수 있는지 살펴보라.
> (**힌트:** `matrix`에 대한 문서를 읽어보다!)
>

## 요인

요인(factor)은 범주형 데이터를 표현하는 특수한 벡터다.
요인은 순서를 갖을 수도, 순서를 갖지 않을 수도 있다.
요인은 `aov()`, `lm()`, `glm()` 같은 모형화 함수와 도식화 방법에도 요긴하다.

요인은 사전에 정의된 값만 포함할 수 있다.
`factor` 함수로 요인을 생성할 수 있다:


~~~{.r}
x <- factor(c("yes", "no", "no", "yes", "yes"))
x
~~~



~~~{.output}
[1] yes no  no  yes yes
Levels: no yes

~~~

그래서, 출력결과는 문자 벡터와 매우 유사함을 볼 수 있다.
하지만, 첨부된 수준 구성요소가 있다.
요인 구조를 살펴보면, 좀더 명확하다:


~~~{.r}
str(x)
~~~



~~~{.output}
 Factor w/ 2 levels "no","yes": 2 1 1 2 2

~~~

구조를 통해 중요한 점이 밝혀진다:
요인이 문자 벡터 처럼 보이지만 (종종 동작하지만), 내부를 까보면 실제로 정수다.
상기 예제에서, "no"는 1로, "yes"는 2로 표현됨을 확인할 수 있다.

모형화 함수에서, 기준 수준이 무엇인지 확인하는 것이 중요하다.
기준 수준은 첫번째 요인이지만, 기본디폴트로 순서는 입력된 단어 알파벳 순서로 결정된다.
수준을 명세함으로써 순서를 변경할 수 있다:


~~~{.r}
x <- factor(c("case", "control", "control", "case"), levels = c("control", "case"))
str(x)
~~~



~~~{.output}
 Factor w/ 2 levels "control","case": 2 1 1 2

~~~

이번 경우에, 명시적으로, "control" 이 1 로, "case" 가 2 로 표현되도록 R에 지정했다.
통계적 모형에서 나온 결과를 해석할 때, 이러한 지정이 매우 중요하다!


## 리스트

다양한 자료형을 조합할 때, 리스트 사용이 필요하다.
리스트는 컨테이너처럼 동작한다.
어떤 유형의 자료구조도 포함할 수 있고, 심지어 본인도 포함될 수 있다!

리스트는 `list` 를 사용해서 생성되거나, 
`as.list()` 를 사용해서 다른 객체를 강제로 전환할 수도 있다:


~~~{.r}
x <- list(1, "a", TRUE, 1+4i)
x
~~~



~~~{.output}
[[1]]
[1] 1

[[2]]
[1] "a"

[[3]]
[1] TRUE

[[4]]
[1] 1+4i

~~~

리스트 각 요인은 출력결과에 `[[` 으로 표기된다.
리스트 내부 각 요소는 길이 1 인 원자 벡터다.

리스트는 더 복잡한 객체를 담을 수 있다:


~~~{.r}
xlist <- list(a = "Research Bazaar", b = 1:10, data = head(iris))
xlist
~~~



~~~{.output}
$a
[1] "Research Bazaar"

$b
 [1]  1  2  3  4  5  6  7  8  9 10

$data
  Sepal.Length Sepal.Width Petal.Length Petal.Width Species
1          5.1         3.5          1.4         0.2  setosa
2          4.9         3.0          1.4         0.2  setosa
3          4.7         3.2          1.3         0.2  setosa
4          4.6         3.1          1.5         0.2  setosa
5          5.0         3.6          1.4         0.2  setosa
6          5.4         3.9          1.7         0.4  setosa

~~~

이번 경우에, 리스트는 길이 1 인 문자 벡터, 10개 항목을 갖는 숫자 벡터,
사전에 R에 적재된 데이터셋(`?data`를 참조) 중에서 하나에서 나온 작은 데이터프레임을 포함하고 있다.
각 리스트 요소에 명칭을 부여해서, `[[1]]` 대신에 `$a`을 볼 수 있는 이유를 알 수 있다.

리스트는 또한 리스트 자신도 담을 수 있다:


~~~{.r}
list(list(list(list())))
~~~



~~~{.output}
[[1]]
[[1]][[1]]
[[1]][[1]][[1]]
list()

~~~

> ## 도전과제 5 {.challenge}
>
> 길이 2 를 갖는 리스트를 생성한다.
> 이번에 학습하면서 각 절에 대한 제목을 리스트에 문자 벡터 형태로 포함시킨다:
>
> * 자료형
> * 자료구조
>
> 지금까지 살펴본 자료형과 자료구조 이름을 각 문자 벡터에 덧붙인다.

## 도전과제 해답

> ## 도전과제 1 에 대한 해답 : Data types {.challenge}
>
> 지금까지 지식을 총동원해서 변수에 값을 대입한다.
> 다음 특징을 갖는 데이터 예제를 생성한다:
>
> 1) 변수명: 'answer', 자료형: 논리형
> 2) 변수명: 'height', 자료형: 숫자형
> 3) 변수명: 'dog_name', 자료형: 문자형
>
> 생성한 각 변수에 대해서, 의도한 데이터가 생성되었는지 테스트한다.
> 예상한지 못한 것을 발견했는가?
>
>
> 
> ~~~{.r}
> answer <- TRUE
> height <- 150
> dog_name <- "Snoopy"
> is.logical(answer)
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] TRUE
> 
> ~~~
>
> 
> ~~~{.r}
> is.numeric(height)
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] TRUE
> 
> ~~~
>
> 
> ~~~{.r}
> is.character(dog_name)
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] TRUE
> 
> ~~~
>

> ## 도전과제 2 에 대한 해답  {.challenge}
>
> 벡터에는 한가지 원자 자료형만 담길 수 있다.
> 만약 다른 자료형을 조합하려하면, 
> R은 최소공배수(가장 쉽게 강제해서 우겨넣을 수 있는 자료형) 벡터를 생성한다.
>
> **우선 실행하지 말고, 다음 명령어가 어떤 작업을 수행할지 추측해 보라:**
>
> 
> ~~~{.r}
> xx <- c(1.7, "a")
> xx
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] "1.7" "a"  
> 
> ~~~
> 
> 
> 
> ~~~{.r}
> typeof(xx)
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] "character"
> 
> ~~~
>
> 
> ~~~{.r}
> xx <- c(TRUE, 2)
> xx
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] 1 2
> 
> ~~~
> 
> 
> 
> ~~~{.r}
> typeof(xx)
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] "double"
> 
> ~~~
>
> 
> ~~~{.r}
> xx <- c("a", TRUE)
> xx
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] "a"    "TRUE"
> 
> ~~~
> 
> 
> 
> ~~~{.r}
> typeof(xx)
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] "character"
> 
> ~~~
>

>
> ## 도전과제 3 에 대한 해답  {.challenge}
>
> `length(x)` 결과는 무엇이라고 생각합니까?
> 직접 실행해 보세요. 맞췄나요? 왜 그런지/ 왜 그렇지 않나요?
>
> 
> ~~~{.r}
> x <- matrix(rnorm(18), ncol=6, nrow=3)
> length(x)
> ~~~
> 
> 
> 
> ~~~{.output}
> [1] 18
> 
> ~~~
> 
> 행렬은 단지 추가된 차원 속성만 갖는 벡터로,
> `length` 함수는 행렬에 전체 요소 갯수를 반환한다.
>

>
> ## 도전과제 4 에 대한 해답  {.challenge}
>
> 또다른 행렬을 생성한다.
> 이번에는 1:50 까지 숫자를 포함하는 10 개 행과 5 개 칼럼을 갖춘다.
> `matrix` 함수는 기본디폴트로 처음 생성시 행과 열로 행렬을 채울 수 있나요?
> 행렬 변경에 대한 방법을 해결할 수 있는지 살펴보라.
> (**힌트:** `matrix`에 대한 문서를 읽어보다!)
>
> 
> ~~~{.r}
> x <- matrix(1:50, ncol=5, nrow=10)
> x <- matrix(1:50, ncol=5, nrow=10, byrow = TRUE) # 행 기준으로 채워넣는다.
> ~~~
>


> ## 도전과제 5 에 대한 해답  {.challenge}
>
> 길이 2 를 갖는 리스트를 생성한다.
> 이번에 학습하면서 각 절에 대한 제목을 리스트에 문자 벡터 형태로 포함시킨다:
>
> * 자료형
> * 자료구조
>
> 지금까지 살펴본 자료형과 자료구조 이름을 각 문자 벡터에 덧붙인다.
>
> 
> ~~~{.r}
> my_list <- list(
>   data_types = c("logical", "integer", "double", "complex", "character"),
>   data_structures = c("vector", "matrix", "factor", "list")
> )
> ~~~
>