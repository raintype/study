# OAuth

http://earlybird.kr/1584 내용을 바탕으로 정리한 내용으로
상세한 내용은 위 URL을 참고

## OAuth 1.0
트위터에 의해 2007년10월 OAuth1.0이 확정되었어나
세션 고정 공격(session fixation attcak)보안 결함으로 폐기

## OAuth 1.0a
2010년 4월에 OAuth1.0a에 해당하는 "The OAuth 1.0 protocol"(RFC5849) IETF 표준 등제
OAuth 1.0으로 등제 되었으나 과거 OAuth1.0과 구분을 위해 OAuth1.0a로 표현한다.

### OAuth1.0a의 장점
 - API를 인증함에 있어 써드 파티 어플리케이션에게 사용자의 비밀번호를 노출하지 않고 인증 할 수 있다.
 - 인증과 권한 부여를 동시에 할 수 있다.
 (해당 시점에 OPenID를 통한 써드파티 인증이 가능하나 권한 부여기능을 없었다.)

### OAuth1.0a의 기본 구성
 - 유저
 - 컨슈머(consumer)
 - 서비스 프로바이더(service provider)
 (3-legged OAuth라고도 불림)

### 인증토큰(access token)
컨슈머는 인증토큰으로 서비스 프로바이더를 사용

### 인증 절차
 1. 사용자가 consumer에게 로그인 요청을 한다.
 2. consumer는 사용자를 서비스 프로바이더의 로그인 화면으로 이동 시킨다.
 3. 사용자는 서비스 프로바이더의 로그인 화면에서 로그인 정보를 입력하여 로그인 한다.
 4. 서비스 프로바이더는 consumer에게 인증토큰을 전달 한다.
 5. consumer는 인증토큰으로 서비스 프로바이더를 사용한다.

(사용자는 consumer에 전달된 인증 토큰이 유출되면
서비스 프로바이더의 관리자 화면에서 그 인증토큰의 권한을 취소(revoke)할 수 있다.)

### 인증토큰의 특징
 - consumer가 아이디/패스워드를 가지고 않고 API를 사용할 수 있다.
 - 필요한 API만 제한적으로 접근할 수 있도록 권한 제어가 가능하다.
 - 사용자가 서비스 프로바이더의 관리 페이지에서 권한 취소가 가능하다.
 - 유저가 패스워드를 변경하여도 인증 토큰은 계속 유효하다.

## OAuth2.0
 OAuth2.0은 OAuth 1.0a와 호환되지 않는다.
 OAuth2.0은 2010년 4월 첫 번째 draft가 등록되었다.
   - OAuth 1.0a에서 불편하다고 느꼈던 모바일에서의 사용성 문제,
   - signature 생성과 같은 개발이 복잡하고 CPU를 많이 소비하는
   - 기능의 단순화
   -  기능과 규모의 확장성

 표준이 매우 커짐
 OAuth2.0의 정식 명칭은 "OAuth 인증 프레임워크(The OAuth 2.0 Authorization Framework)"
 OAuth 1.0a가 인증을 위한 만틍 칼이라면
 OAuth 2.0은 인증을 위한 도구 상자라 할 수 있다.

 ### OAuth 2.0의 개선 사항
  - 간단해 졌다.
    : OAuth 1.0a에서는 https가 필수가 아니었기에 API 호출 시에 signature를 생성해서 호출해야 했다.
    OAuth 2.0 Bearer 토큰 인증 방식을 쓰면 signature 불필요
  - 더 많은 인증 방법 지원
   : OAuth 1.0a는 HMAC 암호화 인증 방식만 지원, OAuth 2.0은 다양한 시나리오에 대응할 수 있게 해준다.
  - 대형 서비스로의 확장성 지원
   : 실제 API 서비스를 하는 서버와 인증 역할을 하는 authorization server의 역할을 명확히 구분함

### OAuth 2.0 기본 구성
 - Resource Owner : 사용자
 - Resource Server : API 서버
 - Authorization Server : 인증서버(API 서버와 같을 수도 있음)
 - Client : 써드파티 어플리케이션

 ### OAuth 2.0 Spec
 - oauth-v2와 oauth-v2-bearer라는 2개 표준이 가장 핵심

