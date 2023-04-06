
----
#### 리엑트 앱 내부 혹은 브라우저의 자바스크립트 코드를 통해서 데이터베이스와 직접통신을 하면 안된다.

<div style="font-size: 20px; color: red; font-weight: 600">왜 안될까?</div>

만약, 직접 연결을 하게 된다면 해당 코드를 통해 **데이터베이스**의 **인증 정보**를 **노출**을 하게 될 것이다. 

왜냐하면 브라우저에서 실행되는 **모든 자바스크립트 코드**는 웹사이트의 **유저**들도 **접근**이 **가능**하기 때문이다.

직접 통신 외에 다른 방법은 **백엔드 서버( 백엔드 API )** 을 사용하는 것

브라우저에서 실행되는게 아닌 다른 서버에서 실행된다.
데이터베이스와 통신하는 백엔드 앱은 사용자가 브라우저에서 백엔드 코드를 확인하기가 어렵기 때문에 인증 코드를 안정적으로 보관 가능.
####
----
#### API (Application Programming Interface)

HTTP요청에 대한 API는 REST API 혹은 GraphQL API를 말한다. 

##### REST API
- 다중 엔드포인트 ( Multiple Endpoint )
	 Endpoint: API가 서버에서 리소스에 접근할 수 있도록 가능하게 하는 URL
	 EX.) GET /users, POST /product, ...
- JSON 데이터
- 어느 서버-사이드 언어, FE-프레임워크에도 적용 가능
- 무상태 ( Stateless )
	 클라이언트-서버 관계에서 서버가 클라이언트 상태를 보존하지 않음, server는 단순히 요청이 오면 응답을 보내는 역할만 수행, 관리는 client가 한다.
	 인증도 보존이 가능하다.
- 구조
	 - HTTP Verb - Path - Body
		 `POST/user { name:'Max' }`
		 - HTTP Verb: `POST, GET, PUT, DELETE, PATCH`
			 - POST 
				 서버에 리소스 **멱등하지 않게** 생성
			 - GET
				 서버에서 리소스 가지고 오기 위해 사용하는 메서드
				 - 특징
					- HTTP Request Message의 Header부분에 담겨서 전송 ( HTTP 메세지에 Body가 없음)
					- 멱등
					- 캐시가 가능
					- 브라우저 히스토리에 저장됨
					- 북마크 가능
					- 길이 제한이 있음
					- 중요한 정보를 다루면 안됨
					- 데이터 요청시만 사용
			 - PUT
				  서버에 리소스 **멱등하게** 생성, **모두** 수정
				  - 특징
					  - HTTP Request Message의 Body부분에 담겨서 전송
					  - 멱등하지 않음 (?)
					  - 캐시가 불가능
					  - 브라우저 히스토리에 저장되지 않음
					  - 북마크되지 않음
					  - 길이 제한이 없음
			 - PATCH
				  서버에 리소스 **일부분** 수정
			 ※ 멱등: 여러번 적용해도 결과가 달라지지 않는 성질
			 - GET - POST 차이
			 - POST - PUT 차이
			 - PUT - PATCH 차이
		 Path: `user`
		 Body: `{ name:'Max' }` ( Optional: Containe Any Data)
	 - 서버-클라이언트 구조
		 클라이언트-사이드에서 **Request**를 보내면
		 서버-사이드에서 해당 **Endpoint**를 구문분석하고, **Response**를 보내는 방식

![[Pasted image 20230406112233.png]]


#####
##### GraphQL API
- 단일 엔드포인트 ( One Endpoint Only )
	 EX.) POST /graphQL
- JSON 데이터
- 어느 서버-사이드 언어, FE-프레임워크에도 적용 가능
- 무상태 ( Stateless )
	 클라이언트-서버 관계에서 서버가 클라이언트 상태를 보존하지 않음, server는 단순히 요청이 오면 응답을 보내는 역할만 수행, 관리는 client가 한다.
	 인증도 보존이 가능하다.
- 구조
	 -  HTTP Verb - Path - Body
		`POST /graphql { query:'query:...' }`
		HTTP Verb: `POST` ( only )
		Path: `graphql`
		Body: `{ query: 'query: ...' }` ( Contains GraphQL Query)
	 - 서버-클라이언트 구조
		클라이언트-사이드에서 POST request를 보내면,
		서버-사이드에서 쿼리 표현식을 분석한 후 Response
		- 구조 
			{
				query{ <- Operation type Ex.) mutation, subscription
					user{ <- Operation "endpoint" [식별자]
						name <- Request 하려는 부분
						age <- Request 하려는 부분
					}
				}
			}
- +요소: 검색하려는 데이터를 구체적으로 가져오거나 보낼 수 있다.
	 데이터 받아오는 양을 줄일 수 있다.

APOLLO 패키지를 사용할 시, graphQL을 보다 효율적으로 사용할 수 있다.
#####

