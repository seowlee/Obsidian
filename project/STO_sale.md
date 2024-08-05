---
created: <% tp.file.creation_date() %>
---


# 📈 STO 판매 
%% STO 등록 / 매각, 판매, 거래 중 내가 맡은 파트%% 

프로젝트 환경 :
+ front - React
+ back - Spring Boot / Gradle / JPA
	+ Gradle Project : 필요한 라이브러리를 가져오고 빌드하는 lifecycle까지 관리해주는 툴

## 📂 DB 테이블 구성
 
#### # goods (상품 테이블)
 
 | column     | type      | 설명                                          |
 | ---------- | --------- | --------------------------------------------- |
 | goods_id | bigint    | 상품 번호 PK                                |
 | goods_nm   | varchar   | 상품 명                                       |
 | stat       | int       | 상품 상태 (등록됨, 판매중 : 0, 판매 완료 : 1) |
 | total_amt  | int       | 전체 가격                                     |
 | sale_amt   | int       | 매각 가격                                     |
 | unit_amt   | int       | 단위 가격                                     |
 | total_cnt  | int       | 전체 개수                                     |
 | order_fee  | double    | 구매 수수료                                   |
 | trade_fee  | double    | 거래 수수료                                   |
 | sale_fee   | double    | 매각 수수료                                   |
 | created_dt | timestamp | 생성일시                                      |
 | updated_dt | timestamp | 수정일시                                      |
 | created_by | varchar   | 생성자                                        |
 | updated_by | varchar   | 수정자                                        |


#### # sale (판매 정보 테이블)
| column        | type   | 설명                 |
| ------------- | ------ | -------------------- |
| sale_goods_id | bigint | 상품 판매 정보 PK FK |
| sale_cnt      | int    | 판매 개수            |
| sale_rate              |  double      |        판매율              |

#### # holding (상품 보유 정보 테이블)
| column      | type   | 설명         |
| ----------- | ------ | ------------ |
| holdings_id | bigint | 보유 번호 PK |
| user_id     | bigint | 유저 번호 FK |
| goods_id    | bigint | 상품 번호 FK |
| goods_cnt   | int    |         보유 상품 개수     | 

#### # transaction (판매 거래 내역 테이블)
| column           | type      | 설명                                   |
| ---------------- | --------- | -------------------------------------- |
| transaction_id   | bigint    | 거래 번호 PK                           |
| goods_id         | bigint    | 상품 번호 FK                           |
| user_id          | bigint    | 유저 번호 FK                           |
| transaction_cnt  | int       | 거래 상품 개수                         |
| transaction_stat | int       | 거래 상태 (0: 정상 판매, 1: 판매 취소) |
| transaction_dt   | timestamp | 거래 시각                              | 

#### # user (유저 테이블)
| column       | type    | 설명           |
| ------------ | ------- | -------------- |
| user_id      | bigint  | 유저 번호 FK   |
| user_account | int     | 유저 보유 금액 |
| user_name    | varchar | 유저 이름      |
| user_stat    | int     | 유저 로그인 상태 (로그아웃 : 0,  로그인 : 1)             |


### Entity
#### Product(Goods) - Sale 
: one-to-one 양방향 연관관계 
Sale 에서 Goods의 goods_id를 PK이자 FK로 가지고 있다.
연관관계의 주인 : Sale
goods 에서 sale 정보도 가져다 쓰기 위해 양방향 매핑.

#### Holding - User / Holding - Goods
Holding 엔티티는 User의 user_id를 FK로 가짐
Holding 엔티티는 Goods의 goods_id를 FK로 가짐
Holding 과 User,  Holding 과 Goods는 모두 Many-to-One 단방향 연관관계

#### Transaction - User / Transaction - Goods
Transaction 엔티티는 User의 user_id를 FK로 가짐
Transaction 엔티티는 Goods의 goods_id를 FK로 가짐
Transaction 과 User,  Transaction 과 Goods는 모두 Many-to-One 단방향 연관관계

## 🏆 화면 구성 및 동작 과정
%% 화면 구성, 프로그램 실행 동작 과정 설명 %%  

#### 화면 구성:
+  홈화면
+ 판매상품목록
+ 상품 세부 정보 및 구매
+ 판매완료상품목록
+ 상품별 보유 유저 목록 
+ 유저별 상품 보유 목록
+ 거래 내역


#### 동작 과정 :

1. 홈화면 에서 판매 정보 초기화와 구매를 진행할 회원 선택
![[Pasted image 20221015220355.png|800]]
	
2. 판매상품목록 페이지로 넘어가 상품  구매 버튼 클릭
![[Pasted image 20221015220456.png|800]]
	
3.  상품페이지에서  조각 개수 선택해 구매
![[Pasted image 20221015220554.png|600]]
	
4. 상품보유목록, 유저 보유목록, 거래내역 페이지에서 추가된 판매 정보 확인
5. 상품보유목록에서 판매된 상품의 판매  취소 후 다시 판매 정보 업데이트 확인
![[Pasted image 20221015221716.png|400]] ![[Pasted image 20221015222844.png|400]] ![[Pasted image 20221015223227.png|400]]
<br />
#### 환경 설정

스프링이 뜰 때 @Configuration 읽고 Service 로직 호출해 스프링 빈에 등록.
Service 는 생성자에서 Repository를 넣어줘야 함.

com/sto/sale/backstosale/config/SpringConfig.java
```java
@Configuration  
public class SpringConfig {  
  
    private final ProductRepository productRepository;  
    private final SaleRepository saleRepository;  
    private final UserRepository userRepository;  
    private final HoldingRepository holdingRepository;  
    private final TransactionRepository transactionRepository;  
    private final EntityManager em;  
  
    @Autowired  
    public SpringConfig(ProductRepository productRepository, SaleRepository saleRepository, UserRepository userRepository, HoldingRepository holdingRepository, TransactionRepository transactionRepository, EntityManager em) {  
        this.productRepository = productRepository;  
        this.saleRepository = saleRepository;  
        this.userRepository = userRepository;  
        this.holdingRepository = holdingRepository;  
        this.transactionRepository = transactionRepository;  
        this.em = em;  
    }  
  
    @Bean  
    public ProductService productService() {  
        return new ProductService(productRepository, saleRepository);  
    }  
  
    @Bean  
    public SaleService saleService() {  
        return new SaleService(saleRepository);  
    }  
  
    @Bean  
    public UserService userService() {  
        return new UserService(userRepository, holdingRepository);  
    }  
  
    @Bean  
    public HoldingService holdingService() {  
        return new HoldingService(holdingRepository);  
    }  
  
    @Bean  
    public TransactionService transactionService() {  
        return new TransactionService(transactionRepository);  
    }  
  
    @Bean  
    public JPAQueryFactory jpaQueryFactory(EntityManager em) {  
        return new JPAQueryFactory(em);  
    }  
  
    @Bean  
    public ModelMapper modelMapper() {  
        ModelMapper modelMapper = new ModelMapper();  
        return modelMapper;  
    }    
  
}
```

