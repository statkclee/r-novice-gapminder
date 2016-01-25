---
layout: page
title: 재현가능한 과학적 분석을 위한 R
subtitle: 데이터프레임과 데이터 불러오기
minutes: 45
---



> ## 학습 목표 {.objectives}
>
> * 데이터프레임과 친숙해 진다.
> * 정규 데이터를 R로 불러 읽어온다.
>

## 데이터프레임

각 칼럼에 다른 원자 자료형이 포함될 수 있음을 제외하면,
데이터프레임(Data frame)은 행렬과 유사하다.
데이터프레임은 정사각형 데이터셋을 조작하고 저장하는 표준 자료구조다.
데이터프레임을 까면, 데이터프레임은 정말 리스트다.
각 요소는 원자 벡터로 동일한 길이를 갖는다는 제약만 추가됐다.
데이터프레임 칼럼 하나를 뽑아내면, 벡터가 된다.
아마도, 데이터프레임이 벡터나 다른 자료구조 보다 더 복잡함을 알게 된다.
하지만, 강력한 성능을 제공한다.


`data.frame` 함수로 데이터프레임을 수작업으로 생성할 수 있다:


~~~{.r}
df <- data.frame(id = c('a', 'b', 'c', 'd', 'e', 'f'), x = 1:6, y = c(214:219))
df
~~~



~~~{.output}
  id x   y
1  a 1 214
2  b 2 215
3  c 3 216
4  d 4 217
5  e 5 218
6  f 6 219

~~~

> ## 도전과제 1: 데이터프레임 {.challenge}
>
> `length` 함수를 사용해서 데이터프레임 `df`에 질의를 던져본다.
> 예상한 결과를 반환하나요?
>

데이터프레임 각 칼럼은 단순히 리스트 요소다. 이런 이유로
데이터프레임에 `length` 길이 질문을 던지게 되면, 칼럼 갯수를 일러준다.
열 갯수를 파악하고자 하면, `nrow` 함수를 사용한다.

데이터프레임에 행과 열을 추가할 때 `cbind` 혹은 `rbind` 함수를 사용한다
(2차원에 대해 연결함수 `c` 와 동등하다):

## 데이터프레임에 칼럼 추가하기

칼럼을 추가하는데, `cbind`를 사용한다:


~~~{.r}
df <- cbind(df, 6:1)
df
~~~



~~~{.output}
  id x   y 6:1
1  a 1 214   6
2  b 2 215   5
3  c 3 216   4
4  d 4 217   3
5  e 5 218   2
6  f 6 219   1

~~~

자동적으로 R이 칼럼에 명칭을 붙임에 주목한다.
칼럼 명칭을 변경하려면 `names` 함수를 사용해서 값을 대입한다:


~~~{.r}
names(df)[4] <- 'SixToOne'
~~~

칼럼을 추가할 때, 칼럼명을 지정할 수도 있다:


~~~{.r}
df <- cbind(df, caps=LETTERS[1:6])
df
~~~



~~~{.output}
  id x   y SixToOne caps
1  a 1 214        6    A
2  b 2 215        5    B
3  c 3 216        4    C
4  d 4 217        3    D
5  e 5 218        2    E
6  f 6 219        1    F

~~~

(`LETTERS` 와 `letters` 는 내장된 붙박이 상수다.)


## 데이터프레임에 행 추가하기

행을 추가하는데, `rbind`를 사용한다:


~~~{.r}
df <- rbind(df, list("g", 11, 42, 0, "G"))
~~~



~~~{.error}
Warning in `[<-.factor`(`*tmp*`, ri, value = "g"): invalid factor level, NA
generated

Warning in `[<-.factor`(`*tmp*`, ri, value = "g"): invalid factor level, NA
generated

~~~

칼럼마다 다수 자료형이 있어, 행을 리스트로 추가함에 주목한다.
그럼에도 불구하고, 예상한 것처럼 동작하지 않는다!
오류 메시지가 무슨 정보를 전달하고 있는가?

