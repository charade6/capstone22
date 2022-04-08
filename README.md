# 장지원 [ 201640133 ] - 캡스톤디자인

팀명 : bit<br />
이름 : 장지원 (팀장: WEB+DESIGN)<br />
졸업작품 소개 : <br />

## **[ 졸업작품 소개 ]**

- 작품명 : EVCAR
  <br />
- 개발환경 : React
  <br />
- 작품 소개 : 전기자동차 소개 페이지입니다.

<br/>

## **[ 개발 일지 ]**

### [ 04/06 ]

- 공공데이터 답변이 오지않아 api 사용불가

- Node.js 웹 스크래핑 테스트<br>
  외부모듈 axios, cheerio사용<br>

```jsx
const axios = require('axios');
const cheerio = require('cheerio');

const getHtml = async () => {
  try {
    return await axios.get('https://www.ev.or.kr/portal/carInfoView');
  } catch (error) {
    console.error(error);
  }
};

getHtml()
  .then((html) => {
    // axios 응답 스키마 data는 서버가 제공한 응답(데이터)을 받는다.
    // load()는 인자로 html 문자열을 받아 cheerio 객체 반환
    const $ = cheerio.load(html.data);
    const data = $("#cont_body > div.itemCont > div:nth-child(1) > h4 > p").text();

    return data;
  });
  .then((res) => console.log(res));
```

