# 03 정규화 논리(첫 번째) - 함수 종속성

# 왜 DB 설계가 중요한가?

데이터의 조작인 쿼리는 DB에 포함된 각 테이블이 적절히 설계돼 있지 않으면 깔끔하게 표현할 수 없다.

관계형 모델의 세계에는 왕도라고 불리는 DB 설계 이론이 있다.

그것이 정규화 이론이다.

# 정규화

## 관계형 모델을 보완하는 이론

정규화 이론이 관계형 모델의 일부는 아니다.

관계형 모델 자체는 릴레이션이 정규화되어 있지 않아도 동일하게 처리할 수 있다.

물론 정규화를 하는 편이 좋은 DB 설계라고 할 수 있다.

위치적으로는 관계형 모델을 보완하는 이론이다.

## 변칙을 방지할 수 있다

정규화의 이점 중 가장 중요한 것은 모순을 방지할 수 있다는 것이다.

모순이란 데이터가 논리적인 불일치가 일어나는 상태를 말한다.

모순이 발생한 상태를 변칙이라고 한다. 

변칙을 방지할 수 있는 것이 정규화가 맡은 가장 큰 역할이다.

### **변칙이 생기는 예**

아래 오민혁의 학년 속성에 두 개의 다른 값이 포함된다.

같은 사람에 대한 학년은 같은 값이어야 한다.

아래처럼 모순이 생기는 상태가 변칙이다.

| 이름 | 학과 | 학년 |
| --- | --- | --- |
| 오민혁 | 관계형 모델 | 1 |
| 오민혁 | 컴퓨터 아키텍처 | 2 |

### **릴레이션의 설계가 상식적이지 않다**

위 그림에 학년이라는 속성이 포함된 것이 이상하다고 생각할지도 모른다.

이런 릴레이션의 부자연스러움은 DB 설계에 문제가 있기 때문이다.

릴레이션의 설계를 잘못한 결과 데이터에 모순이 생긴다.

상식적으로 생각하고 설계해도 방심하면 안된다.

위 같은 실수는 응용프로그램의 변경이 반복될수록 발생할 가능성이 커진다.

**변칙을 일으키는 원인은 중복**

중복이란 한 개의 릴레이션에 같은 데이터가 여러 개 존재하는 것을 말한다.

중복이 있으면 사소한 갱신의 실수로 모순이 생긴다.

- 위 릴레이션에서 오민혁의 학년을 한 개의 행만 3으로 업데이트하면 모순이 생긴다.

모순이 생기지 않게 하려면 중복을 제거하면 된다.

이런 중복을 제거하는 작업이 정규화 이론이다.

# 정규형

정규화는 높은 단계로 갈수록 더 좋은 상태(중복이 적은 상태)가 된다.

## 정규형의 종류

- 제 1 정규형 (1NF)
- 제 2 정규형 (2NF)
- 제 3 정규형 (3NF)
- 보이스코드 정규형 (BCNF)
- 제 4 정규형 (4NF)
- 제 5 정규형 (5NF)
- 제 6 정규형 (6NF)

DB 설계에서 즁요한 것은 BCNF와 5NF 다.

## 제 1 정규형(1NF)

테이블이 1NF가 되기 위한 요건은 다음과 같다.

1. 행이 위에서 아래로 정렬돼 있지 않다.
2. 열이 왼쪽에서 오른쪽으로 정렬돼 있지 않다.
3. 중복되는 행이 존재하지 않는다.
4. 열의 값은 도메인(데이터형)에 속하는 요소의 값을 딱 한 개만 가진다.
5. 모든 열의 값은 정의된 것이어야 하고 각 행은 항상 존재한다.

### 칼럼이나 행의 순서

SQL의 사양으로 테이블의 칼럼에는 순서가 존재한다.

따라서 엄밀히 말하면 테이블은 1NF의 조건을 만족하지 않는다.

이 점은 문제가 되지 않게 할 수 있는데, 칼럼이나 행의 위치에 의존하는 쿼리를 작성하지 않는 것이다.

