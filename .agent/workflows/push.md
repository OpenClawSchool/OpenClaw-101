---
description: GitHub 동기화 — 현재 워크스페이스의 변경 사항을 GitHub 저장소에 푸시 (Commit & Push)
---

# GitHub Push Skill

워크스페이스의 모든 변경 사항(문서, 이미지, 코드 등)을 스테이징하고, 커밋 후 원격 저장소(`origin/main`)로 푸시합니다.

---

## 사용 시점

- "지금까지 작업한 거 깃허브에 올려줘"라고 요청받았을 때
- 다른 워크플로우(리서치, 슬라이드 제작 등)의 마지막 단계에서 자동 동기화가 필요할 때
- 워크스페이스 내 중요한 파일 변화가 생겨 백업이 필요할 때

---

## 워크플로우

// turbo
1. 변경 사항 확인 및 스테이징
   `git add .`

// turbo
2. 커밋 메시지 작성 및 커밋
   `git commit -m "update via OpenClaw conversation: $(Get-Date -Format 'yyyy-MM-dd HH:mm:ss')"`

// turbo
3. 원격 저장소로 푸시
   `git push origin main`

---

## 품질 체크리스트

- [ ] `git status`를 통해 변경 사항이 올바르게 스테이징되었는가?
- [ ] 원격 저장소(`origin`)가 올바르게 설정되어 있는가?
- [ ] 푸시 과정에서 인증 오류(PAT 등)가 발생하지 않았는가?
