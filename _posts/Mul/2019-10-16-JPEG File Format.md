---
title: JPEG File Format
categories:
  - mul
tags:
  - Mul
last_modified_at: 2019-12-22T13:00:00+09:00
toc: false
use_math: true
---

## JPEG file syntax

![](https://i.imgur.com/g7utcp2.png)

* SOI (Start of Image)  - FFD8 /  EOI(End of Image) - FFD9 : 2byte (16bit)
  * 이미지를 처리할 프로그램이 이미지를 받았을때 FFD8을 발견하면 이것이 JPEG이미지라는 것을 판별한다.
  * 마지막에 FFD9를 보면 마지막이라는것을 판단
* 그 속에 있는 데이터를 프레임이라고 한다.
* 프레임은 앞에 프레임 헤더가 붙어있고 여러개의 Scan이라는 것으로 구성이 된다. ($Scan_1$ ~ ${Scan}_{last}$)
  * Sequential Encoding은 한줄한줄 인코딩하는것. 즉 Scan이 하나밖에 없다. 싱글스캔
  * Progressive Encoding은 Scan이 여러개 있는 것. 이전 예제에서는 Scan이 3개.
* 프레임 헤더 앞에는 Tables/misc가 있다. 실제 JPEG안에는 데이터와 헤더뿐만 아니라 몇년 몇월 몇일, 어느 프로그램을 통해 만들었는지 등 잡다한 정보들도 들어가있다.
* Scan 안에는 Scan header와 ECS라는 것으로 구성되어 있다.
  * Scan header는 Scan에 대한 정보를, ECS는 세그먼트 여러개로 구성되어 있다.
* 즉 JPEG은 위와 같이 계층 구조로 이루어져 있다.
  *  An image > Frames > Scans > Segments > MCUs > Data units(8x8 pixel block) > real data 
* 실제 이미지는 대개 Frame과 Scan이 하나로 구성되어 있다.

### Marker Code

* 이러한 정보들이 차례로 들어가 있지 않고 분산되어 있다.

* 마커 코드에 의해 나누어져 있다.

  ![](https://i.imgur.com/GZuvvFD.png)

  * JPEG파일은 먼저 특정한 정보를 찾기 위해 마커 코드를 찾아야 한다. 그리고 나서 마커 코드 이후에 나오는 정보를 얻어서 분석한다.

  * 마커 코드는 2 byte

    ![](https://i.imgur.com/3suJusS.png)

  * 마커 코드는 FF로 시작해서 나머지 코드는 C0에서 FE사이의 코드 값을 갖는다.

  * 그 다음의 2byte는 마커 코드를 제외한 마커 블록의 길이

### Frame Header

![](https://i.imgur.com/Ose2FNi.png)

* 위의 2, 2, 1은 byte수
* SOF는 JPEG 포멧 마커. FFC0면 첫 번쨰 JPEG 알고리즘. FFC1이면 두 번째 ...
* LF는 SOF를 제외한 전체 길이가 들어간다.
* P는 몇비트를 쓰는지. Y Cb Cr을 표시하는데 몇 비트를 쓰는지.
* Y와 X는 가로 세로 해상도. Y가 세로, X가 가로
* Nf는 컴포넌트가 몇 개 있는지. 일반적으로 칼라이미지는 Y Cb Cr로 컴포넌트는 3개이다. 흑백이미지는 Y밖에 없으므로 컴포넌트가 1개.
* Ci는 컴포넌트의 넘버
* Hi, Vi는 샘플링 팩터
* Tqi는 quantization 테이블 넘버. quantization 테이블은 Luminance table, Chrominance table 2개가 있다.

### Sampling Factors

![](https://i.imgur.com/oBthfCM.png)

* Y는 그대로 쓰고 Cb와 Cr은 샘플링 팩터를 통해 줄여서 쓴다.
* Y는 실제 픽셀을 다 끄집어 낸다. Cb와 Cr은 4개의 정보에서 평균을 내서 하나를 뽑아낸다. 
* 그렇게 되면 Y는 4개가 있는데 Cb와 Cr은 하나밖에 없으니 4개의 픽셀에서 하나를 공유한다.
* 그렇게 해서 Cb와 Cr을 줄여서 데이터의 양을 줄인다.
* 이렇게 줄여도 사람은 칼라에 둔감해서 구별하지 못한다.
* 보통은 위와 같이 Y는 그대로 쓰고 Cb와 Cr은 절반으로 줄인다.

### Scan Header

![](https://i.imgur.com/FQ1Pbir.png)

* SOS : 스캔의 마커코드. 각각의 스캔은 FFDA로 시작한다.
* Ls : SOS를 제외한 전체 스캔 블럭의 크기
* Ns : 몇 개의 스캔이 들어있는지. 보통 스캔은 한 개이다.
* Cs : 스캔 컴포넌트의 넘버
* Td, Ta : 허프만 코드 테이블이 들어가있다. 허프만 코드 테이블은 루미넌스용, 크로미넌스용이 있고 그 안에 DC용과 AC용이 나뉘어져 있으니 총 4개가 있다. 각각의 컴포넌트가 사용하는 DC용 허프만 코드 테이블 넘버, AC용 테이블 넘버가 들어가 있다.

### Multiple-Component Control

* 칼라는 기본적으로 컴포넌트가 3개, 흑백은 1개

* 오리지널 이미지를 3개의 컴포넌트로 분리하고, Y컴포넌트의 4개의 픽셀은 Cb와 Cr에선 해상도를 축소시켰으니 1개의 픽셀을 공유한다.

* 하지만 일단 Sampling Factor가 동일하다고 가정해보자

  ![](https://i.imgur.com/4Blkls0.png)

* Non-Interleaved방식

  * 첫번째 스캔에서 A를 전부 쭉 읽고, 두번째 스캔에서 B를 쭉 읽고, 세번째 스캔에서 C를 쭉 읽는다. 각자의 컴포넌트가 서로 섞이지 않고 각자 읽힌다.

* Interleaved 방식

  * A, B, C 컴포넌트가 서로 섞여있다.
  * 실제로는 이 방식으로 코딩이 된다. Non-interleaved방식을 사용하게 되면, 특정 부분을 읽고 싶다고 했을때 3개의 블럭만 읽으면 되는데 끝까지 다 읽어야 출력이 가능하다. 하지만 Interleaved는 읽으면서 출력이 가능하므로.

* Sampling Factor가 다를경우

  ![](https://i.imgur.com/HNNYlGu.png)

  * 가로를 절반으로 줄였다.
  * 이때 Interleaved방식을 사용하면 Sampling Factor와 비율이 동일하게 들어간다.
  * 즉 A 2개가 B의 하나에, C의 하나에 해당되므로 2 1 1개씩 들어간다.
  * 이때 그 하나를 MCU(Minimun coded unit)라고 부른다.
  * Non-interleaved방식의 경우 MCU는 하나의 데이터 유닛(8x8)이다.

### Order of Source Image Data Encoding

![](https://i.imgur.com/JZZjQoK.png)

* 점 하나가 8x8 pixel block이다.
* 그리고 컴포넌트가 4개로 구성되어 있다.
* 8x8 4개가 그다음 컴포넌트의 2개, 그다음 컴포넌트의 2개, 그다음 컴포넌트의 1개.
* 따라서 첫 번째 MCU는 그 한 줄 전부이다.

### Quantization Table

* Quantization Table은 루미넌스용, 크로미넌스용 두 개가 있다.

![](https://i.imgur.com/NQSadfa.png)

* DQT : 이미지 파일에서 Quantization Table을 찾는 마커 코드 (FFDB)
* Lq : DQT를 제외한 전체 길이
* Pq : 보통은 제로가 들어가있음
* Tq : Quantization Table이 두 개가 들어가 있으니 넘버
* Q0 ~ Q63 : Quantization Table

### Huffman Table

![](https://i.imgur.com/1aARw45.png)

* DHT : 실제로 허프만 테이블은 4개가 들어가 있을테니 FFC4를 4개 찾아야 할 것
* Lh : 길이
* Tc : 테이블 클래스
* Th : 루미넌스용과 크로미넌스용이 있을테니 그 테이블 넘버
* Li : i비트짜리 허프만 코드를 몇 개 만들 것인지
* Vi.j : length가 i인 허프만 코드의 카테고리

![](https://i.imgur.com/MT5wFTr.png)

![](https://i.imgur.com/7ZpNmE5.png)

* FFC4를 찾는다. 허프만 테이블 스타트

* 001F : 이 허프만 테이블이 관리하는 영역 길이. 31byte

* 0 : Tc가 0면 DC용, 1이면 AC용.

* 0 : Th가 0면 루미넌스용. 1이면 크로미넌스용

* 그 다음 16byte. 각각 i비트짜리 허프만 코드를 몇개 만들 것인지.

* 그 다음 데이터들은 허프만 코드의 카테고리.

* 코드 만드는 법은 다음과 같다

  * 먼저 2비트 짜리 코드를 하나 만들라고 했으니 00

  * 그리고 3비트 짜리 코드 5개는 00에서 1을 더해서 왼쪽으로 한칸 이동시킨다.

    010 그리고 나머지는 1을 추가시켜서 만든다.

    010 011 100 101 110

    그리고 4비트 짜리 하나를 만들라고 했으니까 여기서 1을 더해서 왼쪽으로 한칸 이동. 1110

    그리고 5비트 짜리 하나 역시 1을 더해서 왼쪽으로 이동. 11110

    이런 식으로 DC용 루미넌스 허프만 코드를 만든다. AC용도 마찬가지.