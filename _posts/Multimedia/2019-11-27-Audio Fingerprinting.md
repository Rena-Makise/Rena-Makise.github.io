---
title: Audio Fingerprinting
categories:
  - multimedia
tags:
  - Multimedia
date: 2019-11-27T13:00:00+09:00
toc: false
use_math: true
---


## Audio Fingerprinting

### Audio Fingerprinting

* 오디오 인식 또는 비디오 인식, 그리고 생채인식(홍채 인식, 얼굴 인식, 지문 인식)

* Fingerprinting에 대해 알게 되면 다른 생채인식에서 활용이 가능하다.

* Audio Fingerprtinting을 활용해서 음악 인식

* 여기서의 Fingerprinting은 인간의 지문을 뜻하는 것이 아니고 지문처럼 유일하게 특정 대상을 식별할 수 있는 정보

* Fingerprinting 시스템에서 미디어 인식은 일반적으로 세 단계로 이루어진다.

  ![](https://i.imgur.com/Xc4UO72.png)

  1. Fingerprint를 추출한다. (feature vector를 추출한다.)  -  accuracy(정확도)가 중요
     * 오디오면 오디오, 비디오면 비디오, 생체면 생체 등 이런 정보를 뽑아내야 한다.
     * 이러한 정보들은 feature vector로 표시가 된다.
       * 실세계의 모든 정보는 n차원상의 한 점으로 표시가 된다.
       * 그 점을 feature vector라고 부른다.
     * 여기서는 정확도가 중요. 잘 뽑아내야 한다.
     * 오디오(비디오) fingerprint는 오디오(비디오) 레코딩을 요약하는 제한된 수의 비트로 구성된 컴팩트 시그니처(정보).
  2. DB의 검색 - performance(성능)가 중요
     * 뽑아낸 fingerprint의 수가 엄청 많다. 따라서 실제 DB에 올라가게 되는 fingerprint의 수는 무수히 많게 된다. 따라서 이것을 어떻게 빨리 찾느냐
     * 이를 위해 indexing과 search 알고리즘이 사용된다
       * 이 둘중에 인덱스가 더 중요.
       * 인덱스는 자료구조다. 자료구조를 잘못만들게 되면 search 알고리즘을 잘 짜도 아무런 소용이 없다. 즉 인덱스는 뼈대.
  3. Fingerprint 매칭
     * input으로 유저가 주는 쿼리와 db에 들어가있는 데이터하고 매치

* 전체 시스템 구조

  ![](https://i.imgur.com/70AObjR.png)

* 오디오로부터 feature vector를 뽑는다. feature vector는 수십억게가 뽑아지게 되고 DB에 저장이 된다.

* 유저가 input을 주면 그 주어진 것으로 부터 정보를 뽑아내서 db의 index구조를 통해 전체를 서치해서 매치되는 것을 결과로 리턴

### Audio Fingerprint Extraction

* Haitsma와 Kalker의 작품

  ![](https://i.imgur.com/8rlwTAT.png)

  1. 오디오 시그널을 프레임 단위로 쪼갠다
     * 소리는 공기의 떨림. 그 공기의 떨림을 전기적인 신호로 위와 같이 표시.
     * 이 전기적인 신호를 샘플링(초당 몇 개의 데이터를 들고온다)해서 비트단위로 저장해서 집어넣으면 오디오 파일이 된다.
  2. 0.0116초마다 샘플링을 한다. 그리고 32비트 단위로 표시. 이 과정을 계속해서 반복한다. 이렇게 원래의 오디오 시그널을 FingerPrint의 집합으로 뽑아낸다.
     * 오디오 시그널을 프레임 단위로 자른다. 이 프레임 단위가 0.0116초
     * 이렇게 데이터를 뽑아서 그것을 32비트로 표시
     * 이것을 sub-fingerprint라고 한다.
  3. global fingerprint precedure는 오디오 스트림을 sub-fingerprint의 스트림으로 변환한다. 이것이 우리가 사용하는 feature vector
     * 하나의 오디오 fingerprint는 32비트 sub-fingerprint들을 쭉 연결시켜놓은것
     * 이 메타 데이터를 가수이름, 앨범이름 발표한 연도 등과 같은 정보와 함께 DB에 저장
     * 이후 유저가 어떤 노래 정보를 찾아내고자 갖다댄다. DB에는 전체 fingerprint가 다 들어있다. 이때 입력되는 부분과 일치하거나 아주 유사한 것을 찾아낸다.
     * 핵심은 fingerprint를 어떻게 뽑아내는지, 그리고 실제로 매칭하는것. 이 두 가지가 핵심.
  
* Fingerprint Example

  ![](https://i.imgur.com/BySBHPq.png)

  * 흰색은 0, 검은색은 1의 비트 패턴이라고 생각하면 된다.
  * 이 한줄이 sub-fingerprint. 위 그림애서는 16비트로 표현이 되어 있는데 32비트라고 생각하면 된다.
  * a는 오리지널 뮤직 데이터. 음반에서 시스템이 fingerprint를 뽑아서 쭉 시간순으로 표시헤 놓은 것
  * b는 유저가 주는 데이터(쿼리). 특정 상황에서의 동일한 음악. 자동차 운전하는 소리가 있을 수도 있고, 사람들 소리도 있을 수 있고 등 노이즈가 있다.
  * c는 a와 b사이의 에러.
  * 핵심은 이렇게 에러가 있어도 잘 찾아내도록 하는 것. 하지만 실제론 잘 안된다. 오리지널 음악을 틀면 잘 찾지만 동일한 음악의 라이브 버전을 틀면 잘 못찾는다. 아직도 개선되어야 할 점이 많이 있다.

* Fingerprint Matching

  ![](https://i.imgur.com/dlePppl.png)

  * LUT테이블에는 해당 Fingerprint를 가지고 있는 노래를 가리키는 포인터들이 들어있다.
  * Fingerprint(sub-fingerprint)가 256개 들어왔다고 가정. 즉 약 3초정도의 음악을 input으로 줬다고 가정.
  * 이 각각 하나하나가 LUT(LookUp Table)테이블에 가면 해당 fingerprint를 가지고 있는 노래를 가리키는 포인터가 들어있다. LInkedList형태로.
  * 이렇게 후보들을 뽑는다. 그 후보들 중에서 match를 해서 쓰레쉬홀드, 임계치를 넘으면 이 음악은 이 음악이다라고 판단을 해서 유저에게 answer로 던져준다.

* Difficulties in Fingerprint Matching

  * Inperfect Matching
    * 매칭이 100%확실하지 않다.
    * 사람이 들으면 동일하지만 기계적으로는 다르다고 나오는 것. 예를 들어 라이브 음악같은거.
    * 노이즈가 끼여있을 수도 있고 통신상의 문제가 있을 수도 있고.
    * 사실 노이즈나 통신상의 문제같은 경우에는 무시하고 잘 찾아내지만 라이브 음악이 문제.
  * Dimensionality Curse - 차원의 저주
    * 하나의 데이터가 32비트로 표시된다.
    * 256개의 fingerprint가 들어왔다고 가정. 약 3초정도.
    * 그러면 32 x 256 = 8192-bit vector가 들어오게 된다. 즉 차원의 수가 8000개가 넘어간다. 
    * 이러한 고차원 데이터를 어떻게 색인할 것인가.
  * 쿼리로 들어오는 fingerprint의 길이와 DB fingerprint의 길이가 다르기 때문에 위치가 다르다.
    * 오리지널 fingerprint에 비해 쿼리가 어느 위치에서 얼만큼 들어올지 모른다.

* Previous Work for Fingerprint Search

  * Haitsuma & Kalker's Approach
    * DB에 존재하는 오리지널 sub-fingerprint 중에서 유저가 주는 쿼리와 최소한 하나의 sub-fingerprint는 매치한다고 생각. 하지만 확실히 있을지 불명확.
    * 시스템 속에 LUT을 유지. 그런데 이 테이블 수가 32비트니까 $2^{32}$, 즉 4GB가 넘으니까 메모리에 올려놓고 있기에 사이즈가 너무 크다. 그렇다고 디스크에서 두고 서치하기도 너무 느리다. 여기서도 어려움이 존재.
    * single match principle
      * 하나라도 일치하는 것이 있으면 그 모든 노래를 다 비교를 한다. 그래서 시간이 많이 걸린다.
      * 그래서 교수님은 multiple match, 여러개를 동시에 찾아서 fingerprint가 매칭되는 갯수가 높은 순서대로 서치를 하여 경우의 수를 줄였음.