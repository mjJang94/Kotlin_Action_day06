# 인터페이스에 선언된 프로퍼티 구현

아래와 같은 추상 프로퍼티가 있는 인터페이스가 있다.
<pre>
<code>
interface User{
  val nickname: String
}
</code>
</pre>

User 인터페이스를 구현하는 클래스가 nickname의 값을 얻을 수 있는 방법을 제공해야하는데, 이 인터페이스를 구현하는 몇 가지 방법들이 있다.

<pre>
<code>
class PrivateUser(override val nickname: String) : User //생성자에 있는 프로퍼티

class SubscribingUser(val email: String) : User{
  override val nickname: String
  get() = email.subStringBefore('@') //커스텀 getter
}

class FacebookUser(val accountId: Int) : User{
  override val nickname = getFacebookName(accountId) //초기화
}
</code>
</pre>

SubscribingUser 와 FacebookUser의 nickname 구현에는 차이가 있다. 비슷해 보이지만 SubscribingUser의 nickname은 매번 호출될 때마다 subStringBefore을 호출하여 계산하는 커스텀 게터를 사용하고 있고, FacebookUser의 nickname은 객체 초기화 시 계산한 데이터를 뒷받침하는 필드에 저장했다가 불러오는 방식이다.

# 접근자의 가시성 변경

접근자의 가시성을 변경할 수 있다.

<pre>
<code>
 var counter: Int = 0
  private set
  
  fun addWord(word: String){
    counter += word.length  
  }
}
</code>
</pre>

이 클래스는 자신에게 추가된 모든 단어의 길이를 합산하는데, 전체 길이를 저장하는 프로퍼티는 클라이언트에게 제공하는 api로써 publick으로 외부에 노출된다.   
하지만 외부 코드에서 단어 길이의 합을 마음대로 바꾸지 못하도록 세터의 가시성을 private으로 지정했다.
