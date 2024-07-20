# 우선 몽고DB 이미지가 필요할 것이다.
# docker pull mongo
FROM node

WORKDIR /app

COPY def_app/package.json .

RUN npm install

COPY def_app/ .

CMD ["node", "app.js"]
# 시작하기전 app.js 에서
# mongoose.connect(
#   'mongodb://localhost:27017/swfavorites',
# 의 부분에서 몽고DB가 먼저 필요하다는 결론을 내림

####################
## 네트워크 추가
####################
# docker run -d --name mongodb_cont --work chomnet mongo
#   Err > docker: Error response from daemon: network chomnet not found.
# 네트워크는 자동으로 만들어지지 않는다.
# docker network create chomnet
# docker run -d --name mongodb_cont --work chomnet mongo
# docker run --rm --network chomnet 
# 모두 연결완료 !

