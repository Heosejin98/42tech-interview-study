# 스트래티지 패턴(Strategy pattern

행위를 클래스로 캡슐화해 동적으로 자유롭게 바꿀 수 있게 해주는 패턴
즉, 전략을 쉽게 바꿀 수 있도록 해주는 디자인 패턴.
게임 프로그래밍에서 게임 캐릭터가 자신이 처한 상황에 따라 공격이나 행동하는 방식을 바꾸고 싶을 때 스트래티지 패턴이 매우 유용함


## 일반 객체지향 기법으로 구현했을 때 발생가능한 문제

- 요구사항
    - 모든 오리는 꽥꽥 소리르 낼 수 있고, 헤엄칠 수 있다.
    - 오리는 모양새가 다르다.
- 결과
    공통사항을 하나로 묶은 Duck 추상 클래스 생성
    - quack: 우는 소리를 내는 기능
    - swim: 수영하는 기능
    - display: 인터페이스(오리는 모양이 다르기 때문에 하위 클래스에서 구현)
    Duck 클래스를 상속받아 다양한 오리 클래스 생성
    - 청둥오리
    - 미국흰죽지
    - 고무오리

```java
public abstract class Duck {
    public void quack() {
        System.out.println("꽥꽥");
    }
    public void swim() {
        System.out.println("헤엄");
    }
    abstract void display();
}
```
```java
public class MallardDuck extends Duck{
    @Override
    void display() {
        System.out.println("천둥오리");
    }
}
```
```java
public class ReadheadDuck extends Duck {
    @Override
    void display() {
        System.out.println("미국흰죽지");
    }
}
```

하지만 이렇게 상속을 통해 구현했을 때 요구사항이 변경됐을 경우?
- 오리에 fly기능을 추가시켜주세요.

