# Render 서비스 새로 만들기 (pomotrip)

예전 `walkmodoro` 서비스를 지우고 `pomotrip` 이라는 이름으로 처음부터 다시 만드는 순서예요.
모두 무료예요. 비밀값은 Render 화면에만 넣고 채팅이나 코드에는 쓰지 마세요.

## 0. 먼저 GitHub 저장소 이름 바꾸기 (1분)

1. https://github.com/keemsub/walkmodoro/settings 열기
2. 맨 위 **Repository name** 을 `pomotrip` 으로 바꾸고 **Rename**
3. 예전 주소로 들어와도 GitHub가 새 주소로 알아서 보내 줘요.

## 1. 예전 서비스 지우기

1. Render 대시보드 → `walkmodoro` 서비스 → **Settings** 맨 아래 → **Delete Web Service**
2. 지우면 `walkmodoro.onrender.com` 은 더 이상 안 열려요.
   - 로그인해서 쓴 기록은 Neon 데이터베이스에 있어서 그대로 남아요.
   - 로그인 없이 쓴 기록(그 휴대폰에만 저장된 것)은 새 주소로 넘어가지 않아요.

## 2. 새 서비스 만들기 (5분)

1. Render 대시보드 → **New → Web Service** → GitHub 저장소 `pomotrip` 선택
   (저장소가 안 보이면 **Configure account** 에서 `pomotrip` 접근을 허용해요)
2. **Name**: `pomotrip` / **Region**: Singapore / **Instance Type**: **Free**
3. Runtime은 **Docker** 그대로 두기
4. **Advanced → Health Check Path**: `/health`

> 💡 `pomotrip` 이라는 이름을 이미 다른 사람이 쓰고 있으면 주소가 `pomotrip-ab12.onrender.com` 처럼 나와요.
> 그래도 괜찮아요. 아래 '내주소'를 실제로 받은 주소로 바꿔서 넣으면 돼요.

## 3. 환경변수 넣기 (Environment)

| 이름 | 값 |
| --- | --- |
| `SECRET_KEY` | **Generate** 버튼으로 새 무작위 값 (다른 키를 재사용하지 마세요) |
| `COOKIE_SECURE` | `true` |
| `DATABASE_URL` | 예전 값 그대로 (Neon) |
| `KAKAO_CLIENT_ID` | 예전 값 그대로 |
| `GOOGLE_CLIENT_ID`, `GOOGLE_CLIENT_SECRET` | 예전 값 그대로 |
| `GOOGLE_SITE_VERIFICATION`, `NAVER_SITE_VERIFICATION` | 새 주소로 다시 등록하고 받은 값 (아래 5번) |

`MOVED_TO` 는 **넣지 마세요.** (이사 안내판용이었는데 이제 필요 없어서 코드에서도 뺐어요)

저장하고 배포가 끝나면 `내주소/health` 를 열어 `"status":"ok"`, `"db":"persistent"` 를 확인해요.

## 4. 로그인 주소 바꾸기

- **카카오** (developers.kakao.com → 내 애플리케이션)
  - 앱 설정 → 플랫폼 → Web 사이트 도메인: `내주소`
  - 카카오 로그인 → Redirect URI: `내주소/auth/kakao/callback`
- **구글** (console.cloud.google.com → API 및 서비스 → 사용자 인증 정보 → OAuth 클라이언트)
  - 승인된 JavaScript 원본: `내주소`
  - 승인된 리디렉션 URI: `내주소/auth/google/callback`

예전 `walkmodoro.onrender.com` 주소들은 지워도 돼요.

## 5. 나머지

- **UptimeRobot**: 모니터 주소를 `내주소/health` 로 수정
- **구글 서치 콘솔 / 네이버 서치어드바이저**: 새 주소로 사이트를 다시 등록 → 받은 확인 값을 Environment에 넣기 → `sitemap.xml` 제출
- **휴대폰 홈 화면 앱**: 예전 아이콘은 지우고 새 주소에서 다시 '홈 화면에 추가'