R이 요인 수준으로 "g"와 "G"를 덧붙이려고 한 것으로 보인다.
왜 그럴까? 첫번째 칼럼을 면밀히 살펴보자.

`$` 연산자를 사용해서 `data.frame` 칼럼에 접근할 수 있다.


~~~{.r}
class(df$id)
~~~



~~~{.output}
[1] "factor"

~~~



~~~{.r}
str(df)
~~~



~~~{.output}
'data.frame':	7 obs. of  5 variables:
 $ id      : Factor w/ 6 levels "a","b","c","d",..: 1 2 3 4 5 6 NA
 $ x       : num  1 2 3 4 5 6 11
 $ y       : num  214 215 216 217 218 219 42
 $ SixToOne: num  6 5 4 3 2 1 0
 $ caps    : Factor w/ 6 levels "A","B","C","D",..: 1 2 3 4 5 6 NA

~~~

데이터프레임을 생성할 때, 
자동으로 R이 첫째와 마지막 칼럼을 문자벡터가 아니 요인으로 만든다.
"g"와 "G"가 이전에 정의된 수준이 아니다.
그래서, 이런 값을 추가하려고 하면 실패한다.
행이 데이터프레임에 추가되는데, 요인 칼럼에는 결측값만 추가된다:



~~~{.r}
df
~~~



~~~{.output}
    id  x   y SixToOne caps
1    a  1 214        6    A
2    b  2 215        5    B
3    c  3 216        4    C
4    d  4 217        3    D
5    e  5 218        2    E
6    f  6 219        1    F
7 <NA> 11  42        0 <NA>

~~~

이런 문제를 비켜가는 방법이 두개 있다:

1. 요인 칼럼을 문자로 전환한다. 이 방식이 편리하지만,
요인구조가 무너진다.

2. 신규 추가분을 수용하는데, 요인 수준을 추가한다.
정말 해당 칼럼을 요인으로 되게 하려면, 이것이 나아갈 올바른 방법이다.

동일한 데이터프레임에 두가지 해법을 모두 시연한다:


~~~{.r}
df$id <- as.character(df$id)  # 문자로 전환한다
class(df$id)
~~~



~~~{.output}
[1] "character"

~~~



~~~{.r}
levels(df$caps) <- c(levels(df$caps), 'G') # 요인 수준을 추가한다
class(df$caps)
~~~



~~~{.output}
[1] "factor"

~~~

좋아요. 이제 다시 행을 추가해 봅시다.


~~~{.r}
df <- rbind(df, list("g", 11, 42, 0, 'G'))
tail(df, n=3)
~~~



~~~{.output}
    id  x   y SixToOne caps
6    f  6 219        1    F
7 <NA> 11  42        0 <NA>
8    g 11  42        0    G

~~~

성공적으로, 마지막 행을 추가했다. 하지만, 원하지 않는 `NA` 값을 갖는 
행도 있다. 어떻게 삭제할까요?

## 행 삭제하고 `NA` 값 처리하기

`NA`를 담고 있는 행을 삭제하는 방식은 몇가지 있다:


~~~{.r}
df[-7, ]  # 음수 부호는 R에게 해당 행을 삭제하게 지시한다.
~~~



~~~{.output}
  id  x   y SixToOne caps
1  a  1 214        6    A
2  b  2 215        5    B
3  c  3 216        4    C
4  d  4 217        3    D
5  e  5 218        2    E
6  f  6 219        1    F
8  g 11  42        0    G

~~~



~~~{.r}
df[complete.cases(df), ]  # 결측값이 없을 때 `TRUE` 값을 반환한다.
~~~



~~~{.output}
  id  x   y SixToOne caps
1  a  1 214        6    A
2  b  2 215        5    B
3  c  3 216        4    C
4  d  4 217        3    D
5  e  5 218        2    E
6  f  6 219        1    F
8  g 11  42        0    G

~~~



~~~{.r}
na.omit(df)  # 동일한 목적을 갖는 다른 함수
~~~



~~~{.output}
  id  x   y SixToOne caps
