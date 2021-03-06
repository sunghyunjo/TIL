# 데드락
두 개 이상의 작업이 서로 상대방의 작업이 끝나기 만을 기다리고 있기 때문에 결과적으로 아무것도 완료되지 못하는 상태. 
    
## 데드락 조건
1.	상호배제(Mutual exclusion) 
: 프로세스들이 필요로 하는 자원에 대해 배타적인 통제권을 요구한다.
2.	점유대기(Hold and wait)
: 프로세스가 할당된 자원을 가진 상태에서 다른 자원을 기다린다.
3.	비선점(No preemption) 
: 프로세스가 어떤 자원의 사용을 끝날 때까지 그 자원을 뺏을 수 없다.
4.	순환대기(Circular wait) 
: 각 프로세스는 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있다.


## 데드락 해결 방법(예방)
1. 상호배제 조건의 제거 
- 데드락은 두개 이상의 프로세스가 공유 불가능한 자원을 사용하여 발생하는 것이므로 공유 불가능한, 즉 상호배제조건을 제거하면 교착상태를 해결할 수 있다.
2. 점유와 대기 조건의 제거
- 한 프로세스에 수행되기 전에 모든 자원을 할당시키고 나서 점유하지 않을 때에는 다른 프로세스가 자원을 요구하도록 하는 방법. 자원 과다 사용으로 인한 효율성. 프로세스가 요구하는 자원을 파악하는 데에 대한 비용, 자원에 대한 내용을 저장 및 복원하기 위한 비용, 기아상태, 무한 대기 등의 문제점이 있다.
3. 비선점 조건의 제거
- 비선 점프로세스에 대해 선점 가능한 프로토콜을 만들어준다.
4. 환형 대기 조건의 제거
- 자원 유형에 따라 순서를 매긴다.

> 이런 방법들은 자원 사용의 효율성이 떨어지고 비용이 많이 드는 문제점이 있다.

## 데드락의 회피
- 자원이 어떻게 요청될지에 대한 추가정보를 제공하도록 요구하는 것으로 시스템에 circular wait가 발생하지 않도록 자원 할당 상태를 검사한다.
- 교착 상태 회피하기 위한 알고리즘으로 크게 두가지가 있다.
1) 자원 할당 그래프 알고리즘 (Resource Allocation Graph Algorithm) 
2) 은행원 알고리즘 (Banker’s algorithm)