## 주의해야 할 예시

```
package hello.hellospring.repository;  
  
import hello.hellospring.domain.Member;  
import java.util.List;  
import org.junit.jupiter.api.AfterEach;  
import org.junit.jupiter.api.Test;  
import static org.assertj.core.api.Assertions.*;  
class MemoryMemberRespositoryTest {  
    MemoryMemberRepository repository = new MemoryMemberRepository();  
  
    @Test  
    public void save() {  
        Member member = new Member();  
        member.setName("spring");  
        repository.save(member);  
  
        repository.findById(member.getId()).get();  
  
        Member result = repository.findById(member.getId()).get();  
  
        //System.out.println("result = " + (result == member));  
        //Assertions.assertEquals(member,result);        assertThat(result).isEqualTo(member);  
  
    }  
  
    @Test  
    public void findByName(){  
        Member member1 = new Member();  
        member1.setName("spring1");  
        repository.save(member1);  
  
        Member member2 = new Member();  
        member2.setName("spring2");  
        repository.save(member2);  
        // shift F6으로 rename이 가능하다!  
        // shift enter를 통해 다 친 코드 중간에서 아래로 내려갈 수 있다.  
  
        Member result = repository.findByName("spring1").get();  
        assertThat(result).isEqualTo(member1);  
    }  
  
    @Test  
    public void findAll(){  
        Member member1 = new Member();  
        member1.setName("spring1");  
        repository.save(member1);  
  
        Member member2 = new Member();  
        member2.setName("spring2");  
        repository.save(member2);  
  
        List<Member> result = repository.findAll();  
  
        assertThat(result.size()).isEqualTo(2);  
    }  
}
```

테스트 코드를 위와 같이 작성하게 되면 findByName부분에서 에러가 나게 된다. 그 이유는 findAll에서 작성한 member1 객체와 findByName에서 작성한 member1 객체가 겹쳐 오류를 발생시키기 때문이다.(Test는 작성된 순서대로 실행되는 것이 아니다. => 알파벳 순서대로 or 작성된 순서의 반대로 실행하는 듯?) 
따라서 테스트코드를 제대로 실행하기 위해서는 각 테스트 케이스를 실행할 때마다 @AfterEach를 추가해주어서 한 테스트가 끝날 때마다 repository를 지워주게 된다.

따라서 

```
@AfterEach  
public void afterEach(){  
    repository.clearStore();  
}
```

위 코드를 추가하여 아래 코드처럼 테스트 케이스를 작성해야 한다.

```
package hello.hellospring.repository;  
  
import hello.hellospring.domain.Member;  
import java.util.List;  
import org.junit.jupiter.api.AfterEach;  
import org.junit.jupiter.api.Test;  
import static org.assertj.core.api.Assertions.*;  
class MemoryMemberRespositoryTest {  
    MemoryMemberRepository repository = new MemoryMemberRepository();  
  
    @AfterEach // 한 함수가 끝날 때마다 동작하게 한다.  
    public void afterEach(){  
        repository.clearStore();  
    }// 이 함수가 없다면 member1 객체를 유지한채로 test를 진행하기 때문에  
    // 다른 함수를 실행할 때 오류가 발생할 수 있다.  
  
    @Test  
    public void save() {  
        Member member = new Member();  
        member.setName("spring");  
        repository.save(member);  
  
        repository.findById(member.getId()).get();  
  
        Member result = repository.findById(member.getId()).get();  
  
        //System.out.println("result = " + (result == member));  
        //Assertions.assertEquals(member,result);        assertThat(result).isEqualTo(member);  
  
    }  
  
    @Test  
    public void findByName(){  
        Member member1 = new Member();  
        member1.setName("spring1");  
        repository.save(member1);  
  
        Member member2 = new Member();  
        member2.setName("spring2");  
        repository.save(member2);  
        // shift F6으로 rename이 가능하다!  
        // shift enter를 통해 다 친 코드 중간에서 아래로 내려갈 수 있다.  
  
        Member result = repository.findByName("spring1").get();  
        assertThat(result).isEqualTo(member1);  
    }  
  
    @Test  
    public void findAll(){  
        Member member1 = new Member();  
        member1.setName("spring1");  
        repository.save(member1);  
  
        Member member2 = new Member();  
        member2.setName("spring2");  
        repository.save(member2);  
  
        List<Member> result = repository.findAll();  
  
        assertThat(result.size()).isEqualTo(2);  
    }  
}
```
