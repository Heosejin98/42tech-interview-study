# Database

## Index

_인덱스의 목적_

추가적인 쓰기 작업과 저장 공간을 활용하여 DB table의 검색 속도를 향상시키기 위한 자료구조.

> 두꺼운 책의 목차와 같다

---

## Usage of Index

Index를 활용하면 데이터를 조회하는 SELECT 외에도 UPDATE나 DELETE의 성능이 함께 향상된다.

```postgres
UPDATE USER SET NAME = 'Mingyu' WHERE NAME = 'Mingkim';
```

만약 Index를 사용하지 않는다면 Full Scan 작업을 수행해야하는데, 이는 처리 속도가 떨어질 수 밖에 없다.

---

## Index Management

DBMS는 Index를 항상 최신의 정렬된 상태로 유지해야 원하는 값을 빠르게 탐색할 수 있다. <br />
그렇기 때문에 Index가 적용된 column에 INSERT, UPDATE, DELETE가 수행된다면 <br />
각각 다음과 같은 연산을 추가적으로 해주어야 하며, 그에 따른 오버헤드가 발생한다. <br />

-   INSERT : 새로운 데이터에 대한 Index 추가

    기존 Block에 여유가 없을 때, 새로운 Data가 입력된다. <br />
    새로운 Block을 할당받은 후, Key를 옮기는 작업을 수행한다. <br />
    Index split 작업 동안, 해당 Block의 Key값에 대해서 DML이 블로킹 된다. <br />

-   DELETE : 삭제하는 데이터의 인덱스를 사용하지 않는다는 작업 진행

    <Table과 Index 상황비교> <br />
    Table에서 Data가 DELETE 되는 경우 : Data가 지워지고 다른 Data가 해당 공간 사용 가능 <br />
    Index에서 Data가 DELETE 되는 경우 : Data가 지워지지 않고, 사용 안 됨 표시만 해둔다. <br />
    고로 Table의 Data수와 Index의 Data 수가 다를 수 있다. <br />

-   UPDATE : 기존의 인덱스를 사용하지 않음 처리하고, 갱신된 데이터에 대해 인덱스를 추가함

    Table에서 UPDATE가 발생하면 Index는 UPDATE 할 수 있다. <br />
    Index에서는 DELETE가 발생하고, 새로운 작업의 INSERT 작업을 하므로 2배의 작업이 소요. <br />

---

## 파일 구성

테이블 생성 시, 3가지 파일이 생성된다.

-   FRM: 테이블 구조 저장파일
-   MYD: 실제 데이터 파일
-   MYI: Index 정보 파일

사용자가 쿼리를 통해 Index를 사용하는 column을 검색하게 되면, 이때 MYI 파일의 내용을 활용한다.

---

## Index 장점

-   테이블을 조회하는 속도와 그에 따른 성능 향상
-   전반적인 시스템 부하 감소

## Index 단점

-   Index 관리를 위한 추가적인 저장공간 필요
-   Index 관리를 위한 추가 작업도 필요
-   여러 사용자가 참여하는 애플리케이션에서 한 페이지를 여러 명이 동시에 수정할 수 있는 병행성이 떨어진다.
-   잘못 사용할 경우 오히려 성능이 저하될 수 있다.

---

## 사용하면 좋은 경우

(1) Where 절에서 자주 사용되는 Column

(2) 외래키가 사용되는 Column

(3) Join에 자주 사용되는 Column

  <br>

## Index 사용을 피해야 하는 경우

(1) Data 중복도가 높은 Column

(2) DML이 자주 일어나는 Column (DML에 취약하기 때문에)

<br>