- SELECT * 로 모든 칼럼의 값을 검색하고 응용프로그램이 칼럼의 위치에 따라 데이터에 접근하는 것
- ORDER BY 절의 인수로 select list 내에서 칼럼의 위치를 지정하는 것 (ORDER BY 1)

또한 칼럼뿐 아니라 행의 순서가 있는 것도 있다.

- ROWID나 ObjectID 같은 것이 그 예이다.

1NF를 만족하기 위해서 이러한 기능을 사용해서는 안된다.

### 중복되는 행을 제거

단순히 똑같은 행의 값이 최대 1개 이상 포함되지 않으면 된다.

따라서 테이블에 기본키나 유니크키처럼 고유성 제약 조건을 붙이면 된다.

중요한 것은 제약 그 자체가 아니라 실제로 저장되는 값이 중복되지 않게 한다는 점에 유의한다.

행의 중복을 해결했다고 해도 정규화는 아니다.

### NULL이 포함되면 안 된다

NULL이 포함돼 있으면 관계형 모델은 무너진다.

테이블에 NULL이 포함되지 않게 하려면 NOT NULL 제약을 걸어서 해결되는 문제가 아니다.

- 만약 현재 NULL인 칼럼에 NOT NULL 제약을 걸면 프로그램이 기본값을 할당한다.
- 나이를 나타내는 칼럼에 NULL 대신에 -1이나 1000 같은 임시 값을 넣어도 안된다.

가장 좋은 방법은 테이블을 나누는 것이다.

칼럼의 값이 NULL이라는 것은 프로그램이 아직 그 데이터가 필요없기 때문이다.

그러므로 다른 단계에 필요한 데이터이고 현재 테이블에는 그 칼럼이 필요가 없다.

### 값의 원자성

관계형 모델에서는 의미가 있는 한 묶음의 데이터를 한 단위로 취급해야 한다.

- 메일 주소를 분해해 버리면 그 문자열은 의미가 없어진다.

열의 값은 도메인 중에서 골라낸 요소의 하나라고 정의하면 혼란은 없다.

컬럼의 값이 도메인에 속하는 값이어야 한다는 말은 반대로 말하면 그렇게 되도록 도메인을 설계해야 한다는 말이다.

### 반복 그룹

1NF의 조건을 만족하지 않는 테이블의 예로 반복그룹이 있다.

반복그룹의 예는 한 개의 열에 여러 개의 값을 콤마와 같은 구분자 등의 형태로 할당한 것이다.

이것은 쿼리가 복잡하고 데이터의 무결성을 지키기 어려워서 프로그램에서 구현할 로직이 늘어나고, 개발 비용이 증가한다.

| 이름 | 학과 |
| --- | --- |
| 오민현 | 데이터베이스,
컴파일러 |
| 한혜린 | 데이터베이스 |

반복 그룹을 해결하는 방법은 반복 그룹 대신에 여러 개의 행에 데이터를 저장하면 된다.

| 이름 | 학과 |
| --- | --- |
| 오민현 | 데이터베이스 |
| 한혜린 | 데이터베이스 |
| 오민현 | 컴파일러 |

이처럼 변경하면 키가 바뀐다는 점에 주의해야한다.

## 후보키와 슈퍼키

후보키는 릴레이션에 포함된 튜플의 값을 고유하게 하는 속성의 집합으로 기약을 말한다.

- 기약은 더는 속성을 줄일 수 없는, 여분의 속성이 없는 상태를 말한다.

후보키는 여러 종류가 나올 수 있다.

SQL은 테이블에 기본키(primary key)가 있지만, 관계형 모델은 기본키 개념이 없다.

릴레이션에 여러 개의 후보키가 있는 경우 이 키들은 어떤 것도 의미적으로나 기능적으로 차이가 없고 어떤 키를 정할지는 주관적인 문제기 때문이다.

후보키는 추가 속성이 없는 속성의 집합이다. 

