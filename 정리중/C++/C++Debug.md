# Debug 

## Exception

- 익셉션 : 최신 컴파일러는 익셉션이 발생하지 않으면, 오버헤드가 거의 없고, 실제로 익셉션이 발생했을 때만 약간의 오버헤드가 발생 


- `throw` 키워드: 익셉션을 던짐
    - 익셉션의 계층구조 클래스: `const char*` 스트링을 리턴하는 what() 메서드


> 익셉션 객체는 항상 const 레퍼런스로.. (객체 슬라이싱 발생 가능성)

- `catch(...)` : 모든 익셉션 타입에 매칭하는 와일드 카드 

- 익셉션을 다시 던질 때 객체 슬라이싱이 발생할 수 있기 때문에 `throw e;` 대신 `throw;` 사용 

- 스택풀기: 익셉션을 던지면, 익셉션을 처리할 수 있는 catch 블록을 찾을 때까지 스택을 풀어서 함수를 종료하는 면
    - 메모리 해제 등 작업 주의
    - C++로 코드를 작성할 때는 반드시 스택 기반 할당 방식 적용


## 테스트

- 화이트박스 테스트 
    - 테스터가 프로그램의 내부 작동 과정을 알면서 테스트를 수행
        - 객체나 서브시스템 수준으로 테스트 수행 => 테스트 커버리지 보장
    - 테스트 코드를 작성해서 테스트를 자동화하기 쉬운 편
- 블랙박스 테스트 
    - 테스터가 프로그램의 내부 구현에 대해 전혀 모른 채 기능만 테스트  
        - 사용자의 행위를 그대로 반영하는 기본적인 기법 (ex. 버튼 클릭, 반응이 없으면 버그)


### 단위 테스트  

1. 테스트 구체화 수준 정하기 
2. 개별 테스트에 대한 브레인스토밍 
3. 샘플 데이터와 결과 마련하기 
4. 테스트 코드 작성하기 

### 회귀 테스트(regression test)

- 개발이 끝나고 나서 검사하거나, 이전에 제대로 작동하던 기능이 현재도 정상 상태를 유지하는지 검사하는 목적으로 수행함

- 회귀 테스트를 제대로 작성했다면, 코드 변경사항으로 인해 문제가 발생한 부분을 즉시 걸러낼 수 있음



- 스모크 테스트 
    - 중요한 기능만 


## 버그 대비 

- 에러 로깅 
    - 트레이스: 버그 발생할 당시의 실행 경로를 추적
        - 로그 파일에 저장 x 
    - 로그에 남겨야 할 유형: 복구할 수 없는 에러, 관리자의 대응이 필요한 에러(메모리 부족, 파일 포맷 오류, 디스크 쓰기 에러, 네트워크 에러)
    - 의도하지 않은 경로로 실행, 변수에 비정상적인 값이 담겨 발생하는 에러 
    - 보안 위험 발생 요인 (허용하지 않은 주소에서 네트워크 접속 시도)

- 디버그 트레이스 
    - 실행 경로 완전 분석, 버그 발생 직전 변숫값 추적 
       - 스레드 ID + 트레이스를 생성한 함수 이름 + 트레이스를 생성하는 코드의 파일이름 
    - 디버그 모드 + 링버퍼(순환 버퍼)로 트레이스 정보 추가 가능 


> C++ 에서는 매크로 자제해야함 (디버깅 힘들어짐, 하지만 로깅할 때는 코드를 간결하게 할 수 있음)

### 런타임 디버그 모드 

- 디버그 모드를 동적으로 제어하는 비동기 인터페이스를 제공 
    - 프로세스간 호출(IPC)을 실행하는 비동기식 커맨드로 만들 수 있음
    - 또는 메뉴 커맨드 형태로 제공 

#### 링버퍼

- 로그 파일을 순환 
    - 트레이스를 디스크보다는 메모리에 저장, 모든 트레이스 메시지를 표준 출력에, 필요할 때만 로그파일에 


- 링 버퍼: 메시지 수가 정해져 있거나 크기가 일정한 메모리 공간에 메시지를 저장 

- 버퍼가 차면, 버퍼의 시작점으로 돌아가서 메시지를 기록 


### 어서션 

- assert 매크로  
    -  false 면 에러 메시지 출력, 프로그램 종료 

- assert를 사용하면, 버그를 조기에 발견 가능 

- 변수 상태에 대해 암묵적으로 가정한 부분마다 항상 어서션을 사용 
    - nullptr 검사  (이런 암묵적 가정은 최소화 하는 것이 좋긴함)


### 크래시 덤프 

- 앱이 갑자기 죽을 때 생성 
    - 서드파티 라이브러리 활용 
        - Breakpad 라는 오픈소스 클로스 플랫폼 

- 심벌 서버: 릴리즈 버전의 소프트웨어에 대한 디버깅 기호를 저장 
    - 사용자가 발생한 크래시 덤프를 해석하는 데 활용 

- 소스 코드 서버: 소스 코드의 수정 내역을 모두 저장 


### 정적 어서션 

- static_assert : 컴파일 시간에 어서션 적용 

- 타입 트레이트와 함께 쓰는 경우가 있음 

    - 템플릿 타입이 주어진 조건에 만족하지 않을 때 컴파일 에러를 발생시킴 
        - `static_assert(is_base_of_v<Base1, T>);`


## 디버깅 테크닉 

- 버그를 재현하는 것 
    - 똑같은 환경, 상황과 비슷한 입력값 시도 
    - 스트레스 테스트 

- 재현 한 후, 버그를 유발시키는 동작을 최소화 하는 방법을 찾음 

### 회귀 버그 디버깅 

- 예전 버전들 중에서 문제가 발생하기 이전과 이후의 버전을 이진 탐색 방식으로 찾아가는 것 

### 메모리 문제 디버깅 

-  메모리 누수 
    - 메모리를 할당한 후, 해제하지 않음 

- 할당 연산과 해베 연산이 일치하지 않음
    - delete, delete[]

- 메모리를 한 번 이상 해제 
    - 두 개의 delete 문 


- 할당받지 않은 메모리 해제 

- 스택 메모리 해제 

- 엉뚱한 메모리에 접근 

- 해제된 메모리에 접근 

> 댕글링 포인터: 포인터가 여전히 해제된 메모리 영역을 가리킬 때 

- 다른 곳에서 할당한 메모리에 접근  (배열 경계 넘는 것)

- 초기화되지 않은 메모리 읽기 

#### **메모리 디버깅 관련 팁**

- C++ 용 메모리 검사 도구 활용 or purify or valgrind  or application verifier 

- 