## Dart #4 Async Programming 비동기 프로그래밍

### CPU Thread

> CPU Thread는 프로그램이 CPU에서 실행되는 작업 단위를 의미

* Dart는 **싱글 스레드** 기반으로 모든 작업이 하나의 스레드에서 순차적으로 처리된다.

* Dart의 비동기 프로그래밍 모델은 실제로 스레드를 여러 개 사용하지 않으면서 효율적으로 멀티태스킹 할 수 있게 해줌. 

<br></br>

### Asynchronous Programming (비동기 프로그래밍)

> Dart에서 비동기 프로그래밍은 동기적 방식으로 처리하기 어려운 작업을 효율적으로 처리할 수 있다.

> 시간이 오래 걸리는 작업을 기다리지 않고, 다른 작업을 먼저 처리할 수 있다.

> 비동기 작업을 처리하기 위해 `Future`, `async`/`await`, `Stream` 기능을 사용.

비동기 프로그래밍이 필요한 경우 : 
* 파일 입출력
* 네트워크 요청 : HTTP 요청으로 데이터 받아올 떄
* 타이머 및 지연 작업 : 일정 시간 동안 대기하거나 지연시키는 작업.

<br></br>

### Dart에서 비동기 프로그래밍 개념

#### `Future`

  ```dart
  void main() {
  Future<String> name = Future.value('코드팩토리');
  Future<int> number = Future.value(1);

  // 동기 코드이므로 즉시 실행됨
  print('함수 시작');

  // 2초 후에 실행될 비동기 작업을 정의
  // 해당 지연이 끝나면 비동기 콜백 함수가 실행됨
  Future.delayed(Duration(seconds: 2), (){
    print('Delay 끝');
  });
  }

  void addNumbers(int number1, int number2){
  print('계산 시작 : $number1 + $number2');

  Future.delayed(Duration(seconds: 2), (){
    print('계산 완료 : $number1 + $number2 = ${number1 + number2}');
  });

  print('함수 완료');
  }
  ```

  `Future` 객체

    * **`Future`**는 나중에 완료될 작업을 나타냄.

    * `Future.value()` : 즉시 완료되는 `Future` 객체를 반환

    * `Future.delayed()` : 지정된 시간이 지난 후에 실행할 비동기 작업을 정의.
  
#### `async/await`

  ```dart
  void main() {
  Future<String> name = Future.value('코드팩토리');
  Future<int> number = Future.value(1);

  addNumbers(1, 1);
  addNumbers(2, 2);
  }

  void addNumbers(int number1, int number2) async {
  print('계산 시작 : $number1 + $number2');

  await Future.delayed(Duration(seconds: 2), (){
    print('계산 완료 : $number1 + $number2 = ${number1 + number2}');
  });

  print('함수 완료');
  }
  ```

  1. `async`

     * `async` 키워드는 이 함수가 비동기 함수임을 나타낸다.

     * 이 함수에서 비동기 작업을 처리하는 부분은 `await`로 대기하는 부분.

  2. `await`

    * `await`는 특정 비동기 작업이 완료될 때까지 기다리는 역할.

  3. 비동기 흐름

     - `addNumbers(1,1)`이 호출되면 "계산 시작: 1 + 1"이 즉시 출력
     - 2초 동안의 지연 작업 발생. 이때 Dart의 이벤트 루프가 비동기 작업을 처리하는 동안 다른 작업이 실행될 수 있게 대기.
     - 2초 후, "계산 완료: 1 + 1 = 2"가 출력
     - 비동기 지연이 끝나면 "함수 완료"가 출력
     - `addNumbers(2,2)`도 동일한 흐름으로 실행.


  **코드 실행 결과**
  ```
계산 시작 : 1 + 1
계산 시작 : 2 + 2
계산 완료 : 1 + 1 = 2
함수 완료
계산 완료 : 2 + 2 = 4
함수 완료
  ```

<br></br>

### Stream

> `Stream`을 이용하면 데이터를 실시간으로 스트리밍하고, 그 데이터를 이벤트로 처리할 수 있다.

> Dart의 `Future`가 하나의 비동기 작업을 처리한다면, Stream은 여러 값을 비동기적으로 처리하는 데 사용.

```dart
import 'dart:async';

void main() {
  // StreamController 생성
  final controller = StreamController();
  final stream = controller.stream;

  final streamListener1 = stream.listen((val){
    print('Listener 1 : $val');
  });

  final streamListener2 = stream.listen((val){
    print('Listener 2 : $val');
  });

  // 데이터를 stream에 추가해주는 메서드
  controller.sink.add(1);
  controller.sink.add(2);
  controller.sink.add(3);
  controller.sink.add(4);
}
```

코드 실행 결과

```
Listener 1 : 1
Listener 2 : 1
Listener 1 : 2
Listener 2 : 2
Listener 1 : 3
Listener 2 : 3
Listener 1 : 4
Listener 2 : 4
```
<br>

1. `Stream`과 `async*`

  * `async*` 키워드를 사용해 비동기 제너레이터 함수를 만듦.
  * 이 함수는 여러 개의 값을 순차적으로 방출
  * `yield`를 통해 값을 스트림에 전달

2. `yield`와 `await`

  * `yield`는 스트림 값을 즉시 방출한다.
  * `await`는 비동기 작업이 완료될 떄까지 기다리는 역할.

```dart
void main() {
 calculate(2).listen((val){
    print('calculate(2) : $val');
  });

calculate(4).listen((val){
    print('calculate(4) : $val');
  });

Stream<int> calculate(int number) async* {
  for(int i = 0; i < 5; i++){
    yield i * number;

    await Future.delayed(Duration(seconds: 1));
  }
}
```

코드 실행 결과

```
calculate(2) : 0
calculate(4) : 0
calculate(2) : 2
calculate(4) : 4
calculate(2) : 4
calculate(4) : 8
calculate(2) : 6
calculate(4) : 12
calculate(2) : 8
calculate(4) : 16
calculate(2) : 10
calculate(4) : 20
```
