---
layout: page
title: 재현가능한 과학적 분석을 위한 R
subtitle: 도움말 찾기
minutes: 15
---




> ## 학습 목표 {.objectives}
>
> * 함수와 특수 연산자에 대한 R 도움말 파일을 불러 읽어온다.
> * CRAN 작업화면(task view)을 사용해서 문제를 해결하는 팩키지를 식별해 낸다.
> * 동료로부터 도움을 구한다.
>

## 도움말 파일 불러 읽어오기

R과 모든 팩키지는 함수에 대한 도움말 파일을 제공한다.
네임스페이스(인터랙티브 R 세션)에 적재된 팩키지에 있는 특정 함수에 대한 도움말은 다음과 같이 찾는다:


~~~{.r}
?function_name
help(function_name)
~~~

RStudio에 도움말 페이지에 도움말이 표시된다. (혹은 R 자체로 일반 텍스트로 표시된다)

각 도움말 페이지는 절(section)로 구분된다:

 - 기술(Description): 함수가 어떤 작업을 수행하는가에 대한 충분한 기술
 - 사용법(Usage): 함수 인자와 기본디폴트 설정값
 - 인자(Arguments): 각 인자가 예상하는 데이터 설명 
 - 상세 설명(Details): 알아야 만 되는 중요한 구체적인 설명
 - 값(Value): 함수가 반환하는 데이터
 - 함께 보기(See Also): 유용할 수 있는 연관된 함수.
 - 예제(Examples): 함수 사용법에 대한 예제들.

함수마다 상이한 절을 갖추고 있다.
하지만, 상기 항목이 알고 있어야 하는 핵심 내용이다.

> ## Tip: 도움말 파일 불러 읽어오기 {.callout}
>
> R에 대해 가장 기죽게 되는 한 측면이 엄청난 함수 갯수다.
> 모든 함수에 대한 올바른 사용법을 기억하지 못하면,
> 엄두가 나지 않을 것이다.
> 운좋게도,  도움말 파일로 인해 기억할 필요가 없다!
>

## 특수 연산자

특수 연산자에 대한 도움말을 찾으려면, 인용부호를 사용한다:


~~~{.r}
?"+"
~~~

## 팩키지에 대한 도움말 얻기

많은 팩키지에 "소품문(vignettes)"이 따라온다: 활용법과 풍부한 예제를 담은 문서.
어떤 인자도 없이, `vignette()` 명령어를 입력하면 설치된 모든 팩키지에 대한 
모든 소품문 목록이 출력된다;
`vignette(package="package-name")` 명령어는 `package-name` 팩키지명에 대한
이용가능한 모든 소품문 목록을 출력하고, `vignette("vignette-name")` 
명령어는 특정된 소품문을 연다.

팩키지에 어떤 소품문도 포함되지 않는다면, 일반적으로 
`help("package-name")` 명령어를 타이핑해서 도움말을 얻는다.

## 함수가 정확하게 기억나지 않을 때

함수가 어느 팩키지에 있는지 확신을 못하거나, 구체적인 철자법을 모르는 경우,
퍼지 검색을 실행한다:


~~~{.r}
??function_name
~~~

## 어디서 시작해야 될지 아무 생각이 없을 때

어떤 함수 혹은 팩키지가 필요한지 모르는 경우,
[CRAN Task Views](http://cran.at.r-project.org/web/views) 사이트가 
좋은 시작점이 된다. 유지관리되는 팩키지 목록이 필드로 묶여 잘 정리되어 있다.


## 코드가 동작하지 않을 때: 동료로부터 도움을 구한다

함수 사용에 어려움이 있는 경우, 10 에 9 경우에 찾는 정답이
이미 [Stack Overflow](http://stackoverflow.com/)에 답글이 달려 있다.
검색할 때 `[r]` 태그를 사용한다:

원하는 답을 찾지 못한 경우, 동료에게 질문을 만드는데 몇가지 유용한 함수가 있다:


~~~{.r}
?dput
~~~
`dput()` 함수는 작업하고 있는 데이터를 텍스트 파일 형식으로 덤프해서 저장한다.
그래서 다른 사람 R 세션으로 복사해서 붙여넣기 좋게 돕는다.


~~~{.r}
sessionInfo()
~~~



~~~{.output}
R version 3.2.3 (2015-12-10)
Platform: x86_64-apple-darwin13.4.0 (64-bit)
Running under: OS X 10.11.2 (El Capitan)

locale:
[1] ko_KR.UTF-8/ko_KR.UTF-8/ko_KR.UTF-8/C/ko_KR.UTF-8/ko_KR.UTF-8

attached base packages:
[1] stats     graphics  grDevices utils     datasets  base     

other attached packages:
[1] knitr_1.12

loaded via a namespace (and not attached):
[1] magrittr_1.5  formatR_1.2.1 tools_3.2.3   stringi_1.0-1 methods_3.2.3
[6] stringr_1.0.0 evaluate_0.8 

~~~

`sessionInfo()`는 R 현재 버젼 정보과 함께 적재된 팩키지 정보를 출력한다.
이 정보가 다른 사람이 여러분 문제를 재현하고 디버그하는데 유용할 수 있다.

> ## 도전과제 1 {.challenge}
> 
> `c` 연결함수에 대한 도움말을 살펴본다.
> 다음 명령어를 실행하면, 어떤 종류 벡터가 생성될 것으로 예상되는가:
> 
> ~~~{.r}
> c(1, 2, 3)
> c('d', 'e', 'f')
> c(1, 2, 'f')`
> ~~~

> ## 도전과제 2 {.challenge}
> 
> `paste` 함수에 대한 도움말을 살펴본다.
> 나중에 이 함수를 사용할 것이다.
> `sep` 와 `collapse` 인자 사이에 차이는 무엇인가?
> 

## 다른 유용한 도움말 기항지

* [Quick R](http://www.statmethods.net/)
* [RStudio cheat sheets](http://www.rstudio.com/resources/cheatsheets/)
* [Cookbook for R](http://www.cookbook-r.com/)

## 도전과제 해답

> ## 도전과제 1 에 대한 해답 {.challenge}
>
> `c` 함수는 벡터를 생성한다.
> 이 경우 모든 요소는 동일한 자료형이 된다.
> - c(1, 2, 3) : 숫자형  
> - c('d', 'e', 'f') : 문자형  
> - c(1, 2, 'f') : 문자형 - 숫자값이 문자로 "강제 전환"되었다.
>

> ## 도전과제 2 에 대한 해답 {.challenge}
> 
> `paste` 함수에 대한 도움말을 살펴본다.
> 나중에 이 함수를 사용할 것이다.
> 
> 
> ~~~{.r}
> help("paste")
> ?paste
> ~~~
>
