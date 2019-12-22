---
title: MPEG – Digital Video Coding Standard
categories:
  - multimedia
tags:
  - Multimedia
date: 2019-10-16T13:00:00+09:00
toc: false
---

## MPEG

### MPEG(Moving Picture Experts Group)

* 비디오 코딩 표준
* 1, 2, 4, 7 21까지 총 5가지 종류가 있음. 7은 비디오 오디오 검색, 21은 저작권에 대한것. 압축은 1,2 4 3가지
* MPEG 비디오 코딩 기술은 근본적으로 통계적인 정보를 이용한다.
* Video Sequnces는 보통 통계적인 시간적인, 공간적인 간격의 중복이 들어가있다. 
* 비디오는 결과적으로 연속적인 이미지의 집합이다.
* JPEG은 인접한 픽셀들의 유사한 데이터를 이용한 것이고 MPEG은 시간축으로 인접한 프레임들간의 유사성을 이용해서 압축한 것이다.
* 특정 이미지 픽셀의 값을 같은 프레임의 인접한 픽셀로부터 유추해 낼 수 있고 앞과 뒤에 있는 인접한 프레임으로부터 유추해 낼 수 있다.

### MPEG-1

* ISO와 IEC가 같이 만듬
* 모션 비디오 인코딩을 위한 표준
* 3개의 파트로 구성되어 있다.
  * MPEG Video
    * 압축률을 1/200까지
  * MPEG Audio
    * 오디오가 MP3파일
  * MPEG System
    * 오디오가 비디오가 섞여있으므로 이를 어떻게 한 줄로 섞을 것인지, 동기화 할 것인지

### Intraframe Coding

* 한 장의 이미지를 코딩하는것. JPEG



### 6 Layers of MPEG-1 Video Sequence