생성자를 통해서 Service가 Controller에 주입.
스프링 컨테이너가 올라가고 세팅되는 시점에 주입되고 끝남. 그다음엔 변경 못하도록 막아버림.

ProductController.java
```java
@RestController  
public class ProductController {  
  
   private ProductService productService;  
  
   @Autowired  
   public ProductController(ProductService productService) {  
      this.productService = productService;  
   }
   ...
}
```

+ 컨트롤러 : 웹 MVC의 컨트롤러 역할  
+ 서비스 : 핵심 비즈니스 로직 구현  
+ 리포지토리 : 데이터베이스에 접근, 도메인 객체를 DB에 저장하고 관리  
+ 도메인 : 비즈니스 도메인 객체, 예) 회원, 주문, 쿠폰 등등 주로 데이터베이스에 저장하고 관리됨

컨트롤러 동작 방식 :
url을 넘기면 스프링 부트에서 내장 톰켓 서버를 거쳐 스프링에 알림
스프링은 Controller의 method에 mapping을 확인하여 method를 호출

<br />

<hr />

## 📋 홈화면		

###  📗 판매 정보 테이블 초기화

%% Drop : 팀 멤버들을 방해하고 있어 버려야 할 일들이 무엇인가요? 예를 들면 관리자의 마이크로 매니징%%
- 상품이 sale table에 저장되어 있지 않다면 추가하기 
#### 💻 구현 코드

##### front
src/pages/HomePage.js
``` js
const handlePostSaleInfo = () => {
  axios
	.post(`/sales/all`, {})
	.then((res) => {})
	.catch((error) => {

	console.log("error", error);
  });
};
```

##### back

Controller에서 mapping 된 url을 찾음.
상품 테이블에 등록된 상품리스트를 반복문으로 돌며 builder를 통해 sale 객체를 생성.
service에 있는 register 함수를 호출. register 함수의 인자로 sale 객체를 전달.
service의 register함수에서는 
goods_id로 이미 판매정보 테이블(sale)에 상품이 등록되었는지 조건을 통해 상품이 판매정보테이블에 등록되지 않은 경우에만 
판매 개수, 판매율이 0으로 초기화된 상품을 판매 정보 테이블에 추가

com/sto/sale/backstosale/controller/SaleController.java
```java
@PostMapping("/sales/all")  
public List<Sale> initialize() {  
   List<Product> p = productService.findAllProducts();  
  
   for (Product product : p) {  
      Sale sale = Sale.builder()  
            .sale_cnt(0)  
            .sale_rate(0.0)  
            .product(product)  
            .build();  
      saleService.register(sale);  
   }  
   List<Sale> sales = saleService.findSales();  
   return sales;  
}
```
com/sto/sale/backstosale/service/SaleService.java
```java
public Long register(Sale sale) {  
   int isRegistered = validateDuplicateProduct(sale);  
   if (isRegistered == 0) {  
      saleRepository.save(sale);  
   }  
   System.out.println(sale.toString());  
   return sale.getProduct().getGoods_id();  
  
}  
  
private int validateDuplicateProduct(Sale sale) {  
   Optional<Sale> newSale = saleRepository.findById(sale.getProduct().getGoods_id());  
   if (newSale.isPresent()) {  
      return 1;  
   } else return 0;
```

###  📗 구매 회원 선택

+ user table에 저장되어 있는 모든 user_id 목록 불러와 구매 회원 선택 리스트 에서 보여주기
+ 선택한 회원의 user_stat을 user table에서 1로 변경

#### 💻 구현 코드

##### 1) user id 목록 가져오기
###### front 

src/pages/HomePage.js
```js
const [userIds, setUserIds] = useState([]);

useEffect(() => {
  axios
	.get(`/user/ids`)
	.then((res) => {
	  setUserIds(res.data);
	})
	.catch((error) => {
		console.log("error", error);
	});
}, []);
```

###### back 


com/sto/sale/backstosale/controller/UserController.java
```java
@GetMapping("user/ids")  
public List<Long> getAllUserIds() {  
    List<Long> userIds = userService.findAllUserIds();  
    return userIds;  
}
```

com/sto/sale/backstosale/service/UserService.java
```java
/**  
 * 모든 userId 목록 조회  
 */  
public List<Long> findAllUserIds() {  
   return userRepository.findAllByUserIds();  
}
```

com/sto/sale/backstosale/repository/UserRepository.java
```java
@Query("select u.user_id from User u")  
List<Long> findAllByUserIds();
```


##### 2) 선택한 유저를 로그인한 상태로 user_stat 업데이트
###### front 

src/pages/HomePage.js
```js
const handleCloseCheck = () => {
  axios
	.post("/user/loggedIn", { userId: user })
	.then((res) => {
		console.log("logging id", res);
	})
	.catch((error) => {
		console.log("error", error);
	});
  setOpen(false);
  navigate("/listOnSale");
};
```

###### back 

com/sto/sale/backstosale/controller/UserController.java
```java
@PostMapping("user/loggedIn")  
public UserLoggedInDto postLoggedInUser(@RequestBody UserLoggedInDto userLoggedInDto) {  
    userService.updateUserStat(userLoggedInDto);  
    return userLoggedInDto;  
}
```

UserService에 정의된 updateUserStat 함수는 
먼저 모든 유저 목록을 조회하여 유저 리스트를 돌며 userDto에 정의한 reset_stat함수를 통해 모든 user_stat을 0으로 초기화 함.
그 다음 구매 회원 선택 view에서 전달 받은 user id와 일치하는 user를 찾아 UserDto 객체에 담고 user_stat을 로그인 중을 나타내는 user_stat 1로 변경.
UserDto를 생성자를 통해 entity로 변환하여 userRepository에 저장함.

