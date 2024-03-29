# 데드락 (DeadLock, 교착 상태)

> 데드락이란 두 개 이상의 작업이 서로 상대방의 작업이 끝나기 만을 기다리고어 무한 대기에 빠지는 상황을 말합니다.
> <img width="881" alt="스크린샷 2022-08-30 오후 12 57 44" src="https://user-images.githubusercontent.com/68188768/187345945-95b6a93d-1c3e-4622-99d9-fc69ad63385a.png">

<br>

## 1. 데드락 발생 조건

### 1.1. 상호 배제(Mutual exclusion)

```
한 리소스는 한 번에 한 프로세스만이 사용할 수 있습니다.
```

### 1.2. 점유와 대기(Hold and wait)

```
어떤 프로세스가 하나 이상의 리소스를 점유하고 있으면 다른 프로세르가 가지고 있는 리소스를 기다립니다.
```

### 1.3. 비선점(No preemption)

```
테스크를마친 후 리소스를 자발적으로 반환할 때까지 기다립니다.
```

### 1.4. 순한 대기(Circular wait)

```
각 프로세스는 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있어 자발적으로 반환할 때까지 기다립니다.
```
위 조건을 모두 만족해야 데드락이 발생하며 순환대기의 경우 점유 대기와 비선점 조건을 만족해야 성립하므로 서로 완전히 독립적인 것은 아닙니다.

<br>

## 2. 데드락 상태 관리

### 2.1. 방지(예방 Prevention)

```
데드락은 발생 조건을 모두 만족해야 하므로 발생 조건 4개지 중 하나라도 발생하지 않게하는 것이 데드락을 예방할 수 있는 방법입니다.
```
- <b>`상호 배제 조건 방지`</b>: 한 번에 여러 프로세스가 공유 자원을 사용할 수 있게 합니다. 다만 추후 동기화 관련 문제가 발생할 수 있습니다.
- <b>`점유와 대기 조건 방지`</b>: 프로세스 실행에 필요한 모든 자원을 한꺼번에 요구하고 허용할 때까지 작업을 보류해서 나중에 또 다른 자원을 점유하기 위한 대기 조건을 성립하지 않도록 합니다.
- <b>`비선점 조건 방지`</b>: 이미 다른 프로세스에게 할당된 자원이 선점권이 없다고 가정했을 때 높은 우선순위의 프로세스가 해당 자원을 선점할 수 있도록 합니다.
- <b>`순환 대기 조건 방지`</b>: 자원을 순환 형태로 대기하지 않고 일정한 방향으로만 자원을 요구할 수 있도록 합니다.

데드락이 발생하지 않는 것이 좋지만, 발생하지 않는 환경을 만들게 된다면 자원을 효율적으로 사용할 수 없습니다. 즉, 위와 같이 데드락 발생조건을 방지해서 데드락을 예방하는 방법은 시스템 처리량이나 자원 사용의 효율성을 떨어트리는 단점이 있습니다.

### 2.2. 회피(Avoidance)

```
데드락이 발생할 가능성이 있는 상태에서는 자원을 할당(Unsafe allocation) 하지 않고 안전한 상태(Safe state)에서만 자원 요청을 허용하는 방법입니다.
- 프로세스수 고정
- 자원의 종류와 수 고정
- 프로세스가 요구하는 최대 자원의 수
- 프로세스는 자원 사용후 반드시 반납해야 합니다.
```
- <b>`은행원 알고리즘(Banker's Algorithm)`</b>
  안전한 상태(Safe state)에 들 수 있는지 여부를 검사해서 데드락 가능성을 미리 조사하는 알고리즘 입니다.
- <b>`자원 할당 그래프 알고리즘(Resource-Allocation Graph Algorithm)`</b>
  자원 할당 그래프에 예약 간선을 추가하여 예약 간산으로 설정한 자원에 대해서만 자원 할당을 요청할 수 있고 사이클이 형성되지 않을 때만 자원을 할당받는 방법입니다.

### 2.3. 탐지와 회복(Detection and Recovery)

```
교착 생태가 자주 발생하는 시스템에서 일반적으로 사용하는 방법입니다.
```

- <b>`탐지기법(Detection)`</b>
  데드락이 발생했는지에 대한 여부를 탐색하고 회복 기법 알고리즘에 활용하게 됩니다. 지속적으로 데드락을 확인하는 작업이 필요해 오버헤드(성능 저하)가 발생하게 됩니다.
  
- <b>`회복(Recovery)`</b>
  데드락이 발생했을 때 해결하는 기법을 의미합니다.
  - 사용자가 데드락이 발생한 프로세스 중 하나의 프로세스를 강제 종료하는 방법이 있습니다.
  - 교착 상태에 속해있는 모든 프로세스를 중지합니다. 이 경우 계속 연산중이던 프로세스들도 모두 일시에 중단되어 부분 결과가 폐기될 수 있는 부작용이 발생할 수 있습니다.
  - 교착 생태가 해결될 때까지 한 프로세스씩 중지합니다. 하지만 매번 탐지 알고리즘을 호출 및 수행해야 하므로 부담이 되는 작업일 수 있습니다.
  - 프로세스들로부터 자원을 선점해서, 교착상태가 해결될 때까지 다른 프로세스들에게 자원을 할당합니다.
  
### 2.4. 무시

```
교착 상태가 드물게 발생하는 시스템에서 일반적으로사용하는 방법으로 Windows나 UNIX를 포함한 대부분의 운영체제에서 사용하게 됩니다.
```
