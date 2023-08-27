## FindVibe - 사진 촬영 위치 예측 웹 서비스
 
[2023년 1학기] 광운대학교 참빛설계학기에 참여했던 창의융합형 웹 프로젝트입니다.
</br>
</br>
진행 기간 : 2023.03 ~ 2023.06

## 핵심기능
![111](https://github.com/KW-FINDVIBE/FINDVIBE/assets/84065412/67b0cddc-eff3-4647-b9cf-200d1352a7c4)
풍경 사진을 딥러닝 모델로 분석하여 사진 촬영 예상 위치정보를 제공하는 것 입니다.

## 나의 기여
### ◽client : typesctipt

<details>
<summary><b>api 요청 함수</b></summary>
<div markdown="1">
  </br>
  
  > client에서 server로 api 요청을 보내는 함수를 정의하고 모듈처럼 사용할 수 있도록 조치함.

  - [client API 폴더](https://github.com/HungKungE/FINDVIBE/tree/main/client/src/API)

  ```sh
  // post 요청으로 api 요청을 통일함. path = api url임.
  export const sendPostRequest = async (path: string, sendData: any | null) => {
   const response = await axios.post(path, sendData);
   return response.data;
  };
  ```
  ```sh
  // multipart를 사용해서 server에 파일을 전달할 때 사용함.
  export const sendMultipartRequest = async (
    path: string,
    formDatas?: File[]
  ) => {
    const form = new FormData();

  if (!formDatas) {
    console.log("no ImgFiles!");
    return;
  }

    formDatas.forEach((formData, i) => {
      form.append("image", formData, "image" + i);
    });
  
    try {
      const response = await axios({
        method: "POST",
        url: `${path}`,
        /* 아래와 같이 헤더 설정하면 boundary가 빠져서 서버가 에러를 뱉어낸다
        headers: {
          "Content-Type": "multipart/form-data",
        },
        */
        data: form,
      });
  
      return response.data;
    } catch (error) {
      console.error(error);
    }
  };
  ```
  
  </br>
</div>
</details>

### ◽server(nodeJs - JavaScript)
- nodeJs Express server : server 구현, Auth/User/Predict/File api의 개발
<details>
<summary><b>routing 기능</b></summary>
<div markdown="1">
  </br>
  
  > api url에 따라서 해당 api로 라우팅 하는 기능을 구현했다.

  - [route](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/route/route.js)

  api 엔드 포인트를 등록함으로써 routing기능을 구현했다.
  </br>
  예를 들어, api url = "/auth/login"이면 login api로 라우팅된다.

  ### 개발 내용 ( 클릭 시, 상세 설명 페이지로 이동 )
  | 종류 | 개발 내용 |
  | ----- | ----- |
  | [user](https://github.com/HungKungE/FINDVIBE/tree/main/server/src/user)| 회원 가입, 닉네임 중복 확인, 닉네임 수정, 비밀번호 수정 api.|
  | [predict](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/predict) | 예측 요청(python, google) api |
  | [file](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/file) | 로컬 저장소의 이미지 파일 제공 api |
  
  </br>
  
</div>
</details>

<details>
<summary><b>database connect</b></summary>
<div markdown="1">
  </br>
  > ORM 중 하나인, sequelize를 사용하여 MySQL database에 쿼리문을 직접 사용하지 않고 접근하는 기능 구현.

  - [connect 폴더](https://github.com/HungKungE/FINDVIBE/tree/main/server/src/connect)

  sequelize를 사용함으로써 다음과 같은 이점을 얻을 수 있었다.

  ```sh
  1. ORM을 통해 직접 쿼리문을 짜지 않아도 database에 접근할 수 있다.
  2. 서버에서는 쿼리문을 사용하지 않기 때문에, SQL 삽입 공격 같은 보안과 관련된 위험이 감소한다.
  3. db 접근 코드를 재사용하기 쉬워진다.
  ```
  </br>
</div>
</details>

<details>
<summary><b>auth api</b></summary>
<div markdown="1">
  </br>
  > 사용자의 인증 관련 기능으로서 jwt와 session저장소를 사용한 로그인 유지가 핵심이다.
  
  #### 개발 목록
  | api | 기능 |
  |-----|-----|
  |[logIn](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/auth/api/login.js)|로그인|
  |[logOut](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/auth/api/logout.js)|로그아웃|
  |[check](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/auth/api/check.js)|session에 저장된 token 유효성을 검사하고 갱신한다.|
  |[sessionAuth](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/api/sessionAuth.js)|api 요청 시, session에 저장된 token을 확인하여 해당 사용자의 유효성을 검사한다.|

  #### session을 사용한 이유

  ```sh
  일반적인 웹 사이트는 다음 2가지 방식 중에 하나를 사용하여 사용자의 유효성 검사를 한다.
  1. jwt ( access token, refresh token )
    - access token : client cookie에 저장
    - refresh token : server에 저장. access token 재발행 시 사용함.
  2. session storage
    - 로그인 한 사용자의 정보를 session storage에 저장.

  프로젝트 초반에는 jwt 인증 방식을 사용했다.

  - client에는 사용자의 기본정보(nickname, email 등)를 가진 access token을 cookie를 저장한다.
  - server에는 token 재발행에 사용할 수 있는 사용자의 핵심정보(db table의 user_id 등)를 가진 refresh token을 저장한다. 

  그래서 server에 api요청을 할 때, access token을 담은 cookie도 header에 담아 전송하여 사용자 인증을 하는 방식이었다.

  그런데, client에서 이 access token을 가진 cookie의 탈취의 가능성이 있다는 문제점을 알게 되었다.
  httpOnly 설정과 refresh token의 도입으로 어느정도 해결할 수 있다고는 하지만 결국 불안은 남아 있다고 생각했다.

  따라서 아예 탈취 관련 생각을 하지 않도록, server session storage 방식을 사용하는 것으로 변경했다.
  사용자의 기본정보를 담던 access token의 역할은 react 상태 관리 모듈 중 하나인 Zustand를 통해 대체했다.
  그리고 session storage에 사용자 정보를 암호화한 token을 저장하고, api요청이 올 때마다 session의 token 유효성을 검사하는 방식으로 사용자 인증 기능을 구현했다.

  한 편, session 저장소는 MongoDB를 사용했다.
  원래는 new session.MemoryStore()를 session 저장소로 사용했으나, 이는 메모리를 저장소로 사용하는 방식이므로
  서버를 재가동 시키면 초기화되기 때문에 로그인 중인 사용자들의 로그인정보가 모드 초기화 되므로 다시 로그인 해야한다는 단점이 있다.
  따라서 개발할 때는 MemoryStore를 사용했고, 배포할 때는 session 저장소로 mongoDB를 사용하여 서버를 재가동시켜도 데이터가 초기화되지 않도록 했다.

  따라서 현재 웹 사이트의 사용자 인증은 다음과 같이 진행된다.
  - 로그인 성공 -> 사용자의 정보를 token으로 만들고 mongo db에 저장. 사용자 기본 정보를 client로 전송 ( Zustand에서 이를 관리 )
  - api 요청 -> session의 token 유효성 검사
  - 로그인 후, 일정시간이 지나면 자동으로 token을 갱신함.
  - 로그 아웃 시, session storage에서 token 삭제함.
  ```
  </br>
</div>
</details>

<details>
<summary><b>user api</b></summary>
<div markdown="1">
  </br>
  
  > 사용자 정보 관련 api이다.

  #### 개발 목록
  | api | 기능 |
  |-----|-----|
  |[signUp](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/user/api/signup.js)|회원가입 api. 비밀번호 암호화에 crypto 모듈을 사용한다. |
  |[CheckNickname](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/user/api/check_nickname.js)|닉네임 중복성을 확인한다.|
  |[UpdateNickname](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/user/api/update_nickname.js)|닉네임을 갱신한다.|
  |[UpdatePassword](https://github.com/HungKungE/FINDVIBE/blob/main/server/src/user/api/update_password.js)|비밀번호를 수정한다.|

  #### Crypto 모듈 사용 이유
  ```sh
  암호화는 크게 단방향 암호화와 양방향 암호화로 나뉜다.
  |종류|암호화|복호화| 방식 |
  |단방향|O|X| 해싱 |
  |양방향|O|O| 대칭키, 비대칭키|

  1. 단방향 암호화 : 해시
  nodeJs server에서 비밀번호 암호화에 사용하는 대표적인 모듈은 bcrypt와 crypto이다.

  두 모듈은 단방향 암호화가 가능하나, bcrypt는 좀 더 보안이 강력한 해시 함수 기능을 제공한다.
  bcrypt : 비밀번호 해싱에 사용. 절차는 다음과 같다.
  1. salt 생성 : 무작위 문자열 salt를 생성하여 비밀번호와 결합한다.
  2. hash 생성: salt로 해시 함수를 실행하여 hash를 생성한다.
  3. hash 비교 : 입력 hash와 저장된 hash를 비교함.

  또한 bcrypt의 특징은 다음과 같다.
  1. 단방향 암호화
  2. 레인보우 테이블 방지
     - salt를 사용하여 같은 비밀번호를 암호화 하더라도 다른 결과가 나옴.
     - 따라서 암호화된 비밀번호들을 보고 암호화 방식을 유추할 수 없음.
  3. 무거운 해싱함수 사용
     - 무차별 대입 공격에 유리함.
     - 대신, 다른 해싱함수보다 느리기 때문에 낮은 성능을 보여줌.
  
  따라서 bcrypt가 낮은 성능을 보여주기도 하고, 무거운 모듈이기 때문에
  상대적으로 가볍고 빠른 crypto의 pbkdf2함수를 사용하기로 결정했다.

  pbkdf2함수도 bcrypt처럼 salt를 사용하여 해싱하므로 레인보우 테이블 방지가 가능하며 보안성이 높다.
  그러나, bcrypt보다 빠르기 때문에 이번 프로젝트에서 사용했다.

  단점이 있다면 비밀번호 복호화를 할 수 없어서 사용자가 비밀번호를 잊어버린다면
  비밀번호 변경 기능을 사용해야 하므로, 사용자는 불편함을 느낄 수 있다.

  ```
  </br>
</div>
</details>

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