com/sto/sale/backstosale/service/UserService.java
```java
/**  
 * 선택한 유저를 로그인된 유저로 업데이트 (user_stat = 1)  
 */
 public UserDto updateUserStat(UserLoggedInDto userLoggedInDto) {  
   List<UserDto> allUsers = userRepository.findByAllUser();  
   
   for (UserDto userDto : allUsers) {  
      userDto.reset_stat();  
      User user = new User(userDto);  
      userRepository.save(user);  
   }  
  
   UserDto userDto = userRepository.findByUserId(userLoggedInDto.getUserId());  
   userDto.update_stat(userLoggedInDto.getUserId());  

   User user = new User(userDto);  
   userRepository.save(user);  
   return userDto;  
}
```

com/sto/sale/backstosale/repository/UserRepository.java
```java
@Query("select new com.sto.sale.backstosale.dto.UserDto(u.user_id, u.user_account, u.user_nm, u.user_stat)"  
      + "from User u")  
List<UserDto> findByAllUser();  
  
@Query("select new com.sto.sale.backstosale.dto.UserDto(u.user_id, u.user_account, u.user_nm, u.user_stat)"  
      + "from User u where u.user_id = :id")  
UserDto findByUserId(@Param("id") Long id);
```

com/sto/sale/backstosale/dto/UserDto.java
```java
public class UserDto {  
   private Long user_id;  
   private Integer user_account;  
   private String user_nm;  
   private Integer user_stat;  
  
   public UserDto(Long user_id) {  
      this.user_id = user_id;  
   }  
  
   public void update_stat(Long user_id) {  
      this.user_id = user_id;  
      this.user_stat = 1;  
   }  
  
   public void reset_stat() {  
      this.user_stat = 0;  
   }
   ...
}
```

<hr />

## 📋 판매상품목록
%% Add : 팀 멤버들이 추가하고 싶어 하는 것들은 무엇인가요? 예를 들면 코드를 merge할 때 빌드 상황을 모니터링할 수 있는 Screen 설치%%
###  📗 판매 상품 리스트 
+ 등록 된 상품들 가운데 판매 중인 상품들이 리스트로 보여지는 페이지
#### 💻 구현 코드

##### front
+ 페이지 이동 Hook
	useNavigate, useParams, useSearchParams
	+ useNavigate :
	  다른 페이지로 이동, 뒤로 가기 시 사용하는 hook.
	+ useParams, useSearchParams : 쿼리 스트링을 받아 오는 Hook
	+ useParams :
	  전달된 Path variable을 useParams custom hook을 통해 가져와 페이지 이동
	+ useSearchParams : 
	  쿼리 스트링(name과 value를 엮어서 데이터를 전송)을 통해 url을 변경시킨다.


useState를 통해 페이지 관련 값 관리
```js
// 한 페이지에 보여지는 상품 리스트
const [products, setProducts] = useState([]);
// 전체 상품 수
const [count, setCount] = useState(5);
// 현재 페이지 번호. default value 1
const [currentPage, setCurrentPage] = useState(1);
// 한 페이지 당 목록 개수. 5개로 정함.
const [postPerPage] = useState(5);
// 페이지 번호 버튼 클릭 시 현재 페이지를 받아 url에 현재 페이지 표시
const [searchParams, setSearchParams] = useSearchParams();
```
판매 상품 목록 클릭 시 useEffect ,
페이지 번호 클릭 시 handlePageChange 함수에서 get 방식 요청을 통해 상품 목록 가져옴.
get 요청 시 현재 페이지와 페이지 당 사이즈 데이터를 포함해 요청.
```js
useEffect(() => {
  axios
	.all([
	  axios.get(`/product/on-sale`, {
		params: {
		  page: currentPage - 1,
		  size: postPerPage,
		},
	  }),
	  axios.get(`/product/on-sale/count`),
	])
	.then(
	  axios.spread((res1, res2) => {
		setProducts(res1.data);
		setCount(res2.data);
		console.log("res1", res1, "res2", res2);
	  })
	)
	.catch((error) => {
	  console.log("error", error);
	});
}, [currentPage, postPerPage, setSearchParams]);

const handlePageChange = (currentPage) => {
  setCurrentPage(currentPage);
  console.log(currentPage);
  axios
	.get(`/product/on-sale`, {
	params: {
	  page: currentPage - 1,
	  size: postPerPage,
	},
  })
  .then((res) => {
	setProducts(res.data);
	setSearchParams({ page: currentPage });
	console.log("current page : ", searchParams.get("page"));
  })
  .catch((error) => {
	console.log("error", error);
  });
};
```

##### back

Spring Data JPA에서 제공하는 페이징과 정렬
=> Pageable 인터페이스 사용.

JpaRepository 의 부모 인터페이스인 PagingAndSortingRepository 에서 페이징과 소팅이라는 기능을 제공.
JpaRepository 사용 시 pageable 인터페이스를 파라미터로 넘겨 페이징 사용.
![[Pasted image 20221016233107.png]]
참고 : https://wonit.tistory.com/483

**PageRequest.of(int page, int size, sort)** 를 통해서 **PageRequest** 를 생성.
PageRequest 객체는 Pageable 인터페이스를 상속받는다.
![[Pasted image 20221016235201.png]]
JPA Repository로 pageable을 인자로 넘겨줌. 
JPA에서 처리하여 query를 생성.

+ Controller : 파라미터 데이터 page, size 를 포함해 요청된 get mapping url을 찾음.
+ Service : **PageRequest.of(int page, int size, sort)** 를 통해서 **PageRequest** 를 생성. JPA Repository로 pageable을 인자로 넘겨줌. 
+ Repository : Pageable 변수를 파라미터로 가지는 쿼리 메소드를 작성

getTotalElements() // 전체 데이터 수

com/sto/sale/backstosale/controller/ProductController.java
```java
@GetMapping("/product/on-sale")  
public List<ProductDto> getJoinProduct(@RequestParam(defaultValue = "0") Integer page, @RequestParam(defaultValue = "5") Integer size) {  
   List<ProductDto> joinProduct = productService.findOnSaleProducts(page, size);  
   return joinProduct;  
}  
  
@GetMapping("/product/on-sale/count")  
public Long getOnSaleProductsCnt() {  
   Long totalOnSaleProductCnt = productService.findOnSaleProductsCnt();  
   return totalOnSaleProductCnt;  
}
```

com/sto/sale/backstosale/service/ProductService.java
```java
/**  
 * 판매 상품 목록 조회 (Product join Sale)  
 */public List<ProductDto> findOnSaleProducts(Integer page, Integer size) {  
   Pageable pageable = PageRequest.of(page, size);  
   return productRepository.findByOnSaleProducts(pageable);  
}  
  
/**  
 * 판매 상품 목록의 판매 상품 총 개수  
 */  
  
public Long findOnSaleProductsCnt() {  
   Pageable pageable = PageRequest.of(0, 5);  
   Page<Product> productPage = productRepository.findByOnSaleProductsCnt(pageable);  
   Long totalCnt = productPage.getTotalElements();  
   return totalCnt;  
}
```

