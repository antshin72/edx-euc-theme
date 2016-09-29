# kmooc-theme
kmooc 유칼립투스 테마

## eucalypus 테마의 구성
유칼립투스의 테마구성은 기존의 테마의 디렉토리 구조와 유사하나 CMS와 LMS가 테마 내부에 존재하는 구성으로 변경 되었으며,  
sass등의 구성도 다소간의 차이가 있는것으로 메뉴얼에 언급되어 있다.

### 테마 디렉토리 구조
```
kmooc-theme
└── README.rst
└── lms
    └── static
        └──images
            └──logo.png
        └──sass
            └──_overrides.scss
            └──lms-main.scss
            └──lms-main-rtl.scss
            └──base
                └──_variables.scss
    └── templates
        └──header.html 
    cms
    └── static
        └──images
            └──studio-logo.png
        └──sass
            └──_variables.scss
    └── templates
```

기본적으로 이전의 디렉토리 구조와 비슷하나 몇가지 주요한 변화점이 있다.  
- 로고 이미지 구성 변경: 반응형웹 적용을 위하여 각 로고별 사이즈 패턴 및 대응 이미지 구성이 필요해짐
- sass에 각각의 역활을 담당하는 scss파일이 구성되고 파일명칭 및 구조가 변경됨.

**그외 js등은 js 디렉토리등을 구성하여 연동하면 될것으로 판단된다.**

### 테마 세부 내용
본 내용은 메뉴얼의 내용을 정리한 내용임.

#### 테마 구성이 가능한 템플릿 파일 목록
|component|template|description|
|----------|-------|--------|
|LMS|header.html|edx의 header파일은 `<body>`노드 하위의 내용이며, `<head>` 부분에 대한 정의는 아니다.<br>EDX에서는 `navigation.html` core에서 copy하여 해당 부분에 대한 구성을 변경하여 작업하는것을 추천한다.|
|LMS|footer.html|core의 `footer.html`을 카피하여 수정하는것을 추천한다.|

**NOTE**
```
테마 작업에서 사용가능한 override의 경우 마코템플릿과 언더스코어를 기반으로 오버라이드를 해야 한다.  
오버라이드 작업을 진행 시 마코템플릿의 `<%static:include%>` 코드를 이용하여 작업이 진행되어야 한다.  
django 템플릿을 오버라이드가 불가능하다.  
```

#### sass 변수의 처리
|Component|File|
|--------|----------|
|LMS|`lms/static/sass/base/_variables.scss`|
|Studio|`cms/static/sass/_variables.scss`|

#### 지원 가능한 테마 assets
이론상으로는 LMS/Studio의 Assets 객체는 모두 오버라이드 가능하다.  
에셋의 객체는 모두 가능하나, 아래의 표를 제외한 assets은 테마를 안탈수도 있다.  
이부분은 알아서 패치해야 한다.  

|Component	|Asset	|Description|
|----|----|----|
|LMS	|`images/logo.png`|	LMS 상단 좌측 로고이미지|
|Studio	|`images/studio-logo.png`| Studio 상단 로고 이미지|
|LMS	|`images/profiles/default_30.png` <br> `images/profiles/default_50.png` <br> `images/profiles/default_120.png` <br> `images/profiles/default_500.png`	|학습자 프로파일 기본 이미지.<br>프로파일을 학습자가 갱신하지 않는경우 출력됨.<br>이미지는 `_넓이사이즈.png` 형식으로 저장된다.|


## 테마 활성화
테마가 설치되어야 하는 경로: `/edx/app/edxapp/themes/테마명`  
테마 활성하를 위하여 아래 3개의 변수가 설정되어야 함
- ENABLE_COMPREHENSIVE_THEMING : Boolean값 comprehensive 테마 설정
- COMPREHENSIVE_THEME_DIRS : 테마 폴더 경로 설정
- DEFAULT_SITE_THEME : 기본 테마명 설정

### ansible 설정을 통한 배포
경로: `/edx/app/edx_ansible/server-vars.yml`
설정내용 예제:

```
EDXAPP_ENABLE_COMPREHENSIVE_THEMING: True
EDXAPP_COMPREHENSIVE_THEME_DIRS:
  - /edx/app/themes
EDXAPP_DEFAULT_SITE_THEME: my-theme
```

위의 코드가 배포되기 위해서는 ansible 명령을 수행하애 한다.
설정된 코드는 lms.env.json에 배포된다

**테마 설정코드 배포 명령**
```
sudo /edx/bin/update edx-platform master
```

### lms.env.json 파일을 직접 설정하여 배포
lms.env.json 파일의 아래 코드를 수정하고 edxapp을 재기동한다.  
```
ENABLE_COMPREHENSIVE_THEMING: true,
COMPREHENSIVE_THEME_DIRS: ["/edx/app/themes" ],
DEFAULT_SITE_THEME: "my-theme"
```