후보키의 슈퍼셋, 즉 추가 속성을 가지는 키를 슈퍼키라고 한다.

슈퍼키도 튜플의 값이 고유하다는 점은 후보키와 같다. 단순히 추가 속성을 가지고 있다.

후보키는 슈퍼키의 일종이며 가지고 있는 속성의 수가 최소한이다.

## 함수 종속성(FD)

2NF ~ BCNF는 함수 종속성에 관한 정의이며 전부 함수 종속성을 이용해 설명할 수 있다.

릴레이션 내의 함수 종속성을 최대한 배제한 것이 BCNF다.

함수 종속성의 정의는 다음과 같다.

![image](https://user-images.githubusercontent.com/43159295/189659461-3599bc9e-12a5-4fd4-9f14-39cd4ec727cc.png)


A의 값을 알면 B의 값을 알 수 있다는 의미다. 

A 값이 다르고 B의 값이 같아도 문제없다.

B는 중복이 허용된다.

참고 [https://developer111.tistory.com/80](https://developer111.tistory.com/80)

함수 종속성이란 키의 성질을 정의한 것이라고 할 수 있다.

일반적으로 키의 값이 정해지면 같은 튜플에 포함된 임의의 속성의 값을 구할 수 있다.

2NF ~ BCNF 정규화는 자명하지 않은 함수 종속성을 없애는 작업이다.

## 제 2 정규형 (2NF)

2NF는 후보키의 진부분집합에서 키가 아닌 속성에 함수 종속성을 제거하는 작업이다.

- 진부분집합이란 부분집합 중 원래 자신의 집합을 제외한 것을 말한다.

이런 함수 종속성을 부분 함수 종속성이라고 한다.

릴레이션이 1NF이며 부분 함수 종속성을 갖지 않으면 2NF가 된다.

- 1NF이며 후보키가 한 개의 속성으로 된 릴레이션은 자동으로 2NF가 된다.

![image](https://user-images.githubusercontent.com/43159295/189659530-7cbffb7f-9a19-4b59-9891-89f03281a58a.png)

아래는 이름 → 학년 이라는 함수 종속이 존재한다.

| 이름 | 학과 | 학년 |
| --- | --- | --- |
| 오민현 | 데이터베이스 | 2 |
| 한혜린 | 데이터베이스 | 1 |
| 오민현 | 컴파일러 | 2 |

### 무손실 분해

함수 종속성을 해결하려면 한 개의 릴레이션을 여러 릴레이션으로 분해해야 한다.

분해할 때 필요한 작업은 프로젝션이다.

한 개의 릴레이션에 대해 다른 두 개의 패턴의 프로젝션을 해 두 개의 릴레이션을 생성한다.

이때 원래 정보를 잃어버리지 않게 두 개의 릴레이션으로 분해할 수 있어서 무손실 분해라고 한다.

무손실 분해란 분해된 릴레이션의 정보를 사용해 원래의 릴레이션을 재구축할 수 있는 것을 말한다.

재구축 할 때 JOIN을 사용한다.

무손실 분해가 가능한지 아닌지의 기준은 함수 종속성과 같은 종속성이다.

함수 종속성이 존재할 때는 종속 관계가 있는 속성만 프로젝션해 한 개의 릴레이션으로 만든다.

- 위 예는 이름 → 학년으로 프로젝션을 수행한다.

다른 패턴으로 프로젝션을 실행하면 정보가 손실된다.

- 이름, 학과  학과, 학년 으로 프로젝션을 해 분해하면 join을 사용해 원래대로 되도릴 수 없다.

| 이름 | 학과 |
| --- | --- |
| 오민현 | 데이터베이스 |
| 한혜린 | 데이터베이스 |
| 오민현 | 컴파일러 |

| 이름 | 학년 |
| --- | --- |
| 오민현 | 2 |
| 한혜린 | 1 |

## 제 3 정규형 (3NF)

3NF는 추이 함수 종속성이라는 함수 종속성을 제거하는 작업이다.

추이 함수 종속성은 키가 아닌 속성 사이의 함수 종속성이다.

- 두 개의 키가 아닌 속성의 집합 X, Y에 함수 종속성 X → Y가 존재한다고 가정
- X의 값은 슈퍼키의 값을 알면 정해진다.
- X의 값이 정해지면 Y의 값도 정해진다.
- 이처럼 단계적인 함수 종속성을 추이라고 표현한다.

![image](https://user-images.githubusercontent.com/43159295/189659588-a55db30e-5ad5-4363-9fcd-028b6372c1b1.png)

학과의 대표번호는 한 개 뿐이다.

학과 → 대표번호 라는 함수 종속성이 존재한다.

| 이름 | 학과 | 대표번호 |
| --- | --- | --- |
| 오민현 | 데이터베이스 | xx-xxx |
| 한혜린 | 데이터베이스 | xx-xxx |
| 오민현 | 컴파일러 | yy-yyy |

2NF와 같이 무손실 분해를 통해 분리한다.

| 이름 | 학과 |
| --- | --- |
| 오민현 | 데이터베이스 |
| 한혜린 | 데이터베이스 |
| 오민현 | 컴파일러 |

| 학과 | 대표번호 |
| --- | --- |
| 데이터베이스 | xx-xxx |
| 컴파일러 | yy-yyy |
|  |  |

## 보이스코드 정규형 (BCNF)

BCNF는 자명하지 않은 함수 종속성이 모두 제거된 상태의 정규형이다.

따라서 더 이상 함수 종속성에 의한 무손실 분해는 할 수 없다.

BCNF는 키가 아닌 속성에서 후보키의 진부분집합에 대한 함수 종속성을 제거한다.

![image](https://user-images.githubusercontent.com/43159295/189659675-e00dde1d-54db-463a-acf1-f8caefb2b02d.png)


아래는 연구실 → 학과라는 함수 종속성이 존재한다.

연구실을 알면 학과를 알 수 있다.

| 이름 | 학과 | 연구실 |
| --- | --- | --- |
| 오민현 | 데이터베이스 | 관계형 데이터베이스 |
| 한혜린 | 데이터베이스 | 분산 데이터베이스 |
| 오민현 | 컴파일러 | 최적화 |
| 정승효 | 컴파일러 | 병렬화 컴파일러 |

[이름, 연구실], [이름, 학과]가 후보키가 될 수 있다.

위처럼 3NF이지만 BCNF가 될 수 없는 릴레이션은 후보키가 되는 속성의 조합이 여러 개 존재한다.

위는 [이름, 연구실] 을 후보키로 사용할 수 있는데 그러면 후보키의 진부분집합에서 키가 아닌 속성에 대한 함수 종속성이 생겨버리므로 2NF에서 발생하는 부분 종속성이 된다.

| 이름 | 연구실 | 학과 |
| --- | --- | --- |
| 오민현 | 관계형 데이터베이스 | 데이터베이스 |
| 한혜린 | 분산 데이터베이스 | 데이터베이스 |
| 오민현 | 최적화 | 컴파일러 |
| 정승효 | 병렬화 컴파일러 | 컴파일러 |
|  |  |  |

이름 → 학과 종속이 생긴다.

따라서 [이름, 연구실], [연구실, 학과]로 프로젝션해 무손실 분배한다.

| 이름 | 연구실 |
| --- | --- |
| 오민현 | 관계형 데이터베이스 |
| 한혜린 | 분산 데이터베이스 |
| 오민현 | 최적화 |
| 정승효 | 병렬화 컴파일러 |

| 연구실 | 학과 |
| --- | --- |
| 관계형 데이터베이스 | 데이터베이스 |
| 분산 데이터베이스 | 데이터베이스 |
| 최적화 | 컴파일러 |
| 병렬화 컴파일러 | 컴파일러 |

참고 [https://developer111.tistory.com/82](https://developer111.tistory.com/82)
