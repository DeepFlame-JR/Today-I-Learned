# OS (Operating System)
컴퓨터를 쉽게 다루게 해주는 인터페이스.
한정된 메모리나 시스템 자원을 효율적으로 분배한다.

## 운영체제와 컴퓨터

### 운영체제의 역할과 구조

#### 역할
1. CPU 스케줄링과 프로세스 관리: CPU 소유권을 어떤 프로세스에 할당할지, 프로세스의 생성과 삭제, 자원 관리 등을 관리
1. 메모리 관리: 한정된 메모리를 어떤 프로세스에 얼마큼 할당해야할지 관리
1. 디스크 파일 관리
1. I/O 디바이스 관리

#### 구조
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FS3U2H%2FbtqHxbOhB2V%2FWeQANBQKfm6F58xkWsJZy0%2Fimg.png">

GUI  
- 사용자가 전자 장치와 상호 작용할 수 있도록 하는 사용자 인터페이스의 한 형태이다.
    - 리눅스 서버에서는 GUI가 아닌 CUI를 사용한다.

**커널**  
- 프로세스 관리, 메모리 관리, 저장장치 관리와 같은 운영체제의 핵심적인 기능을 모아둔 것이다.  
- 자동차:운영체제 = 엔진:커널

**인터페이스**
- 커널에 사용자의 명령을 전달하고, 실행결과를 사용자에게 알려주는 역할
- 커널과 인터페이스를 분리하여 같은 커널이여도 다른 인터페이스 형태로 제작 가능하다.
- 시스템 호출과 드라이버가 있다.

**시스템 호출**  
- 커널이 자신을 보호하기 위해 만든 인터페이스
- 컴퓨터 자원을 보호하기 위해 자원에 직접 접근하는 것을 차단
- modebit 변수가 0일 때 커널 모드 1일 때 유저 모드로 설정되며, 유저 모드일 경우 시스템 콜을 못하게 막아서 한정된 일만 하도록 설정 

**드라이버**   
- 커널과 하드웨어의 인터페이스
- 하드웨어를 제어할 수 있다

### 컴퓨터의 요소
CPU, DMA 컨트롤러, 메모리, 타이머, 디바이스 컨트롤러로 이루어짐

<img src="https://velog.velcdn.com/images%2Fjhw1251%2Fpost%2F1b0f5b51-92a2-4156-bb81-ac43b6be01cb%2Fimage.png" width="400">

#### CPU
- 인터럽트에 의해 단순히 메모리에 존재하는 명령어를 해석해서 실행
    - 커널이 프로그램을 메모리에 올려 프로세스로 만듬
    - 인터럽트: 어떤 신호가 왔을 때 CPU를 잠깐 정지시키며, 인터럽트 핸들러 함수가 실행됨
        - 하드웨어 인터럽트: IO 디바이스에서 발생하는 인터럽트
        - 소프트웨어 인터럽트: 프로세스 오류 등으로 프로세스가 시스템콜을 호출할 때마다 발동됨
- 산술 논리 연산 장치, 제어 장치, 레지스터로 구성  
    - 제어 장치  
        - 프로세스 조작을 지시
        - 입출력 장치 간 통신을 제어하고 명령어들을 읽고 해석, 데이터 처리를 위한 순서를 결정함
    - 레지스터
        - 매우 빠른 임시기억장치
        - 레지스터를 거쳐 CPU에 데이터를 전달
    - 산술 논리 연산 장치
        - 산술 연산과 논리 연산을 계산하는 디지털 회로
        - CPU의 연산처리
            1. 제어장치가 메모리, 레지스터에 계산할 값을 로드. 
            1. 제어장치가 레지스터에 있는 값을 계산하라고 산술 논리 연산 장치에 명령.
            1. 제어장치가 계산된 값을 다시 레지스터에서 메모리로 저장.

#### DMA 컨트롤러
- I/O 디바이스가 메모리에 직접 접근할 수 있도록 하는 하드웨어 장치
- CPU 부하를 막아주며, CPU의 일을 부담하는 보조일꾼

#### 메모리 (RAM)
- 전자회로에서 데이터나 상태, 명령어를 기록.
- CPU는 계산을 담당하고, 메모리는 기억을 담당
- 메모리가 크면 많은 일을 동시에 할 수 있음

## 메모리
CPU는 단지 메모리에 올라온 프로그램의 명령어를 실행할 뿐이다.

### 메모리 계층
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c6/%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B3%84%EC%B8%B5%EA%B5%AC%EC%A1%B0%EA%B7%B8%EB%A6%BC1.png/220px-%EB%A9%94%EB%AA%A8%EB%A6%AC%EA%B3%84%EC%B8%B5%EA%B5%AC%EC%A1%B0%EA%B7%B8%EB%A6%BC1.png">

- 레지스터: CPU 안에 있는 작은 메모리. 휘발성, 속도 가장 빠름. 용량이 가장 적음
- 캐시: L1, L2, L3 캐시를 지칭함. 휘발성, 속도 빠름. 기억 용량이 적음
- 주기억장치(RAM): 휘발성, 속도, 기억 용량이 보통임
- 보조기억장치(HDD,SDD): 휘발성, 속도가 낮음. 기억 용량이 많음

계층이 위로 올라갈 수록 가격이 비싸진다. 이러한 경제성 때문에 계층을 두어 관리함.  
게임에서 '로딩 중'이라는 메시지는 하드디스크 또는 인터넷의 데이터가 `RAM`으로 전송되는 과정이다.

#### 캐시
- 데이터를 미리 복사해 놓은 임시 저장소
- 계층과 계층사이에 위치함
    - 예시) 캐시 메모리와 보조기억장치 사이에는 보조 기억 장치라는 캐싱 계층이 있음
