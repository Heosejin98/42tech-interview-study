# Template Method Pattern

## 템플릿 메소드 패턴

> 어떤 작업을 처리하는 일부분을 서브 클래스로 캡슐화해 전체 일을 수행하는 구조는 바꾸지 않으면서 특정 단계에서 수행하는 내역을 바꾸는 패턴입니다.

- 메소드를 위한 골격을 제공하여 자식 클래스들이 메소드의 특정 부분만 재정의하도록 해야할 때
- 모든 자식 클래스에 공통적으로 정의되는 메소드이지만 약간씩 차이가 있는 경우
- 자식 클래스가 반드시 재정의해야 하는 메소드를 제어하기 위해

<img width="213" alt="스크린샷 2022-09-06 오전 9 01 59" src="https://user-images.githubusercontent.com/68188768/188522098-3acc4807-da2f-4601-bd82-6ee863eba120.png">

- 추상클래스 (Abstract Class): 탬플릿 메서드를 정의하는 클래스로 하위 클래스에 공통 알고리즘을 정의하고 하위 클레이스에서 구현될 기능을 메서드로 정의하는 클래스 입니다.
- 구현클래스 (Concrete Class): 물려 받은 메서드 또는 hook 메서드를 구현하는 클래스로 상위 클래스에서 구현된 템플릿 메서드의 일반적인 알고리즘에서 하위 클래스에 적합하게 오버라이드 하는 클래스 입니다.

### 단계

- 알고리즘을 여러 단계로 나눕니다.
- 나눠진 알고리즘의 단계를 메소드로 선언합니다.
- 알고리즘을 수행할 템플릿 메소드를 만듭니다.
- 하위 클래스에서 나눠진 메소드들을 구현합니다.

### 예제

> 커피와 차를 만드는 법을 비교해 볼 수 있습니다.
- Coffee
```java
public class Coffee {
    public void prepareRecipe() {
        boilWater();
        brewCoffeeGrinds();
        pourInCup();
        addSugarAndMilk();
    }
    public void boilWater() {}
    public void brewCoffeeGrinds() {}
    public void pourInCup() {}
    public void addSugarAndMilk() {}
}
```
- Tea
```java
public class Tea {
    public void prepareRecipe() {
        boilWater();
        steepTeaBag();
        pourInCup();
        addLemon();
    }
    public void boilWater() {}
    public void steepTeaBag() {}
    public void pourInCup() {}
    public void addLemon() {}
}
```

### 해결법

#### 1. 

> 유사한 기능을 제공하면서 중복된 코드가 있는 경우 상속을 이용해서 코드 중복 문제를 피할 수 있습니다.


<img width="333" alt="스크린샷 2022-09-06 오전 9 32 43" src="https://user-images.githubusercontent.com/68188768/188523835-98e538a5-ace4-479c-b649-106de1ac1309.png">

```java
public class Coffee extends CaffeineBeverage {
    public void prepareRecipe() {
        boilWater();
        brewCoffeeGrinds();
        pourInCup();
        addSugarAndMilk();
    }
    public void brewCoffeeGrinds() {}
    public void addSugarAndMilk() {}
}
```
#### 2. 

> 부분적으로 중복되는 경우에도 상속을 활용해 코드 중복을 피할 수 있습니다. 

<img width="499" alt="스크린샷 2022-09-06 오전 9 42 03" src="https://user-images.githubusercontent.com/68188768/188524422-93e27316-d754-4ce4-ad35-dbde278366c9.png">

```java
public abstract class CaffeineBeverage {
    public final prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }
    public final boilWater() {}
    public abstract void brew() {}
    public final pourInCup() {}
    public abstract void addCondiments() {}
}
```
- 서로 다른 이름으로 사용하는 메소드의 이름을 변경합니다.
- prepareRecipe는 template 메소드에 해당하여 알고리즘의 골격을 정의합니다.
- 자식에서 재정의 할 수 없도록 final 메소드를 사용합니다. 
- 하위 클래스에서 제공하는 메소드는 abstract 메소드로 선언합니다.
- 알고리즘의 일부 과정은 하위 클래스에 위임하고 있음으로 추상 클래스가 됩니다.

### 메소드 내 연산 분류

<img width="441" alt="스크린샷 2022-09-06 오전 9 51 23" src="https://user-images.githubusercontent.com/68188768/188525071-dc1ac5f6-d12e-493e-bc43-a4dc44040783.png">

#### Concrete Method

> Template 메소드가 정의된 클래스에 완전히 정의되는 메스드로 하위 클래스 에서는 재정의하지 않아야 하는 연산입니다. 다른 말로 complete method라고 합니다(private 가능)
>  - 자바 : final 키워드로 수식( private이면 final 수식 불필요)
>  - C++ : 일반 함수

#### Abstract Method

> Template 메소드가 정의된 클래스에 선언만 되어 있는 추상 메소드로 하위 클래스에서 반드시 정의하여아하는 연산입니다.(protected, public)

#### Hook Method

> Template 메소드가 정의된 클래스에 기본 행동이 정의되어 있는 메소드로 하위 클래스에서 선택적으로 재정의할 수 있는 연산입니다.(protected, public)
> 해당 단계는 skip할 수 있다는 의미로 빈 메소드로도 정의될 수 있습니다.

<br>

## 장점과 단점

### 장점

- 중복코드를 줄일 수 있습니다.
- 자식 클래스의 역할을 줄여 핵심 로직의 관리가 용이하다.
- 좀 더 코드를 객체지향적으로 구성할 수 있습니다.

### 단점

- 추상 메소드가 많아지면서 클래스 관리가 복잡해집니다.
- 클래스간의 관계와 코드가 꼬일 염려가 있습니다.

<br>
