## FindVibe - 사진 촬영 위치 예측 웹 서비스
 
[2023년 1학기] 광운대학교 참빛설계학기에 참여했던 창의융합형 웹 프로젝트입니다.
</br>
</br>
진행 기간 : 2023.03 ~ 2023.06

## 핵심기능
![111](https://github.com/KW-FINDVIBE/FINDVIBE/assets/84065412/67b0cddc-eff3-4647-b9cf-200d1352a7c4)
풍경 사진을 딥러닝 모델로 분석하여 사진 촬영 예상 위치정보를 제공하는 것 입니다.

## 나의 기여
### ◽client

<details>
<summary><b>api 요청 함수</b></summary>
<div markdown="1">
  </br>
  
  > client에서 server로 api 요청을 보내는 함수를 정의하고 모듈처럼 사용할 수 있도록 조치함.

  - [client API 코드](https://github.com/HungKungE/FINDVIBE/tree/main/client/src/API)

  ### 사용 skills
  <div>
    <img src="https://img.shields.io/badge/Typescript-3178C6?style=for-the-badge&logo=typescript&logoColor=white">
  </div>
  </br>
</div>
</details>


### ◽server(nodeJs)

<details>
<summary><b>Hd note</b></summary>
<div markdown="1">
  </br>
  
  > main server인 nodeJs Express server를 구현했다.

  ### 사용 skills
  <div>
    <img src="https://img.shields.io/badge/Typescript-3178C6?style=for-the-badge&logo=typescript&logoColor=white">
  </div>

  ### 개발 내용 ( 클릭 시, 상세 설명 페이지로 이동 )
  | 종류 | 개발 내용 |
  | ----- | ----- |
  | [route](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/route) | api url에 따라서 해당 api로 라우팅하는 기능 |
  | [connect](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/connect) | MySQL DB에 sequelize라는 ORM을 통해 연결함. |
  | [sessionAuth](https://github.com/HungKungE/FINDVIBE/tree/main/server/src/api) | 로그인 시 서버 session에 저장된 user data를 통해서 api 요청할 때마다 session 유효성 확인하고 갱신함.|
  | [auth](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/auth)| 로그인, 로그아웃, 유효성 확인 api.|
  | [user](https://github.com/HungKungE/FINDVIBE/tree/main/server/src/user)| 회원 가입, 닉네임 중복 확인, 닉네임 수정, 비밀번호 수정 api.|
  | [predict](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/predict) | 예측 요청(python, google) api |
  | [file](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/file) | 로컬 저장소의 이미지 파일 제공 api |
  
  </br>
  
</div>
</details>

- nodeJs Express server : server 구현, Auth/User/Predict/File api의 개발
- 
### ◽server(python)

<details>
<summary><b>Hd note</b></summary>
<div markdown="1">
  </br>
  
  > client에서 server로 api 요청을 보내는 함수를 정의하고 모듈처럼 사용할 수 있도록 조치함.

  - [client API 코드](https://github.com/HungKungE/FINDVIBE/tree/main/client/src/API)

  ### 사용 skills
  <div>
    <img src="https://img.shields.io/badge/Typescript-3178C6?style=for-the-badge&logo=typescript&logoColor=white">
  </div>
  </br>
</div>
</details>

### ◽devOps

<details>
<summary><b>Hd note</b></summary>
<div markdown="1">
  </br>
  
  > client에서 server로 api 요청을 보내는 함수를 정의하고 모듈처럼 사용할 수 있도록 조치함.

  - [client API 코드](https://github.com/HungKungE/FINDVIBE/tree/main/client/src/API)

  ### 사용 skills
  <div>
    <img src="https://img.shields.io/badge/Typescript-3178C6?style=for-the-badge&logo=typescript&logoColor=white">
  </div>
  </br>
</div>
</details>

- DB : MySQL(사용자 정보, 예측 요청 Log DB), MongoDB(사용자 로그인 시, session 저장)
- ORM : sequelize 모듈을 사용해서 직접 쿼리를 보내지 않고 DB에 접근.
- python Flask server : server 구현, predict api의 뼈대 추가
- aws : EC2 인스턴스를 통한 프로젝트 배포 

## 기대효과
- 사진 촬영을 좋아하는 사람들에게 사진의 구도 정보 제공
- 과거에 찍은 사진이 어디에서 찍은 사진인지 알고 싶으나 기억 나지 않을 경우에 위치 정보 제공
- EXIF가 훼손되더라도 위치 좌표 제공 가능

## 한계점
- Google Street View Dataset 특성 상 도로, 하늘, 빌딩 등 겹치는 부분이 많아서 특징점을 추출하는 것이 어려움.
- 사진이 찍힌 위치를 찾기 위해서 방대하고 구체적인 학습데이터가 필요함.

## 프로젝트 자료

- [시연 영상](http://kwcommons.kw.ac.kr/contents4/KW10000001/64896357631b9/contents/media_files/mobile/ssmovie.mp4)
- [API 명세서](https://docs.google.com/spreadsheets/d/1DEYCQ8lVnwUwPz7ZZM6YwL8G-L1OYarQ5-RsWXsN6Og/edit#gid=0)
- [서버 구조](https://github.com/KW-FINDVIBE/FINDVIBE/assets/84065412/5adb4014-6c45-4460-b0d7-c126425b8ed1)
- [페이지 디자인](https://www.figma.com/file/0dwcJ9wtviXQ7Uq0y2Uems/%EC%B0%B8%EB%B9%9B%EC%84%A4%EA%B3%84?type=design&node-id=0-1&mode=design)

## 프로젝트 세팅 (clone 이후)
1. upload_images 폴더 추가 : 사용자가 전송한 사진을 저장하는 dir
2. client, server, sql 폴더의 readme.md를 참고해서 세팅

## 참여 인원 (3명)
- 이근성 : Front-End, 팀장
- 박상찬 : Back-End
- 김형석 : Deep Learning

## 사용 Skills
### Front-End
<div>
  <img src="https://img.shields.io/badge/react-61DAFB?style=for-the-badge&logo=react&logoColor=black">
  <img src="https://img.shields.io/badge/Typescript-3178C6?style=for-the-badge&logo=typescript&logoColor=white">
  <img src="https://img.shields.io/badge/tailwindcss-F7DF1E?style=for-the-badge&logo=tailwindcss&logoColor=white">
</div>

### Back-End
<div>
  <img src="https://img.shields.io/badge/Express-339933?style=for-the-badge&logo=Node.js&logoColor=white">
  <img src="https://img.shields.io/badge/Flask-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/aws-232F3E?style=for-the-badge&logo=amazon&logoColor=white">
  <img src="https://img.shields.io/badge/docker-2496ED?style=for-the-badge&logo=docker&logoColor=white">
  <img src="https://img.shields.io/badge/mysql-4479A1?style=for-the-badge&logo=mysql&logoColor=white">
  <img src="https://img.shields.io/badge/mongoDB-47A248?style=for-the-badge&logo=MongoDB&logoColor=white">
</div>

### Deep Learning

<div>
  <img src="https://img.shields.io/badge/Flask-3776AB?style=for-the-badge&logo=python&logoColor=white">
  <img src="https://img.shields.io/badge/tensorflow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white">
</div>

## 프로젝트 후기
이번 프로젝트는 처음부터 끝까지 혼자서 서버를 구현하고, 각종 api를 개발했다는 점에서
백엔드 개발자로서 역량을 크게 늘릴 수 있었다.
</br>
회원 가입 기능을 개발할 때, 깜빡하고 비밀번호 암호화를 하지 않고 문자열 그대로 DB에 저장해서
테러리스트라는 말을 들었다. 반성하고 암호화에 crypto 모듈을 사용한 경험이 가장 기억에 남는다.
그 이외에는 multipart를 통해서 client에서 img file을 전송받는 기능 구현, python server에 img file 전송하고 예상 결과 반환받는 기능 구현 경험은
추후에 다른 협업 프로젝트 진행 시에 도움이 될것 같다.



