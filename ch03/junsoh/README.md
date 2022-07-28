# Database Keys

데이터 베이스 키(Key)의 개념 및 종류

키(Key)는 데이터베이스에서 조건에 만족하는 튜플을 찾거나 순서대로 정렬할 때 다른 튜플들과 구별할 수 있는 유일한 기준이 되는 Attribute(속성)입니다. 

*튜플 : 릴레이션을 구성하는 각각의 행, 속성의 모임으로 구성된다. 파일 구조에서는 레코드와 같은 개념

튜플의 수 = 카디널리티(Cardinality) = 기수 = 대응수
여기는 잘 이해가 안됨



## Diagram?

![key](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fk6fbM%2Fbtq2gJKG6zc%2FuBcVaFNPJbykvPxX1pVG21%2Fimg.png)


1. 슈퍼키 (Super Key)
![superkey example](https://t1.daumcdn.net/cfile/tistory/995E544F5ADEBA0F0B)

* 슈퍼키는 한 릴레이션 내에 있는 속성들의 집합으로 구성된 키로서 릴레이션을 구성하는 모든 튜플 중 슈퍼키로 구성된 속성의 집합과 동일한 값은 나타내지 않습니다.

* 릴레이션을 구성하는 모든 튜플에 대해 유일성은 만족하지만, 최소성은 만족시키지 못합니다.

- 모든 속성의 조합 중 해당 조합으로 튜플을 특정할 수 있다면 슈퍼키


2. 후보키 (Candidate Key)
![candidatekey example](https://t1.daumcdn.net/cfile/tistory/99BB58475ADEBBD932)

* 릴레이션을 구성하는 속성들 중에서 튜플을 유일하게 식별할 수 있는 속성들의 부분집합을 의미합니다. 

* 모든 릴레이션은 반드시 하나 이상의 후보키를 가져야합니다.

* 릴레이션에 있는 모든 튜플에 대해서 유일성과 최소성을 만족시켜야합니다.

* 기본키가 될 수 있는 키들을 후보키라고 합니다.

- 슈퍼키 중 최소성을 만족하는 키들의 집합을 후보키라함


3. 기본키 (Primary Key)
![primarykey and alternate key](https://t1.daumcdn.net/cfile/tistory/99B4D34B5ADEBEC632)

* 후보키 중에서 선택한 주키(Main Key)

* 한 릴레이션에서 특정 튜플을 유일하게 구별할 수 있는 속성

* Null 값을 가질 수 없습니다. (개체 무결성의 첫번째 조건)

* 기본키로 정의된 속성에는 동일한 값이 중복되어 저장될 수 없습니다.(개체 무결성의 두번째 조건)


4. 대체키 (Alternate Key)

* 후보키가 둘 이상일 때 기본키를 제외한 나머지 후보키들을 말합니다.

* 보조키라고도 합니다.



5. 외래키 (Foreign Key)


* 관계(Relation)를 맺고 있는 릴레이션 R1, R2에서 릴레이션 R1이 참조하고 있는 릴레이션 R2의 기본키와 같은 R1 릴레이션의 속성

* 외래키는 참조되는 릴레이션의 기본키와 대응되어 릴레이션 간에 참조 관계를 표현하는데 중요한 도구로 사용됩니다.

* 외래키로 지정되면 참조 테이블의 기본키에 없는 값은 입력할 수 없습니다. (참조 무결성 조건)

- 전통적으로 범위가 더 작거나 사용빈도가 더 적은 테이블에서 FK를 가지고 있는다고 함


### 테이블의 관계성
나의 선생님 egoing님의 영상(https://www.youtube.com/watch?v=RHrp8xsgVr8&ab_channel=%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9)


[다대다 관계 테이블 구현하기](https://velog.io/@galaxy/%EB%8B%A4%EB%8C%80%EB%8B%A4MN-%EA%B4%80%EA%B3%84-%ED%85%8C%EC%9D%B4%EB%B8%94-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0-%EC%8B%9D%EB%B3%84%EA%B4%80%EA%B3%84-%EB%B9%84%EC%8B%9D%EB%B3%84%EA%B4%80%EA%B3%84)
[JPA 다대다 관계와 그 한계](https://ict-nroo.tistory.com/127)
수퍼키 중에 후보키가 있을 수도 있고 기본키로 설정도 가능하다.
https://hoon93.tistory.com/57