- 빠른 장치와 느린 장치에서 속도 차이에 따른 병목 현상을 줄이기 위함
- 지역성의 원리: 캐시 계층을 두지 않고 캐시를 직접 선택할 때, 자주 사용하는 데이터를 기반으로 설정해야하는 데 그때의 근거
    - 시간 지역성: 최근 사용한 데이터에 다시 접근하려는 특성 (for 문에서 i를 계속 접근한다)
    - 공간 지역성: 최근 접근한 데이터와 같거나 가까운 공간에 접근하는 특성 (배열 arr에 계속 접근)
- 웹 브라우저의 캐시: 웹 브라우저의 작은 저장소 쿠키, 로켈 스토리지, 세션 스토리지
- DB의 캐시: 메인 DB위에 redis DB를 '캐싱 계층'으로 두어 성능을 향상시킴.

#### 캐시히트와 캐시미스
- 캐시히트: 캐시에서 원하는 데이터를 찾음
- 캐시미스: 해당 데이터가 없다면 주메모리(RAM)으로 가서 데이터를 찾아옴
- 캐시매핑: 캐시히트를 위해 매핑하는 방법
    - 직접매핑: 메모리가 1~100이 있고, 캐시가 1~10이 있다면 1:1~10, 2:11~20으로 매핑. 처리가 빠르지만 충돌이 자주 발생.
    - 연관매핑: 순서를 일치시키지 않고 관련 있는 캐시와 메모리를 매핑. 충돌이 적지만 모든 블록을 탐색해야 함으로 속도가 느림.
    - 집합 연관 매핑: 직접매핑과 연관 매핑을 합쳐 놓은 것. 순서는 일치시키지만 집합을 둬서 저장하며 블록화되어 있어 검색이 더 효율적. 메모리가 1~100이 있고 캐시가 1~10이 있다면 캐시 1~5에는 1~50 데이터를 무작위로 저장시킴.

### 메모리 관리
운영체제의 대표적인 할 일 중 하나. 컴퓨터 내의 한정된 메모리를 극한을 활용한다.

#### 가상 메모리
- 메모리가 실제 메모리보다 많이 보이게 하는 기술
- 물리 메모리는 한정적이기 때문에 현재 사용하는 페이지만 올리는 것이 효율적이다
- 가상 주소(가상적으로 주어진 주소), 실제 주소(실제 메모리상에 있는 주소)로 나뉨
    - MMU(Memory Management Unit): 메모리 관리 장치에 의해 가상 주소를 실제 주소로 변환
        - CPU에서 가상주소를 통해 메모리에 접근하면 MMU를 통해 실제주소로 변경된다
    - 페이지 테이블: 가상 주소와 실제 주소가 매핑되어 있고, 프로세스 주소가 존재함
    - TLB: 메모리와 CPU 사이에 주소 변환을 위한 캐시. 속도 향상에 기여.

**Page Fault**
- 가상 메모리에는 존재하지만, 실제 메모리인 RAM에는 현재 없는 데이터에 접근할 경우 나타남
- 페이지 폴트가 전혀 발생하지 않은 것처럼 하기 위해서 `스와핑`진행
    - `스와핑`: Page Falut를 방지하기 위해서 RAM과 하드디스크 사이에 데이터를 조회 정도를 기준으로 올리고 내림을 반복함
- Page: 가상 메모리를 사용하는 최소 크기 단위
- frame: 실제 메모리를 사용하는 최소 크기 단위

#### 스레싱
- 메모리의 Page Fault이 높은 것. 심각한 성능 저하 초래.
- 메모리에 너무 많은 프로세스가 동시에 올라가면 많은 스와핑이 일어나게 됨
- 페이지 폴트가 많이 일어남 > CPU 이용률이 낮아짐 > CPU가 한가한가 해서 더 많은 프로세스를 메모리에 올림
- 해결 방법
    1. 메모리를 늘리거나, HDD를 SDD로 전환
    1. 작업 세트: 프로세스의 과거 사용 이력인 지역성을 통해 페이지 집합을 만들어 미리 로드
    1. PFF: 페이지 폴트 빈도를 상한선과 하한선을 두어 조절. 상한선에 도달한다면 페이지를 늘림.

#### 메모리 할당
- 메모리에 프로그램을 할당할 때는 `시작 메모리 위치`, `메모리의 할당 크기`를 기반으로 할당
- 할당 방법
    1. 연속할당
        - 메모리에 연속적으로 공간을 할당
        - 고정 분할 방식: 메모리를 미리 나누어 관리. 내부 단편화(메모리보다 프로그램이 작아서 들어가지 못 하는 공간이 발생)
        - 가변 분할 방식: 프로그램 크기에 맞게 동적으로 메모리를나눠 사용. 외부 단편화(메모리를 나눈 크기보다 프로그램이 커서 들어가지 못하는 공간이 발생)
    1. 불연속할당
        - 페이징: 메모리를 동일한 크기의 페이지로 나누고, 프로그램마다 페이지 테이블을 두어 프로그램을 할당. 주소 변환이 복잡한 단점
        - 세그멘테이션: 페이지 단위가 아닌 의미 단위(코드, 데이터, 스택, 힙)인 세그먼트로 나눔.  
        - 페이지드 세그멘테이션: 공유나 보안을 의미 단위인 세그먼트, 물리적 메모리는 페이지

## 프로세스와 스레드
- 프로세스: 컴퓨터에서 실행되고 있는 프로그램. CPU 스케줄링의 대상이 되는 작업.
- 스레드: 프로세스 내 작업의 흐름.