1  a  1 214        6    A
2  b  2 215        5    B
3  c  3 216        4    C
4  d  4 217        3    D
5  e  5 218        2    E
6  f  6 219        1    F
8  g 11  42        0    G

~~~



~~~{.r}
df <- na.omit(df)
~~~

## 데이터프레임 조합하기

데이터프레임을 `rbind` 행으로 묶을 수 있다. 하지만, 
행명칭(rownames)에 무슨 일이 발생하는지 주목한다:


~~~{.r}
rbind(df, df)
~~~



~~~{.output}
   id  x   y SixToOne caps
1   a  1 214        6    A
2   b  2 215        5    B
3   c  3 216        4    C
4   d  4 217        3    D
5   e  5 218        2    E
6   f  6 219        1    F
8   g 11  42        0    G
11  a  1 214        6    A
21  b  2 215        5    B
31  c  3 216        4    C
41  d  4 217        3    D
51  e  5 218        2    E
61  f  6 219        1    F
81  g 11  42        0    G

~~~

행명칭(`rownames`)이 유일무이한지 R에서 확인하다.
행명칭을 `NULL`로 설정해서 순차적 번호매김을 되살릴 수 있다:


~~~{.r}
df2 <- rbind(df, df)
rownames(df2) <- NULL
df2
~~~



~~~{.output}
   id  x   y SixToOne caps
1   a  1 214        6    A
2   b  2 215        5    B
3   c  3 216        4    C
4   d  4 217        3    D
5   e  5 218        2    E
6   f  6 219        1    F
7   g 11  42        0    G
8   a  1 214        6    A
9   b  2 215        5    B
10  c  3 216        4    C
11  d  4 217        3    D
12  e  5 218        2    E
13  f  6 219        1    F
14  g 11  42        0    G

~~~


<!-- 
When we add a row we need to use a list, because each column is
a different type!  If you want to add multiple rows to a data.frame,
you will need to separate the new columns in the list:


~~~{.r}
df <- rbind(df, list(c("l", "m"), c(12, 13), c(534, -20), c(-1, -2),
c('H', 'I')))
~~~



~~~{.error}
Warning in `[<-.factor`(`*tmp*`, ri, value = c("H", "I")): invalid factor
level, NA generated

~~~



~~~{.r}
tail(df, n=3)
~~~



~~~{.output}
   id  x   y SixToOne caps
8   g 11  42        0    G
81  l 12 534       -1 <NA>
9   m 13 -20       -2 <NA>

~~~
-->

> ## 도전과제 2 {.challenge}
>
> 여러분에 대한 다음 정보가 담긴 데이터프레임을 생성한다:
>
> * 이름 (First Name)
> * 성 (Last Name)
> * 나이
>
> 그리고 나서, `rbind`를 사용해서 옆 사람에 대한 정보를 추가한다.
>
> 이제, `cbind` 함수를 사용해서 논리형 칼럼을 추가해서,
> "이번 워크샵에서 혼동스러운 것이 있는가?" 라는 질문에 대한 답을 저장한다.
>

## 데이터 불러 읽어오기

