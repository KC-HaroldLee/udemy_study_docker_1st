# node것지 뭐, 이번엔 태그도 붙인다.
FROM node:14

WORKDIR /app

# 순서는 바뀌어도 되지만 나중에 앱 업데이트 시 이미지 생성 시간을 줄이기 위해 package.json부터 COPY
COPY app_ref/package.json .

RUN npm install

COPY app_ref . 
# 자 여기서는 이미지를 만들 때 사용 한 것이지 웹서버에서 만들어진 파일을 보려면 컨테이너 안을 살펴봐야한다.
# 출력결과!
# cat 테스트1.txt
# 텍스트1
# 격리되어 있다는 것을 기억하자.

# 격리되어 있더라도 볼륨을 연결하기 위해 볼륨을 추가
# VOLUME [ "/app/feedback/" ] # named 볼륨을 사용하기 위해 없앰

# 하지만 이렇게 하면 브라우저에서 안 도는데... 이유를 알기 위해
# docker logs 
# 로 확인했더니
# (node:1) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled with .catch(). To terminate the node process on unhandled promise rejection, use the CLI flag `--unhandled-rejections=strict` (see https://nodejs.org/api/cli.html#cli_unhandled_rejections_mode). (rejection id: 1)
# (node:1) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
# 아무튼 정리하면 장치간 링크가 허용되지 않음
# 도커는 컨테이너 파일 시스템 내부의 다른 폴더로 이동하지 않는다.
# 하지만 컨테이너 밖으로 이동시킬 뿐이다.

# 따라서 // await fs.rename(tempFilePath, finalFilePath); 에서 rename을 copyFile로 바꾸고
# await fs.unlink(tempFilePath); 추가
# 본질적으로 파일을 복사하고 그 후에 수동으로 파일을 삭제하는 것이다.

# 그러면 일단 저장은 된다. http://localhost:30001/feedback/aaa.txt로 확인함
# 그리고 !!!STOP!!! 하고 나서 다시 !!!RUN!!! 후
# http://localhost:30001/feedback/aaa.txt
# GET하면 파일은 살아 있다는 것을 볼 수 없다. 
# 이유는 익명의 볼륨을 사용했기 때문이다.

# 그래서 이젠 named 볼륨을 사용할 것이다. # 하드드라이브 내부에서 찾기 어려우니까 결국 바인드 쓸 듯
# 그 전에 만들어졌던 볼륨은 docker volume ls로 확인 가능하다.
# local     5ed8826c36e0484d07f8a3e725731774e5cd43972a6ca5a188af03e75ec3be42
# 이름이 없다...
# 그리고 이 볼륨은 컨테이너가 사라면 같이 사라진다.
# 하지만 익명볼륨이 아주 쓸모없는 것은 아니다.

# named볼륨은 아래 처럼 -v {볼륨이름}:{컨테이너내부경로}
# docker run -p 30001:80 --rm --name feed -v feed:/app/feedback
# 아래처럼 볼륨이름을 체크 할 수 있다.
# local     feed
# 그리고 컨테이너가 사라져도 볼륨은 남는다.
# 그리고 컨테이너를 만들때 해당 볼륨을 선택하면 파일이 그대로 남아있음을 확인할 수 있다.

# 바인드 마운트, 소스코드 개발 반영에도 도움이 된다.
# 즉 "영구적이고 편집 간으한 데이터에 적합하다"
# 이는 도커파일에서 하는 것이 아님 따라서 아래처럼 풀 경로를 이용한다.
# (...) -v (...)/section003_1/app_ref:/app/feedback 1d
# 자 하지만 아래와 같은 에러가 발생한다.
# code: 'MODULE_NOT_FOUND',
# requireStack: [ '/app/server.js' ]
# 그건 KISA 다녀와서~


# server.js app.listen(80); 참고하기
EXPOSE 80

CMD ["node", "server.js"]

# 명령어는 docker build -t feedback-node 
# 이미지 이름을 단다.