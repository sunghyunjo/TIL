# Docker란?

컨테이너 기반의 오픈소스 가상화 플랫폼

- **컨테이너 ?** 다양한 프로그램, 실행환경을 컨테이너로 추상화하고 동일한 인터페이스를 제공하여 프로그램의 배포 및 관리를 단순하게 해주는 수단.

## VM vs Docker

### VM

- Host OS위에 Guest OS전체를 가상화하여 사용하는 방식
- 여러가지 OS를 가상화 할 수 있고, 비교적 사용법이 간단하지만 무겁고 느려 운영환경에서 사용할 수 없다.

![vm vs docker]([https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/vm-vs-docker.png](https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/vm-vs-docker.png))

### Docker

- 프로세스를 격리하는 방식
- 가볍고 빠르게 동작
- CPU나 메모리는 딱 프로세스가 필요한 만큼만 추가로 사용하고 성능적으로도 손실이 거의 없다.

## Image

컨테이너 실행에 필요한 파일과 설정값 등을 포함하고 있는 것

- 상태값을 가지지 않고, 변하지 않는다. (Immutable)
- 컨테이너는 이미지를 실행한 상태라고 볼 수 있고, 추가되거나 변하는 값은 컨테이너에 저장된다.

### layer

- 도커의 업데이트 과정 문제를 해결하기 위해 사용하는 개념.
- 유니온 파일 시스템을 이용하여 여러개의 레이어를 하나의 파일시스템으로 사용할 수 있게 해준다.
- 이미지를 만들기 위해 DockerFile을 만들어 자체 DSL(Domain-Specific Languagl)언어를 이용해 이미지 생성 과정을 적는다.
- Docker HUB에서 이미지를 무료로 관리해준다.

ex. 

```DOCKER
    # vertx/vertx3 debian version
    FROM subicura/vertx3:3.3.1
    MAINTAINER chungsub.kim@purpleworks.co.kr
    
    ADD build/distributions/app-3.3.1.tar /
    ADD config.template.json /app-3.3.1/bin/config.json
    ADD docker/script/start.sh /usr/local/bin/
    RUN ln -s /usr/local/bin/start.sh /start.sh
    
    EXPOSE 8080
    EXPOSE 7000
    
    CMD ["start.sh"]
    
```
