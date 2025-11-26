# 🧶 **Weave 위브**

> <span style="color:green"><b>문해력과 사고력을 엮다, AI 기반 문해력 향상 서비스</span></b>

<img width="800" height="450" alt="weave 소개" src="https://github.com/user-attachments/assets/81d134df-94bb-4a64-a2cd-3f704711a747" /><br>
Weave는 현대인의 저하된 문해력과 집중력 향상을 돕는 AI 학습 서비스로, 
사용자의 요약과 AI의 피드백을 하나의 사이클로<br>학습을 진행합니다.

## 프로젝트 정보
<table><tr><td><b>기획 및 개발 기간</b></td><td>2025.10.22 - 2025.10.31</td></tr>
<tr><td><b>참가 대회</b></td><td>2025 Sumtech 해커톤</td> </tr> </table>

## 프로젝트 소개

### 배경
최근 숏폼 콘텐츠 소비가 늘어나면서 즉각적이고 자극적인 자극에만 반응하는 <b>팝콘 브레인</b>과 이로 인한 <b>문해력 저하</b>가<br> 사회적 문제로 대두되고 있습니다. Weave는 이러한 문제에 맞서 독해력과 어휘력 증진을 돕고자 합니다.

<img width="800" height="450" alt="서비스소개" src="https://github.com/user-attachments/assets/48d1f34f-e53a-4725-ad2c-b1369560da82" />
<img width="800" height="450" alt="서비스소개" src="https://github.com/user-attachments/assets/3d03ba15-a929-4691-9fa5-bf09fce4cc5c" />
<img width="800" height="450" alt="서비스소개" src="https://github.com/user-attachments/assets/ab274453-245a-458c-a262-d097970dec66" />
<img width="800" height="450" alt="서비스소개" src="https://github.com/user-attachments/assets/0a1b6c4a-9629-42bd-9129-0f4ee2e2646e" />
<img width="800" height="450" alt="서비스소개" src="https://github.com/user-attachments/assets/da6860c0-adcc-49f1-9917-65059aeac40c" />

### 기대 효과
<table>
    <tr>
    <td width="33%" align="center">
    <h4>문해력 향상</h4> 긴 글을 읽고 핵심을 파악하는<br>훈련을 통해 집중력을 향상합니다.</td>
    <td width="33%" align="center">
    <h4>자투리 시간 활용</h4> 요약-피드백의 단순한 학습<br>사이클로 구성되어 시간 부담이 적습니다.</td>
    <td width="33%" align="center"> <h4>요약 피드백</h4> AI의 피드백을 통해<br>독해력과 어휘력을 점검합니다.</td>
    </tr>
</table>

## 기술적 고려사항
<img width="800" height="450" alt="사용 기술" src="https://github.com/user-attachments/assets/40b5901d-bf65-4dbc-a088-9bc449e6a18f" />

<details>
<summary><b>[AI] GPT API</b></summary>
<br/>
<ul>
    글 생성, 사용자 요약 피드백 기능에 사용했습니다.
    <br></br>
    <!-- <li> 내용을 입력하세요 !! </li> -->
</ul>
</details>

<details>
<summary><b>[캐싱] REDIS</b></summary>
<br/>
<ul>
    글 저장/분석/랭킹 기능에 <b>RedisJSON</b>과 Redis의 <b>Sorted Set</b>을 사용했습니다.
    <br></br>
    <li><b>메모리 기반 저장</b><br>
    DB보다 빠른 조회·갱신 속도로, <b>글 조회 / 출석 상태 / 학습 통계 / 랭킹 기능</b>에 적합합니다.</li>
    <li><b>초당 수백개의 요청까지 확장성 고려</b><br>
    글 본문 같이 잦은 읽기 작업을 캐싱하여 DB 부하 감소를 고려했습니다.</li>
    <li><b>TTL 설정으로 최신 데이터 유지</b><br>
    스케줄러로 주기적으로 생성한 데이터를 자동 삭제하여 최신 데이터를 유지하도록 했습니다.</li>
    <li><b>RedisJSON</b><br>
    카테고리, 생성일자, 본문으로 구성된 글 객체를 JSON 네이티브로 저장하기 위해 사용했습니다.</li>
    <li><b>Sorted Set</b><br>
    요약 학습 후 사용자의 점수를 sorted set에 저장하여 항상 정렬된 상태 유지합니다. 또한 O(\log N)의 시간 복잡도로 실시간 랭킹 조회 및 갱신이 가능케 했습니다.</li>
</ul>
</details>

<details>
<summary><b>[알림 기능] FCM (Firebase Cloud Messaging)</b></summary>
<br/>
<ul>
    푸시 알림 기능은 사용자의 지속적인 학습 독려합니다.
    <br></br>
    <li><b>Service Worker</b><br>
    브라우저 및 모바일 환경 모두에 알림 전송하도록 했습니다.</li>
    <li><b>비동기 메시지 전송</b><br>
    서버가 FCM 응답을 기다리면서 발생하는 블로킹을 방지하고 서버 성능을 최적으로 사용합니다.</li>
</ul>
</details>

## 설계 및 명세
<b>ERD</b><br>
<img src="https://github.com/user-attachments/assets/51da42b1-512f-44a4-a665-58129643c9df" width="80%"></img>

<b>Swagger API</b><br>
<a href="https://github.com/user-attachments/files/23773436/API.html">API 명세서 다운로드</a>
