# 아이템 65. 리플렉션보다는 인터페이스를 사용하라.

## 리플렉션(java.lang.reflect)

- 런타임에 구체적인 클래스 타입을 알지 못해도 해당 클래스의 정보에 접근할 수 있도록 하는 Java API
- 사용자가 작성한 소스코드는 컴파일 과정에서 바이트 코드로 변환되어 static 영역에 저장되는데,
  런타임에 해당 바이트 코드의 정보들에 직접 접근해 특정 클래스의 정보를 획득하는 방식
- 리플렉션 기능을 이용해 다음의 동작을 수행 가능
  - 특정 클래스의 이름을 이용해 해당 클래스의 Constructor, Method, Field 인스턴스들에 접근할 수 있음
    - 해당 인스턴스들을 이용해 해당 클래스의 멤버 변수들의 타입, 이름, 메서드 시그니쳐 등을 가져올 수 있음
    - 해당 인스턴스들를 이용해 각각에 연결된 실제 생성자, 메서드, 필드의 상태를 조작하거나
      컴파일 시점에 존재하지 않는 클래스를 생성해 사용할 수 있음
- 일반적으로는 프레임워크, 라이브러리에서 사용자가 어떤 사용자 정의 클래스를 생성해 사용할지
  알 수 없기 때문에 이에 대한 문제를 동적으로 해결하기 위해 사용.
  - 애플리케이션 개발 과정에서는 구체적인 클래스를 충분히 알 수 있기 때문에,
    다음의 단점들을 고려하여 굳이 사용하지 않음

### 리플렉션의 문제점

- 리플렉션을 통해 클래스에 접근하거나 사용하는 데 있어서의 자유도를 높일 수 있지만, 다음과 같은 문제점들이 존재
  - 컴파일 시점에 타입을 검사함으로써 얻을 수 있는 이점을 얻을 수 없음
    - 예외처리에 대한 대비가 충분히 되어있지 않은 상태에서 존재하지 않거나 접근할 수 없는
      클래스, 생성자, 메서드 등에 접근을 시도할 경우 예외가 발생할 수 있음
    - private 타입의 인스턴스나 변수에도 접근할 수 있기 때문에
      캡슐화를 해치는 등의 객체지향의 이점을 해치는 문제가 발생할 수 있음
  - 리플렉션을 활용하는 경우 코드의 가독성이 현저히 떨어짐
  - 리플렉션을 통한 접근은 일반적인 접근보다 느린 성능을 보이고, JVM을 최적화하기 어려움

### 리플렉션은 어떻게 사용해야 할 것인가?

- 컴파일 타임에, 소스코드를 작성하는 시점에 확정되지 않은 특정 클래스의 하위 클래스나
  인터페이스의 구현체를 사용하게 되는 것을 염두해야 하는 경우에 제한적으로 사용하는 것이 권장됨
  - 상위 클래스나 인터페이스를 타입으로 정의한 뒤, 상위 클래스를 상속받은
    하위 클래스나 인터페이스의 구현체를 리플렉션을 이용해서 생성해 사용할 수 있음
- 이외에도 테스트 코드의 결과를 효과적으로 검증하기 위해 비공개 필드에 접근하는 데 사용될 수도 있음
