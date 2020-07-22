# JPA 공부

## 1. JPA란 무엇인가?

**JPA**(**Java** **Persistent** **API**)는 자바 플랫폼 SE((JAVA platform. **S**tandard **E**dition))와 

자바 플랫폼 EE(Java Platform, **E**nterprise **E**dition)를 사용하는 응용프로그램에서 관계형 데이터베이스를 관리를 표현하는 자바 API이다.

기존 데이터베이스를 관리하는 객체 Entity Bean(CRUD 쿼리 자동생성을 해준다.)을 JPA라고 바꾸고 JavaSE, JavaEE를 위한 영속성(persistence) 관리와 RDBMS와 OOP객체 사이의 불일치에서 오는 패러다임을 해결하기 위해 객체와 RDBMS를 매핑하는 역할을 하는 ORM을 위한 표준 기술이다. 



![ORM에 대한 그림설명](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F993FC233598FE9DD2B0CD3)

ORM 프레임워크를 사용하면 저장된 자바 객체를 분석하여 이와 관련된 적절한 SQL을 생성하고 데이터베이스에 저장시킨다.





**※JAVA SE**는 일반 사람들이 JAVA라는 언어를 생각할때 떠오르는 플랫폼.

**※JAVA EE**는 대규모,다계층,확장성,신뢰성,보안 네트워킹 어플리케이션의 개발과 실행을 위한 API 및 환경 제공.



## 2.장단점

### 장점

+  쿼리를 직접 생성하는 것이 아닌 만들어진 객체로 데이터베이스를 다루기에 데이터베이스 흐름으로 개발하는게 아닌 객체지향적 개발이 가능하다.
+  객체 중심으로 동작하기 때문에 기존 벤더사가 아닌 타 벤더사의 데이터베이스를 사용할 때 문법을 수정하지 않아도 된다.
+  엔티티 필드가 되는 객체를 다뤄 데이터베이스를 동작시키기 때문에 유지보수가 간결하고, 개발속도가 향상된다.
+  실시간 처리용 쿼리에 최적화 되어있다.

### 단점

+ JPA에서 Native query를 제공하고 있으나, 복잡한 쿼리에서는 Mybatis와 같은 Mapper 방식이 더 효율적이다

+ JPA에 대하여 숙지를 하고있지 않을시, 성능에 최적화되지 않은 결과를 나을 수 있다.

  

## 3.기본 예제 및 설명

### Repository Interface

```
@Repository
public interface ChartRepository extends JpaRepository<Chart,Integer> {
	
}
```

DB에 접근하는 객체인 Repository는 Srping Boot 에서 Repository는 DAO 역할을 한다. 

여기서 extends JpaRepository는 Entity에 대한 기본적인 메소드(CRUD 등)를 제공한다.

JpaRepository<엔티티 클래스 이름 , ID 필드 타입지정> 주의할점은  ID 필드 타입은 기본키 타입으로 해야한다는 것이다. 

//int가 아닌 Integer로 작성.



### Entity

```
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@ToString
@Entity
//lombok이 설치되어있다면
//@Getter 
//@Setter
@BatchSize(size = 10)
@Table(name = "Chart") //연결된 MarinaDB의 DB에 생성되어있는 테이블명으로 작성한다.
public class Chart {
	
	//기본키를 직접 할당하기 위하여 쓰인다.
	@Id 
	//추가 및 키 생성 IDENTITY전략한 이유는 데이터베이스에 이미 테이블을 생성하여 놓았기 때문에 
	//키 생성을 데이터베이스에 위임하기 위하여 사용하였다.
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name = "id") //대응하는 컬럼명 지정
	private Integer id;

	@Column(name = "valueOne", nullable = true, length = 100)
	private Integer valueOne;
	
	@Column(name = "valueTwo", nullable = true, length = 100)
	private Integer valueTwo;

	@Column(name = "valueThree", nullable = true, length = 100)
	private Integer valueThree;

	@Column(name = "valueFour", nullable = true, length = 100)
	private Integer valueFour;
	
	@Column(name = "valueFive", nullable = true, length = 100)
	private Integer valueFive;
	
	@Column(name = "valueSix", nullable = true, length = 100)
	private Integer valueSix;
	
	@Column(name = "valueSeven", nullable = true, length = 100)
	private Integer valueSeven;
	
	@Column(name = "valueEigth", nullable = true, length = 100)
	private Integer valueEigth;
	
	//이하 Getter,Setter는 생략한다.
}
```



Entity를 만들기 전에 DB에 테이블을 생성한다.

생성된 테이블과 동일하게 @Table(name = "Chart")을 설정한다.