com/sto/sale/backstosale/repository/ProductRepository.java
```java
@Query("select new com.sto.sale.backstosale.dto.ProductDto(p.goods_id, p.goods_nm, p.stat, p.total_amt, p.unit_amt, p.total_cnt, p.order_fee, p.trade_fee, p.created_dt, p.created_by, s.sale_cnt, s.sale_rate)"  
      + "from Product p join p.sale s where p.stat = 0")  
List<ProductDto> findByOnSaleProducts(Pageable pageable);

@Query("select p from Product p where p.stat = 0")  
Page<Product> findByOnSaleProductsCnt(Pageable pageable);
```



##  📋 상품 세부 정보 및 구매
%% Keep : 팀 멤버들이 지속했으면 하는 것들은 무엇인가요? 예를 들면 스탠드업 시작을 알리는 노래를 멤버들이 돌아가서면 선정하는 것%%
- 선택한 상품 세부 정보를 확인하고 조각 수를 입력하여 구매를 진행하는 페이지
###  📗 상품 세부 정보
#### 💻 구현 코드

##### front

useEffect 함수를 통해 컴포넌트 렌더링 시 실행됨.
Path variable 로 전달된 goods_id 에 해당하는 상품 데이터를 요청.
이전 홈화면에서 로그인 할 구매 유저를 선택함. 로그인 된 유저 데이터를 요청해 user에 객체를 저장.

src/pages/OrderPage.js
```js
const [detailProduct, setDetailProduct] = useState({});
const [user, setUser] = useState({});

useEffect(() => {
  axios
	.all([
	  axios.get(`/product/order/${goods_id}`),
	  axios.get(`/user/loggedIn`),
	])
	.then(
	  axios.spread((res1, res2) => {
		setDetailProduct(res1.data);
		setUser(res2.data);
	})
  )
  .catch((error) => {
	console.log("error", error);
  });
}, [goods_id]);
```

##### back
null 가능성이 있으면 Optional 로 감싸서 반환.

com/sto/sale/backstosale/controller/ProductController.java
```java
@GetMapping("/product/order/{id}")  
public Optional<ProductDto> getOne(@PathVariable Long id) {  
   Optional<ProductDto> optionalProduct = Optional.ofNullable(productService.findProductId(id));  
   return optionalProduct;  
}
```

com/sto/sale/backstosale/service/ProductService.java
```java
    /**  
    * 판매 상품 한 건 조회 (구매 상세페이지)  
    */   
    public ProductDto findProductId(Long id) {  
      return productRepository.findByProductId(id);  

   }
```

com/sto/sale/backstosale/repository/ProductRepository.java
```java
@Query("select new com.sto.sale.backstosale.dto.ProductDto(p.goods_id, p.goods_nm, p.stat, p.total_amt, p.unit_amt, p.total_cnt, p.order_fee, p.trade_fee, p.created_dt, p.created_by, s.sale_cnt, s.sale_rate)"  
      + "from Product p join p.sale s where p.goods_id = :id")  
ProductDto findByProductId(@Param("id") Long id);
```

###  📗 구매하기
#### 💻 구현 코드
##### front

구매 수량 입력 후 구매 버튼 클릭 시 
1) 구매 가능 수량 과 입력한 구매 수량 비교 
2) 유저 계좌 금액 과 입력한 구매 수량의 금액 비교
두가지 조건에서 구매 가능 시에만 구매 dialog 창을 띄움.
두 조건을 충족 하지 않을 경우 error dialog 창을 띄움.

src/pages/OrderPage.js
```js
const handleClickOptionPurchase = (event) => {
  if (purchaseQuantity > availableQuantity) {
	handleClickFailPurchaseQuantity();
  } else if (user.user_account < totalPurchasePrice) {
	handleClickFailPurchaseLackBalance();
  } else {
	handleClickPurchase();
  }
};
```

정상 구매 dialog 에서 확인 버튼 클릭 시 
1) holding (유저의 상품 보유 정보)
2) sale (상품 판매 정보)
3) transaction (거래 내역)
4) user (유저 정보)
네 테이블의 데이터를 업데이트 해줘야 한다.  post 방식의 요청을 보낸다.
5) goods (상품 정보)
위의 네 테이블에 업데이트 된 내용을 바탕으로 goods (상품 테이블) stat 을 (판매 중인지 판매 완료인지) 업데이트 해줘야 한다.
이를 위해 앞의 네가지 요청 과 goods 테이블 관련 요청에 대하여 async await 를 걸어 비동기 코드를 동기처럼 사용한다.
`await` 키워드를 사용하면 일반 비동기 처리처럼 바로 실행이 다음 라인으로 넘어가는 것이 아니라 결과값을 얻을 수 있을 때까지 기다려준다. 결과값을 다음 변수에 할당 할 수 있다.

참고 :
+ 동기(Synchronous) : 서버에서 요청을 보냈을 때 응답이 돌아와야 다음 동작을 수행할 수 있다.
작업을 동기적으로 처리한다면 요청이 끝날 때까지 기다리는 동안 중지 상태
+ 비동기(Asynchronous) : 요청을 보냈을 때 응답 상태와 상관없이 다음 동작을 수행 할 수 있다.
참고 : https://velog.io/@daybreak/%EB%8F%99%EA%B8%B0-%EB%B9%84%EB%8F%99%EA%B8%B0-%EC%B2%98%EB%A6%AC
```js
const navigate = useNavigate();
const handleCloseCheck = async () => {
  const transactionDate = new Date();
  try {
	const [res1, res2, res3, res4] = await Promise.all([
	  axios.post(`/holding/add`, {
		userId: user.user_id,
		goodsId: goods_id,
		goods_cnt: purchaseQuantity,
	  }),
	  axios.post(`/sale/update`, {
		sale_goods_id: goods_id,
		sale_cnt: purchaseQuantity,
		sale_rate: detailProduct.sale_rate,
		total_cnt: detailProduct.total_cnt,
	  }),
	  axios.post(`/transaction/add`, {
		userId: user.user_id,
		goodsId: goods_id,
		transactionCnt: purchaseQuantity,
		transactionStat: 0,
		transactionDt: transactionDate,
	  }),
	  axios.post(`/user/update`, {
		user_id: user.user_id,
		price: totalPurchasePrice,
	  }),
	]);
	console.log("res1", res1, "res2", res2, "res3", res3, "res4", res4);
	const res5 = await axios.post(`/product/stat/update`, {
	  goodsId: goods_id,
	});
	console.log("res5", res5);
  } catch (error) {
	console.log("error", error);
  }
  setOpenSuccess(false);
  navigate("/listOnSale");
};
```
<br />