![](https://i.imgur.com/Icbkhei.png)

* JPEG도 계층 구조로 되어있듯이 MPEG도 계층 구조로 되어있다.
* 하나의 비디오 시퀀스
* 여러개의 GOP로 구성되어 있다. Group of Picture. 보통 이미지 10장정도로 구성
* GOP는 Picture들로 구성되어 있다. Picture은 말 그대로 이미지 한장
* Picture는 slice로 구성되어 있다.
* 한장의 slice는 macroblock으로 구성되어 있다.
* macroblock은 block(8x8)으로 구성되어 있다.
* 즉 MEPG 비디오는
  * Video Sequence Layer
  * Group of Pictures (GOP) Layer
  * Picture (Frame) Layer
  * Slice Layer
  * Macroblock Layer
  * Block Layer(8x8)

### Goup of Picture Layer

![](https://i.imgur.com/yyGl9An.png)

* 비디오는 GOP들로 구성되어 있다.
* 하나의 GOP는 10장 정도의 이미지의 집합이다.
* i, p, b 세가지 타입의 이미지로 구성되어 있다. (intra (I) pictures, non-intra (P and/or B) pictures)
* itra-coded picture
  * JPEG 이미지라고 보면 된다.
  * 스스로 모든 것을 완비하고 있다. 다른 이미지의 정보를 전혀 활용하지 않고 스스로 인코딩하고 디코딩 할 수 있는 이미지
* nonitra(inter)-coded picture
  * 미리 인코드 되어 있는 다른 이미지들의 정보를 이용해서 만들어진 이미지들
  * Motion-compensated information을 사용함으로써 데이터의 양을 줄인다. MPEG에서 제일 중요
  *  P-picture (predictive-coded picture)
  * B-picture(bidirectionally predictive-coded picture)

![](https://i.imgur.com/kcLofYF.png)

* i-frame은 JPEG으로 코딩된 것이다. 다른 이미지의 정보를 전혀 활용하지 않고 자신만의 정보를 가지고 코딩을 한다.

* p-frame은 자기보다 앞에 나온 i-frame이나 p-frame를 가지고 코딩을 한다. 

  ![](https://i.imgur.com/HZfnezM.png)

  JPEG에서 8x8, 또는 16x16으로 나누어 코딩한다.

  p-frame은 자신보다 앞에 나온 i-frame의 8x8, 또는 16x16에서 그놈과 제일 비슷한 것을 찾는다. 그리고 여기서부터 데이터를 가지고 왔다는 정보를 집어넣는다. (motion vector)

  그리고 당연히 그 정보와 다음정보가 완전히 같진 않을 것이니 그 정보의 차이점(difference)를 추가로 저장한다.

  즉 모든 이미지를 16x16으로 나누어서 코딩하는데 자기가 가지고 있는 오리지널 데이터를 집어넣는 것이 아니라 그 부분과 가장 비슷한 부분을 찾아서 어디서 가져왔다는 정보(motion vector)와 그 차이점(difference)를 집어넣는다. 이것이 p-picture

* b-picture는 양쪽, 앞 뒤에서 받는다.

  자기보다 앞에 나온, 뒤에 나온 i,p-picture로부터 비슷한 부분을 찾는다. 그렇게 찾아서 만든 것이 b-picture

  어디서 가지고 왔는지 위치 정보(motion vecotr)와 실제 내가 가진 데이터와의 차이점(difference) 두 가지를 앞에서와 뒤에서 찾아서 총 4개를 저장해서 만들어지는 이미지가 b-picture

* 결론적으로 P는 자신보다 앞에 나온 i나 p picture로부터 정보를 가지고 오고, b는 자신보다 앞이나 뒤에 나온 i나 p picture로부터 정보를 가지고 온다.

* GOP의 스타트, i-picture은 JPEG, 나머지 p와 b들은 위치정보와 차이값을 가지고 코딩을 한다.

### I-picture

* i-picture는 랜덤 액세스할 수 있는 포인트이다. 즉 비디오를 보다가 특정 위치로 이동시키면 i-picture가 시작되는 곳으로만 갈 수 있다. 빨리 감기나 되감기 역시 i-picture만 쭉 보여준다.
* GOP는 I-picture에서 시작한다.

### P-picture

* forward prediction : 자신과 가장 가까이 있는 앞에 나온 i나 p를 가지고 코딩을 한다.

### B-picture

* forward and backward predictions : 앞 뒤에 있는 i나 p를 가지고 코딩을 한다.



### Temporal Display and Coding Order

![](https://i.imgur.com/lwac1kM.png)

* 실제 코딩하는 순서와 디스플레이하는 순서는 다르다.

### Independence of GOP

![](https://i.imgur.com/zzuQdyw.png)

* 하나의 비디오는 여러개의 GOP들로 구성되는데 각 GOP들은 서로 독립적이진 않다.
* 첫 번째 GOP는 I를 이용해서 P를 코딩하고, I와 P를 이용해 그 사이의 B를 코딩하고, P를 이용해 그 다음 P를 코딩하고 P와 P를 이용해 그 사이의 B를 코딩한다. 그런데 두 번째 GOP는 디스플레이 순서가 I가 아니라 B부터 시작한다.이럴 경우는 자기 뒤에 나오는 I와 앞에 나오는 GOP의 P를 가지고 속에 있는 B를 코딩한다. 이로 인해 인접한 GOP가 서로 독립적이진 않다.

### Macroblock Layer

![](https://i.imgur.com/c6zVXV9.png)

* 루미넌스는 오리지널 그대로 샘플링을 하고 크로미넌스는 해상도를 줄인다.
* 그래서 8x8의 4블록을 Cb와 Cr의 한 블록이 커버를 한다. 이것이 MCU.
* 그래서 이 4개 1개 1개의 블록들을 매크로 블록이라고 한다. 보통 16x16픽셀

### Slice Structures in MPEG-1 

![](https://i.imgur.com/lo0vJU7.png)

* 문제는 통신중에 앞쪽이 깨지면 GOP가 서로 독립적이지 않기 때문에 뒤에도 계속 영향을 받게 된다.
* 따라서 슬라이스라는 것을 만들었다.
* 같은 색깔로 되어 있는 것이 같은 슬라이스에 속하는 매크로 블락
* 한쪽이 노이즈가 생기거나 꺠지더라도 그 다음부분은 꺠지지 않도록 독립적으로 분리를 시킨다.
* 그 분리된 한 조각을 슬라이스라고 한다.

### Macroblock Structure

![](https://i.imgur.com/6sJhUDA.png)

* 매크로 블락의 비율은 다음과 같은 종류가 있는데 보통은 절반으로 줄인 4:2:0을 사용한다.

### Motion Compensation

![](https://i.imgur.com/zCTqtmA.png)

* B-picture가 있고 매크로 블락 16x16이라고 했을 때 b-picture은 앞이나 뒤의 i와 p의 비슷한 부분으로부터 정보를 가져온다.
* 이때 여기서 가져왔다는 정보를 motion vector라고 한다.
* 이 motion vector는 시작점과 델타 x, 델타 y 길이를 가지고 모션 벡터를 표시한다.



### Motion Estimation: Motion Vector Search

![](https://i.imgur.com/nSWgk3g.png)

자신과 비슷한 부분을 찾는데 처음부터 끝까지 찾는데 너무 오래 걸린다. 따라서 그 찾는 범위를  ±150 × ±75 pixels로 줄여놓았다.

