Device Settings
===

Contents
---
- [프린터](#프린터)
  - [LPRng](#lprng)
  - [CUPS](#cups)
  - [프린터 명령어](#프린터-명령어)
    - [lpr](#lpr)
    - [lpq](#lpq)
    - [lprm](#lprm)
    - [lpc](#lpc)
    - [lp](#lp)
    - [lpstat](#lpstat)
    - [cancel](#cancel)
- [사운드 카드](#사운드-카드)
  - [ALSA](#alsa)
  - [OSS](#oss)
  - [사운드 카드 명령어](#사운드-카드-명령어)
    - [alsactl](#alsactl)
    - [alsamixer](#alsamixer)
    - [cdparanoia](#cdparanoia)
- [스캐너](#스캐너)
  - [SANE](#sane)
  - [XSANE](#xsane)
  - [스캐너 명령어](#스캐너-명령어)
    - [sane find scanner](#sane-find-scanner)
    - [scanimage](#scanimage)
    - [scanadf](#scanadf)
    - [xcam](#xcam)

프린터
---
인쇄시스템으로 초기에는 `LPRng`를 기본으로 사용했으나, 최근에는 `CUPS`라는 시스템을 추가로 사용하고 있다.

### LPRng
- 라인 프린터 데몬 프로토콜 사용
- 프린터 스풀링, 네트워크 프린터 서버
- 설정 정보는 `/etc/printcap` 파일에 저장


### CUPS
- 애플이 개발
- 유닉스 계열 OS를 프린터 서버로 사용가능하게 함
- `Ipadmin` 명령을 통해서 웹상에서 제어

**관련파일**

|파일|설명|
|:--:|--|
|/etc/cupts/cupsd.conf|CUPS 프린터 데몬 환경설정 파일|
|/etc/cupts/printers.conf|프린터 큐관련 환경 설정 파일|
|/etc/cupts/classes.conf|CUPS 프린터 데몬의 클래스 설정 파일|
|cupsd|CUPS의 프린터 데몬|

### 프린터 명령어

> 아래부터는 BSD 코드이다.

#### lpr
프린트 작업을 요청하는 명령

```bash
$ lpr [option] [file]
```

**주요 옵션**

|옵션|설명|
|:--:|--|
|-# num|인쇄매수|
|-m|작업 완료 정보를 이메일로 전송|
|-P *프린터명*|기본 설정되지 않은 프린터 사용|
|-T|타이틀 페이지에 들어갈 타이틀명|
|-r|출력한 뒤에 파일 삭제|
|-l|필터링 없이 직접 전송|

#### lpq
프린터 큐에 있는 작업 목록 출력

```bash
$ lpq [option]
```

**주요 옵션**

|옵션|설명|
|:--:|--|
|-a|설정되어 있는 모든 프린터 작업 정보 출력|
|-l|출력 결과 자세히|
|-P *프린터명*|특정 프린터 지정|

#### lprm
프린터 큐에 대기 중인 작업을 삭제하는 명령

```bash
$ lprm [option] [file]
```

**주요 옵션**

|옵션|설명|
|:--:|--|
|-|프린터 큐에 있는 모든 작업 취소|
|-U *사용자명*|지정한 사용자의 인쇄 작업을 취소|
|-P *프린터명*|특정 프린터를 지정|
|-h *서버[:port]*|지정한 서버의 인쇄 작업 취소|

#### lpc
라인 프린터 컨트롤 프로그램, 프린터나 큐 제어에 사용
`lpc` 명령 실행 후 지정한 명령어를 사용

**주요 명령**

|명령|설명|
|:--:|--|
|disable|새로운 프린트 작업 할 수 없도록 한다|
|enable|프린트 작업 가능하게 한다|
|down|지정한 프린터를 사용할 수 없게 한다|
|up|모든 환경을 활성화, 관련 데몬 새롭게 구동|
|help, ?|사용 가능한 명령 출력|
|quit, exit|lpc 명령 종료|

> 아래부터는 System-V 계열 명령어

#### lp
인쇄 명령으로 `lpr`과 유사

```bash
$ lp [option] [file]
```

**주요 옵션**

|옵션|설명|
|:--:|--|
|-d|다른 프린터 지정|
|-n|인쇄 매수 지정(~100)|

#### lpstat
프린터 큐 상태 출력

```bash
$ lpstat [option]
```

**주요 옵션**

|옵션|내용|
|:--:|--|
|-p|프린터 인쇄 가능 여부|
|-t|프린터 상태 정보|
|-a|받아들이는 요청들의 상태|

#### cancel
프린터 작업을 취소하는 명령으로 우선 `lpstat`로 요청ID를 확인해야 한다
```bash
$ cancel Request-ID
```

**주요 옵션**

|옵션|설명|
|:--:|--|
|-a|모든 인쇄 작업 취소|

사운드 카드
---

### ALSA
사운드 카드용 장치 드라이버를 제공하기 위한 리눅스 커널의 요소

### OSS
리눅스 및 유닉스계열 운영체제에서 사운드를 만들고 캡처하는 인터페이스

### 사운드 카드 명령어

#### alsactl
ALSA 사운드카드를 제어

```bash
$ alsactl [option] [command]
```

**주요 옵션**

|옵션|설명|
|:--:|--|
|-d|디버그 모드 사용|
|-f|환경 설정 파일 선택|

**command**

|명령|설명|
|:--:|--|
|store|사운드카드에 대한 정보를 환경설정 파일 저장|
|restore|환경설정 파일로부터 선택된 정보 다시 읽기|
|init|사운드 장치를 초기화|

#### alsamixer
커서 라이브러리 기반의 ALSA 사운드카드 오디오 믹서 프로그램

```bash
$ alsamixer
```

#### cdparanoia
오디오 CD에서 음악 파일 추출할 때 사용

```bash
$ cdparanoia [option]
```

**주요 옵션**

|옵션|설명|
|:--:|--|
|-w|wav 파일 추출|
|-a|Apple AIFF-C 포맷으로 추출|
|-B|Cdda2wav 스타일로 추출|

스캐너
---

### SANE
이미지 관련 하드웨어를 사용할 수 있도록 해주는 API

### XSANE
SANE 스케나 인터페이스를 이용하여 X-Windows 기반으로 만든 프로그램

### 스캐너 명령어
#### sane find scanner
USB 및 SCSI 스캐너와 관련 장치 파일을 찾아주는 명령

```bash
$ sane-find-scanner [option] [device_file]
```

**주요 옵션**

|옵션|설명|
|:--:|--|
|-q|스캐너 장치만|
|-v|자세한 정보|
|-p|병렬 포트 연결된 스캐너만|

#### scanimage

```bash
$ scanimage [option]
```

#### scanadf
자동 문서 공급장치가 장착된 스캐너에서 여러 사진을 스캔

```bash
$ scanadf [option]
```

**주요 옵션**

|옵션|설명|
|:--:|--|
|-h|도움말|
|-d|SANE의 장치 파일명 지정|
|-L|사용 가능한 스캐너 장치 목록 출력|
|--format|이미지 형식 지정(scanimage)|


#### xcam
GUI기반으로 평판 스캐너나 카메라로부터 이미지 스캔