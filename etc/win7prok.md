
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190604-lightgray.svg?style=flat-square)

## 업무용 컴퓨터 포매팅 및 한글설치 (Win7 Pro K 정품)

- 문제점
	1. win7 과 intel 스카이레이크 간의 비호환성 문제 - 설치 시 
	2. usb 3.0 인식 문제
	3. 네트워크 드라이버 문제
	4. 한글화 문제
- 해결 (1)
	* iso 파일만을 사용하는 기존의 방식으로는 설치 드라이브 인식 불가의 문제가 발생한다.
	* 그래서 uefi 를 지원하는 메인보드 제조사의 usb patcher 가 필요했음.
	* asus(asrock?) 메인보드였으므로, 해당 홈페이지에서 **Win7UsbPatcher** 를 다운받아
	* iso 이미지와 결합하여 설치용 usb 를 만들었다.
	* 업무용이므로, 설치 시 제품 키를 입력해야 한다.
- 해결 (2)
	* win7 운영체제는 usb3.0 을 인식하지 못한다
	* 사실상 모든 usb 를 인식 못하고, 심지어 2T 짜리 하드도 자동인식하지 못함(이거는 파티션 설정하지 않은 이유인듯)
	* 하드는 추가 파티션 지정으로 해결하고, usb 인식불가 문제는 intel 에서 제공하는 **win7 용 usb driver** 를 다운받음
	* 모든 usb 를 인식하지 못해 다운받은 usb driver 파일을 옮길 방법이 없으므로,
	* 파일이 존재하는 usb 자체로 새롭게 부팅하고,
	* 거기서 ```shift + F10``` 으로 CMD 모드에 들어가 copy 명령으로 기존의 하드 드라이브에 usb 드라이버 파일을 복사했다.
	* 그리고 정상적으로 window 재부팅한 후, 복사된 드라이버 설치 파일을 실행함으로써 해결함 >> 이제는 잘 인식된다
- 해결 (3)
	* 이제 외부 저장소를 인식할 수 있게 되었으므로,
	* 3DP_NET, 3DP_chip 을 설치해 네트워크 드라이브 및 필요 드라이브를 설치/사용 할 수 있게 되었음
- 해결 (4)
	* win7 은 오래된 버전이고 곧 지원이 종료되므로 정품 한글 버전을 확보하기 어려워 우선 영문판으로 설치함
	* 이후 외부 저장소 인식 및 네트워크 연결 문제가 해결되었으므로,
	* (정품인 경우) Windows Update 옵션 중 한글 패키지 업데이트로 해결하거나,
	* (정품인증 x) 한글화 패키지를 별도로 다운받아 설치하는 식으로 해결하면 된다.
	* 그런데 전자는 ultimate 와 enterprise 버전에서만 유효하고, 후자는 라이센스 문제가 발생할 소지가 있으므로, 애초에 professional k 버전을 받는게 가장 낫다
- 추가 (4)
    * windows7 professional k iso 를 구할 수 있다면 바로 해결됨
    * 그러나 공홈에도 없고 신뢰할 수 없는 파일을 아무데서나 받을 수도 없음
    * 구글에서 파일명으로 검색해 나타나는 (이안소프트?)사이트에서 일단 파일을 받긴 했음
    * 순정 파일인지 확인하기 위해 iso 파일의 sha1 해시값을 비교하는 방법을 활용함
    * sha1 변환기 사이트에서 iso 파일을 해싱한 후 값을 비교하는 [샘플 사이트](http://winsha1.yooneed.one/win7ap.html) 에서 비교했더니 일치한다는 판정이 나옴
    * 그래서 이 iso 파일과 usb patcher 를 이용해 부팅 usb 를 다시 만들고 재설치 진행함!