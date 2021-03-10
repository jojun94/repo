# HTTPS

1. (HTTP over TLS, HTTP over SSL, HTTP Secure)  HTTPS

 ㅇ 웹 서버 및 웹 브라우저 간의 보안용 프로토콜

  ㅇ HTTP 및 SSL/TLS(보안 채널)를 조합시킴
     - HTTP(응용계층)와 TCP(전송계층) 사이에서 일종의 보안 계층을 구현
        . 보안 계층 : HTTP 메세지를 TCP로 보내기 전에 먼저 그것을 암호화하는 계층 (SSL/TLS)

  ㅇ HTTPS 암호화 대상

     - 요청 문서의 URL, 문서 내용, 폼 요소 내용, 쿠키, HTTP 헤더 내용




2. HTTPS 특징

  ㅇ 주요 기능
     - 암호화(Encryption)
     - 인증(Authentication)
     - 변경감지 등

  ㅇ URI 스킴 접두사 : `https`

  ㅇ 사용 포트
     - 만일, URL이 `http`가 아닌 `https` URI 스킴 접두사를 갖으면,
     - 통상적인 HTTP 80번이 아닌, HTTPS 443번 포트 번호를 사용

  ㅇ 표준 : RFC 2818




3. HTTPS 보안 작동 방식

  ㅇ 만일, 클라이언트(웹브라우저)가 평범한 http가 아닌 https 스킴을 갖는 URL 주소를 만나면,
     - 웹서버에 80번이 아닌 443번 포트번호로 `TCP 연결`을 맺고,
     - 바이너리 포멧으로 된 몇몇 보안 매개변수를 교환(핸드세이크, `키 교환`)하고,
     - 그와 관련된 HTTPS 명령들이 그 뒤를 잇게 됨 

  ㅇ 보안 연결 확립을 위한 핸드세이크 (SSL/TLS)
     - 주요 교환 대상 
        . 프로토콜 버전 번호 교환
        . 상호 알고있는 암호 선택
        . 양단(클라이언트/서버) 간의 신원 인증 (주로, 서버 인증서)
           .. (대부분, 클라이언트 인증서는 보안이 보다 강화된 조직에서 만 사용)
        . 보안 채널 암호화를 위한 임시 세션 키 생성
     - 핸드세이크 절차
        . 웹브라우저(클라이언트)가 서버에게 여러 암호 후보들을 보내고 서버 인증서를 요구
        . 서버는 선택한 암호 및 서버 인증서를 보냄
        . 클라이언트가 비밀 정보를 보내고, 클라이언트와 서버가 암호 키를 생성
        . 상호 암호화 통신 시작


4. HTTPS 서버 인증서

  ㅇ X.509에 기반한 인증서로써,                      

​     - 인증서에 웹 사이트 정보가 추가됨

  ㅇ 서버 인증서에 포함된 주요 필드들
     - 인증서 일련번호 및 유효기간
     - 웹 사이트의 이름
     - 웹 사이트의 DNS 호스트명 (FQDN)
     - 웹 사이트의 공개키
     - 서명 기관(인증기관)의 이름
     - 서명 기관(인증기관)의 디지털서명 등

  ㅇ `웹 사이트 정보`에 대한 인증서 주요 검사 (웹브라우저가 수행함)
     - 날짜 검사          : 인증서 유효기간 검사 (만료,활성화 여부 등)
     - 서명자 유효성 검사 : 올바른 인증기관에서 파생된 인증서인지를 검사 (신뢰 사슬)
     - 서명 검사          : 인증서 무결성 검사
     - 사이트 신원 검사   : 인증서에 명시된 도메인명과 실제 도메인명이 부합되는가를 검사
        . 통상, 단일 도메인명이지만, 서버 팜(서버 클러스터)을 위해, 와일드카드(*) 표현 허용
     - 가상 호스팅 검사   : 하나의 서버에 여러 사이트/호스트를 운용하는 경우로써,
                            다소 비 보안적이고 부가적인 관리/절차 등이 필요
        . 원칙적으로, (IP주소):(포트번호 443)에 단 하나의 인증서 만을 매핑시켜 암호화하나,
        . 단일 서버에 여러 호스트가 운영되면, 각각을 식별 보안화하기에는 비용적으로 불리하여,
        . 단순 텍스트(Server Name Indication)로 서버명을 노출(평문)시킬 수 있도록 보안성 완화
           . RFC 4366 (Transport Layer Security (TLS) Extensions)

  ㅇ 웹브라우저는 어떤 사이트에 최초 연결시,
     - 웹 서버 인증서를 가져와서 검증함
        . 웹브라우저는 이미 신뢰할만한 인증기관 인증서를 갖고 출하되어,
        . 디지털 서명 검증이 즉시 가능 함 (보통, 웹브라우저 주소창 근처에 검증 결과를 보임)
          - 만일, 상대 웹서버가 신뢰할 수 없으면, 
        . 해당 서명기관의 신뢰 및 진행 여부를 사용자에게 물어보는 대화상자를 보여줌





> 참고
>
> https://ko.wikipedia.org/wiki/HTTPS
>
> https://webactually.com/2018/11/16/http%EC%97%90%EC%84%9C-https%EB%A1%9C-%EC%A0%84%ED%99%98%ED%95%98%EA%B8%B0-%EC%9C%84%ED%95%9C-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C/
>
> > Http -> Http migration
>
> http://www.ktword.co.kr/abbr_view.php?m_temp1=3132
>
> https://opentutorials.org/course/1334/4894