앞서 `gapminder` 데이터셋을 얻은 것을 기억하는가?
데이터 출처에 대해 호기심이 있다면, 
[Gapminder 웹사이트](http://www.gapminder.org/data/documentation/)를 방문하기 
바란다.

이제, `gapmidner` 데이터를 R로 불러 읽어보자.
데이터를 불러 읽어오기 전에, 해당 자료를 먼저 살펴보는 것이 좋은 접근법이 된다.
새로 학습한 쉘 기술을 사용해서 데이터를 살펴보자:


~~~{.r}
cd data/ # 프로젝트 폴터 data 디렉토리로 이동한다.
head gapminder-FiveYearData.csv
~~~




~~~{.output}
country,year,pop,continent,lifeExp,gdpPercap
Afghanistan,1952,8425333,Asia,28.801,779.4453145
Afghanistan,1957,9240934,Asia,30.332,820.8530296
Afghanistan,1962,10267083,Asia,31.997,853.10071
Afghanistan,1967,11537966,Asia,34.02,836.1971382
Afghanistan,1972,13079460,Asia,36.088,739.9811058
Afghanistan,1977,14880372,Asia,38.438,786.11336
Afghanistan,1982,12881816,Asia,39.854,978.0114388
Afghanistan,1987,13867957,Asia,40.822,852.3959448
Afghanistan,1992,16317921,Asia,41.674,649.3413952

~~~

파일 확장자가 암시하듯이, 파일에 콤마로 구분된 값(comma-separated values, CSV)을 담겨있다.
그리고 헤더행도 담겨진 것으로 보인다.

`read.table`을 사용해서 해당 데이터를 R로 불러 읽어온다.


~~~{.r}
gapminder <- read.table(
  file="data/gapminder-FiveYearData.csv",
  header=TRUE, sep=","
)
head(gapminder)
~~~



~~~{.output}
      country year      pop continent lifeExp gdpPercap
1 Afghanistan 1952  8425333      Asia  28.801  779.4453
2 Afghanistan 1957  9240934      Asia  30.332  820.8530
3 Afghanistan 1962 10267083      Asia  31.997  853.1007
4 Afghanistan 1967 11537966      Asia  34.020  836.1971
5 Afghanistan 1972 13079460      Asia  36.088  739.9811
6 Afghanistan 1977 14880372      Asia  38.438  786.1134

~~~

데이터 구조를 알기 때문에, `read.table`에 적절한 인자를 명세할 수 있다.
이러한 인자가 없다면, `read.table` 함수가 유의미하도록 최선을 다하지만, 
데이터 구조를 `read.table` 함수에 명시적으로 전달하는 것이 항상
신뢰성을 높인다.
`read.csv` 함수는 CSV 파일을 불러 적재하는데 편리한 단축키를 제공한다.


~~~{.r}
# 상기 명령어에 대한 read.csv 버젼.
gapminder <- read.csv(
  file="data/gapminder-FiveYearData.csv"
)
head(gapminder)
~~~



~~~{.output}
      country year      pop continent lifeExp gdpPercap
1 Afghanistan 1952  8425333      Asia  28.801  779.4453
2 Afghanistan 1957  9240934      Asia  30.332  820.8530
3 Afghanistan 1962 10267083      Asia  31.997  853.1007
4 Afghanistan 1967 11537966      Asia  34.020  836.1971
5 Afghanistan 1972 13079460      Asia  36.088  739.9811
6 Afghanistan 1977 14880372      Asia  38.438  786.1134

~~~

> ## 이것저것 다양한 Tip {.callout}
>
> 1. 자주 마주하게 되는 또다른 파일 형태는 탭-구분 형식이다.
탭을 구분자로 지정하는데, `"\t"`을 사용한다.
>
> 2. 인터넷에서 파일도 불러 읽어올 수 있는데, 파일 경로명을
> 웹 주소로 교체하면 된다.
>
> 3. 엑셀 스프레드쉬트를 일반 텍스트 파일로 먼저 변환하지 않고,
> 직접 `xlsx` 팩키지를 사용해서 불러 읽어올 수 있다.
>

분석을 재현가능하게 만들려면, 코드를 스크립트 파일에 저장해야 된다.
이 점에 대해 추후 살펴볼 것이다.

> ## 도전과제 3 {.challenge}
>
> `file` -> `new file` -> `R script`로 가서 `gapminder` 데이터셋을 
> 불러 읽어오는 R 스크립트를 작성한다.
> `scripts/` 디렉토리에 R 스크립트 파일을 저정하고, 버젼 제어에 추가한다.
>
> `source` 함수를 사용해서 스크립트를 실행한다.
> `source` 함수에 파일 경로를 인자로 넣는다 (혹은 RStudio "source" 버튼을 누른다).
>

## 데이터프레임 사용하기: `gapminder` 데이터셋

방금 학습한 것을 요약하는데, 예제 데이터로 살펴보자
(수년에 걸친 다양한 국가에 대한 기대수명).

R에는 데이터 구조에 대한 정보를 얻어내는데 사용되는 함수가 몇개있다:


~~~{.r}
class() # 자료구조가 무엇인지?
typeof() # 원자 자료형은 무언인지?
length() # 길이는 얼마인지? 2차원 객체는?
attributes() # 메타데이터를 갖고 있는지?
str() # 전체 객체에 대한 전체 요약
dim() # 객체 차원 - nrow(), ncol() 도 시도해 본다.
~~~

상기 함수를 사용해서 `gapminder` 데이터셋을 탐색해 본다.


~~~{.r}
typeof(gapminder)
~~~



~~~{.output}
[1] "list"

~~~

데이터프레임 '내부를 까면' 리스트라는 것을 기억한다.


~~~{.r}
class(gapminder)
~~~



~~~{.output}
[1] "data.frame"

~~~

`gapminder` 데이터는 "데이터프레임"으로 저장되어 있다.
데이터를 불러 읽어올 때 기본디폴트 설정된 자료구조다.
(앞서 들어봤듯이) 혼합된 칼럼 자료형을 저장하는데 유용하다.

칼럼 일부를 살펴보자.

> ## 도전과제 4: 실제 데이터셋에 나오는 자료형 {.challenge}
>
> 앞서 불러온 `gapminder` 데이터프레임 첫 행 6개만 살펴보자:
>
> 
> ~~~{.r}
> head(gapminder)
> ~~~
> 
> 
> 
> ~~~{.output}
>       country year      pop continent lifeExp gdpPercap
> 1 Afghanistan 1952  8425333      Asia  28.801  779.4453
> 2 Afghanistan 1957  9240934      Asia  30.332  820.8530
> 3 Afghanistan 1962 10267083      Asia  31.997  853.1007
> 4 Afghanistan 1967 11537966      Asia  34.020  836.1971
> 5 Afghanistan 1972 13079460      Asia  36.088  739.9811
> 6 Afghanistan 1977 14880372      Asia  38.438  786.1134
> 
> ~~~
>
> 각 칼럼에 대한 자료형이 무엇인지 각자 적어본다.
>



~~~{.r}
typeof(gapminder$year)
~~~



~~~{.output}
[1] "integer"

~~~



~~~{.r}
typeof(gapminder$lifeExp)
~~~



~~~{.output}
[1] "double"

~~~

대륙(continent) 칼럼에 대한 자료형을 무엇이라고 예상하나요?


~~~{.r}
typeof(gapminder$continent)
~~~



~~~{.output}
[1] "integer"

~~~

"문자형"이라고 대답을 예상했다면, 정답을 보고서 놀랄 것이다.
해당 칼럼에 대한 클래스를 살펴보자:


~~~{.r}
class(gapminder$continent)
~~~



~~~{.output}
[1] "factor"

~~~

R의 기본디폴트 설정된 동작 중 하나가 데이터를 불러 읽어올 때 
어떤 텍스트 칼럼도 "요인"으로 처리한다는 것이다.
이런 연유는 텍스트 칼럼이 흔히 범주형 자료를 표현하기 때문이다.
요인은 R에서 통계적 모형 함수로 적절하게 다줘져야 될 필요가 있다.

하지만, 이런 동작은 분명하지는 않고, 많은 사람 손을 거치게 되면 그렇다.
이런 동작을 비활성화 시키고 데이터를 다시 불러 읽어온다.


~~~{.r}
options(stringsAsFactors=FALSE)
gapminder <- read.table(
  file="data/gapminder-FiveYearData.csv", header=TRUE, sep=","
)
~~~

요인으로 자동 변환하는 기능을 끄게 되면, 통계 모형을 돌릴 때 명시적으로 변수를 요인으로 
변환할 필요가 있음을 기억한다.
이러한 선택옵션이 유용할 수 있는데 이유는 생각할 수 있게 해주며,
범주에 대한 순서를 명세하기 쉽도록 한다.

하지만, R 커뮤니티에서 기본 디폴트 설정으로 내버려 두는 것이
더 낫다는 사람들도 많다.

> ## Tip: 선택옵션 변경 {.callout} 
> R이 사작될 때, 프로젝트 디렉토리에 있는 `.Rprofile` 파일에 적힌 코드는 모두 실행된다.
> `.Rprofile` 파일에 변경사항을 저장해서 기본디폴트 동작으로 영구 변경으로 설정할 수 있다.
> 하지만, **주의하라!** R에 대한 기본디폴트 선택옵션을 변경하면,
> 다른 사람이 작성한 프로그램이 여러분 환경에서 올바르게 동작하지 못할 수 있다.
> 역으로도 그렇다. 이런 사유로, 이러한 변경 대부분은 가시적으로 스크립트에 적어놔야 된다는 
> 주장도 있다.
>

데이터를 불러 읽어올 때, 수행해야 하는 첫번째 작업은
설사 명령어가 경고나 오류가 없다손 치더라도 예상한 것과 일치하는지 검사한다.
"structure" 구조를 축약한 `str` 함수가 이런 목적에 정말 유용하다:


~~~{.r}
str(gapminder)
~~~



~~~{.output}
'data.frame':	1704 obs. of  6 variables:
 $ country  : chr  "Afghanistan" "Afghanistan" "Afghanistan" "Afghanistan" ...
 $ year     : int  1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
 $ pop      : num  8425333 9240934 10267083 11537966 13079460 ...
 $ continent: chr  "Asia" "Asia" "Asia" "Asia" ...
 $ lifeExp  : num  28.8 30.3 32 34 36.1 ...
 $ gdpPercap: num  779 821 853 836 740 ...

~~~

객체가 `data.frame` 데이터프레임이고 관측점(행) 1,704 이며 변수(칼럼) 6 임을 
볼 수 있다. 아래에서, 각 칼럼 명칭 다음에, ":" 이 나아고, 해당 칼럼에 변수 자료형과 함께,
첫 몇개 항목이 출력된다.

앞서 논의했듯이, 데이터프레임 칼럼 명칭과 행 명칭을 불러오거나 변경할 수 있다:


~~~{.r}
colnames(gapminder)  
~~~



~~~{.output}
[1] "country"   "year"      "pop"       "continent" "lifeExp"   "gdpPercap"

~~~



~~~{.r}
copy <- gapminder
colnames(copy) <- letters[1:6]
head(copy, n=3)
~~~



~~~{.output}
            a    b        c    d      e        f
1 Afghanistan 1952  8425333 Asia 28.801 779.4453
2 Afghanistan 1957  9240934 Asia 30.332 820.8530
3 Afghanistan 1962 10267083 Asia 31.997 853.1007

~~~

> ## 도전과제 5 {.challenge}
>
> `names` 함수를 사용해서 칼럼 명칭을 변경한 것을 상기한다.
> 어떤 것을 사용할지 문제가 되는가?
> 문제가 되는지 파악하는데 `?names` 과 `?colnames` 도움말을 
> 사용해서 검사한다.



~~~{.r}
rownames(gapminder)[1:20]
~~~



~~~{.output}
 [1] "1"  "2"  "3"  "4"  "5"  "6"  "7"  "8"  "9"  "10" "11" "12" "13" "14"
[15] "15" "16" "17" "18" "19" "20"

~~~

왼쪽 꺾쇠 괄호 숫자가 보이는가?
출력 행에 대한 첫번째 항목 숫자를 나타낸다.
그래서, 5번째 행에 대한 행명칭은 "5" 다.
이번 경우, 행명칭은 단순히 행번호가 된다.

이 정보를 불러 읽어오고 변형하는 연관된 방법이 몇가지 있다.
`attributes`는 행명칭과 칼럼명칭과 함께 클래스 정보도 제공한다.
반면에 `dimnames`는 행명칭과 칼럼명칭만 전달한다.

두 경우 모두, 출력객체는 `list`로 저장된다:


~~~{.r}
str(dimnames(gapminder))
~~~



~~~{.output}
List of 2
 $ : chr [1:1704] "1" "2" "3" "4" ...
 $ : chr [1:6] "country" "year" "pop" "continent" ...

~~~

## 리스트가 함수 출력으로 사용되는 방식을 이해한다.

`gapminder` 데이터셋으로 기본적인 선형 회귀식을 실행한다:


~~~{.r}
# 연도와 기대수명 사이 관계가 어떻게 되나?
reg <- lm(lifeExp ~ year, data=gapminder)
~~~

모형에 대해서 깊이 다루지는 않지만, 간략하게는 다음과 같다:

* `lm` 함수는 선형 통계 모형을 추정한다.
* 첫번째 인자는 공식으로, `a ~ b` 은 `a`가 종속(혹은 반응) 변수로 
   `b` 독립변수의 함수라는 의미다.
* `lm` 함수에 `gapminder` 데이터프레임을 사용하게 지정해서,
`lifeExp` 와 `year` 변수를 찾는 위치를 일러준다. 

출력결과를 살펴보자:


~~~{.r}
reg
~~~



~~~{.output}

Call:
lm(formula = lifeExp ~ year, data = gapminder)

Coefficients:
(Intercept)         year  
  -585.6522       0.3259  

~~~

그다지 많지는 않다? 하지만, 구조를 살펴보면...


~~~{.r}
str(reg)
~~~



~~~{.output}
List of 12
 $ coefficients : Named num [1:2] -585.652 0.326
  ..- attr(*, "names")= chr [1:2] "(Intercept)" "year"
 $ residuals    : Named num [1:1704] -21.7 -21.8 -21.8 -21.4 -20.9 ...
  ..- attr(*, "names")= chr [1:1704] "1" "2" "3" "4" ...
 $ effects      : Named num [1:1704] -2455.1 232.2 -20.8 -20.5 -20.2 ...
  ..- attr(*, "names")= chr [1:1704] "(Intercept)" "year" "" "" ...
 $ rank         : int 2
 $ fitted.values: Named num [1:1704] 50.5 52.1 53.8 55.4 57 ...
  ..- attr(*, "names")= chr [1:1704] "1" "2" "3" "4" ...
 $ assign       : int [1:2] 0 1
 $ qr           :List of 5
  ..$ qr   : num [1:1704, 1:2] -41.2795 0.0242 0.0242 0.0242 0.0242 ...
  .. ..- attr(*, "dimnames")=List of 2
  .. .. ..$ : chr [1:1704] "1" "2" "3" "4" ...
  .. .. ..$ : chr [1:2] "(Intercept)" "year"
  .. ..- attr(*, "assign")= int [1:2] 0 1
  ..$ qraux: num [1:2] 1.02 1.03
  ..$ pivot: int [1:2] 1 2
  ..$ tol  : num 1e-07
  ..$ rank : int 2
  ..- attr(*, "class")= chr "qr"
 $ df.residual  : int 1702
 $ xlevels      : Named list()
 $ call         : language lm(formula = lifeExp ~ year, data = gapminder)
 $ terms        :Classes 'terms', 'formula' length 3 lifeExp ~ year
  .. ..- attr(*, "variables")= language list(lifeExp, year)
  .. ..- attr(*, "factors")= int [1:2, 1] 0 1
  .. .. ..- attr(*, "dimnames")=List of 2
  .. .. .. ..$ : chr [1:2] "lifeExp" "year"
  .. .. .. ..$ : chr "year"
  .. ..- attr(*, "term.labels")= chr "year"
  .. ..- attr(*, "order")= int 1
  .. ..- attr(*, "intercept")= int 1
  .. ..- attr(*, "response")= int 1
  .. ..- attr(*, ".Environment")=<environment: R_GlobalEnv> 
  .. ..- attr(*, "predvars")= language list(lifeExp, year)
  .. ..- attr(*, "dataClasses")= Named chr [1:2] "numeric" "numeric"
  .. .. ..- attr(*, "names")= chr [1:2] "lifeExp" "year"
 $ model        :'data.frame':	1704 obs. of  2 variables:
  ..$ lifeExp: num [1:1704] 28.8 30.3 32 34 36.1 ...
  ..$ year   : int [1:1704] 1952 1957 1962 1967 1972 1977 1982 1987 1992 1997 ...
  ..- attr(*, "terms")=Classes 'terms', 'formula' length 3 lifeExp ~ year
  .. .. ..- attr(*, "variables")= language list(lifeExp, year)
  .. .. ..- attr(*, "factors")= int [1:2, 1] 0 1
  .. .. .. ..- attr(*, "dimnames")=List of 2
  .. .. .. .. ..$ : chr [1:2] "lifeExp" "year"
  .. .. .. .. ..$ : chr "year"
  .. .. ..- attr(*, "term.labels")= chr "year"
  .. .. ..- attr(*, "order")= int 1
  .. .. ..- attr(*, "intercept")= int 1
  .. .. ..- attr(*, "response")= int 1
  .. .. ..- attr(*, ".Environment")=<environment: R_GlobalEnv> 
  .. .. ..- attr(*, "predvars")= language list(lifeExp, year)
  .. .. ..- attr(*, "dataClasses")= Named chr [1:2] "numeric" "numeric"
  .. .. .. ..- attr(*, "names")= chr [1:2] "lifeExp" "year"
 - attr(*, "class")= chr "lm"

~~~

상당한 정보가 중첩 리스트로 저장되어 있다!
구조함수(`str()`)는 이용가능한 모든 데이터를 볼 수 있게 한다.
이번 경우 `lm` 함수가 반환하는 데이터다.

이제, `summary` 함수를 살펴본다:


~~~{.r}
summary(reg)
~~~



~~~{.output}

Call:
lm(formula = lifeExp ~ year, data = gapminder)

Residuals:
    Min      1Q  Median      3Q     Max 
-39.949  -9.651   1.697  10.335  22.158 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)    
(Intercept) -585.65219   32.31396  -18.12   <2e-16 ***
year           0.32590    0.01632   19.96   <2e-16 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 11.63 on 1702 degrees of freedom
Multiple R-squared:  0.1898,	Adjusted R-squared:  0.1893 
F-statistic: 398.6 on 1 and 1702 DF,  p-value: < 2.2e-16

