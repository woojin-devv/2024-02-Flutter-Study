## Flutter의 사용 용도

1. 모바일 앱 개발 : Android와 iOS 플랫폼 모두에서 네이티브 성능을 가지는 앱을 한번의 코드베이스로 개발 가능.
   
2. 웹 어플리케이션 : Flutter 코드를 사용해 웹 브라우저에서 실행되는 웹 어플리케이션을 만들 수 있음.
   
3. 데스크탑 어플리케이션 

4. 임베디드 시스템

## Dart 언어의 특징

1. Dart는 Google에서 개발한 프로그래밍 언어.

2. Dart는 JS처럼 싱글 스레드에서 비동기 작업을 관리하며 이벤트 루프를 통해 코드를 실행.

3. Dart는 클래스 기반의 객체지향 언어.

4. 모던 언어 기능 : 널 안정성, 제네릭, async/await와 같은 최식 프로그래밍 언어 기능을 지원.
<br> </br>
------------------------

## Dart #1 기본기

<br>

- parameter / argument : 매개변수
- positional parameter : 순서가 중요한 파라미터
- optional parameter : 있어도 되고 없어도 되는 파라미터
- named parameter : 이름이 있는 파라미터 (순서 중요X)
- arrow function : 화살표 함수

<br>

1. Optional Parameter (선택적 매개변수)

  * 위치 기반 선택적 매개변수는 **대괄호 []**로 정의
  * 기본값 설정하지 않으면 **null**로 간주

<br>

2. Named Parameter (이름이 있는 매개변수)  

  * 함수 호출 시 인자 전달 순서에 구애받지 않음
  * 이름이 있는 매개변수는 **중괄호 {}**사용
  * `required` 키워드를 사용하면, 특정 이름이 있는 매개 변수가 반드시 제공되어야 한다고 명시

<br></br>

### 널 안정성(null safety) 관련 연산자

1. `?` (Nullable  타입 표시)

  * Dart의 널 안정성 기능으로 인해 모든 변수는 기본적으로 널을 허용하지 않음.(non-nullable 타입)
  * `?`을 사용하면 nullable 타입으로 정의할 수 있어 해당 변수에 null 값을 하용
  * 따라서 null일 가능성을 처리해야 함
  
2. `!` (Null Assertion Operator)

  * `!`는 널이 아님을 확신할 때 사용하는 연산자

  ```dart
  String? name; // name은 null일 수 있다.

  void main() {
    name = 'Alice';
    print(name!); // null이 아님을 확신하고 사용.
  }
  ```

<br></br>

### `typedef` : 타입 정의(Type Alias)

  > 타입에 별칭을 붙이는 기능으로, 함수 타입을 정의하거나, 복잡한 타입을 단순하게 표현

  Operation 타입 : Dart에서는 일반적으로 함수의 타입을 의미
  Signature (시그니처) : 함수의 타입 정보를 나타냄. 매개변수의 타입과 반환 타입으로 구성.

  ```dart
  void main() {
    // 함수 타입에 typedef 사용
    Operation operation = add;

    int result = operation(10, 20, 30);
  }
  // 3개의 int 값을 받아 int 값을 반환하는 함수 타입에 `Operation`이라는 별칭 부여
  typedef Operation = int Function(int x, int y, int z);

  // 함수 `add`는 `Operation`타입으로 취급
  int add(int x, int y, int z) => x + y + z;
  int subtract(int x, int y, int z) => x - y- z;

  int calculate(int x, int y, Operation operation){
    return operation(x, y, z);
  }
  ```

  시그니처 예시 :
  ```dart
  // 함수 시그니처 : int Function(int, int)
  int add(int a, int b) {
    return a + b;
  }
  ```
<br> 

