---
title: 데이터 암호화 기법
categories:
  - web
tags:
  - Web
date: 2019-10-11T13:00:00+09:00
toc: false
use_math: true
---

### SHA-256

* 개인정보가 되는 정보(비밀번호, 토큰정보 등)를 암호화 할 때 주로 사용
* SHA(Secure Hash Algorithm)의 한 종류. 값을 입력받아 고정된 길이(256bit)의 해시 값을 출력
* 출력 속도도 빠르고 안정성 문제에도 큰 단점이 발견되지 않아 가장 많이 사용한다.

### BASE 64

* 닉네임이나 컨텐츠 내용과 같이 한글처리와 내용보호를 위한 인코딩 기법이다.
* 바이너리 데이터를 텍스트로 바꾸는 인코딩
* 바이너리 데이터를 Character Set에 영향을 받지 않는 공통 아스키 영역의 문자열로 바꾸는 인코딩
* 비록 처리해야 할 데이터 양이 늘어날지라도 안전한 데이터 전송을 위해 주로 사용한다.