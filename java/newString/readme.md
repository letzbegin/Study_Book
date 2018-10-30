# String pool이란?
* heap영역 내부에 String을 관리하는 영역이다.
* String은 만들어 질때마다 새로운 객체를 생성한다. 그러면 메모리 효율이 떨어지기 때문에 `pool` 영역을 만들어 같은 내용일 경우 해당 값을 참조한다

# new String()과 ""의 차이
* 위에서 서술 했든 ""로 만들면 해당 객체를 string pool 에서 관리한다. 때문에 같은 내용이면 수에 관계없이 참조값은 하나이다
* new String()으로 생성시 string pool에 의해 관리되지 않는다. 때문에 만들수록 그 수만큰 객체가 생성된다. 효율은 떨어지며 관리도 GC에 의해 관리된다

# Intern()
* new String()으로 생성 했어도 intern()메소드를 통해서 string pool에 관리하에 들어가게 할 수 있다.