####
----
#### HTTP 요청을 하는 방법 2가지

리엑트의 본질은 JS이기 때문에 우리가 원하는 어떤 HTTP 요청이든 전달이 가능

-   axois 패키지: 어떤 자바스크립트 라이브러리를 사용하는 것에 관계 없이ㅋ HTTP 요청
-   JS 내장 Fetch API: 브라우저 내장형 HTTP 요청 & 응답 처리 브라우저가 사용할 수 있게 해주는 함수 
	- `fetch('https://', { method: POST });` 

위 함수는 promise 객체를 반환
`promise`객체는 약속에 대한 앞으로의 발생되는 오류나, 호출에 대한 응답 반응을 알려준다. 
`promise`객체는 어떠한 즉각적 행동 대신, 데이터를 전달하는 객체이다. 
HTTP 요청 전송은 비동기 작업이기 때문에, 코드의 결과를 바로 사용할 수 없으니,
사용하는 방법이 또 2가지가 있는데, `.then()`메서드와 `async`, `await` 문법을 이용해서 사용하는 방법이 있다.

1. `then()` 메서드
	  `.then(response => { return response.json(); })` 메서드를 활용해서 받아온 응답을 JSON으로 처리하고 
	  그 뒤  `then(data => {})` 메서드를 사용해서 해당 JSON 데이터를 어떻게 활용할 것인지, 알려주며,
	  `.catch()` 메서드를 활용해서 오류를 잡아낸다.
```js
function fetchMoviesHandler() {
	const response = await fetch('https://swapi.dev/api/films', { 
		method: 'POST',
		body: JSON.stringify(movie), // <- 해당 데이터를 JSON 형식으로 바꿔주는 JSON.stringify
		headers: {
			'Content-Type': 'application/json' // 백엔드 앱은 헤더를 통해 어떤 데이터가 전달되었는지 알 수 있게 해준다.
		}
	})
		.then((response) => {
		return response.json();
		})
		.then((data) => {
			const transformedMovies = data.results.map((movieData) => {
				return {
					id: movieData.episode_id,
					title: movieData.title,
					openingText: movieData.opening_crawl,
					releaseDate: movieData.releasae_date,}
				};
			)};
		)
}
```

2.  `async`, `await` 문법
```js
async function fetchMoviesHandler() {
	const response = await fetch('https://swappi.dev/api/films', { 
		method: 'POST',
		body: JSON.stringify(movie), // <- 해당 데이터를 JSON 형식으로 바꿔주는 JSON.stringify
		headers: {
			'Content-Type': 'application/json' // 백엔드 앱은 헤더를 통해 어떤 데이터가 전달되었는지 알 수 있게 해준다.
		}
	});
	const data = await response.json();
	const transformedMovies = data.results.map((movieData) => {
		return {
			id: movieData.episode_id,
			title: movieData.title,
			openingText: movieData.opening_crawl,
			releaseDate: movieData.releasae_date,
		};
	});
}
```

####
----
#### HTTP 오류 처리

[HTTP 응답 상태 코드](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

`.then` 메서드를 활용해서 HTTP 요청을 처리한다면 `.catch` 메서드를 활용해서 오류 확인

`async`, `await` 문법을 사용하면 `try-catch`를 활용

axios 패키지 라이브러리는 에러 상태 코드를 실제 에러로 취급하지만,
FetchAPI는 에러 상태 코드를 실제 에러로 취급하지 않는다.
- 실제 에러 취급 방법
```js
try {
	if(!response.ok) {
		throw new Error('Something went wrong!');
	}
} catch(error) {
	setError(error.message);
}
```

####
----
#### 요청에 useEffect() 사용하기

사용자가 페이지를 방문하자마자 데이터를 가지고 오고 싶다면,
useEffect() 훅을 사용한다.

HTTP 요청 전송은 컴포넌트의 상태를 바꾸기 때문에 
<button>요청</button> 버튼을 클릭할 때마다 useState() 훅을 사용하면 컴포넌트의 재평가가 발생할 때마다 호출되기 때문에 무한 루프 문제가 발생할 가능성이 높으니 useEffect() 훅을 사용한다.
그래서 useEffect 훅은 컴포넌트가 렌더링(재렌더링 X)되는 주기 안에서 사용되어야 하는 코드가 있을 때 유용

```js
useEffect(() => {
	fetchMoviesHandler();
}, [fetchMoviesHandler]);
// useEffect 함수 내에서 사용하는 모든 의존성을 의존성 배열에 표시해야 함. 
// 위의 의존성 포인터는 함수를 가리키며 함수는 객체이기 때문에 컴포넌트가 
// 재 렌더링될 때마다 함수 또한 바뀐다. <- 무한 루프 발생 가능 
const fetchMoviesHandler = useCallback(() => {}, []);
// 위의 fetchMoviesHandler 함수가 재사용되는 것을 막기 위해 사용하는 useCallback // 함수
```

####
----
