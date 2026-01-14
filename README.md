# VoiceMeeting Schedule Management (음성 회의 요약 + URL 캘린더)

Discord 음성 채널에서 **회의를 녹음**하고, NAVER **CLOVA Speech(STT)** 와 **CLOVA Studio(요약)** 를 통해  
회의 내용을 **텍스트로 변환/요약**한 뒤 Discord 채팅으로 공유합니다.  
또한 **FullCalendar 기반 웹 캘린더**에서 일정을 관리하며, 일정 데이터는 **DB 없이 URL에 인코딩하여 저장/공유**합니다.

---

## ✨ 주요 기능

### 1) 음성 회의 녹음 → STT → 요약(Discord Bot)
- `!음성녹음시작` : 봇이 사용자의 음성 채널에 접속 후 녹음 시작
- `!음성녹음종료` : 녹음 종료 후
  - 사용자별 PCM 파일 생성 → **FFmpeg로 FLAC 병합**
  - **CLOVA Speech**로 한국어 STT 수행(화자별 텍스트 정렬)
  - **CLOVA Studio 요약 API**로 요약문 생성
  - 요약 결과를 Discord 채팅으로 출력(옵션: 파일 저장)

### 2) 웹 캘린더(FullCalendar)
- 일정 추가/수정/삭제
- 일정 데이터는 **URL 파라미터 `a`** 에 JSON을 **Base64 인코딩**해 저장
- **복사 버튼**으로 URL을 공유하면, 동일한 일정이 그대로 로드됨(DB 불필요)
- (코드 포함) 공휴일 표시 로직 포함

### 3) 캘린더 링크 공유 봇(옵션)
- 봇이 접속 시 캘린더 링크를 특정 채널에 자동 안내
- `/create_channel <채널명>` 등 간단한 명령 제공

---

## 🧩 구성(간단 아키텍처)

Discord 음성채널  
→ (Bot 녹음: PCM)  
→ FFmpeg(FLAC 병합)  
→ CLOVA Speech(STT)  
→ 화자별 텍스트 정리  
→ CLOVA Studio(요약)  
→ Discord 채팅으로 요약 전송

일정 관리(Web)  
→ 이벤트(JSON)  
→ URL 파라미터 `a`에 Base64 인코딩 저장  
→ 공유 링크로 재접속 시 복원
