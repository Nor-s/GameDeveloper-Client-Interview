- static 멤버 함수는 오버라이드 불가 
    - static 함수는 virtual function table 에 등록되지 않음 

- 함수의 static local 변수는 코드가 처음 실행될 때만 초기화됨 
    -   Static variables are created at the start of the problem. If the initializer is constexpr, they can be initialized at this point. If not, they are zero-initialized at this point. For local statics with a non-constexpr initializer, they are reinitialized the first time the variable definition is encountered.