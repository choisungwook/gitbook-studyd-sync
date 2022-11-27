# Multi Stage

{% embed url="https://youtu.be/RyildWeQ5rI" %}

###

## Multi Stage 활용사례

Multi Stage가 무엇인이 살펴보기전에 활용사례부터 볼게요! 활용사례부터 보면 나도 써봐야겠다는 마음이 들것같아서 글 도입에 소개하고자 합니다.



### 도커 이미지 크기 줄이기

가장 많이 활용하는 사례는 도커이미지 줄이기입니다. 모든 도커 이미지를 줄일 수 없고 빌드 단계가 있는 도커 이미지 등 단계가 분리되어 있는 Dockerfile만 적용가능합니다.

아래 예제는 Multi Stage를 사용했을 경우 도커 이미지 크기가 얼마나 줄었는지 보여주는 예입니다.

<figure><img src="../.gitbook/assets/image (7) (1).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
여기서 잠깐! 왜 도커 이미지 크기를 줄여야할 까요? 바로 도커 이미지 관리할 때 저장 장치(예: 하드 디스크)용량을 줄이기 위해입니다.

개발자가 아닌 관리자 입장에서 도커 이미지 크기를 줄이는 것은 얼마나 중요한지 아실겁니다. 애플리케이션 버전별로 도커 이미지를 관리해야 하는데, 도커 이미지 버전 한개당 500MB라고 하면.. 버전 2개를 저장하는데 벌써 1GB가 필요하네요.
{% endhint %}

### 빌드 속도 향상

Multi Stage를 사용하면 Dockerfile작업을 병렬로 실행하여 빌드 속도를 향상 시킬 수 있습니다. 단, 조건이 있는데요. 병렬로 실행하고자 하는 작업이 연관관계가 없어야 합니다.



## Docker build란?

Multi Stage는 Docker build의 옵션 기능입니다. 그러므로 Docker build가 무엇인지 이해해야합니다.



Docker build는 간단합니다. Docker build는 Dockerfile에 적힌 내용을 해석하여 도커 이미지를 만드는 과정입니다.

<figure><img src="../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

## Multi Stage란?

<mark style="color:red;">Multi Stage는 여러 개의 BaseImage를 사용하여 docker build를 수행</mark>합니다. BaseImage는 Docker build에 사용하는 기본이미지입니다. Dockerfile에서 FROM에 설정된 이미지에 해당합니다.



<mark style="color:red;">FROM키워드를 기준으로 작업공간이 분리</mark>되는데, 분리된 작업공간을 stage라고합니다. Multi Stage는 stage가 2개 이상일 때 해당합니다.

<figure><img src="../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>



위 예제를 Docker build 실행하면, docker build로그에서 stage가 출력됩니다. stage가 2개이므로 2개의 작업영역이 분리되었습니다.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

## Multi Stage 생성되는 도커 이미지 갯수

stage가 여러개라고 해서 도커이미지가 여러개 생성되는 거는 아닙니다. <mark style="color:red;">가장 마지막에 실행된 stage작업이 도커 이미지로 생성</mark>됩니다.

아래 예제는 우분투 정보를 출력하는 Dockerfile입니다. 우분투18, 우분투16버전 정보를 출력하지만, 가장 마지막에 실행된 우분투16버전 출력이 도커이미지로 생성됩니다.

