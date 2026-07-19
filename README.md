# <img src="./src/shared/asset/images/character.png" width="60" /> Plaza

> 모임 멤버들의 위치와 선호도를 기반으로 최적의 장소와 장소 간 동선을 추천해주는 모임 장소 추천 서비스

여러 명이 각자 다른 곳에서 출발하는 모임에서, 멤버들의 출발지·선호 장소 유형·분위기·모임 시간대를 입력받아 **동선상 합리적인 지역과 장소 코스**를 추천합니다.

- **저장소:** https://github.com/underrRndezvous/FrontEnd

---

## 기술 스택

![React](https://img.shields.io/badge/React_18-61DAFB?style=flat&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat&logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat&logo=vite&logoColor=white)
![TailwindCSS](https://img.shields.io/badge/Tailwind_CSS-06B6D4?style=flat&logo=tailwindcss&logoColor=white)
![Zustand](https://img.shields.io/badge/Zustand-000000?style=flat&logo=zustand&logoColor=white)
![TanStack Query](https://img.shields.io/badge/TanStack_Query-FF4154?style=flat&logo=reactquery&logoColor=white)
![dnd kit](https://img.shields.io/badge/dnd--kit-333333?style=flat)
![Naver Maps](https://img.shields.io/badge/Naver_Maps-03C75A?style=flat&logo=naver&logoColor=white)

---

## 담당 역할 및 기여

| 기능                | 구현 내용                                                                 |
| ------------------- | -------------------------------------------------------------------------- |
| 모임 생성 플로우    | 이름/목적/시간대/장소 선호도/출발지/요약 6단계 폼 UI 및 상태 연결          |
| 지도 & 코스 편집    | 네이버 지도 마커 렌더링, 드래그 정렬, 추천 장소 코스 편입                 |
| 공유 기능           | 카카오톡 커스텀 템플릿 공유 연동                                          |
| 상태 관리           | Zustand 전역 스토어 설계, sessionStorage persist                          |
| API 연동            | 모임 생성/조회, 가게 상세 조회 API 및 TanStack Query 훅 설계              |
| 공통 UI/라우팅      | StepFormLayout, stepNavigation 등 공용 컴포넌트, 라우트 테이블 구성       |

---

## 주요 구현

**1. 원형 UI 기반 시간대 선택 위젯**

아침·점심·오후·저녁 4개 구간을 시계처럼 인지할 수 있는 원형 선택 UI를 만들었습니다. 4분면 버튼 그리드 전체를 45도 회전시키고, 안에 들어가는 텍스트는 반대로 -45도 회전시켜 다시 수평으로 보이게 했습니다. store에는 `MORNING` 같은 enum 값으로 저장하고, 화면에는 매핑 테이블을 거쳐 DOM id로 변환해 사용했습니다.

---

**2. dnd-kit 기반 장소 우선순위 드래그 정렬**

`@dnd-kit/sortable`로 장소 리스트의 순서를 드래그로 바꿀 수 있게 했습니다. 드래그 핸들 버튼에만 이벤트를 연결해서, 항목을 클릭해 상세로 이동하는 동작과 드래그 동작이 서로 부딪히지 않게 분리했습니다. 순서가 바뀌면 `arrayMove`로 배열을 갱신해 바로 반영됩니다.

---

**3. 네이버 지도 기반 코스 편집**

결과 화면에서 `react-naver-maps`로 추천 지역의 장소들을 지도에 표시했습니다. 현재 코스에 포함된 장소는 순번이 있는 핀으로, 그 외 장소는 원형 마커로 구분해서 보여줍니다. 코스에 없는 마커를 클릭하면 상세 모달이 뜨고, 여기서 바로 코스에 추가할 수 있어 추천받은 지역을 둘러보며 동선을 직접 짤 수 있습니다.

---

**4. TanStack Query 기반 서버 상태 관리**

추천 요청은 `useMutation`, 상세 조회는 `useQuery`로 나눠서 관리했습니다. 여러 장소의 상세 정보가 한 번에 필요한 화면에서는 `Promise.all`로 병렬 조회하는 훅을 따로 만들어 순차 호출로 인한 대기 시간을 줄였습니다.

---

## 트러블슈팅

**1. 공유 링크로 직접 진입하면 결과 화면이 비어 보이는 문제**

결과 화면은 이전 페이지에서 `navigate`의 `location.state`로 넘겨준 추천 데이터를 그리는 방식이었습니다. 그런데 카카오톡 공유 링크로 이 화면에 바로 들어오거나 새로고침을 하면 페이지가 새로 마운트되면서 `location.state`가 비어, 데이터를 그릴 수 없는 문제가 있었습니다. 공유 기능은 state 없이 들어오는 경우를 전제로 하는데, 화면 쪽 로직은 state가 항상 있다고만 가정하고 있었던 게 원인이었습니다.

URL의 모임 ID로 서버에서 데이터를 다시 받아오는 `useMeetingDetail` 쿼리를 추가했습니다. `location.state`가 있으면 그대로 쓰고, 없으면 이 쿼리 결과로 대체하도록 했습니다. 이후로는 공유 링크로 처음 들어오든 새로고침을 하든 같은 결과 화면을 볼 수 있습니다.

---

**2. 다중 출발지 입력 시 커서가 튀는 문제**

출발지는 여러 개를 추가·삭제할 수 있고, 입력창에 보이는 문자열과 스토어에 저장되는 구조화된 주소(`first`/`second`/`third`)를 함께 다뤄야 했습니다. 처음에는 입력할 때마다 스토어를 바로 갱신했는데, 그러면 리렌더 때마다 입력값이 스토어의 구조화된 값으로 재조합되면서 커서 위치가 튀거나 입력 중이던 글자가 사라지는 문제가 있었습니다.

그래서 화면에 보이는 표시값과 스토어 상태를 분리했습니다. 입력하는 동안에는 로컬 상태만 바뀌고, 항목이 추가되거나 삭제될 때만 두 상태를 동기화합니다. 주소 검증도 입력 시점이 아니라 "다음" 버튼을 눌러 제출하는 시점에 한 번만 `region.json`과 대조하도록 했습니다.

---

## 프로젝트 구조

[FSD(Feature-Sliced Design)](https://feature-sliced.design/) 아키텍처를 참고해 계층별로 디렉터리를 구성했습니다.

```
src/
├── app/            # 앱 셸, 라우팅, 프로바이더 (최상위 계층)
├── pages/          # 스텝별 페이지 (step0 ~ step3)
├── widgets/        # 여러 feature/entity를 조합한 도메인 단위 UI 블록
├── features/       # 사용자 행동 단위 비즈니스 로직
├── entities/       # 도메인 모델(모임, 장소 등)
├── shared/         # 공용 UI, API 클라이언트, 훅, 에셋 (최하위 계층)
├── store/          # Zustand 전역 상태
├── hooks/          # 커스텀 훅
└── types/          # TypeScript 타입 정의
```
