표준 작명 프로토콜로 인테넷의 리소스에 이름을 부여할 수 있다.
이렇게 부여된 이름은 표준화된 구조로 이루어져 있다.
이런 이름을 URL이라고 부른다.

# 2.1 인터넷의 리소스 탐색하기
URL의 역할: 브라우저가 정보를 찾는데 필요한 리소스 위치 지시
URI: 
    URN: 리소스의 위치에 상관 없이 이름만으로 식별 가능. 이를 구현하려면 인프라가 필요
    URL: 리소스의 위치를 명시하여 식별
        예시
            URL:  http://www.joes-hardware.com/seasonal/index-fall.html
            해석
                scheme: http
                서버 위치: www.joes-hardware.com
                리소스 경로: /seasonal/index-fall.html

## 2.1.1 URL이 있기 전에는
일관적인 리소스 위치 기술 방법이 없었다.
애플리케이션마다 이런 기술 방법이 달랐다.
URL 등장 후에는
    리소스에 접근하는 일관되고 추상화된 방법 제공
    단일 브라우저로 리소스 종류에 따라 어떻게 보여줄지 결정

# 2.2 URL 문법
스킴에 따라 문법이 조금씩 달라질 수 있으나 일관된 규칙은 있다.
구성: `<scheme>://<username>:<password>@<hostname>:<port>/<path>;<param>?<query>#<fragment>`
컴포넌트의 의미
    scheme: 사용 프로토콜
    username: 리소스 접근에 필요한 계정
    password: 리소스 접근에 필요한 비밀번호
    hostname: 리소스를 호스팅하는 장비의 호스트명 혹은 IP 주소
    port: 호스트 중 리소스를 제공하는 서버에 접근하기 위한 번호
    path: 서버 내 리소스의 위치
    param: path만으로 리소스를 찾을 수 없는 경우 이를 메꿀 정보. 이름과 값 쌍으로 구성되며, 경로 조각마다 이를 지닐 수 있다.
        예시: http://www.joes-hardware.com/hammer;sale=false/index.html;graphics=true
        해석: hammer 조각은 값이 false인 sale 파라미터를, index.html 조각은 true값인 graphics란 파라미터를 지닌다.
    query: 동적 리소스를 컨텐츠로 제공 받을 때 어떤 리소스가 필요한지 범위를 제한하는데 쓰임
    framgent: 리소스 객체의 일부를 가리키며, 서버는 객체 단위로 다루기 때문에 이는 브라우저에서만 의미 있다.
        예시: http://www.joes-hardware.com/inventory-check.cgi?item=12731&color=blue
        해석: DB에게 데이터를 요청할 때 item은 12731번을, color는 blue라는 조건에 맞는 데이터를 가져와라.

# 2.3 단축 URL
역할: URL 전체(절대 URL)를 기술하는 불편함을 없애고자 

## 2.3.1 상대 URL
기저 URL과 합쳐져서 리소스의 위치를 나타낸다.
예: http://www.joes-hardware.com/tools.html 문서 내  `<a href=./hammers.html>Hammer</a>`
해석: 이 경우 base URL은 문서의 URL이며, http://www.joes-hardware.com/hammer.html 가 리소스의 절대 URL이다.
상대 URL -> 절대 URL 변환 과정
    1. 기저 URL 찾기
        - 리소스에 명시된 경우
        - 리소스의 URL을 기저 URL로 이용하는 경우
        - 기저 URL을 찾을 수 없는 경우
    2. URL 분해하기: 상대 참조 해석

## 2.3.2 URL 확장
브라우저에 URL 일부만 입력했는데 자동 완성되는 기능
종류
    호스트명 확장: 호스트명 일부를 입력하면 완전한 호스트명을 붙인다
    히스토리 확장: 입력된 URL 일부가 방문 이력과 앞부분이 일치하면 자동 완성

# 2.4 완전하지 않은 문자
'안전하다'의 의미: URL에 입력한 문자에 소실이 없어야 한다.
안전하지 않은 일이 발생하는 이유: 프로토콜이 다르고 각 프로토콜마다 사용하는 장치가 달라서
기존에는: 안전한 알파벳 문자만으로 가능한 범위를 제한했다
지금은: 그 외 문자도 이스케이프 인코딩으로 URL에 넣음을 허용했다

## 2.4.1 URL 문자 집합
전통적: US-ASCII 문자 집합
확장적: + 이스케이프 문자열

## 2.4.2 인코딩 체계
이스케이프 방법: %와 두 자리 16진수 숫자로 안전하지 않은 문자를 표현
예 
    http://www.joes-hardware.com/%7Ejoe 의 %7E는 '~'의 인코딩이다.
    http://www.joes-hardware.com/more%20tools.html 의 %20은 공백의 인코딩이다.

## 2.4.3 문자 제한
콜론이나 슬래시 같은 URL 컴포넌트 구분자는 URL 내에서 특별한 의미로 쓰인다.
이런 경우 같이 예약된 문자들을 본래 기능이 아닌 경로 컴포넌트 일부 같은 목적으로 쓰려면 이스케이프 인코딩해야한다.

## 2.4.4 좀 더 알아보기
URL 인코딩은 URL을 최초로 입력 받는 브라우저 같은 어플리케이션에서 함이 최선이다.
URL 디코딩은 URL을 최초로 해석하는 어플리케이션에서 함이 최선이다.
모든 문자를 인코딩하겠다는 것은 좋은 생각이 아니다.
        이유: 안전한 문자를 인코딩하지 않는 어플리케이션도 있다.

# 2.5. 스킴의 바다
(자주 쓰이는 스킴 목록과 각 스킴의 설명은 책을 읽어라.)

# 2.6. 미래
왜 URL을 잘 쓰고 있는데도 URN을 도입하려 하는가? 
    리소스가 이동되면 URL로는 이를 찾을 수가 없는 문제가 뜨기 때문(그 유명한 404 에러)
    URN을 구현할 인프라로 PURL(Persistent URL)을 사용할 수 있다.