### Client 구분
 - Confidential Client : 웹서버가 API를 호출하는 경우 등과 같이 client 증명서(client_secret)를 안전하게 보관할 수 있는 client_secret
 - Public Client : 브라우저기반 어플리케이션이나 모바일 어플리케이션과 같이 client 증명서를 안전하게 보관할 수 없는 client.
 redirect_uri를 통해서 client를 인증한다.

 ### 인증방식(Grant types)
 3-legged와 2-legged 모두 지원
 client 종류와 시나리오에 따라 4가지 인증방식을 지원하나
 Authorization Code Grant와 Implicit Grant를 제외하고는 일반적인
 3-legged OAuth가 아니기 때문에 open API에서는 많이 사용되지 않는다.
 - Authorization Code Grant
  웹 서버에서 API를 호출하는 등의 시나리오에서 Confidential Client가 사용하는 방식
  서버 사이드 코드가 필요한 인증 방식이며 인증 과정에서 client_secret이 필요하다.
  로그인시에 페이지 URL에 response_type=code 라고 넘긴다.
  - Implicit Grant
   token과 scope에 대한 스팩 등은 다르지만 OAuth 1.0a과 가장 비슷한 방식
   Public Client인 브라우저 기반의 어플리케이션(Javascript application)이나 모바일 어플리케이션에서 사용 된다.
   client 증명서를 사용할 필요가 없으며 OAuth 2.0에서 가장 많이 사용되는 방식이다.
   로그인 시에 페이지 URL에 response_type=token 을 넘긴다.
  - Password Credentials Grant
   2-legged 방식의 인증
   client에 아이디/패스워드를 저장해 놓고 아이디/패스워드로 직접 access token을 받아오는 방식
   cleint를 신뢰할 수 없을 때는 위험
   API 서비스의 공식 어플리케이션이나 신뢰할 수 있는 client에 한해서만 사용하는 것을 추전
   로그인 시에 API에 POST로 grant_type=password 를 넘긴다.

  - Client Credentials Grant
   어플리케이션이 Confidential Client 일때 id와 secret을 가지고 인증하는 방식
   로그인 시에 API에 POST로 grant_type=client_credentials 라고 넘긴다.
  - Extension
   OAuth 2.0은 추가적인 인증방식을 적용할 수 잇는 길을 열어 놓았다.
   이러한 과도한 확장성을 메인 에디터인 Eran Hammer는 매우 싫어 했다고 한다.

Password Credentials Grant와 Client Credentials Grant는 기본적으로
OAuth의 프로세스를 따르지 않기 때문에 반드시 인증된 client에만 사용되여야 하며 가능하면 사용하지 않는 것이 좋다.

### 다양한 토큰(Access token)지원
 OAuth 2.0은 기본적으로 Bearer 토큰, 즉 암호화하지 않은 토큰을 주고 받는 것으로 인증한다.
 기본적으로 HTTPS를 사용하기 때문에 토큰을 안전하게 주고받는 것은 HTTPS의 암호화에 의존한다.
 OAuth 2.0은 MAC토큰과 SAML 형식의 토큰을 지원할 수 있지만 MAC 토큰 스팩은 업데이트 되지 않아 기한이 만료된 상태이고,
 SAML 토큰 형식은 아직도 활발하게 수정 중이기 때문에 사용할 수 없는 상태 이다.
 OAuth 2.0은 다양한 토큰 타입을 지원하지만 실질적으로는 Bearer 토큰만 지원한다.

 ### Refresh token
 client에서 같은 access token을 오래 사용하면 해킹에 노출될 위험이 높아 진다. 이로 인해 OAuth 2.0에서는 refresh token 개념을 도입했다.
 access token의 만료기간을 가능한 짧게 하고 만료 되면 refresh token을 새로 갱신하는 방법이다.
 개발자 사이에 논란이 많은 방법으로 토큰의 상태를 관리해야 해서 개발이 복잡해 질뿐 아니라 토큰이 만료되면 다시 로그인 하도록 하는 것이 보안 면에서 더 안전하다는 의견이 있다.

 ### API 권한 제어(Scope)
 OAuth 2.0은 써드파티 어플리케이션의 권한을 설정하기 위한 기능이다.
 scope의 이름이 스펙에 정의되어 있지는 않으며 여러 권한을 요청할 때에는 콘마 등을 사용해서 로그인 시에 scope을 넘겨준게 된다.
 http://example.com/oauth?...&scope=read_article,update_profile

 ### OAuth 2.0은 위험한가?
 - OAuth 1.0a에서는 token과 함께 token 비밀번호를 같이 받는데 2.0에서는 이 토큰 비밀번호가 없어져서 실제로 클라이언트를 구분하기 힘들어 졋다.
 - OAuth 1.0a의 signature 기능을 없애서 SSL에 의존함으로서 더 위험해졌다. 이렇게 된것은 기업 솔루션에 적용 되기 위한 것이었다.
 - OAuth 2.0에서 토큰 유효기간을 설정 할 수 있고 또한 만료 되면 갱신 되어야 한다. 이 기능은 자체적으로 인코딩 되어서 저장되는 토큰을 위한 것인데 이런 토큰은 권한을 취소(revoke)하는데에 문제가 있기 때문에 또 다시 유효기간을 짧게 가져가게 된다.
 - 권한 부여 방식
 OAth 2.0에서는 너무 많은 방직을 지원하고 있다.
 이는 여러 보안상의 문제점의 가능성을 의미 한다.

 Eran Hammer는 OAuth 2.0은 너무 많은 부분들을 허용하고 있고 복잡하기 때문에 안전하지 않다고 주장

 정리하면, 가장 많이 사용되는 Grant type인 Authorization Code 방식이나 Implicit 방식을 사용하고 access token으로 Bearer 토큰을 사용하며 HTTPS를 통해서 서비스하고 널리 사용되는 라이브러리들을 사용해서 구현한다면 OAuth 2.0은 안전하다고 할 수 있다.