##### back

###### 1) holding

com/sto/sale/backstosale/controller/HoldingController.java
```java
@PostMapping("/holding/add")  
public HoldingDto createHoldingData(@RequestBody HoldingDto holdingDto) {  
   System.out.println("통신 성공");  
   System.out.println("user_id : " + holdingDto.getUserId());  
   System.out.println("goods_id : " + holdingDto.getGoodsId());  
   System.out.println("goods_cnt : " + holdingDto.getGoods_cnt());  
   holdingService.addHoldingData(holdingDto);  
   return holdingDto;  
}
```

user 가 goods를 goods_cnt 만큼 구매 했을 때,
user_id와 goods_id 가 모두 일치하는 행이 존재하는지 repository에 정의한 findByHoldingData함수를 통해 찾는다. 
user_id와 goods_id 가 모두 일치하는 행이 존재하면 기존의 데이터에 상품 개수(goods_cnt) 만 업데이트 해주고,
user_id와 goods_id 가 모두 일치하는 행이 존재 하지 않는 경우 추가된 데이터를 그대로 새로운 holding 인스턴스를 생성하여 insert 해준다

시도했던 것 ==> 
생성자를 통하지 않고 ModelMapper를 통해 dto를 Entity로 변환
변수들이 매칭 되지 않음
```java
ModelMapper modelMapper = new ModelMapper(); EmployeeEntity employeeEntity = modelMapper.map(employeeDto, EmployeeEntity.class);
```

참고 :
DTO : 프로세스 간의 데이터를 전달하는 객체
Entity : Persistent 영역과 통신을 위해 사용되는 객체
com/sto/sale/backstosale/service/HoldingService.java
```java
    /**  
    * 보유 데이터 추가, 업데이트  
    */  
   public HoldingDto addHoldingData(HoldingDto addedHoldingDto) {  
      HoldingDto holdingDto = holdingRepository  
            .findByHoldingData(addedHoldingDto.getUserId(), addedHoldingDto.getGoodsId());  
      System.out.println(addedHoldingDto.toString());  
  
      if (holdingDto != null) {  
         holdingDto.update_holding(addedHoldingDto.getGoods_cnt());  
         System.out.println("holdingdto " + holdingDto.toString());  
      } else {  
         holdingDto = addedHoldingDto;  
         System.out.println("insertdto " + holdingDto.toString());  
  
      Holding holding = new Holding(holdingDto);  
      System.out.println("---- mapper -----");  
      System.out.println(holding.toString());  
  
      holdingRepository.save(holding);  
  
      return addedHoldingDto;  
   }
```

com/sto/sale/backstosale/repository/HoldingRepository.java
```java
@Query("select new com.sto.sale.backstosale.dto.HoldingDto(h.holding_id, u.user_id, p.goods_id, h.goods_cnt)"  
      + "from Holding h join h.user u join h.product p where u.user_id = :user_id and p.goods_id = :goods_id")  
HoldingDto findByHoldingData(@Param("user_id") Long user_id, @Param("goods_id") Long goods_id);
```

com/sto/sale/backstosale/dto/HoldingDto.java
```java
public void update_holding(Integer goods_cnt) {  
    this.goods_cnt += goods_cnt;  
}
```

###### 2) sale

판매 정보 테이블은 이전에 초기화 해 두었기 때문에 구매한 상품 id에 해당하는 dto를 찾아 
판매 개수와 판매율을 업데이트 하여 엔티티로 변환해 save 한다.

com/sto/sale/backstosale/service/SaleService.java
```java
    /**  
    * 상품 판매 정보 업데이트  
    */  
   public SaleDto addSaleData(SaleDto addedSaleDto) {  
      SaleDto saleIdDto = saleRepository.findBySaleProductId(addedSaleDto.getSale_goods_id());  
      saleIdDto.update_sale(addedSaleDto.getSale_cnt(), addedSaleDto.getTotal_cnt());  
      Sale sale = new Sale(saleIdDto);  
      saleRepository.save(sale);  
  
      return saleIdDto;  
   }
```

판매 개수, 판매율 업데이트
com/sto/sale/backstosale/dto/SaleDto.java
```java
    public void update_sale(Integer sale_cnt, Integer total_cnt) {  

      this.sale_cnt += sale_cnt;  
      this.sale_rate = 100.0 * this.sale_cnt / total_cnt;  
   }
```

###### 3) transaction

post 요청 시 전달받은 데이터를 포함하는 엔티티를 생성하여 추가한다.
com/sto/sale/backstosale/service/TransactionService.java
```java
/**  
 * 거래 내역 추가 (insert)  
 */
 public TransactionDto addTransactionData(TransactionDto transactionDto) {  
   Transaction transaction = new Transaction(transactionDto);  
   transactionRepository.save(transaction);  
   return transactionDto;  
}
```

###### 4) user

구매 금액 만큼 user_account에서 빼주기.
com/sto/sale/backstosale/service/UserService.java
```java
/**  
 * user 계좌 업데이트  
 */  
public UserDto updateUserAccount(UserAccountDto addedUserDto) {   
   UserDto userDto = userRepository.findByUserId(addedUserDto.getUser_id());  
   userDto.update_user(addedUserDto.getUser_id(), addedUserDto.getPrice());  
   User user = new User(userDto);  
   userRepository.save(user);  
   return userDto;  
}
```

com/sto/sale/backstosale/dto/UserDto.java
```java
public void update_user(Long user_id, Integer buyPrice) {  
   this.user_id = user_id;  
   this.user_account -= buyPrice;  
}
```

###### 5) product

해당 상품의 판매 개수와 전체 개수 비교를 통해 상품 stat을 업데이트 한다. 
판매 개수가 전체 개수와 일치하게 되면 판매 완료 상태 (stat = 1)

com/sto/sale/backstosale/service/ProductService.java
```java
/**  
 * 상품 판매 완료 시 상품 상태(stat) 업데이트  
 */  
public ProductStatDto changeGoodsStat(CompletionSaleDto completionSaleDto) {  
   ProductStatDto productStatDto = productRepository.findByProductIdWithStat(completionSaleDto.getGoodsId());  
 
   SaleDto saleDto = saleRepository.findBySaleProductId(completionSaleDto.getGoodsId());  

   productStatDto.update_stat(saleDto.getSale_cnt(), saleDto.getTotal_cnt());  
    
   Product product = new Product(productStatDto, saleDto);  
   productRepository.save(product);  
   return productStatDto;  
}
```

