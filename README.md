# standup-bot

매일 정해진 시간에 팀원들에게 스탠드업 질문을 DM으로 보내고, 답변을 모아서 채널에 요약해주는 Slack 봇입니다. 회의 없이 비동기 스탠드업을 운영하고 싶은 팀을 위한 도구입니다.

## 왜 만들었나

원격 팀에서 매일 같은 시간에 모이는 스탠드업 회의가 시간대 때문에 항상 애매했습니다. 대신 각자 편한 시간에 DM으로 답하고, 봇이 자동으로 채널에 정리해서 올려주는 방식이 훨씬 잘 맞았습니다.

## 주요 기능

- 지정한 시간에 팀원 전원에게 자동으로 DM 발송
- "어제 한 일 / 오늘 할 일 / 막힌 것" 3가지 질문 (커스터마이즈 가능)
- 응답을 모아 채널에 스레드로 요약 게시
- 특정 요일 제외 설정 (주말, 공휴일)
- 응답 안 한 사람 리마인더 자동 발송

## 데모

봇이 보내는 DM:
```
👋 좋은 아침입니다! 오늘의 스탠드업입니다.

1️⃣ 어제 한 일은?
2️⃣ 오늘 할 일은?
3️⃣ 막히는 부분이 있나요?
```

채널 요약 (매일 오전 10시 자동 게시):
```
📋 오늘의 스탠드업 요약 (8/23)

@Alice
어제: 로그인 API 리팩토링
오늘: 결제 모듈 테스트 작성
막힘: 없음

@Bob
어제: 디자인 리뷰
오늘: 온보딩 플로우 구현
막힘: 피그마 접근 권한 필요 🔴
```

## 설치

### 1. Slack 앱 생성

1. [api.slack.com/apps](https://api.slack.com/apps)에서 새 앱 생성
2. `chat:write`, `im:write`, `users:read` 권한 추가
3. 워크스페이스에 설치 후 Bot Token 복사

### 2. 봇 배포

```bash
git clone https://github.com/yourname/standup-bot.git
cd standup-bot
npm install
cp .env.example .env
```

`.env` 파일 설정:
```
SLACK_BOT_TOKEN=xoxb-your-token
SLACK_CHANNEL_ID=C0123456789
STANDUP_TIME=09:00
SUMMARY_TIME=10:00
TIMEZONE=Asia/Seoul
```

### 3. 실행

```bash
npm start
```

또는 Docker로:
```bash
docker build -t standup-bot .
docker run -d --env-file .env standup-bot
```

## 설정 커스터마이즈

`config.json`에서 질문과 대상자 조정 가능:

```json
{
  "questions": [
    "어제 한 일은?",
    "오늘 할 일은?",
    "막히는 부분이 있나요?"
  ],
  "participants": ["U0123", "U0456"],
  "skipDays": ["saturday", "sunday"],
  "reminderAfterMinutes": 60
}
```

## 명령어

Slack에서 직접 제어도 가능합니다:

```
/standup now       지금 바로 스탠드업 요청 보내기
/standup summary    오늘 요약 즉시 게시
/standup skip        오늘 스탠드업 건너뛰기
```

## 기술 스택

- Node.js + `@slack/bolt` 프레임워크
- 스케줄링: `node-cron`
- 데이터 저장: SQLite (응답 히스토리 보관용)

## 로드맵

- [ ] 주간 요약 리포트 (막힌 것 트렌드 분석)
- [ ] 여러 채널/팀 동시 운영 지원
- [ ] Notion/Linear 연동해서 "오늘 할 일" 자동 채우기

## 기여

이슈와 PR 환영합니다. 로컬 개발 환경 설정은 `CONTRIBUTING.md`를 참고하세요.

## 라이선스

MIT