### Signature와 `typedef`의 결합

  ```dart
  // `MathOperation`은 두개의 int를 받아 int를 반환하는 함수 시그니처를 정의한 typedef
  typedef MathOperation = int Function(int a, int b);

  int subtract(int a, int b) {
    return a - b;
  }
  int multiply(int a, int b) {
    return a * b;
  }

  // `performOperation`는 `MathOperation` 타입의 함수와 두개의 정수를 받아 해당 함수를 호출
  void performOperation(MathOperation operation, int x, int y) {
    print(operation(x, y));
  }

  // `subtract1와 `multiply` 함수 모두 `MathOperation` 시그니처와 일치하기 때문에
  // `performOperation`에 전달 가능.
  void main() {
    performOperation(subtract, 10, 5);
    performOperation(multiply, 10, 5);
  }
  ```

<br></br>

--------------------------


## Dart #3 Functional Programming 함수형 프로그래밍 

> Dart에서 `Map`은 키-값 쌍으로 데이터 저장하는 컬렉션 타입

* 키와 값의 타입은 제네릭으로 정의할 수 있으며, `Map<K, V>` 형태로 사용

  >> **제네릭으로 정의한다**는 것은 키와 값의 타입을 고정하지 않고 다양한 타입을 지원할 수 있도록 설계한다는 의미.

<br>

### Map 생성 방법

* 기본 생성자

```dart
void main() {
  Map<String, String> countries = {
    'KR' : 'Korea',
    'JP' : 'Japan',
  };
  print(countries)l
}
```

* **리스트나 이터러블**에서 `Map`을 생성

  > 이터러블(Iterable)이란?

**이터러블**은 반복 가능한 객체를 뜻하며, Dart에서 `Iterable`은 리스트, 세트, 멥 과 같은 데이터 구조에서 사용할 수 있다.

* 이터터블은 요소를 순차적으로 접근할 수 있는 객체
* 이터러블은 iterator 메서드를 통해 각 요소를 반복(iterate)할 수 있다
* 이터러블은 보통 `for-in` 구문으로 쉽게 순회 가능

<br>

```dart
void main() {
  List<String> blackPink = ['로제', '지수', '제니', '리사'];

  // map() 함수를 사용해 문자열을 추가한 새로운 리스트 생성
  final newBlackPink = blackPink.map((x){
    return '블랙핑크 $x';
  });

  print(blackPink);
  print(newBlackPink);
}
```

<br>

`map()` 함수는 이터러블은 반환하므로, `newBlackPink`를 리스트로 만들고 싶다면 `toList()` 사용해야 함.

<br></br>

### `where` 메서드로 조건 필터링

`where()`는 Dart에서 이터러블의 요소를 조건에 맞게 필터링할 때 사용하는 함수

```dart
void main() {
  List<Map<String, String>> people = [
    {
      'name': '로제',
      'group': '블랙핑크',
    },
    {
     'name': '뷔',
      'group': 'BTS',
    },
  ];

print(people);

final blackPink = people.where((x) => x['group'] == '블랙핑크').toList();
}
```

* `where()` 함수는 이터러블을 반환하므로, 결과를 리스트로 반환하려면 toList()사용해야함.

<br></br>

### `fold()` 함수

`fold()` 함수는 이터러블의 모든 요소를 하나의 값으로 누적하는 데 사용

이터러블의 각 요소를 순차적으로 처리하면서, 이전 연산의 결과와 현재 요소흫 기반으로 값을 계산하는 방식.
**초기값**과 **누적 함수**를 받아서 이터러블의 요소를 처리한다.

> ```T fold<T>(T initialValue, T Function(T previousValue, E element) combine)```

* `T` : 반환값의 타입
* `initialValue` : 초기 누적값
* `combine` : 두 매개변수를 받는 함수. `previousValue`(이전 누적값)과 `element`(현재 요소)를 받아서 새로운 값을 반환

<br>

#### 동작 과정

1. `initialValue`는 누적값의 초기상태를 의미. 이 값에서 계산을 시작.

2. 이터러블의 첫 번쨰 요소와 `initialValue`를 `combine`함수에 전달하여 새로운 누적값 계산.

3. 계산된 누적값과 다음 요소를 다시 `combine` 함수에 전달하여 계속해서 처리.

4. 이 과정이 모든 요소에 대해 반복.   

```dart
void main() {
  List<Map<String, int>> people = [
    {'name': 'Alice', 'age': 30},
    {'name': 'Bob', 'age': 25},
    {'name': 'Charlie', 'age': 35},
  ];

  // 초기값 0에서 시작하여 각 사람의 age값을 더해나감
  int totalAge = people.fold(0, (sum, person) => sum + person['age']!);

  print(totalAge); // 출력: 90
}
```