Integer 는 Wrapper class 이므로 Integer 끼리 비교 시 == 연산자를 사용하면 객체의 주소 값을 비교한다.
따라서 두 Integer 값을 == 연산자로 비교시 항상 false 를 뱉는다.
값끼리 비교하려면 equals 메소드를 사용한다.

com/sto/sale/backstosale/dto/ProductStatDto.java
```java
    public void update_stat(Integer sale_cnt, Integer total_cnt) {  
        this.total_cnt = total_cnt;  
        this.sale_cnt = sale_cnt;  

        boolean res = this.sale_cnt.equals(this.total_cnt);  
        
        if (this.sale_cnt.equals(this.total_cnt)) {  
            this.stat = 1;  
        }  
    }
```



## 📋 판매완료 상품목록
%% Improve : 팀 멤버들이 개선했으면 하는 것들은 무엇인가요? 예를 들면 기술부채를 줄이기 위해 업무의 20%를 리팩토링에 투자하는 것%%
- 판매 완료 된 상품들을 리스트로 나타낸다.
- goods_ stat 이 1인 것만 가져옴.
#### 💻 구현 코드
##### back

com/sto/sale/backstosale/controller/ProductController.java
```java
@GetMapping("/product/sold-out")  
public List<ProductDto> getSoldOutProduct(@RequestParam(defaultValue = "0") Integer page, @RequestParam(defaultValue = "5") Integer size) {  
   List<ProductDto> soldOutProduct = productService.findSoldOutProducts(page, size);  
   return soldOutProduct;  
}
```

com/sto/sale/backstosale/service/ProductService.java
```java
/**  
 * 판매 완료 상품 목록 조회 (Product join Sale)  
 */
 public List<ProductDto> findSoldOutProducts(Integer page, Integer size) {  
   Pageable pageable = PageRequest.of(page, size);  
   return productRepository.findBySoldOutProducts(pageable);  
}
```

com/sto/sale/backstosale/repository/ProductRepository.java
```java
@Query("select new com.sto.sale.backstosale.dto.ProductDto(p.goods_id, p.goods_nm, p.stat, p.total_amt, p.unit_amt, p.total_cnt, p.order_fee, p.trade_fee, p.created_dt, p.created_by, s.sale_cnt, s.sale_rate)"  
      + "from Product p join p.sale s where p.stat = 1")  
List<ProductDto> findBySoldOutProducts(Pageable pageable);
```
<br />

## 📋 상품보유목록
+ 각 상품 별 보유자 리스트를 합쳐서 보여줌.
###  📗 각 상품별 보유자들 
#### 💻 구현 코드
##### front

get 요청을 통해
+ 상품 아이디 : {goods.goodsId}, 
+ 상품명 : {goods.goodsNm}
+ 판매상품 개수 : {goods.sumGoodsCnt} 
+ 보유자들 : {goods.userIds}
정보를 가지는 object의 리스트를 보여준다.

src/pages/GoodsHoldingPage.js
```js
useEffect(() => {
  axios
	.get("/holding/goods")
	.then((res) => {
	  setHoldings(res.data);
	})
	.catch((error) => {
	  console.log("error", error);
	});
}, []);
```

##### back

com/sto/sale/backstosale/controller/HoldingController.java
```java
@GetMapping("/holding/goods")  
public List<GoodsHoldingDto> getListHolding() {  
   List<GoodsHoldingDto> listHolding = holdingService.findListHolding();  
   return listHolding;  
}
```


###### querydsl

참고한 사이트 :
아트앤 가이드의 미술품 공동 소유권 현황.
![[Pasted image 20221017102324.png|500]]

하나의 상품에 대해 보유한 사람들을 연달아 합쳐져 있는 문자열로 받고자 함
=> mysql 의 group_concat 사용하기
![[Pasted image 20221017102753.png|500]]
group_concat 함수는 jpa 에서 제공하지 않음. 기본적인 집계함수 만 제공.
Spring Boot에 **QueryDSL**을 사용하여 
+ 상품 별(group_by goods_id) 
+ 총 판매개수 (집계함수 sum())
+ 보유자들(group_concat) 을 리스트 형태로 반환.

개선 할 점 => 
+ 우선은 service에서 querydsl을 사용.
	querydsl 사용을 위해 구현체를 따로 만들어 사용하는 예시를 봤는데 학습 필요
+ querydsl을 사용하는 것보다 엔티티 간의 연관관계를 활용해서 가져오는 것이 더 좋은 방법이라고 한다.
	여기서는 one-to-many를 활용해 goods 엔티티에 대해 holdings 엔티티의 특정 컬럼을 가져오는 방법을 시도했었는데 실패.
	연관관계를 이용하는 방법을 공부 할 것.

com/sto/sale/backstosale/service/HoldingService.java
```java
    /**  
    * 상품 별 판매 개수, 보유자 리스트  
    */  
   public List<GoodsHoldingDto> findListHolding() {  
      QHolding qHolding = QHolding.holding;  
      List<GoodsHoldingDto> list = jpaQueryFactory  
            .select(Projections.fields(  
                  GoodsHoldingDto.class,  
                  product.goods_id.as("goodsId"),  
                  product.goods_nm.as("goodsNm"),  
                  qHolding.goods_cnt.sum().as("sumGoodsCnt"),  
                  Expressions.stringTemplate("group_concat({0})", qHolding.user.user_id).as("userIds")  
            ))  
            .from(qHolding)  
            .join(qHolding.product, product)  
            .join(qHolding.user, user)  
            .groupBy(qHolding.product.goods_id)  
            .fetch();  
  
      return list;    
   }
```


###  📗 판매 취소
#### 💻 구현 코드
##### front

정상 구매 시 업데이트 했던
1) sale (상품 판매 정보)
2) transaction (거래 내역)
3) user (유저 정보)

4) holding (유저의 상품 보유 정보)
5) goods (상품 정보)

