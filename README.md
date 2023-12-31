# Docker_Prac
🚢Docker_Prac : Docker을 연습하기 위한 레포🚢

## 기술 스택

### 📚 Tech Stack
#### 💻 Development
<img src="https://skillicons.dev/icons?i=java,spring,mysql,docker& perline="/>

#### ⌛ Developed Period
##### 2023.12.31

## 진행 방식

**도커 파일 작성:**
```dockerfile
# 기본 이미지 선택
FROM some-base-image

# 작업 디렉토리 설정
WORKDIR /usr/src/app

# ARG 속성 추가 - 여러번 사용되는 문자열이나 숫자 등을 변수로 만들어주는 속성
ARG JAR_PATH=./build/libs

# 로컬 빌드 경로에서 JAR 파일을 이미지로 복사
COPY ./build/libs/app1-0.0.1-SNAPSHOT.jar ./build/libs/app1-0.0.1-SNAPSHOT.jar

# 애플리케이션 실행 명령어 설정
CMD ["java","-jar","./build/libs/app1-0.0.1-SNAPSHOT.jar"]
```

### 도커 이미지 빌드
```
docker build -t springbootapp .
```

### 도커 컨테이너 생성 및 실행
```
docker run -d --name my_app_container springbootapp
```


# 이슈사항

> 어플리케이션 코드의 변경으로 인해 다시 빌드하는 상황의 문제점
- build -> 도커 이미지 생성 -> run -> 도커 컨테이너 생성
  - war 파일의 생성이야 어쩔 수 없지만, war파일이 변경되었다고 해서 Docker Image 자체를 다시 생성하고 컨테이너를 다시 실행하는 것은 매우 비효율적


## 원인이 뭘까? 🧐
> COPY를 통해 어플리케이션 실행에 필요한 파일을 컨테이너 내부로 넣어주기 때문입니다.
따라서 어플리케이션 코드가 변경되었다면, 그에 따라 파일들을 다시 컨테이너 내부로 넣어주어야 하는 상황이 발생한 것입니다.

## 어떻게 해결하나요? 🧐
> Volume 옵션을 사용
- COPY를 통해 파일을 복사하는 것이 아닌, 실행에 필요한 파일을들 컨테이너 내부에서 참조할 수 있도록 해줍니다. 이를 가능하게 하는 것이 바로 Volume입니다.

## Volume 옵션
> Volume을 사용하면 로컬 경로에 존재하는 모든 파일들을 도커 컨테이너 내부에서 사용할 수 있습니다.

다음과 같이 사용합니다.

> docker run <다른 옵션들> -v 참조할 경로(로컬):참조하는 경로(컨테이너 내부) <이미지 식별자>

필자의 경우
~~~
docker run -p 50000:8080 -v $(pwd):/usr/src/app springbootapp
~~~

- pwd는 현재 디렉토리의 경로를 출력하는 명령어입니다.<br/>
  참조하는 경로는 Working Directory에서 참조하기 때문에 Working Directory의 경로를 적어주었습니다.

## 💡 Commit Convention

|       Tags       |               Explanation               |
| :--------------: | :-------------------------------------: |
|       Feat       |            새로운 기능 추가             |
|       Fix        |                버그 수정                |
| !BREAKING CHANGE |         커다란 API 변경의 경우          |
|     !HOTFIX      |          급한 치명적 버그 수정          |
|      Build       |           빌드 관련 파일 수정           |
|      Design      |        CSS를 포함 UI 디자인 변경        |
|       Docs       |                문서 수정                |
|      Style       | 코드 포맷팅, 세미콜론 누락, 코드 변경 X |
|     Refactor     |              코드 리팩토링              |
|     Comment      |        필요한 주석 추가 및 변경         |
|       Test       |            테스트 코드 수정             |
|      Rename      |         파일, 폴더명 이름 수정          |
|      Remove      |             파일, 폴더 삭제             |
|      chore       |            빌드, 패키지 수정            |