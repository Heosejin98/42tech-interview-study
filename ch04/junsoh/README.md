# 저장 프로시저 (Stored PROCEDURE)

쿼리문들의 집합으로, 어떤 동작을 여러쿼리를 거쳐 일괄적으로 처리할 때 사용()

## 일반적인 SQL의 동작 방식

![first query](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeonC1n%2FbtrsveLwpzz%2F2LW8jxvicnJF8A4tsoXOTk%2Fimg.png)
![second query](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnLIz0%2FbtrsDRHEAJy%2FFZO2sg5Ov3MkLbn9hqdGMK%2Fimg.png)

## 저장 프로시저의 동작 방식

1. create procedure
![create procedure](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FWkSyb%2FbtrsA5TPCej%2Fo9FfHxIIv09kKSm8m1t6jk%2Fimg.png)
2. 첫 실행
![first call](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlDzG3%2FbtrssvT0GCz%2FbSmaVtJa02k8sGnwFhQ1WK%2Fimg.png)
3. 두 번째 실행
![second call](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FczmEtH%2FbtrsDRnmdt1%2FvwlLLZO7eF1reeDK31FmFK%2Fimg.png)

* 위 그림을 봤을 때는 둘의 차이가 거의 보이지 않지만, 일반적인 sql에서 반복 호출 할 때, query문이 조금이라도 바뀌는 경우 매번 최적화와 컴파일을 다시 수행해야하지만 저장 프로시저는 파라미터만 교체하여 수행하면 되기 때문에 빠른 실행이 가능함.

## 장점
1. 하나의 요청으로 여러 SQL문을 실행할 수 있다. (네트워크 부하를 줄임, 쿼리문을 직접 전달하지 않고 매개변수만 전달)
2. 저장 프로시저를 처음 실행하면 최적화, 컴파일 단계를 거쳐 캐시에 저장되는데, 이 후에 해당 프로시저를 실행하게 되면 메모리에 있는 것을 가져와 사용하므로 실행속도가 빠르다.
3. C#이나 Java등으로 만들어진 응용프로그램에서 직접 SQL문을 호출하지 않고 저장 프로시저의 이름을 호출하도록 설정하는 경우가 많음, 따라서 수정이 필요한 경우 저장 프로시저 파일만 수정하면 되기 때문에 유지보수 측면에서 유리
4. 사용자별로 테이블에 권한을 주는게 아닌 저장 프로시저에만 접근 권한을 주는 방식으로 보안을 강화할 수 있음. 실제 테이블에 접근하여 다양한 조작을 하는 것은 위험하므로 실무에서는 실제로 개발자에게 저장 프로시저 권한만 주는 방식을 사용한다.
```
use mysql;
select host, name, Insert_priv from user;

revoke execute on procedure TEST.createUser FROM common_user@localhost;
```
5. 매개변수로 전달된 것은 문자열 취급을 하기 때문에 sql injection 공격을 방어할 수 있다. -- 내 의견
## 단점
1. 저장 프로시저의 경우 첫 번째 수행시 최적화가 이루어져서 인덱스 사용 여부가 결정되는데 첫 번째 실행했을 때와 그 이후 실행에서 요청 데이터의 양이 달라지게 된다면 오히려 성능이 떨어질 수 있음
따라서 인덱스 사용여부가 불분명하다면 실행시마다 다시 컴파일 되도록 설정하기도 한다.

2. 데이터베이스 제품에 대한 설명하는 구문 규칙이 SQL/ PSM 표준과의 호환성이 낮기 때문에 코드 자산으로의 재사용성이 나쁘다.
3. 비즈니스 로직의 일부로 사용하는 경우 업무의 사양 변경 시 외부 응용 프로그램과 함께 저장프로시저의 정의를 변경할 필요가 있다. 이때 불필요한 수고와 변경 실수에 의한 장애를 발생시킬 가능성이 있다.
* 전통적이 시스템을 개발했던 기업, 이를 유지보수하는 기업의 경우 business logic을 DB단에서 구현하는 경우가 많았다고 함(저장 프로시저를 이용한?), 하지만 최근에는 데이터의 양이 방대해지면서 복잡한 business logic을 DB단에서 처리하기는 너무 큰 부하를 가져 올 수 있기 때문에 backend에서 처리하는 것이 트렌드인거 같음

## 각 DBMS별 예제
[저장프로시저 - wikipedia](https://ko.wikipedia.org/wiki/%EC%A0%80%EC%9E%A5_%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80#DBMS별_예제)

- 출처:  
    https://devkingdom.tistory.com/323  
    https://ko.wikipedia.org/wiki/%EC%A0%80%EC%9E%A5_%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80