# <img src="./src/shared/asset/images/character.png" width="60" /> Plaza - 모임 장소 추천 서비스
<img width="512" height="300" alt="KakaoTalk_20250910_202620675" src="https://github.com/user-attachments/assets/3527779b-3532-48ad-aee5-275571447ac3" />

<br>
 <br>
 >> 모임 멤버들의 위치와 선호도를 기반으로 최적의 장소와 장소들 사이 동선을 추천해주는 웹 애플리케이션

<!-- 배포 링크가 있다면 아래 주석을 해제하세요 -->
<!-- 🔗 [서비스 바로가기](https://your-deploy-url.vercel.app) -->

<br/>

## 주요 기능
<img width="230" height="400" alt="스크린샷 2025-09-10 202716" src="https://github.com/user-attachments/assets/bee430b9-23e3-46ad-b9c8-e482d37876e3" />

<img width="230" height="400" alt="스크린샷 2025-09-10 202733" src="https://github.com/user-attachments/assets/aa27a282-5f83-4d15-9cbf-d905a9de2e1d" />

<br>

### 1. 모임 생성
- 모임 이름과 목적 입력
- 요일 및 시간대 선택
- 장소 유형 선호도 설정 (음식점, 카페, 술집, 액티비티)
- 드래그 앤 드롭으로 장소 우선순위 정렬

### 2. 출발지 설정



- 카카오 지도 기반 출발지 검색
- 멤버별 출발 위치 지정

### 3. 장소 추천 결과
- 네이버 지도에 추천 장소 표시
- 장소별 평점, 영업시간, 리뷰 정보 제공
- 모임 정보 수정 기능

<br/>

## 기술 스택

| 분류 | 기술 |
|------|------|
| **Framework** | React 18, TypeScript |
| **Build** | Vite |
| **Styling** | Tailwind CSS |
| **상태 관리** | Zustand, TanStack React Query |
| **지도** | Kakao Maps SDK, React Naver Maps |
| **애니메이션** | Framer Motion |
| **DnD** | @dnd-kit |
| **HTTP** | Axios |
| **배포** | Vercel |

<br/>

## 프로젝트 구조

```
src/
├── app/            # 앱 셸, 라우팅, 프로바이더
├── pages/          # 스텝별 페이지 (step0 ~ step3)
├── widgets/        # 도메인별 위젯 컴포넌트
├── features/       # 비즈니스 로직
├── entities/       # 도메인 모델
├── shared/         # 공용 UI, API, 훅, 에셋
├── store/          # Zustand 전역 상태
├── hooks/          # 커스텀 훅
└── types/          # TypeScript 타입 정의
```

<br/>