![상속](https://t1.daumcdn.net/cfile/tistory/262BC7495725DF5F1D)

```java
public abstract class Duck {
    public void quack() {
        System.out.println("꽥꽥");
    }
    public void swim() {
        System.out.println("헤엄");
    }
    public void fly() {
        System.out.println("훨훨");
    }
    abstract void display();
}
```
그런데 이렇게 해버리면 날 수 없는 오리들에게도 모두 fly기능이 추가 되기 때문에 프로그램에 버그가 생기게 된다.

급한 불을 끄기 위해 fly기능이 필요없는 subclass에서 fly 메소드를 오버라이드 해주었다.
```java
public class RubberDuck extends Duck {
    @Override
    void display() {
        System.out.println("고무오리");
    }
    @Override
    public void quack() {
        // 고무오리는 꽥꽥 울지 않음
        System.out.println("삑삑");
    }
    @Override
    public void fly() {
        // 날지 못하기 때문에 빈 메소드
    }
}
```

하지만 이런식으로 처리했을 경우, 변경이 필요한 subclass를 생성해야할 때, 계속 오버라이드해주어야하므로 코드의 중복이 발생한다.

그렇다면 공통된 부분은 상속으로 구현하고 다른 부분을 인터페이스로 구현한다면?
![인터페이스 사용](https://t1.daumcdn.net/cfile/tistory/233A6B4D5725E41127)
```java
public interface Flyable {
    public void fly();
}

public interface Quackable {
    public void quack();
}

public class RubberDuck extends Duck {
    @Override
    void display() {
        System.out.println("고무오리");
    }
}

public class RedheadDuck extends Duck implements Flyable {
    @Override
    public void display() {
        System.out.println("고무오리");
    }

    @Override
    public void fly() {
        System.out.println("훨훨");
    }
}
```
인터페이스를 사용하면 이런식으로 직접 구현이 들어가야하기 때문에 fly의 알고리즘이 바뀐다면 flyable를 구현한 구현체들의 코드를 모두 바꿔야하는 불상사가 발생함

이런 문제점들을 해결하면서 유연하게 대처할 수 있는 방법은 무엇일까

## 디자인 원칙

- 디자인 원칙1
    - 애플리케이션에서 달라지는 부분을 찾아내고, 달라지지 않는 부분으로부터 분리시킴
    - 바뀌는 부분을 따로 캡슐화시키면 나중에 바뀌지 않는 부분에는 영향을 끼치지 않고, 바꾸는 부분만 고치거나 확장할 수 있다.

    위의 예제에서 보면 fly와 quack은 Duck클래스에서 달라지는 부분이기 때문에 이러한 행동을 클래스에서 분리하여 클래스 집합 형태로 만들어야함

## 행동 디자인
![행동분리](https://t1.daumcdn.net/cfile/tistory/24032D435725E8FC28)
- 디자인 원칙2
    - 구현이 아닌 인터페이스에 맞춰서 프로그래밍
- 최대한 유연하게 만들어야함
    - 위에 발생한 문제들은 오리의 행동들이 유연하지 못해 발생한 문제들이었음
    - 프로그램 시작 시, 오리의 행동을 초기화 하거나 중간에 동적으로 변경할 수 있도록 만들어야한다.
- 각 행동을 인터페이스로 표현하고, 각 행동을 구현할 때 해당 인터페이스를 구현하도록 만듬
    - interface FlyBehavior -> (class FlyWithWings, FlyNoWay), interface QuackBehavior -> (class Quack, class MuteQuack)

## 행동기반의 Duck 클래스
- 디자인 원칙3
    - 상속보다는 구성을 활용한다.

```java
public abstract class Duck {
    FlyBehavior flyBehavior;
    QuackBehavior quackBehavior;

    public void performQuack() {
        this.quackBehavior.quack();
    }

    public void performFly() {
        this.flyBehavior.fly();
    }

    abstract public void swim();
    abstract public void display();
}
```

- 기존 Duck코드에서는 행동 관련 메소드를 각 서브클래스에서 직접 수행했지만, 변경된 Duck에서는 행동클래스에게 수행을 위임함
- 이코드에서는 오리가 어떤 소리를 내는지, 어떻게 나느지 중요하지 않음, 그저 해당 행동을 실행시킬 수 있다는 사실이 중요함.

### 실제 오리 클래스들
1. MallardDuck
```java
public class MallardDuck extends Duck {
    public MallardDuck() {
        quackBehavior = new Quack();
        flyBehavior = new FlyWithWings();
    }

    @Override
    public void swim() {
        System.out.println("헤엄은 매우 잘해요");
    }

    @Override
    public void display() {
        System.out.println("물오리 입니다");
    }
}

public class FlyWithWings implements FlyBehaivor {
    @Override
    public void fly() {
        System.out.println("훨훨");
    }
}

public class Quack implements QuackBehavior {
    @Override
    public void quack() {
        System.out.println("꽥꽥");
    }
}

public class main {
    public static void main(String[] args) {
        Duck mallard = new MallardDuck();
        mallard.performFly();
        mallard.performQuack();
    }
}
```

### 행동을 동적으로 변경하는 방법
```java
abstract class Duck {
    // 이전 코드와 동일

    public void setFlyBehavior(FlyBehavior flyBehavior) {
        this.flyBehavior = flyBehavior;
    }

    public void setQuackBehavior(QuackBehavior quackBehavior) {
        this.quackBehavior = quackBehavior;
    }
}

public class Main {
    public static void main(String[] args) {
        Duck mallard = new MallardDuck();
        mallard.performFly();
        mallard.performQuack();
        // 행동 변경
        mallard.setFlyBehavior(new FlyNoWay());
        mallard.setQuackBehavior(new MuteQuack());
        mallard.performFly();
        mallard.performQuack();
    }
}

// 결과
// 훨훨
// 꽥꽥
// 날지 못해요
// <<< 조용 >>>
```

![결과](https://t1.daumcdn.net/cfile/tistory/24165B475725F4050C)

[스트래티지 패턴](https://2dongdong.tistory.com/43)