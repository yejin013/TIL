# JVM (Java Virtual Machine)

## JVM 동작 순서
1. 클래스 Load
2. (main을 제외한) static 멤버 초기화
3. 상속관계 파악
4. main 수행

## JVM 메모리 구조

<img src="https://user-images.githubusercontent.com/45252618/200121341-142f25dd-7ee5-48da-825a-4a87124598c3.jpg">
<img src="https://user-images.githubusercontent.com/45252618/200121344-fe12d323-a91f-4122-ac9e-f78fd5dc5e6b.jpg">

### Instance 영역
- `new`로 생성한 객체들이 올라가는 영역

Young Generation은 전체 Heap 영역의 1/2보다 약간 적게 설정하는 것이 좋고, Survivor Space는 Young Generation의 1/8 정도의 크기가 적당하다.

**young gen**
- eden
  - `new` 연산자가 실행할 때 객체가 생성되는 영역
  - 꽉 차면 `Minor GC`가 아직 살아있는 객체를 S0 영역으로 이동 
- S0 (Survive 0)
  -  `eden` 영역에서 살아남은 객체가 이동한 영역
  -  꽉 차면 `Minor GC`가 아직 살아있는 객체를 S1 영역으로 이동 (age 증가) 
- S1 (Survive 1)
  - `S0` 영역에서 살아남은 객체가 이동한 영역
  - 꽉 차면 `Minor GC`가 아직 살아있는 객체를 old gen 영역으로 이동 (age 증가)

cf) `Minor GC`는 매우 빠르고 효율적이다. 소요시간은 1초 미만이다. JVM Thread Processing을 멈추게 하는 등의 부작용도 발생하지 않는다. 

**old gen**
- 생명 주기가 긴 `오래된 객체`가 있는 영역 
- `Major GC`가 작동한다.

cf) `Major GC`는 객체들이 Lived 상태로 있는지 여부를 파악하기 위해 모든 Thread의 실행을 멈추고 살아있는 객체를 표시하고 사용하지 않는 객체는 제거하여 heap을 정리한다. 그렇게 때문에 CPU에 부하를 가하게 되고 `Minor GC`에 비해 10배 이상의 시간을 사용하여 성능에 악영향을 준다.

### Method 영역
- 프로그램을 종료하지 않는 이상 삭제되지 않는다.

**일반**
- 생성자, 메소드 등이 저장되는 영역

**static**
- main을 제외한 static 메소드와 변수(값) 저장

**final**
- final로 정의된 메소드와 변수(값) 저장 

**String Literal Pool**
- `""`로 정의된 String Literal 저장
- new로 생성된 String 값은 저장되지 않는다. (Instance 영역에 저장)
- method 영역이므로 프로그램이 종료되지 않는 이상 사라지지않기 때문에 계속 사용할 값들을 String Literal로 정의해주는 것이 좋다.

### Stack
- main을 비롯한 local 메소드에서 정의한 값들이 올라가는 영역
- thread별 메소드 실행시 생성되는 primitive data를 올리는 영역

### PC-register
어디를 수행했는지 알고 있다.

**main을 닫으면 demon 시스템이기 때문에 관련 데이터가 전부 삭제된다.**
  