다섯 개의 테이블에 취소 정보를 다시 업데이트 해주어야 한다.
위에서 구현했던 구매 시 요청들과 마찬가지로
기존의 holding 테이블에 저장된 정보들을 바탕으로 sale, transaction, user 테이블을 업데이트 해주고
그 다음으로 holding 테이블에서 판매 취소된 상품 행들을 삭제하고, goods 테이블의 판매 상태를 업데이트 하기 위해
async await 를 걸어 비동기 코드를 동기처럼 사용한다.
`await` 키워드를 사용하여 1)2)3) 에 대한 요청에 대해 결과를 얻은 후 4)5)에 대해 요청을 보낸다.

src/pages/GoodsHoldingPage.js
```js
const handleClickCancelSale = async (goodsId) => {
  setGoodsId(goodsId);
  alert(`cancel ${goodsId}`);
  try {
	const [res1, res2, res3] = await Promise.all([
	  axios.post(`/sale/delete`, {
		goodsId: goodsId,
	  }),
	  axios.post(`/transaction/cancel`, {
		goodsId: goodsId,
	  }),
	  axios.post(`/user/delete`, {
		goodsId: goodsId,
	  }),
	]);
	console.log("res1", res1, "res2", res2, "res3", res3);
	const [res4, res5] = await Promise.all([
	  axios.delete(`/holding/delete`, {
		data: { goodsId: goodsId },
	  }),
	  axios.post(`/product/stat/reset`, {
		goodsId: goodsId,
	  }),
	]);
	console.log("res4", res4, "res5", res5);
  } catch (error) {
	console.log("error", error);
  }
  refreshPage();
};
```

##### back
Controller에서 mapping된 url을 찾아 Service의 함수를 호출
Service에서 Repository에 정의한 메소드로 판매 취소한 goods_id와 일치하는 데이터를 가져와 정보를 업데이트 or 삭제 한다.

###### 1) sale
+ 판매 취소 선택한 상품 에 대해 상품 판매 정보 (판매 개수, 판매율)를 0으로 초기화

com/sto/sale/backstosale/service/SaleService.java
```java
/**  
 * 선택된 상품 판매 취소. 판매 정보 삭제 (0으로 초기화)  
 */
 public SaleDto resetGoodsSale(CancellationSaleDto cancellationSaleDto) {  
   SaleDto saleDto = saleRepository.findBySaleProductId(cancellationSaleDto.getGoodsId());  
   saleDto.delete_sale();  
   Sale sale = new Sale(saleDto);  
   saleRepository.save(sale);  
   return saleDto;  
}
```

com/sto/sale/backstosale/dto/SaleDto.java
```java
    public void delete_sale() {   
      this.sale_cnt = 0;  
      this.sale_rate = 0.0;  
   }
```

###### 2) transaction
+ 거래 취소 시 기존 거래 내역에서 transaction stat을 1(판매 취소)로 변경하고 거래 시간을 취소된 현재 시간으로 업데이트 한다.
+ 기존의 거래내역은 취소 표시만 업데이트 한채로 남겨두고 새로운 취소 내역을 추가하고자 함
	=> 업데이트가 되지 않고 insert만 두번 일어남.
	=> 해결책 : 기존의 거래내역에서 상태를 판매 취소로 업데이트
	
com/sto/sale/backstosale/service/TransactionService.java
```java
    /**  
    * 선택된 상품 판매 취소. 거래 취소 내역 추가  
    */  
   public List<TransactionDto> resetGoodsTransaction(CancellationSaleDto cancellationSaleDto) {  
      System.out.println("delete");  
      System.out.println("delete: " + cancellationSaleDto.getGoodsId());  
      List<TransactionDto> transactionDtos = transactionRepository.findBySelectedTransactions(cancellationSaleDto.getGoodsId());  

      for (TransactionDto dto : transactionDtos) {  
         dto.cancle_previousTransaction(cancellationSaleDto);  
         Transaction canclePrevious = new Transaction(dto);  
         transactionRepository.save(canclePrevious);  
      }  
  
      return transactionDtos;  
   }
```

com/sto/sale/backstosale/dto/TransactionDto.java
```java
public void cancle_previousTransaction(CancellationSaleDto cancellationSaleDto) {  
   if (this.transactionStat == 0) {  
      this.transactionStat = 1;  
      this.transactionDt = new Date();  
   }  
}
```

###### 3) user
HoldingRepository의 findByHoldingsAmount 메소드를 통해
holding엔티티와 user, goods 엔티티를 조인한 dto에서 판매 취소 상품 id와 일치하는 행을 찾아 List에 저장.
리스트를 돌며 판매 취소 상품을 가진 유저에게 HoldingAmountDto에서 가져온 상품 판매 개수와 단위 가격을 곱한 환불가격을 
refund_user 함수를 통해 user_account에 더해줌. 

com/sto/sale/backstosale/service/UserService.java
```java
/**  
 * 선택된 상품 판매 취소. 유저 테이블에서 환불 금액 업데이트  
 */  
public List<HoldingAmountDto> restoreUserAccount(CancellationSaleDto cancellationSaleDto) {  
   System.out.println("refund" + cancellationSaleDto);  
   List<HoldingAmountDto> holdingAmountDtos = holdingRepository.findByHoldingsAmount(cancellationSaleDto.getGoodsId());  
   for (HoldingAmountDto holding : holdingAmountDtos) {  
      UserDto userDto = userRepository.findByUserId(holding.getUserId());  
      Integer amount = holding.getGoods_cnt() * holding.getUnit_amt();  
      userDto.refund_user(holding.getUserId(), amount);  
      User user = new User(userDto);  
      userRepository.save(user);  
   }  
   return holdingAmountDtos;  
}
```

com/sto/sale/backstosale/repository/HoldingRepository.java
```java
@Query("select new com.sto.sale.backstosale.dto.HoldingAmountDto(h.holding_id, u.user_id, p.goods_id, p.unit_amt, h.goods_cnt)"  
      + "from Holding h join h.user u join h.product p where p.goods_id = :goods_id")  
List<HoldingAmountDto> findByHoldingsAmount(@Param("goods_id") Long goods_id);
```
com/sto/sale/backstosale/dto/UserDto.java
```java
public void refund_user(Long user_id, Integer refundAmount) {  
   this.user_id = user_id;  
   this.user_account += refundAmount;  
}
```

###### 4) holding

Repository.delete를 통해 판매 취소된 goods_id를 가진 행들을 holding table에서 모두 삭제.

com/sto/sale/backstosale/service/HoldingService.java
```java
/**  
 * 선택된 상품 판매 취소. 보유 테이블에서 선택 상품 포함된 행 삭제  
 */  
public List<HoldingDto> findGoodsHoldings(CancellationSaleDto cancledDto) {  
   List<HoldingDto> deletedHoldingDtos = holdingRepository.findByGoodsHoldings(cancledDto.getGoodsId());  
   for (HoldingDto deletingHolding : deletedHoldingDtos) {  
      Holding holding = new Holding(deletingHolding);  
      holdingRepository.delete(holding);  
   }  
   return deletedHoldingDtos;  
}
```

