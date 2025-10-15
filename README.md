# bomberman_rl
Setup for a project/competition amongst students to train a winning Reinforcement Learning agent for the classic game Bomberman.

학생들이 클래식 게임 '봄버맨'에서 승리하는 강화학습(RL) 에이전트를 훈련시키기 위한 프로젝트/경진대회용 설정입니다. 이 환경은 Pygame을 기반으로 하며, 에이전트 개발, 훈련, 평가 및 게임 리플레이 기능을 제공합니다.

## ✨ 주요 기능

*   **Pygame 기반 GUI**: 게임 진행 상황을 시각적으로 확인할 수 있는 GUI 제공
*   **플레이 및 훈련 모드**: 개발한 에이전트를 직접 플레이하거나 강화학습 알고리즘으로 훈련 가능
*   **게임 리플레이**: 진행된 게임을 저장하고 다시 볼 수 있는 기능 (`.pt` 파일)
*   **다중 에이전트 지원**: 최대 4명의 에이전트가 동시에 게임에 참여
*   **다양한 시나리오**: `classic`, `coin-heaven` 등 여러 게임 모드 제공
*   **상세한 로깅 및 통계**: 게임 결과와 에이전트의 행동을 로그 및 JSON 파일로 저장하여 분석 용이

## 📂 프로젝트 구조

```
bomberman_rl/
├── agent_code/
│   ├── coin_collector_agent/ (코인 수집 에이전트 예시)
│   ├── peaceful_agent/       (폭탄을 사용하지 않는 에이전트 예시)
│   ├── rule_based_agent/     (규칙 기반 에이전트 예시)
│   └── tpl_agent/            (새로운 에이전트 개발을 위한 템플릿)
│       ├── callbacks.py      (에이전트 행동 로직)
│       └── train.py          (에이전트 훈련 로직)
├── assets/                   (게임 이미지 및 폰트)
├── logs/                     (게임 및 에이전트 로그)
├── replays/                  (게임 리플레이 파일)
├── screenshots/              (비디오 생성을 위한 스크린샷)
├── main.py                   # 게임 실행을 위한 메인 스크립트
├── environment.py            # 게임 세계(World) 및 GUI 구현
├── agents.py                 # 에이전트 기본 클래스 및 백엔드 관리
├── settings.py               # 게임 설정 (맵 크기, 규칙, 보상 등)
├── replay.py                 # 게임 리플레이 기능 구현
└── events.py                 # 게임 내 이벤트 상수 정의
```

## 🚀 시작하기

### 의존성 설치

이 프로젝트는 `pygame`과 `numpy`가 필요합니다.

```bash
pip install pygame numpy
```

### 게임 실행 방법

`main.py` 스크립트를 통해 게임을 실행할 수 있습니다.

윈도우에서 실행하기 위해서는, vcxsrv 가 필요합니다.

**1. 기본 규칙 기반 에이전트들로 게임 플레이하기**

```bash
# 4개의 규칙 기반 에이전트로 5라운드 게임을 실행합니다.
python main.py play --n-rounds 5 --agents rule_based_agent rule_based_agent rule_based_agent rule_based_agent
```

**2. 나만의 에이전트로 플레이하기**

`tpl_agent`를 복사하여 나만의 에이전트를 만든 후, `--my-agent` 인자를 사용합니다.

```bash
# 'my_awesome_agent'와 3개의 규칙 기반 에이전트가 대결합니다.
python main.py play --my-agent my_awesome_agent
```

**3. 에이전트 훈련하기**

`--train 1` 인자를 추가하면 첫 번째 에이전트(`my-agent`)가 훈련 모드로 실행됩니다. 훈련 로직은 `agent_code/<your_agent>/train.py`에 구현해야 합니다.

```bash
python main.py play --my-agent my_awesome_agent --train 1 --n-rounds 100
```

**4. 게임 리플레이 보기**

`--save-replay` 옵션으로 게임을 저장한 후, `replay` 명령으로 다시 볼 수 있습니다.

```bash
# 게임 플레이 및 저장
python main.py play --my-agent my_awesome_agent --save-replay

# 저장된 리플레이 실행 (replays/ 폴더의 파일 경로)
python main.py replay "replays/Round 01 (YYYY-MM-DD HH-MM-SS).pt"
```

**5. GUI 없이 빠르게 실행하기**

`--no-gui` 플래그를 사용하면 GUI 없이 빠르게 많은 라운드를 실행할 수 있어 훈련에 유용합니다.

```bash
python main.py play --my-agent my_awesome_agent --train 1 --n-rounds 1000 --no-gui
```

## 📝 나만의 에이전트 만들기

1.  `agent_code/tpl_agent` 폴더를 복사하여 새로운 이름(예: `my_awesome_agent`)으로 변경합니다.
2.  `agent_code/my_awesome_agent/callbacks.py`의 `act` 함수를 수정하여 에이전트의 행동 로직을 구현합니다. `game_state`를 분석하여 최적의 행동(`'UP'`, `'DOWN'`, `'LEFT'`, `'RIGHT'`, `'WAIT'`, `'BOMB'`)을 반환해야 합니다.
3.  (선택) 강화학습을 적용하려면 `train.py`의 `game_events_occurred`, `end_of_round` 함수에 훈련 로직(모델 업데이트, 보상 설계 등)을 구현합니다.