![result](https://postfiles.pstatic.net/MjAyMjA0MDhfMTA5/MDAxNjQ5NDI4ODYzMjk1.UecpIYfIY7E1xFV0w9tY5xar_nhPmnbKBTlyf5KOIOsg.HtTcxv9IroUBYPQJVjOSmViDmFNjLRlM4SprDksBemYg.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-04-08_222538.png?type=w773)

<br>

페이지 전체 데이터 가져오기 위해 배열로 받음 + 태그 자동으로 찾게함

```jsx
~
const carList = [];
// itemCont 안에 모든 infoBox를 찾아 bodyList에 저장
const bodyList = $("div.itemCont").children("div.infoBox");
// bodyList를 순회하면서 함수 실행
bodyList.each(function(i,item){
  carList[i] = {
    maker: $(this).find("h4 > span").text(),
    title: $(this).find("h4 > p").text(),
    people: $(this).find("dl > dd:nth-child(2)").text(),
    speed: $(this).find("dl > dd:nth-child(3)").text(),
    mileage: $(this).find("dl > dd:nth-child(4)").text(),
    battery: $(this).find("dl > dd:nth-child(5)").text(),
    money: $(this).find("dl > dd:nth-child(6)").text(),
    tel: $(this).find("dl > dd:nth-child(7)").text()
  }
});

return carList;
})
.then((res) => console.log(res));
```

![result2](https://postfiles.pstatic.net/MjAyMjA0MDhfNzAg/MDAxNjQ5NDI4ODc2Nzkw.x7dcQIyt6V9u2iXI8Iu2GixFJpmxU0tQ0xaeUzMt4bcg.YIsqSrf0V7IwOsrOs0dgoXB5AA4rJZjNm1iI7zqIKeQg.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-04-08_165756.png?type=w773)

<br>

불필요한 \n\t 삭제

```jsx
~
const search = '\n|\t';
const replacer = new RegExp(search, 'g');

~

money: $(this).find("dl > dd:nth-child(6)").text().replace(replacer, ''),
```

![result3](https://postfiles.pstatic.net/MjAyMjA0MDhfMTMx/MDAxNjQ5NDI4ODc2Nzg5.5364e4RrXpQ2MM1VtXL4ISnPBt7V9FFoUHCg1YL10SAg.jBhU0YMOX4wHsKrVGHRMioJCrcDsVme0FXxDfjFACE0g.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-04-08_175543.png?type=w773)

---

### [ 03/30 ]

- ❗ 공공데이터포털 전기차 충전소 운영정보 활용신청<br />
  -> 인증키 오류로 인하여 문의접수<br>
  ![공공데이터1](https://postfiles.pstatic.net/MjAyMjA0MDFfMjY1/MDAxNjQ4ODIxMjg1NTg0.e1tuw39zKLycPKqMzLI72b0UUQJRNrxOhArGRtbw6-Qg.KmSvlf16uvzDrB8QcvMEfg0s9t-tv1By4T6irCUu4Ywg.PNG.charade6/1.PNG?type=w773)
  ![공공데이터2](https://postfiles.pstatic.net/MjAyMjA0MDFfMjAw/MDAxNjQ4ODIxMjg1NTQ5.I0aC7_91xlhf2gesqWj3MPxMctNAQAUz0nVmWpRAuKQg.TwrpP2KVBb5aWVP-nSB0me8wDrk5Q5VEoU2er7hNmK8g.PNG.charade6/3.PNG?type=w773)
  ![공공데이터3](https://postfiles.pstatic.net/MjAyMjA0MDFfODIg/MDAxNjQ4ODIxMjg1NTU5.msGnrFGcnKdq3Iv7bbAWTi3APtEr58O6rpmyOHmQG5Yg.qE_H9DXCz7orDZv7BMNzSL4qgywmrcYoYOG20UPfh_cg.PNG.charade6/2.PNG?type=w773)

- 카카오맵 API 키 발급 - 리액트 hook사용하여 테스트 지도 출력

```jsx
import { useEffect } from "react"

const { kakao } = window

const MapContainer = () => {
  useEffect(() => {
    const container = document.getElementById("Map")
    const options = {
      center: new kakao.maps.LatLng(37.4036304411, 126.93015112),
      level: 3,
    }
    const map = new kakao.maps.Map(container, options)
  }, [])

  return <div id="Map" style={{ width: "500px", height: "500px" }}></div>
}

export default MapContainer
```

![map](https://postfiles.pstatic.net/MjAyMjA0MDFfNTcg/MDAxNjQ4ODIzNDMyODY5.drXS5BILYMs62hZpheodqNQHLTzKNk60e_t_kcnn6ngg.GGSPZE_PXZ-I0ULcrgqFw2NS50wxgm-ibDHhxePwfXgg.PNG.charade6/%EC%A0%9C%EB%AA%A9_%EC%97%86%EC%9D%8C.png?type=w773)

<br />

---

### [ 03/23 ]

주제 확정<br />
작품 기술서 작성<br />
로고 제작 - 1번으로 확정<br />
![Logo](https://postfiles.pstatic.net/MjAyMjA0MDJfMjIy/MDAxNjQ4ODI5NzkzMzU5.B4lFh_JzhevasoqiLkG9fl1esaGK1Bm4bWSAIiHHlNsg.93H3AMYt5LpCXMxgCO5K7o0L0_2biUPL0FJpuf755mUg.PNG.charade6/LOGO.png?type=w773)

메인페이지 디자인 - [URL](https://xd.adobe.com/view/33f1e908-2110-4dd0-971a-23997cf09deb-66a2/)

<br>

---

### [ 03/09 ]

작품 주제선정 토의<br />
사용할 프로그램 선정<br />

<details>
    <summary>작품기술서 작성법</summary>
    
    팀규칙 - 규율, 규칙 작성

    작품기획(브레인 스토밍)
    As-Is 현재상태(현재 프로세스)
        - 프로세스 목록 작업(엑셀), 체계도 작성(계층구조), 업무흐름(플로우 차트)

    To-Be 미래상태(이상적인 상황, 개선된 프로세스)
        - 프로세스 목록 작업(엑셀), 체계도 작성(계층구조), 업무흐름(플로우 차트)

    비슷한 사례 - 유사한 프로젝트의 url, 이름, 특징
    예상 장애요인 - 개발시의 문제점 ex>유사사이트를 찾을 수 없음, 스터디가 덜됨

</details>
