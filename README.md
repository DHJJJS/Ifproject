# Ifproject

# C++ 미니프로젝트 - 3 vs 3 하이퍼 FPS 점령전

1인칭 슈팅(FPS) 기반의 **6인 팀 점령전 게임**입니다.  
C++과 언리얼 엔진을 활용하여 플레이어와 AI가 함께 작동하는 멀티 직업 전투 시스템을 구현했습니다.

---

## 🎮 게임 개요

- **장르**: 1인칭 PVP FPS  
- **모드**: 점령전 (Capture Point)  
- **플레이 방식**: 3vs3 팀전 (플레이어 1명 + AI 2명 vs AI 3명)  
- **테마**: SF 기반 전장  

---

## 🧙 직업 시스템

| 직업 | 체력 | 탄창 | 특징 |
|------|------|------|-------|
| 탱커 | 450  | 25   | 방벽/돌진 등 라인 컨트롤 |
| 딜러 | 200  | 30   | 대미지, 빠른 이동기 |
| 힐러 | 200  | 20   | 힐, 보호막, 부활 기능 |

- 직업 선택 후 캐릭터 생성  
- 나머지 직업은 자동으로 아군 AI로 배치  
- 적군은 자동으로 3인 구성  

---

## ⚔️ 전투 시스템

- **기본 체력**: 200 HP (직업별 차등)  
- **기본 공격 데미지**: 20  
- **궁극기 데미지**: 70  
- **스킬 쿨타임**: 10초  
- **궁극기 발동 조건**: 아이템 3개 획득 시 자동 활성화  

---

## 🛠️ 기능 요약

### 🔫 총기 시스템
- **탄약 소모 후 자동** or **언제든지 수동(R키) 재장전**
- 재장전 소요 시간은 2초  
- 총알 발사 → 충돌 시 대미지 적용  
- 직업별 총기 구분 (`GunClass` 할당)

### 🧠 AI 시스템
- 적 탐지 시 자동 공격, 스킬 우선순위 적용  
- 아군 체력/상황에 따라 힐/방벽/부활 발동  
- 상태 기반 동작 (Idle, Searching, Attacking 등)

### 🗺️ 맵 & 점령 시스템
- 최대 3개 점령 지점 중 현재는 A 거점 구현  
- 거점 내 **단독일 경우 SubGauge 상승**  
- SubGauge = 3 → 점령권 획득 → 점령 게이지 누적  
- **99% 시 적이 있으면 정지**, 100% 달성 시 승리  

### 🧩 UI 시스템
- 직업 선택창  
- 스킬/궁극기/체력/탄창 실시간 UI  
- 궁극기 아이템 습득 시 UI 게이지 점등  
- 점령 진행률, SubGauge 시각화  

---

## 📁 디렉토리 구조 (요약)

📦 Source/IfProject  
- **Character/**  
  - IfProjectCharacter.h / .cpp → 공통 캐릭터 베이스 클래스  
  - DealerCharacter.h / .cpp  → 딜러 전용 클래스  
  - TankerCharacter.h / .cpp  → 탱커 전용 클래스  
  - HealerCharacter.h / .cpp  → 힐러 전용 클래스  
  - JobType.h        → 직업 enum 정의  

- **Weapon/**  
  - Gun.h / .cpp       → 총기 클래스  
  - IfProjectProjectile.h / .cpp → 발사체 클래스  

- **GameSystem/**  
  - IfProjectGameMode.h / .cpp → 게임 규칙 및 점령전 로직  
  - IfProjectHUD.h / .cpp    → UI 및 HUD 관리  

- IfProject.cpp / .h       → 프로젝트 모듈 초기화  
- IfProject.Build.cs      → 언리얼 빌드 설정 파일  

---

## 🔍 내부 시스템 세부 구성

### 🎯 스킬 시스템
- `SetupPlayerInputComponent()`를 통해 Q, E, R, F 키에 각 직업 스킬 바인딩  
- 직업별 캐릭터 클래스에서 고유 쿨타임, 효과, 사용 조건 정의  
- `Tick()`을 활용한 스킬 지속시간 제어 및 상태 갱신 포함  

### 💥 총기 및 발사체
- `Gun` 클래스: 총기 Actor (직업별 GunClass로 지정)  
- `IfProjectProjectile`: 총알 역할, `ProjectileMovementComponent`로 추적  
- 충돌 시 데미지 처리 및 파괴 기능 포함  

### 🧩 HUD 시스템
- `AIfProjectHUD`에서 직접 HUD 구성 (`DrawHUD()` 사용)  
- 체력/탄창/스킬 UI 요소를 직접 캔버스에 출력  
- 추후 UMG로 개선 가능  

### 🧠 게임 모드 & 캐릭터 스폰
- `AIfProjectGameMode`에서 플레이어 및 AI 캐릭터 스폰  
- 팀 및 직업 선택 시스템 포함  
- 승리/패배 조건도 여기서 처리  

---

## 🧪 실행 방법 (요약)

1. 언리얼 엔진 4.27로 프로젝트 오픈  
2. 레벨에서 `Play` 또는 `Standalone` 실행  
3. 직업 선택 후 5초 카운트 후 전투 시작  

---

## 🔧 개발 환경

- 언리얼 엔진 4.27  
- C++17  
- Windows 11  
- Visual Studio 2022  

---

## 📜 향후 개선 버킷리스트

- B, C 거점 추가 및 맵 구조 확장  
- 킬로그 기능 재도전  
- 총기 에셋 시각 개선  
- 각 직업 스킬별 애니메이션 보강  
- 팀 기반 멀티 플레이 연동 기능 구현  

---

## 🙋‍♀️ 팀원 기여
![그림1](https://github.com/user-attachments/assets/54d1f86c-6c1c-4cf7-940c-b829431031a8)

---

## 📝 라이선스

본 프로젝트는 교육 및 학습 목적의 미니 프로젝트이며, 별도 라이선스를 포함하지 않습니다.
"""

# 저장
with open(final_readme_path, "w", encoding="utf-8") as f:
    f.write(readme_content)

final_readme_path
