# 202030411 김태현

# 10월 25일 강의
서버는 두가지 상황에서 데이터를 불러온다.  
-> 빌드 시점에 정적 페이지 생성 / getStaticProps  
-> 앱 실행 도중 데이터 호출 / getServerSideProps  
*getStaticProps는 서버에서 가져오지만 변하지 않는다. 

Node.js는 fetch api를 제공하지 않는다. 다른 라이브러리 소개 ↓

1. HTTP 클라이언트 라이브러리  
클라이언트 및 서버에서 동일한 API를 쓸 수 있는 Axios가 유명하다.   

2. Node.js 내장 HTTP 라이브러리  
별도의 의존성 설치 없이 쓸 수 있지만 Axios 보다 설정할 작업이 많다.  

# 10월 23일 강의
누적 레이아웃 이동(Cumulative Layout Shift)  
-> 이미지를 불러오면 영역이 밀려나는 현상  
-> Next.js는 CLS현상을 내장 Image 컴포넌트를 사용해서 해결한다.  

Image 컴포넌트 기능 
-> 이미지 로드 시점을 필요할 때까지 지연시킴  
-> 이미지 사이즈 최적화로 사이즈를 1/10이하로 줄여줌  

Next.js는 기본으로 실행되고 있는 서버에서 자동 이미지 최적화 작업을 처리한다.  
-> 컴퓨팅 자원이 충분하지 않은 작은 서버는 이로 인해 성능에 영향을 미칠 수 있다.  
-> 외부 서비스를 통해 자동 이미지 최적화 가능 (next.config.js loader 속성 지정)  

디렉터리 구조 구성 (컴포넌트 구성)  
아토믹 디자인 원칙에 따르면 컴포넌트는 atoms, molecules, organisms, templates으로 나눈다.  

# 10월 4일 강의
_app.js와 _document.js 페이지 커스터 마이징 (프로젝트 레이아웃)  

_app.js: 각 페이지마다 별도의 컴포넌트를 불러오지 않고 모든 페이지에서 같은 내비게이션 바를 사용할 수 있다. (전역 스타일)  

_document.js: head html body 같은 기본적인 태그를 커스텀할땐 재정의하지 않고 상속받아서 커스텀한다.  
Main 태그: Main 태그 외부의 컴포넌트는 브라우저에서 초기화되지 않는다.  
페이지 간에 공통으로 사용되는 컴포넌트가 있다면 반드시 _app.js 파일에서 컴포넌트를 써야한다.  

NextScript 태그: 하이드레이션과 같은 작업을 처리할 수 있는 커스텀 스크립트가 위치한 곳이다. 

주의!  
_app.js와 _document.js는 파일을 커스터마이징 할 땐 getServerSideProps 또는 getStaticProps 함수를 쓸 수 없다.  
모든 페이지를 렌더링할 때마다 서버에서 특정 데이터를 불러와야 한다면 getInitalProps 함수를 써야한다.  


# 10월 2일 강의
라우팅 시스템: 경로 매개변수를 사용해서 동적 페이지를 만들 수 있다.

## 첫번째 방법 - 서버에서 처리하기
```javascript
export function getServerSideProps({params}){
  const {foo} = params
  return {props: {foo}}
}

// 1) 단일 동적 경로 -pages/bar/[foo].jsx
export default function FooPage(props) {
  console.log(props)
  return <h1>{props.foo}</h1>;
}

// 2) 다중 동적 경로 - pages/bar/[...foo].jsx
  export default function FooPage(props) {
    return <h1>{props.foo.join("&")}</h1>;
  }

// 3) 선택적 다중 동적 경로 (경로에 slug가 없어도 매핑된다)
// pages/bar/[[...foo]].jsx
export default function FooPage(props) {
  console.log(props)
  return <h1>{props.foo ? props.foo.join('&') : 'Home'}</h1>;
}

주의할 점: [...foo], [[...foo]] 중 하나만 만들어야 함
```
## 두번째 방법 - 클라이언트에서 처리하기(?)
```javascript
import { useRouter } from "next/router";

export default function Greet() {
  //localhost:3000/greet/mitch?learning_nextjs=true slug말고 url 파라미터도 가능
  const { query } = useRouter();
  console.log(query);
  return <h1>Hello {query.name}!</h1>;
}
```
### router.push 메서드
Link 컴포넌트와는 다르게 연결된 페이지를 미리 불러오지는 못하기 때문에
이벤트가 발생한 경우 클라이언트를 특정 페이지로 보낼 때 쓰는 건 좋지만
클라이언트 내비게이션에 쓰는 건 좋지 않다

### 자동 이미지 최적화 기능
CLS현상(이미지를 불러오면 영역이 밀려나는 현상)을 Next.js는 내장 Image 컴포넌트를 사용해서 해결한다.  
그리고 webP형식으로 이미지를 제공한다. WebP는 손실/비손실 압축 이미지 파일을 위한 이미지 포맷이다.  

# 9월 25일 강의
[동적 라우팅 방법]  
페이지 라우팅은 [slugName].js 파일, 앱 라우팅은 slug 디렉토리가 필요하다.  

```javascript
// app/foo/[fooId]/page.jsx
export default function fooId(props) {
    return (
      <h1>
        foo {props.params.fooId} / {props.searchParams.country}
      </h1>
    );
  }
```

# 9월 18일 강의
추석 휴강

# 9월 11일 강의
Next.js는 다양한 렌더링 전략을 제공한다. 예를 들어 CSR, SSR, ISR  
어떤 페이지를 빌드 시점에 정적으로 생성하거나 실행 시점에 동적으로 생성할 것인지 정할 수 있다.  
애플리케이션의 특정 페이지에 대한 요청이 있을 때 마다 페이지를 다시 생성할 수도 있다.  
또한 반드시 클라이언트에서 렌더링해야 할 컴포넌트도 지정할 수 있다.

