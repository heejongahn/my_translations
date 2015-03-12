# Retiring a great interview problem
[original](http://thenoisychannel.com/2011/08/08/retiring-a-great-interview-problem)

소프트웨어 엔지니어를 인터뷰하는 것은 쉽지 않다. Jeff Atwood는
[코드를 쓸 줄이라도 아는](http://blog.codinghorror.com/why-cant-programmers-program/)
사람을 고용하는 것이 얼마나 어려운지에 대해 한탄하곤 한다. 테크 언론들은 산발적으로
나를 민망하게 만드는
["최고의" 인터뷰 질문들 따위의 기사](http://royal.pingdom.com/2008/08/15/the-best-job-interview-questions-from-microsoft-google%E2%80%A6-and-ikea/)
들을 발간한다 - 뭐, IKEA의 질문은 꽤나 마음에 들었지만. [Codility] (http://codility.com/)나
[Interview Street](http://www.interviewstreet.com/recruit/home/) 같은
스타트업들은 이러한 *(역: 괜찮은 소프트웨어 엔지니어를 고용하기 힘든)* 상황을
고용 담당자들에게 그들의 코딩인터뷰를 아웃소싱할 것을 제안할 기회로 보기도 한다.
한편, Diego Basch를 비롯한 어떤 이들은 지원자들이 화이트보드 코딩 시험에 그만 좀
시달리게 하라고 우리를 안달하는 중이다.

나는 이 상황에 대한 해결책 같은 것은 가지고 있지 않다. 나 또한 IQ 테스트나
지원자를 속여 넘기기 위한 질문들이 형편없는 소프트웨어 엔지니어링 지원자들을
평가하는데에 있어 형편없는 방법이라는 것에 동의한다. 최선의 경우에도 그러한
테스트들은 딱 하나의 바람직한 자질만을 테스트할 수 있고, 최악의 경우에는 그러한
문제들은 단순히 지원자가 비슷한 문제를 본 적이 있거나, 우연히 핵심적인 통찰에
도달할 수 있는지를 시험하는 도박이 되어 버린다. 코딩 테스트는 앞으로 매일 코딩을
하는 것이 일이 될 사람들을 평가하기 위한 훨씬 좋은 도구이지만, 전통적인 인터뷰-
전화를 통해서든 면접을 통해서든 - 역시 코딩 능력을 테스트하기 위한 차선의 선택이
될 수 있다. 또한, 코딩 질문이 문제 해결 능력, 해결법을 작동하는 코드로 옮기는
과정, 혹은 그 둘 다 중에 무엇을 테스트해야 할지도 명확하지 않다.

이러한 난관들에 대항해, Endeca, Google, LinkedIn에서 지난 몇 년간 유용하게
써먹은 인터뷰 질문을 준비해 보았다. 이 글의 끝 부분에서 이야기 하겠지만, 이
문제를 공개한다는 것은 사실 좀 슬픈 일이다. 하지만 일단은 그 문제가 무엇인지,
그리고 왜 그 문제가 그리도 효과적인지부터 이야기 해 보도록 하겠다.

### 그 문제 (The Problem)

나는 이것을 "낱말 쪼개기" 문제라고 부르는데, 자세한 내용은 아래와 같다:

> 단어들의 사전*(역: 자료구조로서의 dictionary가 아닌 단순히 legit한 단어의
목록으로서의 사전을 의미하는듯)*와 입력 문자열이 주어졌을 때, 가능할 경우 입력
문자열을 띄워쓰기로 분리된(space-separated) 사전 내의 단어들의 순열로
쪼개보아라. 예를 들어, 입력 문자열이 "applepie"이고 사전이 영어 단어들을
포함한다면 "apple pie"라는 문자열을 결과로서 리턴해야 한다.

내가 의도적으로 이 문제의 특정 부분들을 애매하게 내버려두었거나 충분히 설명하지
않음으로서 지원자에게 그것들을 지적할 기회를 주었음에 주목하라. 아래는
지원자들이 물어볼 수 있을 만한 질문들과 그에 대한 나의 답변이다:

>Q: 만약 입력 문자열 자체가 사전에 들어 있는 단어라면?

>A: 하나의 단어는 space-separated 문자열의 특수한 케이스로 생각할 수 있다.


>Q: 두 개의 단어로 쪼개지는 경우만 고려하면 되나?

>A: 그렇진 않지만, 거기서부터 시작하면 한결 수월할 것이다.


>Q: 만약 입력 문자열이 사전에 들어있는 단어들로 쪼개질 수 없다면?

>A: null, 혹은 그에 해당하는 다른 것을 리턴하도록 짜 보아라.


>Q: stemming(역: 두 단어가 합쳐지며 한 단어의 일부분이 변형되는 현상)이나 맞춤법
교정 등도 신경 써야 하나?

>A: 그런 경우는 말고 그저 분리된 그대로의 단어들이 사전에 있는 케이스만 생각하면 된다.


>Q: 만약 여러가지 가능한 분할 방법이 존재한다면?

>A: 그 중 유효한 하나의 경우만 리턴하면 된다.


>Q: 사전을 트리 / 접미사 트리 / 피보나치 힙 으로 구현하려고 하는데...

>A: 사전을 구현할 필요는 없다. 그냥 합리적인 방법으로 접근할 수 있다고 가정해.


>Q: 이 사전에는 어떤 기능이 있는데?

>A: 주어진 문자열이 들어 있는지 찾아보는 거. 그거만 있으면 되잖아.


>Q: 사전의 크기가 어떻게 되지?

>A: 입력 문자열보다는 훨씬 크고, 메모리 문제를 일으키진 않을 정도야.

지원자가 이러한 세부사항에 대해 어떻게 협상해 오는지를 보는 것은 꽤나 유익한데,
지원자의 기본적인 자료 구조/알고리즘 관련 기본적인 이해 정도는 물론이거니와
커뮤니케이션 스킬, 주의력 등을 파악하는데에 도움을 주기 때문이다.

### FizzBuzz 문제 (A FizzBuzz Solution)

문제의 명세에 관한 이야기는 이정도면 충분하고, 해결책에 대해 이야기 해보자. 어떤
지원자들은 두 개의 단어로의 분할만 고려하면 되는, 좀 더 단순화된 버전의
문제로부터 시작한다. 나는 이것을 일종의 [FizzBuzz 문제](http://c2.com/cgi/wiki?FizzBuzzTest)
*(역: 링크 참조. 1부터 100까지의 숫자를 출력하되 3의 배수에는 숫자 대신 Fizz를,
5의 배수에는 숫자 대신 Buzz를, 3과 5의 공배수에는 FizzBuzz를 출력하는 프로그램을
짜는 문제로, 대부분의 형편없는 지원자를 가려내기 위한 문제라는 의미로 사용됨)*로
간주하는데, 능숙한 소프트웨어 엔지니어라면 적어도 그들이 고른 프로그래밍 언어로
다음 코드와 동등한 정도의 답변을 충분히 낼 수 있을 것이라 기대한다.

````
String SegmentString(String input, Set<String> dict) {
  int len = input.length();
  for (int i = 1; i < len; i++) {
    String prefix = input.substring(0, i);
    if (dict.contains(prefix)) {
      String suffix = input.substring(i, len);
      if (dict.contains(suffix)) {
        return prefix + " " + suffix;
      }
    }
  }
  return null;
}
````

나는 위의 코드를 짜지 못 하는 지원자들을 만나보았는데, 심지어 그들 중에는 구글의
technical phone screen을 통과한 지원자도 있었다. Jeff Atwood가 말했듯이,
FizzBuzz 문제는 면접관들이 프로그램을 짤 줄도 모르는 프로그래머들을
인터뷰하는데에 시간을 낭비하는 것을 막는 훌륭한 방법이다.

### 일반해 (A General Solution)

당연하지만, 좀 더 흥미로운 문제는 입력 문자열이 임의의 갯수의 사전 내 단어로
분할될 수 있는 일반적인 경우에 대한 것이다. 이 문제에 접근하는 데에는 여러가지
방법이 있지만, 가장 직관적인 방법은 [재귀적 백트래킹](http://en.wikipedia.org/wiki/Backtracking)이다.
아래는 위의 코드를 이용해 작성한 전형적인 해결책이다:

````
String SegmentString(String input, Set<String> dict) {
  if (dict.contains(input)) return input;
  int len = input.length();
  for (int i = 1; i < len; i++) {
    String prefix = input.substring(0, i);
    if (dict.contains(prefix)) {
      String suffix = input.substring(i, len);
      String segSuffix = SegmentString(suffix, dict);
      if (segSuffix != null) {
        return prefix + " " + segSuffix;
      }
    }
  }
  return null;
}
````

소프트웨어 엔지니어링 직군에 지원한 많은 지원자들이 삼십분 내에 위와 같은,
혹은 비슷한 해결책(예를 들면 explicit stack을 이용한 해결책)을 떠올리지 못한다.
아마 그들 중에도 능숙하고 생산적인 사람이 다수 있을거라 생각하지만, 난 적어도 -
특히 대규모 검색 기능을 필요로 하는 회사에서라면 더더욱 - 그들을 정보 검색 혹은
머신 러닝 문제를 해결하게 하기 위해 고용하진 않을 것이다.

### 실행 시간 분석하기 (Analyzing the Running Time)

하지만 잠깐, 할 일이 남았다! 지원자가 위와 같은 해법에 도달했다면, 나는 그/그녀에게
입력 문자열의 길이 n에 따른 그 알고리즘의 worst-case 실행 시간에 대한 big-O
분석을 요청한다. 지원자가 내놓는 답변은 보통 O(n)에서 O(n!)까지 제각각이다.

나는 보통 아래의 힌트를 제공한다:

>하나의 알파벳으로 이루어진 단어들만을 포함하는 비정상적인 사전을 한 번
생각해보라("a", "aa", "aaa", ... 와 같이). 이 때 입력 문자열이 연속된 n-1개의 'a'로
이루어져 있고, 맨 끝에 'b'가 나온다면 무슨 일이 벌어질까?

지원자는 재귀적 백트래킹 방법을 이용할 경우 주어진 입력 문자열의 가능한 모든
분할을 탐사할 것을 깨닫고, 이 분석이 결국 가능한 분할의 경우의 수를 찾는 것으로
대신할 수 있음을 알게 될 수 있을 것이다. 그 수가 O(2^n)이라는 것을 보이는 것은
[이 힌트](http://en.wikipedia.org/wiki/Power_set)와 함께 독자들을 위한
연습문제로 남겨 둔다.

### 효율적인 해법 (An Efficient Solution)

만약 지원자가 여기까지 도달했다면, 나는 그에게 O(2^n)보다 좀 더 나은 방법이
없을지 물어본다. 많은 지원자들은 이것이 답을 유도해내는 질문이라는 것을 알아채고,
좀 더 나은 이들은 [다이나믹프로그래밍](http://20bits.com/articles/introduction-to-dynamic-programming/)
혹은 [메모이제이션](http://en.wikipedia.org/wiki/Memoization)을 사용할 수 있음을
깨닫는다. 아래는 메모이제이션을 이용한 해법이다:

````
Map<String, String> memoized;

String SegmentString(String input, Set<String> dict) {
  if (dict.contains(input)) return input;
  if (memoized.containsKey(input) {
    return memoized.get(input);
  }
  int len = input.length();
  for (int i = 1; i < len; i++) {
    String prefix = input.substring(0, i);
    if (dict.contains(prefix)) {
      String suffix = input.substring(i, len);
      String segSuffix = SegmentString(suffix, dict);
      if (segSuffix != null) {
        memoized.put(input, prefix + " " + segSuffix);
        return prefix + " " + segSuffix;
      }
    }
  memoized.put(input, null);
  return null;
}
````

이 때도 지원자는 worst-case 분석을 할 수 있어야 한다. 핵심이 되는 통찰은
SegmentString은 기존의 입력 문자열의 접미사에서만 호출되고, 입력 문자열의
접미사는 O(n)개 밖에 존재하지 않다는 것이다. substring 명령이 상수 시간 내에
실행된다는 가정 하에 위의 메모이제이션 해법에서의 worst-time 실행 시간이
O(n^2)라는 것을 보이는 것 또한 독자들을 위한 연습문제로 남겨둔다. *(역: 해당
가정은 Java 7 update 6부터는 유효하지 않다.
[링크](http://stackoverflow.com/questions/4679746/time-complexity-of-javas-substring)
참조.

### 내가 이 문제를 사랑하는 이유 (Why I Love This Problem)

내가 이 문제를 사랑하는데에는 많은 이유가 있다. 몇 가지만 나열해보면:

* 이 문제는 상용화 프로그램을 개발하는 과정에서 실제로 나타났던 문제이다. 나는
    Endeca에서 검색 질의를 재작성 기능을 구현 한 적이 있는데, 철자 교정과 유의어
    확장을 위해 실제로 이 문제를 해결해야 했다.

* 이 문제는 어떤 특수한 지식을 필요로 하지 않고, 그저 문자열, 집합, 맵, 재귀,
    그리고 다이나믹 프로그래밍 / 메모이제이션의 간단한 응용만으로 해결 할 수
    있다.  모두 컴퓨터공학과의 학부 1~2학년때 다루는 내용이다.

* 코드는 너무 뻔하지 않으면서도 너무 복잡하지도 않아 면접이든 전화 인터뷰이든
    45분 시간 제한 내에 충분히 짤 수 있다.

* 생각을 필요로 하지만, 지원자를 속이려하거나 수수께끼가 아니다. 그보다는 문제에
    대한 이론적 분석과 기본적인 컴퓨터과학의 도구들의 응용을 필요로 한다.

* 이 문제에 대한 지원자의 퍼포먼스가 이원적이지 않다. 최악의 지원자의 경우
    fizzbuzz 문제조차 45분만에 구현해내지 못한다. 최고의 지원자는 메모이제이션을
    이용한 해법을 10분만에 구현해냄으로서 당신이 - 사전이 메인 메모리에
    들어가기엔 너무 큰 경우엔 어떻게 할지 등을 물어봄으로서 - 문제를 더
    흥미롭게 만들 수 있게 해 준다. 대부분의 지원자들은 그 중간의 어디쯤의
    퍼포먼스를 보여준다.

### 행복한 은퇴 (Happy Retirement)

불행히도, 모든 좋은 것들엔 끝이 있기 마련이다. 얼마 전에 한 지원자가 이 문제를
Glassdoor에 포스팅한 것을 알게 되었다. 그 글에 있는 해법은 이 글에서 다루어진
수준까지는 파고들지 못했지만, 나는 이 정도로 좋은 문제는 적어도 품위 있게 은퇴할
자격이 있다고 판단했다.

좋은 인터뷰 문제를 생각해 내는 것은 어렵지만, 비밀을 지키는 것 또한 쉽지 않다.
[어쩌면 보다 적은 비밀만을 지키는 것이 한 방법이 될 수 있을 것이다](http://thenoisychannel.com/2010/12/22/the-secret-may-be-to-keep-fewer-secrets).
이상적인 인터뷰 문제는 관련된 사전 지식이 있더라도 큰 도움이 되지 않는 문제다.
*(역: 문제에 대한 정보를 미리 알고 있어도 실제로 문제를 푸는데에는 결국 자기
실력이 중요한 문제를 이야기하는 것 같은데 매끄럽게 해석을 못 하겠네요.) 나는
나의 동료들과 함께 그러한 접근법을 시도해 보는 중이다. 당연하지만, 만약 우리가
그런 방식을 효율적으로 사용할 수 있게 되면 관련된 정보를 공유할 것이다.

그 전까지는, 위의 "단어 쪼개기" 문제를 본 모든 사람들이 이 문제를 그들의 실력을
검증할 수 있는 테스트로 여겨주길 바란다. 어떤 문제도 완벽할 수는 없고, 하나의
인터뷰 문제에 대한 퍼포먼스가 그 지원자가 엔지니어로서 얼마나 잘 활약할지에 대한
완벽한 지표가 될 수도 없다. 그럼에도 불구하고, 이 녀석은 꽤나 괜찮은 녀석이였고,
우리는 이 문제를 꽤나 그리워 할 것이다.