{% hint style="info" %}
예제: [https://github.com/choisungwook/dockerfile\_multistage\_practice/blob/main/multi\_stage\_example.Dockerfile](https://github.com/choisungwook/dockerfile\_multistage\_practice/blob/main/multi\_stage\_example.Dockerfile)
{% endhint %}

<figure><img src="../.gitbook/assets/image (21) (1).png" alt=""><figcaption></figcaption></figure>

## Multi Stage 적용 예 - 도커이미지 크기 줄이기

가장 마지막에 실행된 stage내용이 도커 이미지에 적용된다고 했죠? 이 특성을 이용하여 도커 이미지 크기를 줄일 수 있습니다. <mark style="color:red;">stage를 build하는 stage와 build결과를 실행하는 stage로 분리</mark>합니다. 예를 들어서 프론트엔드를 예로 들어볼게요.



reactjs, vuejs등 개발자가 만든 결과를 npm build로 빌드를 수행합니다. bulid를 위해 nodejs도 설치하고 npm도 설치해야 합니다. 그리고 각각 종속성이 걸린 라이브러리도 설치해야합니다. 고작 몇 MB인 html, css, js를 만드는데 몇 100MB라이브러리를 설치해야합니다. 그래서 도커 이미지가 불필요하게 엄청 커집니다.



하지만 stage를 분리하면 build에 필요한 라이브러리는 stage1에만 설치할 수 있습니다. build 결과물만 satge2에 복사해서 도커이미지로 생성합니다. stage2에서는 html을 로드하는 proxy와 build결과물만 있기 때문에, stage를 분리하기 전보다 도커 이미지 크기가 엄청 줄어듭니다.

<figure><img src="../.gitbook/assets/image (25).png" alt=""><figcaption></figcaption></figure>

stage간 파일 복사는 COPY --from으로 가능합니다.

<figure><img src="../.gitbook/assets/image (23) (1).png" alt=""><figcaption></figcaption></figure>



## Multi Stage적용 전/후 비교

Multi Stage를 적용하지 않으면 stage한 개에 모든 작업 내용이 실행됩니다. 그러므로 최종 명령어를 실행하기 위한 불필요한 것들이 설치됩니다. 최종 실행과 관련이 없는데도 말이죠 .

아래 예제는 npm build를 수행하기 위해 nodejs, npm등을 설치합니다. 그리고 최종적으로 nginx로 배포하기 위해 nginx를 설치합니다.

```docker
FROM ubuntu:18.04

RUN apt-get update -y \
    && apt-get install curl -y

# install nodejs
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash - \
    && apt-get install -y nodejs

# build
COPY my-app /usr/src/app
WORKDIR /usr/src/app
RUN npm install && npm rum build

# install nginx
RUN apt-get install -y nginx
RUN mv ./build/* /var/www/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```



docker build를 수행하면 도커 이미지 크기가 엄청 크다는 것이 확인됩니다.

```shell
docker build -t <이미지이름>:<tag> .
```

<figure><img src="../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>



Multi Stage를 적용하면 build에 필요한 것들을 분리할 수 있습니다.

```docker
FROM node:16-alpine as builder

WORKDIR /app
COPY ./my-app ./

RUN npm install && npm run build

FROM nginx:1.21.0-alpine as production
COPY --from=builder /app/build /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
```



docker build를 하면 stage 한개를 사용한 것보다 이미지 크기가 엄청 줄어들었습니다.

```shell
docker build -t <이미지이름>:<tag> .
```

<figure><img src="../.gitbook/assets/image (19) (1).png" alt=""><figcaption></figcaption></figure>

## 참고자

* [https://docs.docker.com/develop/develop-images/multistage-build](https://docs.docker.com/develop/develop-images/multistage-build/https://reactjs.org/docs/create-a-new-react-app.htmlhttps://nodejs.org/ko/docs/guides/nodejs-docker-webapp/)
* [https://reactjs.org/docs/create-a-new-react-app.htm](https://docs.docker.com/develop/develop-images/multistage-build/https://reactjs.org/docs/create-a-new-react-app.htmlhttps://nodejs.org/ko/docs/guides/nodejs-docker-webapp/)
* [https://nodejs.org/ko/docs/guides/nodejs-docker-webapp/](https://docs.docker.com/develop/develop-images/multistage-build/https://reactjs.org/docs/create-a-new-react-app.htmlhttps://nodejs.org/ko/docs/guides/nodejs-docker-webapp/)
