# 컨테이너

- 순차(선형) 컨테이너: vector, deque, list, forward_list, array
- 컨테이너 어댑터: queue, priority_queue, stack
    - 기존 컨테이너의 인터페이스를 제한하여 만든 기능이 제한되거나, 변형된 컨테이너를 의미
    - 반복자를 지원하지 않음, STL 알고리즘 불가
    - vector, deque 기반... 

- 연관 컨테이너: map, multimap, set, multiset 
- 비정렬 연관 컨테이너(해시 테이블): unordered_map, unordered_multimap, unordered_set, unordered_multiset
- 기타 컨테이너: 표준 C 스타일 배열, string, 스트림, bitset
    - 전통적인 분류에 따르면 입출력 스트림은 컨테이너가 아님 (원소저장 x) 
    
- 이기종 컨테이너(heterogenous): pair, tuple
    - 다른 타입을 가짐


- 표준라이브러리 컨테이너의 원소에 접근하는 기능을 위해 반복자 패턴을 사용
    - 특수한 스마트 포인터인 반복자 
        - `std::distant` 함수를 사용하여 두 반복자 사이의 거리를 구할 수 있음
    - 컨테이너를 순차적으로 접근할 때는 인덱스로 원소를 하나씩 조회하는 것 보다 반복자를 사용하는 것이 더 효율적
        - vector는 예외, list, map, set에 대해서는 반복자를 사용하는 것이 항상 빠름

## 연관

- 노드 기반 데이터 구조라 부름 
    - `extract` 메서드로 이동 가능 
    - `merge` 메서드로 병합 가능 (목적대상에 이미 그 원소가 있으면 이동 x)

### map

- 트리구조(레드블랙), log(n) 삽입 삭제 검색,

### unordered_map


- 해쉬, 삽입 삭제 검색이 상수시간, key 충돌 가능성, key의 타입에의해 성능이 달라질 가능성(int가 가장 빠름, string 사용시 map 과 unordered_map 의 성능의 차이가 크지 않음)

- 해쉬 구조인 unordered_map이 메모리가 모여있기에 캐시히트율이 좀 더 높음 

#### Hash

- 데이터를 여러 개의 버킷에 나눠서 저장
1. 주어진 key의 해시값을 계산(hash func)
2. 이를 버킷 개수로 나눈 나머지를 구해서 어떤 버킷에 들어갈지 계산

- 다른 key여도 같은 버킷에 들어갈 수 있음
    -  해시 충돌에 대응하기 위해 std::unordered_map은 각 버킷마다 linked list로 (key, value) 쌍을 저장
        -  하나의 버킷에 모든 데이터가 삽입될 가능성이 있음
        -  데이터 처리위해 linked list를 순회 => 최악의 경우 시간복잡도가 O(n)
    - 기본 hash function: `std::hash`
        -  지원 타입: int, double 등의 primitive type, std::string 등
 
 

- 

## deque, vector

- 여러개의 배열로 관리

std::deque는 여러 개의 버퍼에 데이터를 나눠서 저장

-  std::vector는 할당된 공간이 전부 차면 배열을 통째로 새로 할당
    - std::deque는 버퍼 하나만 할당
        - 데이터 삽입 항상 O(1)

- Vector의 요소들은 메모리상에 연속적으로 존재하는 것이 보장
    - C배열의 라이브러리와 상호작용해야 하는 상황 유리
    - 공간지역성을 고려 해야하는 상황 유리
- deque는 메모리에서 요소들이 연속적 x 

### vector 사용시 주의점 

- push_back을 사용할 때, vector가 재할당이 일어나서 반복자가 무효화될 수 있음
- 반복자가 참조하는 원소가 삭제할 대상의 다음 원소를 가리키고 있을 때, 원소를 삭제하면 반복자가 무효화될 수 있음

> deque는 원소를 추가해도 반복자가 무효화되지 않음

## list, forward_list 

- list: double linked list 
- forward_list : single linked list

> splice 연산을 적용하여 다른 list에 list를 추가 가능 (인수로 전달한 원본 list는 변경됨)

## 사용 예

### vector 

- 라운드 로빈 스케줄링 (round-robin scheduling)
    - 프로세스 목록에 나온 순서대로 하나씩 할당, 마지막 프로세스의 실행이 끝나면 다시 첫번째 프로세스로 돌아가서 할당
    
### priority_queue

- 에러 상관관계 
    - 시스템 에러 처리 시스템 : 에러 상관관계 우선순위에 따라 에러 처리