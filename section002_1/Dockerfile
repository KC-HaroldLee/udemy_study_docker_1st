# 이미지 이름 (일반적으로 FROM으로 시작한다)
FROM node

# 초기 워크디렉토리 설정
WORKDIR /app

# COPY {from} {to} 이 기본이다.

# 처음에 사용하다가 아래와 같은 이유로 주석처리 됨.
# COPY ./app_ref /app 


# 초기 워크디렉토리가 지정되었으므로 아래처럼 해도 되기는 한다.
# COPY . ./

# 기본적으로 카피한 순간을 스냅숏으로 저장한다.
# 따라서 app 코드가 바뀐다고 해서 코드가 업데이트 되는 것은 아니다.
# 이미지는 빌드되면 잠기고 읽기전용이라는 것이 원칙임을 기억하자.
# 하지만 다시 빌드 하면 알겠지만... 레이어 기반이므로 그렇게 오래 걸리지는 않는다.
# 재평가 (re-evaluated)된다. 그리고 빨리 진행된 이유는 캐시 데이터를 사용하기 때문!
# 종속성을 생가하여 "각 레이어에 영향을 미칠" 명령어를 배치를 신경쓰자.
# "RUN npm install" 이 "COPY ./app_ref /app"보다 늦은 이유를 생각하면 여기 예제에서는 쉽다.
# 자, 따라서 node.js의 특성을 고려한다면.
# 아래 순서가 아닌
# COPY ./app_ref /app 
# RUN npm install

# node.js의 설정파일만 일단 Copy하고, npm install 후 COPY ./app_ref /app 을 뒤로 미루면 반복되는 작업의 수가 준다.
COPY ./app_ref/package.json /app
RUN npm install
COPY ./app_ref /app 



# 서버를 실행하기 위함
# 처음에 사용하다가 위와 같은 이유로 주석처리 됨.
# RUN npm install

# 틀린 이유 : 이미지가 빌드 될때마다 실행된다.
# RUN node server.js

EXPOSE 80

# 이는 컨테이너가 실행 될때만 실행된다. BASH!
# 배열로 전송됨을 잊지 말자.
CMD ["node", "server.js"]



###################
### 도커파일을 이미지화!
###################
# > docker build .
# [+] Building 4.2s (9/9) FINISHED                                             docker:default
#  => [internal] load build definition from Dockerfile                                   0.1s
#  => => transferring dockerfile: 667B                                                   0.0s
#  => [internal] load metadata for docker.io/library/node:latest                         0.0s
#  => [internal] load .dockerignore                                                      0.0s
#  => => transferring context: 2B                                                        0.0s
#  => [1/4] FROM docker.io/library/node:latest                                           0.3s
#  => [internal] load build context                                                      0.1s
#  => => transferring context: 2.13kB                                                    0.0s
#  => [2/4] WORKDIR /app                                                                 0.1s
#  => [3/4] COPY ./app_ref /app                                                          0.1s
#  => [4/4] RUN npm install                                                              3.4s
#  => exporting to image                                                                 0.1s
#  => => exporting layers                                                                0.1s
#  => => writing image sha256:0e35ab36bbc8c656a09a234dcee8aaac69ab38c5817e721b68fbad0b4  0.0s

# > docker run 0e35ab36bbc8c656a09a234dcee8aaac69ab38c5817e721b68fbad0b4
# 완료되지 않는 이유는 > 마지막 CMD ["node", "server.js"] 가 실행되고 있기 때문

# 접속이 되지 않는 이유는 다음 프래그가 있어야 했기 때문> -p 80:80