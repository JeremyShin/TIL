# Kotlin Unit Test 메서드

- [레퍼런스1](https://coding-food-court.tistory.com/157), [레퍼런스2](https://velog.io/@myspy/%EC%9B%90%EC%8B%9C%EA%B0%92-%ED%8F%AC%EC%9E%A5VO)
- 코틀린 테스트는 Spec이라는 여러 종류의 테스트 스타일을 지원 (아래의 클래스를 상속하여 사용)
    - StringSpec, FunSpec, ShouldSpec, WordSpec, BehaivorSpec, AnnotationSpec 등
- String Spec - 가장 간단 - 어노테이션 없이, fun 키워드 없이 테스트명을 지을 수 있어 매력적
    
    ```kotlin
    class StringSpecTest : StringSpec() {
    	init {
    		"strings.length should return size of string" {
    			"hello".length shouldzbe 5
    		}
    	}
    }
    ```
    
- Fun Spec - 함수 형태로 테스트 코드 작성
    
    ```kotlin
    class FunSpecTests : FunSpec({
    	test("String length should return the length of the string") {
    		"sammy".length shouldBe 5
    		"".length shouldBe 0
    	}
    })
    ```
    
- Should Spec - FunSpec과 비슷. test 키워드 대신 should 키워드 사용
    
    ```kotlin
    class ShouldSpecTest : ShouldSpec({
    	should("return the length of the string") {
    		"sammy".length shouldBe 5
    		"".length shouldBe 0
    	}
    })
    ```
    
- Word Spec - .. (얜 뭐지..? .. 설명이해안감 ㅠㅠ)
    
    ```kotlin
    class WordSpecTests : WordSpec({
    	"String.length" should {
    		"return the length of the string" {
    			"sammy".length shouldBe 5
    			"".length shouldBe 0
    		}
    	}
    })
    ```
    
- Behavior Spec - BDD(behavior-driven development) 스타일 코드 작성
    - given, when, then 키워드가 주어지고 then 안의 블럭에서 실제 테스트 진행
    
    ```kotlin
    class BehaviorSpecTests : BehaviorSpec({
    	given("a broomstick") {
    		`when`("I sit on it") {
    			then("I should be able to fly") {
    				// test code
    			}
    		}
    		`when`("I throw it away") {
    			then("it should come back") {
    				// test code	
    			}
    		}
    	}
    })
    ```
    
- Describe Spec - DCI 패턴을 사용하고 싶을 때 사용 (DCI 패턴 모름)
    
    ```kotlin
    import io.kotest.core.spec.style.DescribeSpec
    
    internal class UserServiceTest : DescribeSpec({
        val userRepository: UserRepository = mockk()
        
        describe("UserService") {
            var user: User = createUser()
            var request: ResetPasswordRequest = mockk()
    
            context("비밀번호를 비교할 때") {
                var request: EditPasswordRequest = mockk()
                slot<Long>().also { slot ->
                    every { userRepository.getById(capture(slot)) } answers { createUser(id = slot.captured) }
                }
                // ...
    
                it("확인용 비밀번호가 일치한다면 변경한다") {
                    // ...
                }
    
                it("확인용 비밀번호가 일치하지 않으면 예외가 발생한다") {
                    // ...
                }
            }
        }
    })
    ```
    
- Annotation Spec - Junit 스타일로 테스트 코드 진행하는 방법
    
    ```kotlin
    class AnnotationSpecExample : AnnotationSpec() {
    	@BeforeEach
    	fun beforeTest() {
    		println("Before each test")
    	}
    
    	@Test
    	fun test1() {
    		1 shouldBe 1
    	}
    
    	@Test
    	fun test2() {
    		3 shouldBe 3
    	}
    }
    ```
    
- Metcher - 테스트 작성을 도와주는 요소들을 명칭
    
    ```kotlin
    class MatcherTest : StringSpec() {
    	init {
    		"hello world" shouldBe haveLength(11)
    		"hello" should include("11")
    		"hello" should endWith("lo")
    		"hello" should match("he..")
    
    		val list = emptyList<String>()
    		val list2 = listOf("aaa", "bbb", "ccc")
    		val map = mapOf<String, String>(Pair("aa", "11"))
    
    		list should beEmpty1()
    		list2 shouldBe sorted<String>()
    		map should contain("aa", "11")
    		map should haveKey("aa")
    		map should haveValue("11")
    	}
    }
    ```
    
    - ShouldBe : 동일함을 체크하는 mecher
    - haveLength() : length 체크
    - include() : 파라미터 포함되어 있는지 확인
    - endWith() : 파라미터 끝 포함됐는지 확인
    - match() : 파라미터 매칭여부 확인
    - shouldBeLowerCase() : 소문자 작성 여부 확인
    - beEmpty() : 원소가 비어있는지 확인
    - sorted<String>() : 해당 자료형이 정렬되어있는지 확인
    - contain() : 원소가 포함되어 있는지 확인
    - haveKey() : Key값 확인
    - haveValue() : Value 값 확인
