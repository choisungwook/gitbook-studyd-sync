# 변수 ARG

{% embed url="https://youtu.be/3GnWXTCKTEM" %}

## Dockerfile 변수

Dockerfile도 프로그래밍 함수처럼 변수를 사용할 수 있습니다.



변수선언 키워드는 ARG입니다.사용방법은 <변수이름> = <디폴트 값> 입니다. 선언한 변수는 리눅스 변수처럼 달러로 접근할 수 있습니다.

```docker
ARG <변수이름>=<디폴트 값>
```



아래 예제 Dockerfile는 tmp라는 변수를 선언하고 디폴트 값을 a로 초기화했습니다. 그리고 echo명령어로 tmp변수 값을 출력했습니다.

```docker
FROM busybox
 
ARG tmp=a
RUN echo $tmp
```

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>



## 외부 초기화

Dockerfile ARG로 선언된 변수는 외부에서 값을 초기화할 수 있습니다. docker build명령어를 실행할 때, build-arg로 초기화합니다.

```shell
docker build --build-arg tmp=helloworld
```

<figure><img src="../.gitbook/assets/image (27).png" alt=""><figcaption></figcaption></figure>



외부 초기화를 이용하면 다른사람과 협력할 때 유용합니다. 변수값만 초기화 하면 동적으로 변하는 값을 쉽게 제어할 수 있으니까요! 아래 예제는 Spring공식문서에 소개되어 있는 springboot도커이미지 만드는 방법입니다.

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption><p>출처: https://spring.io/guides/gs/spring-boot-docker/</p></figcaption></figure>