###### 5) goods
취소 상품의 goods_id를 통해 취소 상품을 찾고,
productStatDto.reset_stat() 을 통해 판매 취소한 상품의 상태를 0으로 초기화.

com/sto/sale/backstosale/service/ProductService.java
```java
/**  
 * 선택한 상품 판매 취소 시 상품 상태(stat) 초기화  
 */  
public ProductStatDto resetGoodsStat(CancellationSaleDto cancellationSaleDto) {  
   ProductStatDto productStatDto = productRepository.findByProductIdWithStat(cancellationSaleDto.getGoodsId());  
   SaleDto saleDto = saleRepository.findBySaleProductId(cancellationSaleDto.getGoodsId());  
  
   productStatDto.reset_stat();  
 
   Product product = new Product(productStatDto, saleDto);  
   productRepository.save(product);  
   return productStatDto;  
}
```


## 📋 유저보유목록
###  📗 각 유저가 가진 상품 목록과 개수

%% Drop : 팀 멤버들을 방해하고 있어 버려야 할 일들이 무엇인가요? 예를 들면 관리자의 마이크로 매니징%%
- 관리자의 마이크로 매니징
###  📗 구매 회원 선택
## 📋 거래내역
- 상품의 판매 내역이 보여지는 페이지
- 정렬은 최신순 Sort.by("transaction_dt").descending()

###  📗 거래내역 가져오기
#### 💻 구현 코드
##### front

거래 내역 탭 클릭 시 useEffect ,
페이지 번호 클릭 시 handlePageChange 함수에서 get 방식 요청을 통해 상품 목록 가져옴.
get 요청 시 현재 페이지와 페이지 당 사이즈 데이터를 포함해 요청.

src/pages/TransactionHistoryPage.js
```js
useEffect(() => {
  axios
	.all([
	  axios.get(`/transaction/all`, {
		params: {
		page: currentPage - 1,
		size: postPerPage,
	  },
	}),
	axios.get(`/transaction/all/count`),
  ])
  .then(
	axios.spread((res1, res2) => {
	  setTransactions(res1.data);
	  setCount(res2.data);
	  console.log("res1", res1, "res2", res2);
	})
  )
  .catch((error) => {
	console.log("error", error);
  });
}, [currentPage, postPerPage]);

const handlePageChange = (currentPage) => {
  setCurrentPage(currentPage);
  console.log(currentPage);
  axios
	.get(`/transaction/all`, {
	params: {
	  page: currentPage - 1,
	  size: postPerPage,
	},
  })
  .then((res) => {
	setTransactions(res.data);
	setSearchParams({ page: currentPage });
	console.log("current page : ", searchParams.get("page"));
  })
  .catch((error) => {
    console.log("error", error);
  });
};
```

##### back

Pageable 인터페이스 사용하여 페이징, 정렬
+ Controller : 파라미터 데이터 page, size 를 포함해 요청된 get mapping url을 찾음.
+ Service : **PageRequest.of(int page, int size, sort)** 를 통해서 **PageRequest** 를 생성. JPA Repository로 pageable을 인자로 넘겨줌. 
+ Repository : Pageable 변수를 파라미터로 가지는 쿼리 메소드를 작성

com/sto/sale/backstosale/controller/TransactionController.java
```java
@GetMapping("/transaction/all")  
public List<TransactionDto> getAllTransactions(@RequestParam(defaultValue = "0") Integer page, @RequestParam(defaultValue = "5") Integer size) {  
   List<TransactionDto> transactions = transactionService.findAllTransactions(page, size);  
   return transactions;  
}  
  
@GetMapping("/transaction/all/count")  
public Long getAllTransactionCnt() {  
   Long totalTransactionCnt = transactionService.findAllTransactionCnt();  
   return totalTransactionCnt;  
}
```

com/sto/sale/backstosale/service/TransactionService.java
```java
/**  
 * 모든 거래 내역 조회  
 */  
public List<TransactionDto> findAllTransactions(Integer page, Integer size) {  
   Pageable pageable = PageRequest.of(page, size, Sort.by("transaction_dt").descending());  
   return transactionRepository.findByAllTransactions(pageable);  
}  
  
/**  
 * 모든 거래 내역의 총 개수  
 */  
public Long findAllTransactionCnt() {  
   Pageable pageable = PageRequest.of(0, 5);  
   Page<Transaction> transactionPage = transactionRepository.findByAllTransactionCnt(pageable);  
   Long totalCnt = transactionPage.getTotalElements();  
   return totalCnt;  
}
```

com/sto/sale/backstosale/repository/TransactionRepository.java
```java
@Query("select new com.sto.sale.backstosale.dto.TransactionDto(t.transaction_id, p.goods_id, u.user_id, t.transaction_cnt, t.transaction_stat, t.transaction_dt)"  
      + "from Transaction t join t.product p join t.user u")  
List<TransactionDto> findByAllTransactions(Pageable pageable);  
  
@Query("select t from Transaction t")  
Page<Transaction> findByAllTransactionCnt(Pageable pageable);
```


## 😱개선사항

### 1) JPA N + 1 문제 해결하기
N + 1 이란 : 연관관계들의 매핑에 의해서 관계가 맺어진 다른 객체가 함께 조회되는 경우. 1개의 쿼리에 N개의 쿼리가 추가 발생.
참고 : https://velog.io/@jinyoungchoi95/JPA-%EB%AA%A8%EB%93%A0-N1-%EB%B0%9C%EC%83%9D-%EC%BC%80%EC%9D%B4%EC%8A%A4%EA%B3%BC-%ED%95%B4%EA%B2%B0%EC%B1%85

### 2) ModelMapper 사용 시 안된 이유 
==> naming 때문 인것 같다?
naming convention 에 대해 학습. named query에도 활용되는 것 같다.

## 🤔 느낀점
%%회고를 통해서 앞으로 더 좋은 방향으로 나아가기 위해서 결정해야할 사항들은 어떤 것들이 있었나요? %%
- 소통
- 하고싶었던 핵심 기능 구현은 다 한것 같다.
## 🤔 앞으로 공부 하고 싶은 것
+ jpa 와 객체 지향 프로그래밍에 대한 공부
+ 이더리움 토큰

[[회고]]