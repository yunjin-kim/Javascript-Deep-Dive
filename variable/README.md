# 변수

변수(Variable)는 값을 참조하는 값으로 값은 언제든지 변할 수 있지만 이 값을 가리키는 이름은 변하지 않는다

컴퓨터는 모든 데이터를 0, 1로 제어한다
사름은 0과 1로만 컴퓨터를 제어할 수 없기에 사람에게 익숙한 형태로 데이터를 다룬다

익숙한 형타란 숫자, 문자, 날짜 등이 있다
이를 **데이터 타입(Data Type)**이라고 부른다
변수는 이 데이터 타입을 다루기 위한 제 1단계이다

우리가 10이라는 데이터를 다룬다면 10이라는 데이터를 저장할 메모리가 필요하다
컴퓨터의 특정 연산을 거쳐 10이라는 데이터는 메모리 셀에 담기게 된다

메모리는 데이터를 저장할 수 있는 메모리 셀의 집합체이다
각 셀은 고유의 메모리 주소를 갖는다
이 메모리 주소는 메모리 공간의 위치를 나타내며 0부터 시작해서 메모리의 크기만큼 정수로 표현된다

**변수는 하나의 값을 저장하기 위해 확보한 메모리 공간 자체 또는 그 메모리 공간을 식별하기 위해 붙인 이름을 말한다**

변수는 프로그래밍 언어에서 값을 저장하고 참조하는 메커니즘으로 **값의 위치를 가리키는 상징적인 이름**이다


## 식별자

변수 이름을 식별자(Identifier)라고도 한다
**식별자는 어떤 값을 구별해서 식별할 수 있는 고유한 이름을 말한다**

**식별자는 값이 아니라 메모리 주소를 기억하고 있다**
식별자로 값을 구별해서 식별한다는 것은 식별자가 기억하고 있는 메모리 주소를 통해
메모리 공간에 저장한 값에 접근할 수 있다는 의미이다
식별자는 메모리 주소에 붙인 이름이라고 할 수 있다

변수, 함수, 클래스 등의 이름과 같은 식별자는 정의 규칙이 있다

- 식별자는 반드시 글자, $, 밑줄(_) 로 시작한다
- 식별자에는 글자, 숫자, $, 밑줄(_) 만 쓸 수 있다
- 유니코드 문자도 쓸 수 있다
- 예약어는 식별자로 쓸 수 없다


## 변수 선언
변수 선언이란  변수를 생성하는 것을 말한다
자세히 말하면 값을 저장하기 위한 메모리 공간을 확보하고 변수 이름과 확보된 메모리 공간의 주소를 연결해
값을 저장할 수 있게 해주는 것이다

**변수를 사용하려면 반드시 선언이 필요하다. 변수를 선언할 때는 var, let, const 키워드를 사용한다**

``js
const num;
``
위 변수 선언문은 변수 이름을 등록하고 값을 저장할 메모리 공간을 확보한다
변수를 선언한 이후 아직 변수에 값을 할당하지 않았다
따라서 변수 선언에 의해 확보된 메모리 공간에는 자바스크립트 엔진에 의해 undefined가 할당되어 초기화된다
이것은 자바스크립트의 독특한 특징이다

자바스크립트 엔진은 변수 선언을 다음과 같은 단계를 거쳐 수행한다
- 선언 단계: 변수 이름을 등록해서 자바스크립트 어닞ㄴ에 변수의 존재를 알린다
- 초기화 단계: 값을 저장하기 위해 메모리 공간을 확보하고 암묵적으로 undefined를 할당해 초기화한다


## 변수 선언의 실행 시점과 변수 호이스팅
```js
console.log(score); // undefined

let score; // 변수 선언문
```
변수 선언문보다 변수를 참조하는 코드가 앞에 있다
자바스크립트 코드는 인터프러터에 의해 한 줄씩 순차적으로 실행된다
- 인터프러터: 프로그래밍 언어의 소스 코드를 바로 실행하는 컴퓨터 프로그램 또는 환경, 원시 코드를 기계어로 번역하는 컴파일러와 대비된다
console.log가 실행되는 시점에는 아직 score 변수가 실행되지 않았으므로 
참조 에러가 발생할 것 같지만 undefined가 출력된다
그 이유는 **변수 선언이 소스코드가 한 줄씩 순차적으로 실행되는 시점, 즉 런타임이 아니라 그 이전 단계에 먼저 실행되기 때문이다**

