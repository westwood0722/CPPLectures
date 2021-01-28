# 함수 템플릿(function template)

다음과 같이 정수 두 수와 실수 두 수를 더하는 add 함수를 고려하자. C++의 함수 오버로딩 기능을 활용하여 다음과 같이 함수를 정의할 수 있다. 

``` cpp
int add(int& x, int& y) { return x+y; }
```

```cpp
double add(double& x, double& y) {return x+y; }
```

두 함수를 살펴보면 입력 파러미터의 자료형만 다를 뿐 알고리즘은 같다. 이 외에 입력 파러미터의 자료혐이 다르면 다른 함수를 정의하여야 한다. 
이와 같이 다른 자료형에 대해서 함수를 정의하면 전체 프로그램의 길이가 늘어나고 작업 도중 실수의 가능성이 존재하고 함수의 알고리즘이 변경되면 오버로딩된 모든 함수의
코드도 수정해 주어야 한다. 
이러한 단점을 해결하기 위해 처라하는 데이터의 자료형이 달라도 알고리즘이 같은 경우 자료형을 일반화 시키고 자료형에 대해 알고리즘을 일반화할 수 있다. C++ 언어에서는 
여러 자료형에 대해 함수나 클래스를 일반화하는 것이 가능하다. 얖에서 살펴보았던 add함수를 일반화한 함수를 정의하기 위해서 C++에서는 ```template``` 이라는 키워드가 사용된다.
```template```은 특정 형태를 가지는 틀의 의미로 C++에서 프로그래머가 원하는 자료형을 명시하면 자료형에 맞는 알고리즘 코드를 만들어 내는 틀이라 생각하면 된다. 
이처럼 템플릿을 사용하여 프로그래밍을 하는 것을 일반화 프로그래밍(Generic Programming)이라고 한다. 

템플릿을 만들기 위해서 template 선언이 필요하다. template 선언은 다음과 같다. 
```cpp
template <typename T>
```
T라는 자료형(타입)에 대한 템플릿을 선언한다는 의미이며 여러 자료형을 가지는 템플릿의 선언은 다음과 같이 하면 된다.
```cpp
template <typename T1, typename T2,..>
```
T, T1, T2 등은 실제 프로그램 시 실제 자료형으로 대체된다. 즉 사용자가 해당 템플릿을 사용하려면 T 자료형이 선언된 위치에 값 또는 변수를 대입하여 템플릿을 호출하면
컴파일 시 대입된 자료형을 기반으로 자료형을 추론하고 해당 자료형에 맞는 함수 또는 클래스를 구체화(specialization)한다. 컴파일러가 함수를 호출할 때 사용되는 자료형에 따라 구체적인 함수의 소스 코드를 만든다. 구체회를 통해 생성된 함수를 구체화된 함수(specialized function)라고 한다.

앞의 add함수를 자료형 T에 대한 템플릿 함수로 졍의 예는 다음과 같다.
```cpp
template <typename T>
T add(T& x, T& y) { return x + y;}
```
구체화하는 과정은 다음과 같다. 

1. 컴파일러는 함수를 호출하는 문장을 컴파일 시 함수 정의를 찾는다.
2. 템플릿으로 선언된 함수 정의를 발견하고 구체화 한다.
3. 구체화: 함수 호출 add(a, b); 문의 파러미터 a, b의 자료형이 int 이면 템플릿의 일반 자료형 T에 int를 대입하여
구체화된 ```int add(int& x, int& y) {...}``` 코드를 만들어 낸다. 
4. 구체화된 함수의 소스코드를 컴파일하고 함수를 실행한다. 

함수 템플릿은 컴파일되지도 호출되지 않는 함수의 틀이다. 템플릿의 역할은 제너릭(generic)함수를 선언하고 컴파일 시점에
구체화하기 위한 틀을 만드는 것이다. 

구체화한 함수 템플릿을 사용한 입력한 두 수를 더하는 예는 다음과 같다.

```cpp
#include <iostream>
using namespace std;

template <class T> //함수 템플릿 선언
T add(T& x, T& y) // 템플릿 인수
{
    return x + y; //함수 템플릿 구체화
}

int main()
{
    int a = 1, b = 3;
    double c = 3.5, d = 2.7;
    
    //정수형인 두 수를 더했을 때
    int ans1 = add(a, b); //템플릿 인수를 int형으로 바꾼 함수 호출
    cout << a << "+" << b << "=" << ans1 << ", ";

    double ans2 = add(a, b); //템플릿 인수를 double형으로 바꾼 함수 호출
    cout << a << "+" << b << "=" << ans2 << ", ";

    //실수형인 두 수를 더했을 때
    int ans3 = add(c, d); //템플릿 인수를 int형으로 바꾼 함수 호출
    cout << c << "+" << d << "=" << ans3 << ", ";

    double ans4 = add(c, d); //템플릿 인수를 double형으로 바꾼 함수 호출
    cout << c << "+" << d << "=" << ans4;

   
    return 0;
}
```
이 프로그램의 결과는 

``1+3=4, 1+3=4, 3.5+2.7=6, 3.5+2.7=6.2`` 이다.

결과가 출력되는데 템플릿 인수를 int형으로 바꿔 함수를 호출하면 실수형끼리 더해도 소수점 이하는 잘려나가게 된다. 
이처럼 함수 템플릿을 사용하면 이처럼 자료형이 다른 경우에도 자료형에 대해 알고리즘을 일반화시킬 수 있다.

## 함수 템플릿의 장점
* 템플릿은 함수의 작성을 쉽고 함수 코드 재사용이 가능하다.
* 소프트웨어의 생산성과 유연성을 높인다. 

## 함수 템플릿의 단점
* 템플릿이 지원되지 않는 컴파일러가 있어 코드의 다른 머신에 포팅에 취약하다. 
* 템플릿 관련 컴파일 오류 메시지 지원이 약해 디버깅에 어려움이 있다.

템플릿을 사용하여 제너릭 함수나 제너릭 를래스를 만들어 프로그램을 작성하는 새로운 프로그래밍 패러다임이 등장하였다. 