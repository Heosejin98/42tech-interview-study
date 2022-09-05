# Singleton Pattern

## Definition

<br />

> 애플리케이션이 시작될 때, 어떤 클래스가 최초 한 번만 메모리를 할당(static)하고 해당 메모리에 인스턴스를 만들어 사용하는 패턴.

<br />

쉽게 얘기하면 싱글톤 패턴은 하나의 인스턴스만 생성하여 사용하는 디자인 패턴,

즉, `객체의 인스턴스가 오직 1개만 생성되는 패턴`을 의미한다. 

싱글톤 패턴을 구현하는 방법은 여러 가지가 있지만, 객체를 미리 생성해두고 가져오는 방법이 가장 단순하고 안전하다.

<br />

---

## Example 

<br />

### Java

<br />

```Java
public class Singleton {

    private static Singleton instance = new Singleton();
    
    private Singleton() {
        // ...
    }

    public static Singleton getInstance() {
        return instance;
    }

    public void say() {
        System.out.println("hi, there");
    }
}
```

<br />

### Golang 

```go

type Singleton struct {
    Instance string
}

// private 
func (sg *Singleton) instanceConstructor() {
    // ...
}

// public
func (sg *Singleton) GetInstance() {
    return sg.Instance
}

func (sg *Singleton) Say() {
    fmt.Println("hi, there");
}
```
> 인스턴스가 필요할 때, 똑같은 인스턴스를 만들지 않고 기존의 인스턴스를 활용.

<br />

---

## Why Singleton Pattern ? 

<br />

이렇게 인스턴스를 오직 한 개로만 가져가게 되면 몇 가지 이점이 있다. 

<br />

### 1. 메모리 측면

<br />

최초 한번만 고정된 메모리 영역을 사용하기 때문에 추후 해당 객체에 접근할 때 메모리 낭비를 방지할 수 있다. 

또한 이미 생성된 인스턴스를 활용하니 속도 측면에서도 이점이 있다고 볼 수 있다. 

<br />

### 2. 공유의 이점

<br/>

싱글톤 인스턴스는 전역으로 사용되기 때문에 서로 다른 객체간에 데이터 공유가 쉽다.

하지만 여러 객체의 인스턴스에서 싱글턴 인스턴스의 데이터에 동시에 접근하게 되면 동시성 문제가 발생할 수 있어서 이 점에 유의해야한다. 

<br />

보통 싱글톤 패턴은 공통된 객체를 여러 개 생성해서 사용해야 하는 상황, 

예를 들면 데이터베이스의 커넥션 풀, 스레드 풀, 캐시, 로그 기록 객체 등에 많이 사용한다.

이 외에도 도메인 관점에서 인스턴스가 한 개만 존재하는 것을 보증하고 싶은 경우 싱글톤 패턴을 사용하기도 한다. 

<br />

---

## Problems

<br />

싱글톤 패턴을 적용하면 위와 같은 효율적인 측면에서의 이점이 있지만, 다음과 같은 문제점이 발생할 수 있다.

개발자는 이러한 문제점과 이점의 trade-off를 잘 고려해야한다. 

<br />

### 개방-폐쇄 원칙 위배

<br />

만약 싱글톤 인스턴스가 혼자 너무 많은 일을 하거나, 많은 데이터를 공유시키면 다른 클래스들 간의 결합도가 높아지게 되는데, 이때 개방-폐쇄 원칙이 위배된다.

결합도가 높아지면 유지보수가 힘들고 테스트도 원활하게 진행하기 힘들다. 

<br />

### 동시성 문제

<br />

정적 팩토리 메서드에서 객체 생성을 확인하고 생성자를 호출하는 경우에, 멀티스레드 환경에서는 동시성 문제가 발생할 수 있다. 

멀티 스레드 환경에서 동기화 처리를 하지 않았다면 인스턴스가 2개가 생성되는 문제도 발생할 수 있다.

이러한 문제를 해결하기 위해서 동기화 처리를 한다면 효율 저하 및 추가적인 코드 작성이 발생한다.

<br />