자바스크립트 엔진은 소스코드를 실행하기 전에 소스코드의 평가 과정을 거쳐 소스코드를 실행할 준비를 한다
소스코드를 실행을 위한 준비 단계인 소스코드의 평가 과정에서 자바스크립트 엔진은 변수 선언을 포함한
모든 선언문(변수 선언문, 함수 선언문)을 소스코드에서 찾아내 먼저 실행한다
그리고 소스코드의 평가과정이 끝나면 비로소 변수 선언을 포함한 모든 선언문을 제외하고 소스코드를 한줄씩 실행한다
따라서 변수 선언이 소스코드의 어디에 위치하는지와 상관없이 어디서든지 변수를 참조할 수 있다

**변수 선언문이 코드의 선두로 끌어올려진 것처럼 동작하는 자바스크립트의 고유의 특징을 변수 호이스팅**이라 한다

변수 선언뿐만 아니라 var, let, const, function, function*, class 키워드를 사용해서
선언한 모든 식별자(변수, 함수, 클래스 등)는 호이스팅이 된다
모든 선언문은 런타임 이전 단계에서 먼저 실행되기 때문이다


## 값의 할당

변수에 값을 할당할 때는 할당 연산자 **=**을 사용한다
```js
let score; // 변수 선언
score = 80; // 값의 할당
```
하나의 문으로 축약
```js
let score = 80; // 변수 선언과 값의 할당
```
**변수 선언은 소스코드가 순차적으로 실행되는 시점인 런타임 이전에 먼저 실행되지만 값의 할당은 소스코드가 순차적으로 실행되는 런타임에 실행된다**

변수에 값을 할달할 때는 이전 값 undefined가 저장되어 있던 메모리 공간을 지우고 그 메모리 공간에 할당 값 80을 새롭게 저장하는 것이 아니라
새로운 메모리 공간을 확보하고 그곳에 할당 값 80을 저장한다


## 값의 재할당
```js
let score = 80;
score = 100;
```
재할당은 현재 변수에 저장된 값을 버리고 새로운 값을 저장하는 것이다
재할당은 변수에 저장된 값을 다른 값으로 변경한다. 그래서 변수라고 하는 것이다
만약 **값을 재할당할 수 없어서 변수에 저장된 값을 변경할 수 없다면 변수가 아니라 상수라 한다**
상수는 단 한번만 할당할 수 있는 변수다

재할당도 마찬가지로 이전 값이 저장되어 있는 메모리 공간을 지우고 그 메모리 공간에 재할당 값을 저장하는 것이 아니라
새로운 매모리 공간을 확보하고 그 메모리 공간에 재할당 값을 저장한다

이전 값은 어떤 삭별자와 연결되어 있지 않으므로 필요하지 않은 것이다
이러한 불필요한 값드은 가비지 콜렉터에 의해 메모리에서 자동 해제된다
단 메모리에서 언제 해제될지는 예측할 수 없다

- 가비지 콜렉터: 애플리케이션이 할당한 메모리 공간을 주기적으로 검사하여 더 이상 사용되지 않는 메모리를 해제하는 기능이다
              더 이상 사용되지 않는 메모리는 어떤 식별자도 참조하지 않는 메모리 공간을 의미한다
              자바스크립트는 가비지 콜렉터를 내장하고 있는 매니지드 언어로서 가비지 콜렉터를 통해 메모리 누수를 방지한다

- 언매니지드 언어, 매니지드 언어
프로그래밍 언어는 메모리 관리 방식에 따라 언매니지드 언어와 매니지드 언어로 구분할 수 있다
C 언어와 같은 언매니지드 언어는 개발자가 명시적으로 메모리를 할당하고 해제하기 위해 malloc()과 free()같은 저수준 메모리 제어 기능을 제공한다
언매니지드 언어는 메모리 제어를 개발자가 주도할 수 있으므로 개발자의 역량에 따라 최적의 성능을 확보할 수 있지만
그 반대의 경우 치명적인 오류를 생산할 가능성도 있다
자바스크립트 같은 매니지드 언어는 메모리의 할당 및 해제를 위한 메모리 관리 기능을 언어 차원에서 담당하고 개발자의
직접적인 메모리 제어를 허용하지 않는다
더 이상 사용하지 않는 메모리 해제는 가비지 콜렉터가 수행하며 이 또한 개발자가 괸여할 수 없다
매니지드 언어는 개발자의 역량에 의존하는 부분이 상대적으로 작아져 어느 정도 일정한 생산서을 확보할 수 있지만
성능면에서 어느 정도의 손실은 감소할 수 밖에 없다