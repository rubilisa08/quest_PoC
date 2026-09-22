# 실적발표 음성 → AI 브리핑 PoC

[AI Business Briefing & Intelligence Publisher](https://github.com/rubilisa08/AI-project_MVP)의 확장 PoC입니다. 기존 프로젝트는 PDF/Excel/뉴스 URL만 다뤘는데, 이 PoC는 **실적발표(컨퍼런스콜) 음성**을 새로운 입력 채널로 추가해 STT(Whisper) → 리스크 브리핑까지 이어지는 흐름을 검증합니다.

- 문제 정의: [PROBLEM_DEFINITION.md](PROBLEM_DEFINITION.md)
- PoC 노트북: [notebooks/earnings_call_briefing_poc.ipynb](notebooks/earnings_call_briefing_poc.ipynb)

## 실행 방법 (Google Colab 권장)

1. 이 저장소를 GitHub에 올린 뒤, `notebooks/earnings_call_briefing_poc.ipynb`를 Colab에서 엽니다.
   - Colab 주소창에 직접 열기: `https://colab.research.google.com/github/rubilisa08/quest_PoC/blob/main/notebooks/earnings_call_briefing_poc.ipynb`
2. 위에서부터 순서대로 셀을 실행합니다. **GPU가 필요 없습니다** (Whisper `small` 모델은 CPU 런타임에서도 충분히 빠릅니다).
3. 5번 셀(`ANTHROPIC_API_KEY` 입력)에서:
   - Colab 좌측 "🔑 Secrets"에 `ANTHROPIC_API_KEY`를 등록해두면 자동으로 사용됩니다.
   - 키가 없으면 그냥 Enter를 누르세요 — 기존 프로젝트의 `MOCK_LLM` 방식과 동일하게 결정론적 목업 로직으로 자동 전환되어, 키 없이도 전체 파이프라인이 끝까지 실행됩니다.
4. 로컬(Jupyter)에서 돌리고 싶다면 `pip install openai-whisper gTTS anthropic`과 `ffmpeg` 설치가 필요합니다.

## 무엇을 검증하는가

1. **AI 모델 선정 및 실행**: OpenAI Whisper(STT)를 실제로 실행해 한국어 음성을 텍스트로 변환 (선정 이유는 노트북 3번 섹션 참고)
2. **PoC 흐름**: 대본(텍스트) → gTTS(음성 합성) → Whisper(STT) → Claude(리스크 추출, 기존 프로젝트와 동일한 "원문 인용 기반" 설계) → 타임스탬프 근거가 붙은 브리핑
3. **개선 효과 검증**:
   - 정량: STT 문자 유사도(원본 대본 대비), 처리 시간 대 수작업 청취 예상 시간 비교
   - 정성: 기존 프로젝트가 Excel 경로로 찾아낸 리스크(단기 유동성 부족, 높은 부채비율)를 음성 경로로도 동일하게 찾아내는지 확인

## 왜 AI-project_MVP와 같은 재무 숫자를 쓰는가

`sample_data/financial_sample.xlsx`(부채비율 233%, 유동비율 88.9%)와 동일한 숫자로 가상 실적발표 대본을 작성했습니다. 이렇게 하면 "같은 재무 데이터를 텍스트 경로(Excel)로 넣었을 때와 음성 경로(STT)로 넣었을 때 같은 리스크가 나오는가"를 직접 비교할 수 있어, 이번 확장이 기존 파이프라인과 일관된 결과를 내는지 검증하기 좋습니다.

## 한계

- 가상의 회사(㈜예시전자)와 TTS로 합성한 음성을 사용합니다 — 실제 기업 데이터/녹음이 아닙니다(저작권·기밀 문제 회피 목적).
- 실제 컨퍼런스콜의 배경 소음, 다수 화자, 전문용어 발음 환경은 반영되지 않았습니다.
- 화자 분리(Diarization)는 이번 PoC 범위 밖입니다.
