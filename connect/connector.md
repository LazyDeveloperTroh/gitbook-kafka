---
description: >-
  커넥터는 프로듀서 역할을 하는 소스 커넥터와 컨슈머 역할을 하는 싱크 커넥터로 나뉜다. DB 에서 토픽으로 레코드를 전송하는 프로듀서를
  반복적으로 만들어야 하는 경우 소스 커넥터가 유용할 수 있고, 토픽의 데이터를 파일로 저장하는 컨슈머를 반복적으로 만들어야 하는 경우 싱크
  커넥터가 유용하게 사용될 수 있다.
---

# 커넥터

### 커넥터 플러그인

싱크 커넥터, 파일 소스 커넥터를 기본 플러그인으로 제공한다. 이외에 추가적인 커넥터를 사용하고 싶은 경우 플러그인 형태로 커넥터 JAR 파일을 추가하여 사용할 수 있다.

#### 오픈소스 커넥터

HDFS 커넥터, AWS S3 커넥터, JDBC 커넥터, 엘라스틱서치 커넥터 등 다양한 커넥터가 존재한다. 컨플루언트 허브(https://www.confluent.io/hub/) 에서 검색할 수 있다.
