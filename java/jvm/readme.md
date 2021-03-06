출처:http://asfirstalways.tistory.com/158

# 자바 가상 머신 (JVM)

---
* JVM이란 자바 가상머신의 약칭.
* 자바 응용프로그램(어플리케이션)울 클래스 로더를 통해 읽고 API를 이용 실행하는것.
* JVM은 OS와 JAVA사이의 __중개자__ 역할을 수행하여 __OS__ 의 구속되지 않게 해준다.
* JVM은 __메모리관리, GC(가비지컬렉션)__ 을 수행한다.
* JVM은 __스택__ 기반의 가상머신이다.

---
## 자바프로그램 실행 과정
>1. 프로그램이 실행되면 JVM은 OS로 부터 메모리를 할당 받는다. _(이때 JVM은 메모리를 용도에 따라 분리하여 이용한다.)_
>2. 자바 컴파일어(javac)가 자바 소스코드(.java)를 읽어 바이트 코드(.class)로 변환한다.
>3. Class loader를 통해 class 파일을 JVM으로 로딩한다.
>4. 로딩된 class 파일들은 Execution engine을 통해 해석된다.
>5. 해석된 바이트 코드는 Runtime Date Areas에 배치되어 실질적인 수행이 이루어지게 된다. _(그 과정속에서 쓰레드와 GC같은 작업이 이루어진다)_

## JVM 구성

### Class Loader(클래스 로더)
* JVM내로 클래스(.class파일)를 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈.
* Runtime 시에 동적으로 클래스를 로드. [런타임과 컴파일 타임의 차이]()
* jar파일 내 클래스들을 JVM위에 탑재하고 사용하지 않는 클래스들은 메모리에서 삭제 한다.

### Execution Engine(실행 엔진)
* 클래스를 실행시키는 역할.
* 클래스 로더가 데이터 영역에 배치시킨 바이트 코드를 실행시키는 역할.
* 바이트 코드를 JVM내에서 좀 더 기계어에 가까운 형태로 변경(두가지 방식)한다.

> 1. #### interpreter(인터프리터)

> 2. #### JIT(Just - In - Time)
> * 인터프리터의 단점을 보안하기 위한 컴파일러.
> * 인터프리터 방식으로 실행하다 일정 정도를 넘어서는 메서드를 `네이티브 코드`로 컴파일하여 이후 네이티브 코드로 직접실행.

> 3. #### GC(가비지 컬렉션)
> * `young`영역과 `old` 영역으로 구분되어 존재
> * 각각의 영역을 하나의 `eden` 영역과 두개의 `survivor` 영역이 존재
> * `eden`영역에 객체가 가득차면 GC이 발생, `eden`영역의 값이 `survivor`영역으로 이동, `eden`영역을 객체를 삭제
> * `survivor`가 가득차면 `old`영역으로 이동, `survivor`객체 삭제
> * `old`영역이 가득차면 영구 영역으로 이동 , `old`영역 

## Runtime Data Area
* 프로그램을 수행하기 위해 OS에서 할당 받는 메모리 공간

### Thread(쓰래드)
> 1. PC Register
>   * 쓰래드 실행시 실행
>   * 쓰래드가 어떤 부분을 어떤 명력으로 실행할지에 대한 기록을 하는부분으로 JVM 명령의 주소를 갖는다

> 2. JVM stack(스택영역)
>   * 임시로 사용후 소멸되는 데이터를 저장하는 영역.
>   * 각종 형태의 변수, 임시데이터, 스레드, 메소드의 정보를 저장.
>   * 메소드 호출 시 마다 각각의 스택 프레임이 생성, 프레임별로 삭제 한다.
>   * 임시로 사용후 소멸되는 데이터를 저장하는 영역.

> 3. Native method stack(네이티브 메소드 스택)
>   * 바이트 코드가 아닌 기계어로 작성된 프로그램을 실행하는 영역
>   * JAVA 언어가 아닌 다른 언어로 작성된 코드를 위한 영역
>   * JAVA Native interface를 통해 바이트 코드로 전환하여 저장
>   * 커널(_OS의 핵심기능으로 하드웨어의 자원을 어플리케이션에 할당해 주는 부분_)이 스택을 잡아 독자적으로 프로그램을 실행시키는 영역

> 4. Method/Class/Static Area(클래스 데이터 공간)
>   * 클래스 정보를 처음 메모리 공간에 올릴때 __초기화되는 대상을 저장하기 위한 메모리 영역__
>   * static 영역의 경우 __프로그램의 시작부터 종료시 까지 메모리에 존재__
>   * __Runtime Constant Pool__ 이라는 별도의 관리영역이 존재, 상수 자료형을 저장하여 중복을 막고 참조한다.

>>   * Field Information (맴버변수의 이름, 데이터타입, 접근 제어자에 대한 정보)
>>   * Method Infomation (메소드 이름, 리턴타입, 매개변수, 접근제어자 정보)
>>   * Type Infomation (class, interface 여부 저장, type의 속성, 이름, super class 의 이름)

> 5. Heap(힙 )
>   * 객체를 저장하는 가상 메모리 공간. 
>   * new 연산자로 생성된 객체와 배열을 저장.
>>   * Permanent Generation 
>>   * New/Young 영역
>>   * Old 