@Column(name = "valueEigth", nullable = true, length = 100)
	private Integer valueOne;

또한 테이블에 컬럼과 동일하게 맞춰주면 된다.

Getter,Setter같은 경우는 lombok이 설치되어있다면 @Getter,@Setter 어노테이션만 이용하면

따로 Getter,Setter를 하지 않아도된다.

※lombok이 설치가 안되어있을시에, @Getter,@Setter을 어노테이션을 사용하게되면 오류가 발생한다.



### Service

```
@Service
public class ChartService {

	@Autowired // 스프링부트가 자동으로 객체를 주입
	ChartRepository chartRepo;

	public List<Chart> findAll() {
		// findAll() 메소드로 테이블의 레코드 리스트를 가져옴.
		List<Chart> list = chartRepo.findAll(); 
		return list;
	}
}
```

기본 findAll()메소드를 이용하여 데이터베이스 정보를 select한다.



### Controller

```
@Controller
public class ChartController {

	@Autowired
	ChartService service;
	
	@GetMapping("/") 
	public ModelAndView boardList() {
		//findAll()메소드로 DB에 있는 값들을 list에 담는다.
		List<Chart> list = service.findAll(); 
		ModelAndView nextView = new ModelAndView("/main");
		nextView.addObject("chartList", list);		
		return nextView;
	}
}
```

new ModelAndView("/main")와 nextView.addObject("ChartList", list);를 생성하여 

/main Page에  "ChartList"를 main Page로 보내준다.



## Jsp

```
<table border="1" width="20%" height="80">
    <tr>
        <td>category3</td>
        <td>
            ${chartList.valueOne}
        </td>
    </tr>
    <tr>
        <td>category3</td>
        <td>
            ${chartList.valueTwo}
        </td>
    </tr>
    <tr>
        <td>category3</td>
        <td>
            ${chartList.valueThree}
        </td>
    </tr>
    .
    .
    .
    .
</table>
```

//Test를 위한 Jsp파일

호출된 main Page에서 값이 잘 넘어오는지 확인하면 정상적으로 값이 잘 넘어올 것이다.



## 4.JpaRepository method

| **method** | **기능**                                                  |
| ---------- | --------------------------------------------------------- |
| save()     | *레코드 저장 (insert, update)*                            |
| findOne()  | *primary key로 레코드 한건 찾기*                          |
| findAll()  | *전체 레코드 불러오기. 정렬(sort), 페이징(pageable) 가능* |
| count()    | *레코드 갯수*                                             |
| delete()   | *레코드 삭제*                                             |

JpaRepository를 상속하면 사용할 수 있는 기본기능 메소드들 이다.



| **method**     | **설명**                                        |
| -------------- | ----------------------------------------------- |
| findBy로 시작  | 쿼리를 요청하는 메서드 임을 알림                |
| countBy로 시작 | 쿼리 결과 레코드 수를 요청하는 메서드 임을 알림 |

기본기능 외 기능을 추가하고 싶다면 위의 규칙에 맞게 Repository에 메소드를 추가하면 된다.

```
public interface ChartRepository extends JpaRepository<Chart,Integer> {
	List<Chart>	findByid(Integer categoryId);
}
```

```
List<Chart> chart = chartRepository.findById(1);
```

여기서 주의할점은 findBy다음에는 @Entity에 있는 Column명을 써야지만 맵핑이 되는 것 이다.



| **메서드 이름 키워드** | **샘플**                                             | **설명**                             |
| ---------------------- | ---------------------------------------------------- | ------------------------------------ |
| And                    | *findByEmailAndUserId(String email, String userId)*  | *여러필드를 and 로 검색*             |
| Or                     | *findByEmailOrUserId(String email, String userId)*   | *여러필드를 or 로 검색*              |
| Between                | *findByCreatedAtBetween(Date fromDate, Date toDate)* | *필드의 두 값 사이에 있는 항목 검색* |
| LessThan               | *findByAgeGraterThanEqual(int age)*                  | *작은 항목 검색*                     |
| GreaterThanEqual       | *findByAgeGraterThanEqual(int age)*                  | *크거나 같은 항목 검색*              |
| Like                   | *findByNameLike(String name)*                        | *like 검색*                          |
| IsNull                 | *findByJobIsNull()*                                  | *null 인 항목 검색*                  |
| In                     | *findByJob(String … jobs)*                           | *여러 값중에 하나인 항목 검색*       |
| OrderBy                | *findByEmailOrderByNameAsc(String email)*            | *검색 결과를 정렬하여 전달*          |

위에 테이블은 Query 메소드에 **포함할 수 있는 키워드**이다.

