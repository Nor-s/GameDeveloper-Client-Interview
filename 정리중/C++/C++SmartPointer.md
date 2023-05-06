smart pointer 에 대해
    - unique, shared, weak ... auto 
    - unique: 소유자가 하나, `move`를 통해 소유권이동, shared로 변경 가능
    - shared: 레퍼런스 카운터 사용, unique 포인터로 변경 불가
    - weak: shared 포인터를 사용할 때 발생하는 문제 (순환참조 문제)로인해 메모리 해제가 제대로 되지 않을 수 있음
    - `make_*` 함수로 생성하는 것이 좋음 (new로 할당할 시 실패하면 메모리 누수가 발생)
