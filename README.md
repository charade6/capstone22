# 장지원 [ 201640133 ] - 캡스톤디자인

팀명 : bit<br>
이름 : 장지원 (팀장: FRONTEND + DESIGN)<br>
졸업작품 페이지 : [EVCAR](https://evcar.netlify.app)<br>

## **[ 졸업작품 소개 ]**

- 작품명 : EVCAR<br>
- 개발환경 : React, Css3, Figma, PhotoShop, Illustrator<br>
- 작품 소개 : React hooks를 사용하여 제작한 전기자동차 소개 페이지입니다.

<br>

## **[ 개발 일지 ]**

### [ 05/25 ]

- 검색페이지 제작

```jsx
import { useState } from "react"
import evcarData from "../asset/evcarData.json"

function Search() {
  const tabList = {
    maker: [
      "기아",
      "대창모터스",
      "르노삼성",
      "마이브",
      "벤츠코리아",
      "볼보자동차",
      "스텔란티스코리아",
      "쌍용자동차",
      "쎄뽀모빌리티",
      "에디슨이브이",
      "테슬라",
      "폴스타오토모티브",
      "한국GM",
      "현대",
      "BMW",
    ],
    vehicleScale: ["경형", "소형", "준중형", "중형", "준대형"],
    vehicleType: ["해치백", "SUV", "세단"],
    efficiency: [3, 4, 5, 6, 7],
    subsidy: ["~ 300", "301 ~ 400", "401 ~ 500", "501 ~ 600", "601 ~ 700"],
  }
  // 속성 개수파악
  const tabLength = Object.keys(tabList)
  const [fltArr, setFltArr] = useState([])

  return (
    <div className="sub-page">
      <div className="banner ban2" />
      <div className="main-content">
        <div className="cont-inner">
          <h2>Search</h2>

          <div className="search_cont">
            <div className="cont_detailsearch">
              {tabLength.map((category, index) => {
                let tab
                let catName
                switch (index) {
                  case 0:
                    tab = tabList.maker
                    catName = "제조사"
                    break
                  case 1:
                    tab = tabList.vehicleScale
                    catName = "차급"
                    break
                  case 2:
                    tab = tabList.vehicleType
                    catName = "외형"
                    break
                  case 3:
                    tab = tabList.efficiency
                    catName = "전비"
                    break
                  case 4:
                    tab = tabList.subsidy
                    catName = "보조금"
                    break
                  default:
                    break
                }

                return (
                  <dl className="row" key={index}>
                    <dt>{catName}</dt>
                    <dd>
                      <ul className="itembox">
                        {tab.map((value, i) => {
                          return (
                            <li key={i}>
                              {
                                // 체크박스가 체크 / 해제 되어있을경우 fltArr배열에 추가/삭제
                              }
                              <input
                                type="checkbox"
                                onChange={(e) => {
                                  if (e.currentTarget.checked) {
                                    setFltArr(
                                      fltArr.concat({ category, value })
                                    )
                                  } else {
                                    setFltArr(
                                      fltArr.filter(
                                        (item) => item.value !== value
                                      )
                                    )
                                  }
                                }}
                              />
                              <span>{value}</span>
                            </li>
                          )
                        })}
                      </ul>
                    </dd>
                  </dl>
                )
              })}
            </div>
          </div>
        </div>
      </div>
    </div>
  )
}
```

![searchpage1](https://postfiles.pstatic.net/MjAyMjA1MjlfMjQw/MDAxNjUzODM1Mzg3MjY0.5R4WV-4Hvq2MVRxjrK2Gfo3yY2GhiGz6F4YkYwZr430g.DX4tB58gdcRx028WA8-cUtH5o_-rZfxmPFeQyrIxnjEg.PNG.charade6/%EC%9C%84%ED%83%AD.png?type=w773)

<br>

```jsx
<div className="cont_carList">
  <ul>
    {
      // fltArr 비어있을경우 NoData 출력
    }
    {!fltArr.length ? (
      <div>NO DATA</div>
    ) : (
      evcarData.carList
        .filter((e) => {
          switch (fltArr[0].category) {
            case "maker":
              return e.maker === fltArr[0].value
            case "vehicleScale":
              return e.vehicleScale === fltArr[0].value
            case "vehicleType":
              return e.vehicleType === fltArr[0].value
            case "efficiency":
              return (
                fltArr[0].value <= e.efficiency &&
                e.efficiency < fltArr[0].value + 1
              )
            case "subsidy":
              return (
                parseInt(fltArr[0].value.slice(-3)) - 100 < e.subsidy &&
                e.subsidy <= parseInt(fltArr[0].value.slice(-3))
              )
            default:
              break
          }
        })
        .map((list, index) => {
          return (
            <li className="items" key={index}>
              <div className="item-box">
                <img src={list.img} alt={list.name} />

                <div className="item-detail">
                  <p>
                    {list.maker}
                    &nbsp;&nbsp;
                    {list.name}
                  </p>
                  <div className="spec">
                    <span>
                      {list.vehicleScale}/{list.vehicleType}
                    </span>
                    <span>{list.people}인승</span>
                    <span>최고속도 {list.distance}km/h</span>
                    <span>
                      (상온) {list.mile_roomtmp}km (저온)
                      {list.mile_coldtmp}km
                    </span>
                    <span>복합전비 {list.efficiency}</span>
                    <span>
                      {list.battery_type}({list.capacity}kWh)
                    </span>
                    <br />
                    <span>제조사번호 {list.tel}</span>
                  </div>
                </div>
                <p>
                  보조금
                  <br />
                  {list.subsidy} 만원
                </p>
              </div>
            </li>
          )
        })
    )}
  </ul>
</div>
```

![searchpage](https://postfiles.pstatic.net/MjAyMjA1MjlfOTYg/MDAxNjUzODM1Mzk2MTk3.O8c89guudrdBOMVTfQDeOZ6dBmh69YSzC1HcSvgWgoYg._YJ29N8r0q8IO6p333JCZpZGP2bTyEshaA9X6RzWmVcg.PNG.charade6/%EA%B2%80%EC%83%89%ED%8E%98%EC%9D%B4%EC%A7%80.png?type=w773)

<br>

---

### [ 05/18 ]

- 검색창

```jsx
import { useState, useEffect, useRef } from "react"

function MapContainer({ maps }) {
  const inputRef = useRef()
  const [searchList, setSearchList] = useState(maps)

  const search = () => {
    // input값이 없을경우 전부마킹
    if (!inputRef.current.value) {
      setSearchList(maps)
      return
    }
    const searchValue = searchList.filter(
      (list) => list.addr.indexOf(inputRef.current.value) !== -1
    )
    if (!searchValue.length) {
      alert("검색 결과가 없습니다.")
      return
    }
    setSearchList(searchValue)
  }

  useEffect(() => {
    // 충전소 위치 마킹
    let markers = searchList.map(function (list) {
      let marker = new kakao.maps.Marker({
        position: new kakao.maps.LatLng(list.lat, list.lng),
      })
      let contents =
        "<div style='padding:5px; width:100%; font-size:14px'>" +
        "<b style='font-size:16px'>" +
        list.statNm +
        "</b><br><p>주소 : " +
        list.addr +
        "<br>중전기 타입 : " +
        list.chgerType +
        "<br>충전기 상태 : " +
        list.stat +
        "<br>이용시간 : " +
        list.useTime +
        "<br>기관 연락처 : " +
        list.busiCall +
        "</p></div>"

      let infowindow = new kakao.maps.InfoWindow({
        content: contents,
      })

      // 마커호버이벤트로 변경
      kakao.maps.event.addListener(marker, "mouseover", function () {
        infowindow.open(map, marker)
      })
      kakao.maps.event.addListener(marker, "mouseout", function () {
        infowindow.close()
      })
      return marker
    })

    clusterer.addMarkers(markers)

    // input 값이 있을경우 검색결과 위치로 이동
    if (inputRef.current.value) {
      let bounds = new kakao.maps.LatLngBounds()
      let center = searchList.map(
        (position) => new kakao.maps.LatLng(position.lat, position.lng)
      )
      for (let i = 0; i < center.length; i++) {
        bounds.extend(center[i])
      }
      map.setBounds(bounds)
    }
  }, [searchList])

  return (
    <div className="map_cont">
      <div className="sch_cont">
        <input
          ref={inputRef}
          type="text"
          placeholder="지역명을 입력"
          // 엔터키입력시 검색
          onKeyPress={(e) => {
            if (e.key === "Enter") {
              search()
            }
          }}
        />
        <button onClick={search}>
          <FaSearch size="20" />
        </button>
      </div>
      <div id="Map"></div>
    </div>
  )
}

export default MapContainer
```

![1](https://postfiles.pstatic.net/MjAyMjA1MjJfMjcx/MDAxNjUzMjMxMzUxNzUy.oIIpaFSqt9_Z1a2Fc7n5LxpQD0HiWsTfKFPP7thz50Eg.OxRJTyb8f_hy8C4SQcvRoJs9zIJP-zDYululSHlVPcgg.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-05-22_230311.png?type=w773)

---

### [ 05/11 ]

- 로딩 추가

```jsx
const [isLoading, setIsLoading] = useState(true)

~

return (
    <div className="sub-page">
      <div className="banner ban3" />
      <div className="main-content">
        <div className="cont-inner">
          <h2>Map</h2>
          {isLoading ? (
            <div>
              <h2>로딩중</h2>
            </div>
          ) : (
            <MapContainer props={api} />
          )}
        </div>
      </div>
    </div>
  )
```

- 카카오맵api 이용

```jsx
// 충전소 위치 마킹
for (let i = 0; i < list.length; i++) {
  let marker = new kakao.maps.Marker({
    map: map,
    position: new kakao.maps.LatLng(list[i].lat, list[i].lng),
    clickable: true,
  })
  let infowindow = new kakao.maps.InfoWindow({
    content: "<div style='padding:5px'>주소 : " + list[i].addr + "</div>",
    removable: true,
  })
  marker.setMap(map)

  // 마커 클릭 이벤트
  kakao.maps.event.addListener(marker, "click", function () {
    infowindow.open(map, marker)
  })
}
```

![1](https://postfiles.pstatic.net/MjAyMjA1MTNfOTMg/MDAxNjUyNDUzMzE2NDky.ggHgLaKjJAoDuNeWCu71wg3QAGxR9V5Mc5zxDMKg3X0g.4tyPDpRkkvnU8MsfXRjm4GVQawFR62q0KShHLR9RNs8g.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-05-13_212045.png?type=w773)<br>
![2](https://postfiles.pstatic.net/MjAyMjA1MTNfMTk1/MDAxNjUyNDUzMzExOTgw.qnAvGlpJLNwLcB6oYxXLEhc7n8k4Lpi9okUflmBm7nMg._B5Su9oBz_bCcfTYR_TBfBMEWBYVJpv5lVuiI6w_tDsg.PNG.charade6/sdadasd.png?type=w773)<br>
마커가 너무 많아 클러스터로 변경

```jsx
let clusterer = new kakao.maps.MarkerClusterer({
  map: map,
  averageCenter: true,
  minLevel: 8,
})

let markers = list.map(function (position) {
  let marker = new kakao.maps.Marker({
    position: new kakao.maps.LatLng(position.lat, position.lng),
  })
  kakao.maps.event.addListener(marker, "click", function () {
    let infowindow = new kakao.maps.InfoWindow({
      content: "<div style='padding:5px'>주소 : " + position.addr + "</div>",
      removable: true,
    })
    infowindow.open(map, marker)
  })
  return marker
})

clusterer.addMarkers(markers)

// 클러스터 클릭시 맵 확대
kakao.maps.event.addListener(clusterer, "clusterclick", function (cluster) {
  let level = map.getLevel() - 1
  map.setLevel(level, { anchor: cluster.getCenter() })
})
```

![3](https://postfiles.pstatic.net/MjAyMjA1MTNfNTEg/MDAxNjUyNDUzMzE2NDQ0.wzFhOz25CjA3M9AYNpdE260sJBSBZPCO-hYrL3SWio8g.uKBAwgG03BZNfpyif5gQDT278XYC30kvbSqWf0_yvWIg.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-05-13_232300.png?type=w773)

---

### [ 05/04 ]

- 메인 페이지 완성, 서브페이지 프레임 디자인<br>
  ![1](https://postfiles.pstatic.net/MjAyMjA1MDZfMTIz/MDAxNjUxODQ4NzA0NjI5.nBa6RrGfhdVARZQeW-Ev8iZavdomSVQy0T6_BCH6__Qg.G_gMoCXgmaQHDD0H-qxPq_N5SAhG86n5CCKCUic46i8g.JPEG.charade6/%EC%9B%B9_%EC%BA%A1%EC%B2%98_6-5-2022_231321_localhost.jpeg?type=w773)
  ![2](https://postfiles.pstatic.net/MjAyMjA1MDZfMTM0/MDAxNjUxODQ4NzA0OTYy.3S-Yo9hwPCQoMr2B9cGS__mxAA0z11wSwr4ZSMYEejUg.UMx0hnegmi8Ix9bOPYcoDEPz56Kyiklxv-b9JVZ0EzEg.JPEG.charade6/%EC%9B%B9_%EC%BA%A1%EC%B2%98_6-5-2022_231333_localhost.jpeg?type=w773)
  ![3](https://postfiles.pstatic.net/MjAyMjA1MDZfMjk1/MDAxNjUxODQ4NzA0NzA4.mmkkz1EHLHVGU05r3w41U0oZ6as7h-4Fzr91q0SSLXcg.Uukthn00AXB1Tnusf7eq0YQF4fbnWtZ8w8qkZgVUt-Yg.JPEG.charade6/%EC%9B%B9_%EC%BA%A1%EC%B2%98_6-5-2022_23279_localhost.jpeg?type=w773)

- 카카오맵api 추가<br>

```jsx
import { useEffect } from "react"

const { kakao } = window

const MapContainer = () => {
  useEffect(() => {
    // 카카오맵 컨테이너
    const container = document.getElementById("Map")
    const options = {
      center: new kakao.maps.LatLng(37.57002, 126.97962),
      level: 3,
    }
    const map = new kakao.maps.Map(container, options)

    console.log(map)

    // 내위치 불러오기
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(function (pos) {
        let latitude = pos.coords.latitude
        let longitude = pos.coords.longitude
        let accuracy = pos.coords.accuracy
        let level = map.getLevel()
        let locPosition = new kakao.maps.LatLng(latitude, longitude)
        // 위치정확도가 낮을경우 맵레벨 증가
        if (accuracy > 80) {
          map.setLevel(
            (level += Math.round(Math.log(accuracy / 50) / Math.LN2))
          )
          console.log(Math.round(Math.log(accuracy / 50) / Math.LN2))
        }
        displayMarker(locPosition)
      })
    }
    function displayMarker(locPosition) {
      let marker = new kakao.maps.Marker({
        map: map,
        position: locPosition,
      })
      marker.setMap(map)
      map.setCenter(locPosition)
    }

    // 줌 컨트롤
    let zoomControl = new kakao.maps.ZoomControl()
    map.addControl(zoomControl, kakao.maps.ControlPosition.BOTTOMRIGHT)

    // 로드뷰
    // map.addOverlayMapTypeId(kakao.maps.MapTypeId.ROADVIEW)

    // 마커생성
    // let marker = new kakao.maps.Marker({
    //   positions: new kakao.maps.LatLng(latArray[i], longArray[i]),
    // })
  }, [])

  return <div id="Map"></div>
}

export default MapContainer
```

![map](https://postfiles.pstatic.net/MjAyMjA1MDZfMTg3/MDAxNjUxODQ4NzA1MzQ1.zwFBhGhHRRzPJBdK1dMZc08g2HySiUKPLneJItECI-Ig._lgts-kOoAlO6kjU1oZBVMDWwiUiTrC9EMTE_MgSWn8g.JPEG.charade6/%EC%9B%B9_%EC%BA%A1%EC%B2%98_6-5-2022_232724_localhost.jpeg?type=w773)

- 공공데이터 api 불러오기<br>

```jsx
async function getApi() {
  await axios({
    method: "get",
    url: `http://apis.data.go.kr/B552584/EvCharger/getChargerInfo?serviceKey=gIgWSDzCeTDpTHNna3UfLVrfBmHbLPDu8IRh%2FvJuoHy5Sp1OFCc9r6uWHIqcEpCF8pWmul9zZMDQLafiKcrx3Q%3D%3D&pageNo=1&numOfRows=10`,
  }).then(function (res) {
    const dataSet = res.data
    console.log(dataSet)
  })
}
useEffect(() => {
  getApi()
}, [])
```

![err](https://postfiles.pstatic.net/MjAyMjA1MDZfMjM2/MDAxNjUxODQ4NzQ5ODk3.rvvqtddm8ZreDBl9SkkafENxw-lew_z05M4NyofgBT8g.j5C_PhN6wJ8GJlJVpg43phsOuoit6Uu3axxzMe0xHckg.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-05-06_232912.png?type=w773)

❗ cors 오류<br>
리액트 앱의 주소와 공공데이터 API의 주소는 다르기때문에<br>
브라우저의 cors 정책에 따라서 공공데이터를 가져오는 것이 허용되지 않는다.
=> package.json에 proxy 추가

```jsx
{
  ~
  },
  "proxy": "http://apis.data.go.kr/"
}
```

```jsx
// url 변경
async function getApi() {
  await axios({
    method: "get",
    url: `/B552584/EvCharger/getChargerInfo?serviceKey=gIgWSDzCeTDpTHNna3UfLVrfBmHbLPDu8IRh%2FvJuoHy5Sp1OFCc9r6uWHIqcEpCF8pWmul9zZMDQLafiKcrx3Q%3D%3D&pageNo=1&numOfRows=10`,
  }).then(function (res) {
    const dataSet = res.data
    console.log(dataSet)
  })
}
```

![s](https://postfiles.pstatic.net/MjAyMjA1MDZfOTQg/MDAxNjUxODQ4NzUzODUx.ui95l8fWSiEYVbbGnrnk3QVMyeBR8WkcUdT4bVI0K2sg.K1xjVTPo40QMBr1jf2tZPMVv9Nk-ufCPvJN2_eq9-Ewg.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-05-06_233440.png?type=w773)

<br>

---

### [ 04/27 ]

- repository 생성, 기본 틀 제작

- 버튼 필터 연습<br>

```jsx
import { useState } from "react"
import JsonTest from "./JsonTest"

function App() {
  // state 설정
  const [input, setInput] = useState("")

  // input이 바뀔때마다 state에 저장
  const handleChange = (e) => {
    setInput(e.target.value)
  }

  return (
    <div className="App">
      <div>
        <div>
          <input type="radio" name="param" value="기아" onChange={handleChange} />
          기아
          <br />
          <input type="radio" name="param" value="대창모터스" onChange={handleChange} />
          대창모터스
        </div>
        // state값 전달
        <JsonTest param={input} />
      </div>
    </div>
  )
}

export default App

-------

function JsonTest({ param }) {
  // 전달값과 비교하여 필터
  const makerFilter = carData.carList.filter(
    (carList) => carList.maker === param
  )

  return (
    <div>
      <ul>
        // 필터의 값이 없으면 No Data 출력
        {makerFilter.length === 0 ? (
          <h2>No Data</h2>
        ) : (
          // 필터값이 있을경우 출력
          makerFilter.map((carList, index) => (
            <li key={index}>
              {carList.maker}
              <ul>
                <div>
                  <img src={carList.img} alt="carimg" width="250px" height="150px" />
                </div>
                <div>
                  <li>{carList.name}</li>
                  <li>승차인원 : {carList.people}인승</li>
                  <li>최고속도출력 : {carList.distance}km/h</li>
                  <li>
                    1회충전주행거리 : (상온){carList.mile_roomtmp}km (저온)
                    {carList.mile_coldtmp}km
                  </li>
                  <li>
                    배터리 : {carList.battery_type}({carList.capacity}kWh)
                  </li>
                  <li>국고보조금 : {carList.money}만원</li>
                  <li>제조사번호 : {carList.tel}</li>
                </div>
              </ul>
            </li>
          ))
        )}
      </ul>
    </div>
  )
}

export default JsonTest
```

![filter1](https://postfiles.pstatic.net/MjAyMjA0MjlfMTAg/MDAxNjUxMjEzMTE3OTU0.adyghzwJni-Tq5u_3nQIyz1lOU8CqDC5OejBCbmb-yMg.vdbQPvfe7li5P_y4nMasIp5pJhYDS3umPg3U6J8n2Cwg.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-04-29_151756.png?type=w773)
![filter2](https://postfiles.pstatic.net/MjAyMjA0MjlfMjIg/MDAxNjUxMjEzMTE3OTkw.lTj_KKCrRSUYUMC3cVBtkTmvos76GvuvNMPwIqhICCsg.aLiUj8_ISy_w1oWJL5O-f8JJ9Vz8WPxxazaZXQlheSMg.PNG.charade6/%ED%99%94%EB%A9%B4_%EC%BA%A1%EC%B2%98_2022-04-29_151820.png?type=w773)

- youtube 백그라운드 넣기<br>
  iframe 사용 -> react-youtube-background로 변경

```jsx
import YoutubeBackground from "react-youtube-background"
import Header from "./Header"
import Footer from "./Footer"

function Home() {
  return (
    <div>
      <Header />
      <YoutubeBackground videoId="gONEwEUVr-s" className="youtube" />
      <div className="section sec1">
        <div className="sec-inner">hello</div>
      </div>
      <div className="section sec2">
        <div className="sec-inner">hello</div>
      </div>
      <div className="section sec3">
        <div className="sec-inner">hello</div>
      </div>
      <Footer />
    </div>
  )
}

export default Home
```

- 브라우저 라우터 사용

```jsx
import Home from "./component/Home"
import Info from "./component/Info"
import Search from "./component/Search"
import Map from "./component/Map"
import { BrowserRouter, Route, Switch } from "react-router-dom"

function App() {
  return (
    <div className="App">
      <BrowserRouter>
        <Switch>
          <Route exact path="/" component={<Home />} />
          <Route path="/info" component={<Info />} />
          <Route path="/search" component={<Search />} />
          <Route path="/map" component={<Map />} />
        </Switch>
      </BrowserRouter>
    </div>
  )
}

export default App
```

```
❗오류
export 'Switch' (imported as 'Switch') was not found in 'react-router-dom' (possible exports: BrowserRouter, HashRouter, Link, MemoryRouter, NavLink, Navigate, NavigationType, Outlet, Route, Router, Routes, UNSAFE_LocationContext, UNSAFE_NavigationContext, UNSAFE_RouteContext, createPath, createRoutesFromChildren, createSearchParams, generatePath, matchPath, matchRoutes, parsePath, renderMatches, resolvePath, unstable_HistoryRouter, useHref, useInRouterContext, useLinkClickHandler, useLocation, useMatch, useNavigate, useNavigationType, useOutlet, useOutletContext, useParams, useResolvedPath, useRoutes, useSearchParams)
```

react-router-dom 버전 변경으로 Switch 사용불가 -> [공식문서](https://reactrouter.com/docs/en/v6/upgrading/v5) 참고하여 수정

```jsx
function App() {
  return (
    <div className="App">
      <BrowserRouter>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/info" element={<Info />} />
          <Route path="/search" element={<Search />} />
          <Route path="/map" element={<Map />} />
        </Routes>
      </BrowserRouter>
    </div>
  )
}
```

<br>

---

### [ 04/13 ]

- 공공데이터 API 사용불가로 비슷한 API로 대체<br>
  한국전력공사*전기차 충전소 운영정보 -> [한국환경공단*전기자동차 충전소 정보](https://www.data.go.kr/tcs/dss/selectApiDataDetailView.do?publicDataPk=15076352)

  <br>

- 페이지 디자인 - 각자 XD로 디자인<br>
  지원:![bad1](https://postfiles.pstatic.net/MjAyMjA0MTVfMTc2/MDAxNjUwMDI4NTgyOTI4.-NXJ39ukEYHYzcR2CB_9topXVknA0hes1dlwgQd925sg.dcJDUWPvm_j2bFDE4xKrx-OIKMlhmKlhcrzeaxFr2tAg.JPEG.charade6/%EC%9B%B9_1920_%E2%80%93_1_page-0001.jpg?type=w773)
  <br>
  도현:![bad2](https://postfiles.pstatic.net/MjAyMjA0MTVfODEg/MDAxNjUwMDI4NTY1NzI5.Uu_2ZtzOqlQTyiUXdrixRmX4d1Fpfozzki_-KN33GGwg.rMvzXm_PuuuWe2gB2FBFCQ6cq3FIhRYezLqGX0plgrYg.JPEG.charade6/0005.jpg?type=w773)
  <br>
  디자인이 마음에 들지 않아 XD -> Figma 디자인툴 변경<br>
  실시간 협업하여 디자인 새로 제작<br>
  [Figma링크](https://www.figma.com/file/9dfq1s8VXeOC4mYkayag2z/evcar?node-id=1%3A2)<br>
  ![mainp](https://postfiles.pstatic.net/MjAyMjA0MTVfMTAg/MDAxNjUwMDI4NTg0NzU0.1RYroS1-ZErfOSXPhAGnH_OzYE7tFgSXTlg26YbxIdgg.KlybjauM2dSjwmg8jwK9u6nkms2iKacet6hqQjIFjscg.JPEG.charade6/0001.jpg?type=w773)
  메인페이지 완성, 서브페이지 와이어프레임만 제작

<br>

- axios + cheerio -> 정적페이지 크롤링만 가능<br>
  크롤링하려는 [페이지](https://www.ev.or.kr/portal/carInfoView)가 post형식으로 출력하므로 <br>
  주소가 바뀌지않아 cheerio로 크롤링 불가<br>
  -> 배포페이지변경, selenium모듈 학습필요<br>
  -> json파일 직접입력하여 출력하는것으로 변경<br>

```jsx
{
    "carList": [
        {
            "name": "EV6 롱레인지 2WD 19인치(2022년)",
            "maker": "기아",
            "people": 5,
            "distance": 185,
            "mile-roomtmp": 483,
            "mile-coldtmp": 446,
            "battery": 77.506,
            "money": 700,
            "tel": "080-200-2000"
        },
        {
            "name": "EV6 롱레인지 2WD 20인치(2022년)",
            "maker": "기아",
            "people": 5,
            "distance": 185,
            "mile-roomtmp": 445,
            "mile-coldtmp": 411,
            "battery": 77.506,
            "money": 700,
            "tel": "080-200-2000"
        },
        {
            "name": "EV6 롱레인지 4WD 19인치(2022년)",
            "maker": "기아",
            "people": 5,
            "distance": 188,
            "mile-roomtmp": 458,
            "mile-coldtmp": 414,
            "battery": 77.506,
            "money": 700,
            "tel": "080-200-2000"
        },
        {
            "name": "EV6 롱레인지 4WD 20인치(2022년)",
            "maker": "기아",
            "people": 5,
            "distance": 188,
            "mile-roomtmp": 407,
            "mile-coldtmp": 380,
            "battery": 77.506,
            "money": 700,
            "tel": "080-200-2000"
        },
        {
            "name": "EV6 스탠다드 2WD 19인치(2022년)",
            "maker": "기아",
            "people": 5,
            "distance": 185,
            "mile-roomtmp": 377,
            "mile-coldtmp": 347,
            "battery": 58.124,
            "money": 700,
            "tel": "080-200-2000"
        },
        {
            "name": "EV6 스탠다드 4WD 19인치(2022년)",
            "maker": "기아",
            "people": 5,
            "distance": 188,
            "mile-roomtmp": 362,
            "mile-coldtmp": 324,
            "battery": 58.124,
            "money": 700,
            "tel": "080-200-2000"
        },
        {
            "name": "니로(HP)(2022년)",
            "maker": "기아",
            "people": 5,
            "distance": 167,
            "mile-roomtmp": 385,
            "mile-coldtmp": 348.5,
            "battery": 64.08,
            "money": 700,
            "tel": "080-200-2000"
        }
    ]
}
```

---

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

- ❗ 공공데이터포털 전기차 충전소 운영정보 활용신청<br>
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

<br>

---

### [ 03/23 ]

주제 확정<br>
작품 기술서 작성<br>
로고 제작 - 1번으로 확정<br>
![Logo](https://postfiles.pstatic.net/MjAyMjA0MDJfMjIy/MDAxNjQ4ODI5NzkzMzU5.B4lFh_JzhevasoqiLkG9fl1esaGK1Bm4bWSAIiHHlNsg.93H3AMYt5LpCXMxgCO5K7o0L0_2biUPL0FJpuf755mUg.PNG.charade6/LOGO.png?type=w773)

메인페이지 디자인 - [URL](https://xd.adobe.com/view/33f1e908-2110-4dd0-971a-23997cf09deb-66a2/)

<br>

---

### [ 03/09 ]

작품 주제선정 토의<br>
사용할 프로그램 선정<br>

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
