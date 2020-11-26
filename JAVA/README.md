* [String, StringBuffer, StringBuilder의 차이점](#String,-StirngBuffer,-StringBuilder의-차이점)

## String, StringBuffer, StringBuilder의 차이점

- 먼저, String과 다른 두 클래스(StringBuffer, StringBuilder)의 차이점을 알아보면 String은 immutable(불변)하고 다른 두 클래스는 mutable(변하기 쉬움)하다는 것이다.

### 1. String

문자열을 사용할 때 가장 많이, 보편적으로 이용되는 클래스다.
String 객체는 한 번 생성되면 할당된 메모리 공간이 변하지 않는다. 

``` java
String str = new String("example");
```
위와 같이 쓰거나, 리터럴 문자열로 바로 대입할 수도 있다. 
제일 많이 쓰는 클래스인 만큼, 빠르고 쉽게 문자열을 할당하고 사용할 수 있다는 장점이 있다.
하지만, 이 String 클래스의 단점은 **문자열에 변화를 줄 때 시간 소모가 너무 많이 든다**는 것이다.
String 객체에 할당된 메모리 공간이 변하지 않기 때문에 +연산자 혹은 concat 메소드를 통해 다른 문자열을 붙이면 새로운 String 객체를 만든 후, 새 String 객체에 연결된 문자열을 저장하고, 그 객체를 참조한다.
(즉, String 클래스 객체는 Heap 메모리 영역(가비지 컬렉션이 동작하는 영역)에 생성되며, 한 번 생성된 객체의 내부 내용을 변화시킬 수 없다. 기존 객체가 제거되면 Java의 가비지 컬렉션이 회수한다.)

``` java
for(int i = 0; i < 1000000; i++){
  str += "a";
}

System.out.println(str);
```

위의 코드에서 답은 aaaa... 이다. 여기서 주목해야 할 부분은 **+=**이다. Concatenation 작업은 메모리 초기화와 생성을 이루며 진행되기 때문에 Concatenation 작업이 많거나 loop 내에 있으면 성능이 안 좋아진다.
따라서 자세히 보면 str += "a" 는 str 뒤에 a를 붙이는 것이 아니라 str 뒤에 a를 붙인 new String을 할당하는 것이다.
concatenation 연산이 16만번 이상이 되면 실행 시간이 10초가 넘어간다고 하니, 해당 연산이 많을 경우에는 사용하지 않는 것이 좋다. (위의 코드 실행 시 1분!)

### 2. StringBuffer + StringBuilder

StringBuffer와 StringBuilder는 위의 String과 다르게 가변적(mutable)이다.

따라서 두 클래스 모두 문자열에 변화를 주면 해당 문자열에서 변경시킬 수 있다.

``` java
StringBuilder string = new StringBuilder();
for(int i = 0; i < 1000000; i++){
  string.append("a")
}

System.out.println(string);
```

위와 같이 사용하면 되고, StringBuffer도 비슷하게 사용하면 된다.
String과는 다르게 append 시 "a"가 붙은 새로운 문자열을 생성하는 것이 아니라, 기존 문자열에 "a"를 붙인다. 그래서 String에 비해 메모리 파기와 생성에 드는 시간이 단축된다.

### 3. StringBuilder vs StringBuffer

 두 클래스는 모두 mutable 특성을 가지고 있지만, 두 클래스의 차이점은 **thread-safe 지원 여부**이다.
 **StringBuffer는 String과 같이 thread-safe의 특성을 가진다.** 즉, 멀티쓰레드 환경에서 사용하기 적합하다.
 반대로, StringBuilder는 멀티쓰레드에서 쓰기엔 좋지 않다. 하지만, **StringBuilder는 멀티쓰레드가 아닌 환경**에서 StringBuffer보다 **더 빠른 성능**을 보인다.
 
 - 정리
 
|     | String | StringBuilder | StringBuffer|
|-----|--------|--------------|---------------|
|변화 특성| immutable | mutable | mutable |
|Thread-safe 지원 여부 | O | O | X |