Next.js는 CSR과 SSR의 장점을 모두 가져오기 위해서 hydration을 한다.  
Hydration은 서버에서 렌더링한 페이지에 스크립트 코드를 집어넣어서 연동하는 작업이다.

## SSR

[장점]  
페이지를 서버에 렌더링한다는 것은 쿠키 관리, API, 데이터 검증 같은 작업을 서버에서 처리한해서 안전한다.  
SSR을 사용하면 클라이언트가 요청할 때마다 페이지를 다시 렌더링할 수 있는 서버가 필요하다.  
반면 CSR이나 정적 사이트 방식은 Vercel이나 netlify에 배포할 수 있다.

```javascript
export async function getServerSideProps(){
  fetch()
  return{ props:{user: userdData}}
}

function IndexPage(props){
	return <div>{props.user.name}</div>
}
```
getServerSideProps 예약 함수는 서버에서 실행된다.  
페이지에서 외부 API를 호출하거나 데이터베이스에 접근하는 등 동적 작업을 헤야 한다면 해당 함수를 페이지에서 EXPORT 해야한다.  

항상 염두에 둘 점은 Next.js가 빌드 시점에 정적으로 페이지를 만든다는 점이다. 그리고 페이지를 기본적으로 서버에서 렌더링하기 때문에  
window, document 같은 브라우저 API를 제공하지 않는다. 이런 부분은 CSR이 필요하다.  
window, document 같은 브라우저 API를 못 쓰는 문제는 다양한 방법으로 할 수 있지만 대게 브라우저에 특정 컴포넌트를 렌더링하도록 지정하는 방식으로 처리한다.   

방법1  
useEffect 훅을 사용해서 컴포넌트가 마운트된 후 해당 기능을 실행하도록 만들 수 있다.  
그러면 Next.js는 컴포넌트가 반환하는 HTML마크업을 렌더링하고 Higjlight.js 스크립트를 페이지에 끼워 넣는다.  
그리고 해당 컴포넌트가 브라우저에 마운트되면 라이브러리 함수를 클라이언트에서 호출하고 실행하도록 만든다.   

방법2  
Process.browser 변수 or typeof window   

방법3  
Dynamic 함수   

## CSR

[장점]  
네이티브 애플리케이션처럼 느껴지는 웹 애플리케이션  
ㄴ전체 자바스크립트 번들을 다운로드한다는 것은 웹 애플리케이션이 렌더링할 모든 페이지가 이미 브라우저에 다운로드되어 있다는 뜻이다. 즉 콘텐츠를 바꾸기 위해 새로 고칠 필요가 없다.   

지연 로딩과 성능  
최소로 필요한 HTML 마크업만 렌더링한다. 예를 들어 모달은 마크업으로 존재하지않는다.     

서버 부하 감소  
ㄴ전체 렌더링 과정이 브라우저에서 일어나기 때문에 강력한 서버가 필요 없다. Aws lamda나 Firebase와 같은 서버리스 환경에서 웹 앱을 제공할 수도 있다.   

## ISR(증분 정적 재생성)
어느 정도의 주기로 정적 페이지를 다시 렌더링하고 해당 내용을 업데이트 할지 정할 수 있다.  
getStaticProps 예약 함수를 사용한다.


# 9월 4일 강의
Chocolatey:  윈도우에서 사용할 수 있는 커맨드 라인 패키지 매니저  

NVM (Node Version Manager): 개발 환경에 따라 Node.js의 버전을 변경해야 하는 상황에 대처하기 위해 필요한 모듈

vercel은 보일러 플레이트를 제공한다  
https://github.com/vercel/next.js/tree/canary/examples

# 8월 28일 강의
[Next.js 탄생 배경]

리액트는 클라이언트 사이드에서만 작동하기 때문에 SEO 효과를 거의 볼 수 없고, 첫 화면에 웹 애플리케이션을 표시하기 위해 실행 초기에 성능 부담이 생긴다.  
이 문제를 해결하기 위해, 웹 애플리케이션을 서버에서 미리 렌더링 해두는 방법이 연구되기 시작했다.   
서버 사이드 렌더링을 사용할 수 있다면 리액트 앱을 순수한 HTML 페이지로 미리 렌더링해 둔 다음 브라우저가 이를 다운로드하여  
즉각 화면에 표시하고 클라이언트에서 자바스크립트 번들을 다 받으면 사용자가 웹 앱과 상호작용할 수 있게 된다.   

Next.js는 타입스크립트로 작성된 프레임워크다. 그리고 바벨 컴파일러를 사용한다.  
바벨: 최신 자바스크립트 코드를 하위 호환성을 보장하는 코드로 바꿔 줌  
웹팩: 라이브러리, 페이지, 기능에 대해 컴파일 된 코드를 전부 포함하는 번들을 만들어 줌  
Next.js 14부터는 Webpack-> Tubopack를 사용한다   

[Pages Router & App Router]  

-Pages Router
<ul>
  <li>pages 디렉터리 내에 파일을 생성함으로써 라우팅 설정.</li>
</ul>

-App Router 
<ul>
  <li>Next.js 13에서 도입된 새로운 라우팅 접근 방식</li>
  <li>app 디렉터리 내에 파일을 생성하여 라우팅을 설정</li>
</ul>

현재 Next.js는  app 디렉터리와 pages 디렉터리를 병행할 수 있다.

Next.js는 리액트에 없는 다양한 기능을 제공한다.
<ul>
  <li>서버 사이드 랜더링</li>
  <li>정적 사이트 생성</li>
  <li>증분 정적 재생성</li>
</ul>


