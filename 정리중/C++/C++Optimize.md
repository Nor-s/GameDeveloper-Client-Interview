# C++ 최적화 관련

## Copy Elision 

- 컴파일러가 복사 또는 이동 연산자를 회피 할 수 있으면 회피하는 것을 허용 
    - RVO인 경우, class type의 template object 가 동일한 유형의 복사될 때

- Guaranteed 키워드로 확실하게 보장 가능

## RVO, NRVO 

> https://post.naver.com/viewer/postView.nhn?volumeNo=9735713&memberNo=559061

- RVO, NRVO는 이름의 유무 차이

- 함수의 반환값이 특정 객체의 값 형식일 때 복사생성자 회피 
    - 임시객체의 복사생성자, 소멸자 호출을 회피하는 것

### 주의 

- 반환할 객체에 다른 짓을 하면 안됨 .
    - if else 문으로 반환할 값종류가 바뀔 가능성이 있으면 x

- 리턴할 때 std::move()  x
    - 기본적으로 지역변수를 리턴할 때, rvalue로 취급되므로  기본이 이동연산


## EBO

공백 기본 클래스 최적화 (Empty Base Optimization)

- 공백 클래스: 개념적으로 차지하는 메모리 공간이 없어야함
    - non-static 멤버가 없음 (static 멤버를 가질 수 있음)
    - virtual 함수가 없음 (non-virtual 멤버 함수를 가질 수 있음)
    - virtual base 클래스가 없음 (typedef나 enum 등을 가질 수 있음)

하지만, C++ 독립 구조 객체는 반드시 크기가 0을 넘어야한다는 제약 (char 끼워넣어 0을 넘김)

만약, 상속에 사용한다면 이러한 제약을 최적화해줌 (크기가 0)