~~~

예상했듯이, 기대수명은 시간이 흐름에 따라 서서히 증가하고 있다.
그래서, 유의적인 양의 관계를 보여준다!


## 도전과제 해답


> ## Solution to Challenge 2 {.challenge}
>
> 여러분에 대한 다음 정보가 담긴 데이터프레임을 생성한다:
>
> * 이름 (First Name)
> * 성 (Last Name)
> * 나이
>
> 그리고 나서, `rbind`를 사용해서 옆 사람에 대한 정보를 추가한다.
>
> 이제, `cbind` 함수를 사용해서 논리형 칼럼을 추가해서,
> "이번 워크샵에서 혼동스러운 것이 있는가?" 라는 질문에 대한 답을 저장한다.
>
> 
> ~~~{.r}
> my_df <- data.frame(first_name = "Software", last_name = "Carpentry", age = 17)
> my_df <- rbind(my_df, list("Jane", "Smith", 29))
> my_df <- rbind(my_df, list(c("Jo", "John"), c("White", "Lee"), c(23, 41)))
> my_df <- cbind(my_df, confused = c(FALSE, FALSE, TRUE, FALSE))
> ~~~

> ## Solution to Challenge 5 {.challenge}
> 
> `names` 함수를 사용해서 칼럼 명칭을 변경한 것을 상기한다.
> 어떤 것을 사용할지 문제가 되는가?
> 문제가 되는지 파악하는데 `?names` 과 `?colnames` 도움말을 
> 사용해서 검사한다.
>
> 
> ~~~{.r}
> m <- matrix(1:9, nrow=3)
> colnames(m) <- letters[1:3] # 예상한 대로 동작한다.
> names(m) <- letters[1:3]  # 행렬을 파괴한다.
> ~~~
