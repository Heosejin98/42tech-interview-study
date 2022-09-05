# Page 교체 알고리즘

필요한 페이지가 메모리에 없을 때 page-fault가 발생하고 Backing Store에서 해당 페이지를 찾아 빈 프레임에 로딩해햐하는데, 이때 빈 프레임이 없을 경우 희생 당할  프레임을 고르는 알고리즘이 페이지 교체 알고리즘

페이지 교체 알고리즘의 목표는 page-fault 발생 비율을 줄이는 것

## FIFO

![FIFO](https://blog.kakaocdn.net/dn/bGqpiD/btrpdovhuvd/9JIyEkk7DUmK6uCeO2ud40/img.png)
- FIFO 알고리즘은 이름 그대로 가장 먼저 메모리에 올라온 페이지를 가장 먼저 내보내는 알고리즘
- 구현이 간단하지만 성능은 좋지 않음
- 메모리에 적재된 시간을 저장하거나 큐를 이욯해 저장할 수 있음.
- Belady's Anomaly 현상이 발생할 수 있음

* Belady's Anomaly
일반적으로 생각했을 때 사용하는 page frame이 많아질 수록 page fault가 덜 발생할 것이라고 예측하지만 이런 예측과는 다르게 page frame이 높아도 page fault가 더 많이 발생하는 경우가 생기는데 이를 belady's anomaly라고 함
![example](https://mblogthumb-phinf.pstatic.net/data42/2008/11/11/49/belady%2527s-anomaly_cookatrice.gif?type=w800)

## OPT(Optimal)

![OPT](https://blog.kakaocdn.net/dn/etkbwZ/btro8wOpRuo/yrVmZdpsBI8PienpO92bc0/img.png)
- OPT 알고리즘은 앞으로 가장 오랫동안 사용하지 않을 페이지를 교체하는 알고리즘
- 모든 페이지 교체 알고리즘 중 page-fault 발생이 가장 적음
- Belady's Anomaly 현상이 발생하지 않음
- 실제로 구현하기 거의 불가능한 알고리즘이어서 사용하기 보다는 연구 목적을 위해 사용

## LRU(Least Recently Used)

![LRU](https://blog.kakaocdn.net/dn/4z1Wa/btro38mZn1V/2k9TKkjOJiGm7A9LkCndR1/img.png)
- 가장 오랫동안 사용하지 않은 페이지를 교체하는 알고리즘
- 최적 알고리즘과 비슷한 효과를 낼 수 있음
- 성능이 좋은 편
- 많은 운영체제가 채택함
- time stamping에 의한 오버헤드 존재

## LFU(Least Frequently Used)

![LFU](https://blog.kakaocdn.net/dn/k3rlo/btro8xT5B3y/DU9CSseZMbyWRwuo6X230K/img.png)
- 참조횟수가 가장 적은 페이지를 교체하는 알고리즘
- 교체 대상이 여럿이면 가장 오랫동안 사용하지 않은 페이지를 교체
- 최근 적재된 페이지가 교체될 우려가 있음

## MFU(Most Frequenlty Used)

![MFU](https://blog.kakaocdn.net/dn/bcvcNG/btroVqoH7Ic/RFGqK4Lsiqls1FfXGN4Fs1/img.png)
- LFU와 반대로 참조 횟수가 가장 많은 페이지를 교체하는 알고리즘
굳이?

## NUR(Not Used Recently)

![NUR](https://blog.kakaocdn.net/dn/C9OaF/btrmY5SOBut/mSCrnGLVbPnKdxG1eeUuW1/img.png)
- LRU와 근사한 알고리즘으로, time stamping 오버헤드를 피하기 위해 참조 비트와 변형 비트를 두어 우선 순위를 정함
- 우선 순위: 참조 x, 수정 x -> 참조 x, 수정 o -> 참조 o, 수정 x -> 참조 o, 수정 o

## SCR(Second Chance Replacement)
![SCR](https://blog.kakaocdn.net/dn/b0vmFA/btrmYdjwJ26/FELwkyLAX64kzH54Ns6FTK/img.png)
- FIFO의 단점을 보완하는 기법
- 보통 원형큐를 이용하는 듯(Clock 알고리즘)
- 참조 비트를 이용하며, 참조 비트가 0인 경우 교체 대상이며, 1인 경우 0으로 바꾸고 한 번 더 기회를 줌