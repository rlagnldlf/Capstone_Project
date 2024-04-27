![스팀 이미지](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/e65af0a2-63d4-4056-a03b-56e586b53ed1)

## MobileBERT를 활용한 스팀게임 리뷰 분석 프로젝트
<!--
badge icon 참고 사이트
https://github.com/danmadeira/simple-icon-badges
-->
<img src="https://img.shields.io/badge/python-%233776AB.svg?&style=for-the-badge&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/pytorch-%23EE4C2C.svg?&style=for-the-badge&logo=pytorch&logoColor=white" />
<img src="https://img.shields.io/badge/pycharm-%23000000.svg?&style=for-the-badge&logo=pycharm&logoColor=white" />

## 1. 개 요 A4용지로 1장정도 (그림포함)
### 1.1 문제정의


[[스팀](https://store.steampowered.com/)]은 [[밸브 코퍼레이션](https://www.valvesoftware.com/)]이 개발하고 운영 중인 세계 최대 규모의 전자 소프트웨어 유통망으로 스팀 클라이언트를 통해 게임을 구입, 관리할 수 있으며 다양한 커뮤니티 기능을 통해 다른 유저들과 소통할 수 있다. 현재 약 5만개의 게임을 서비스 하고 있으며 가입 계정이 10억 개를 돌파하고 월 평균 사용자가 1억명을 넘는 등 명실상부한 ESD업계의 1인자이다.
### 1.2 게임 리뷰의 영향력

게임 리뷰는 게임 구매 결정에 대한 큰 영향을 미치는 중요한 요소중 하나로 사용자 리뷰는 보다 개인적이고 실제 경험을 기반으로 합니다. 이러한 리뷰는 사용자들 간의 소통 촉진과 게임에 대한 정보를 공유하는데 도움을 주고 긍정적인 리뷰는 마케팅 효과에도 도움이 됩니다. 또한 게임 개발자들은 리뷰를 통해 자신의 게임에 대한 피드백을 받을 수 있어 게임을 개선하고 향상시킬 수 있습니다.
<img src="스팀 리뷰 이미지.png" />
# 1. 개 요
## 2. 데이터
### 2.1 원시 데이터
[[Steam Reviews 데이터셋](https://www.kaggle.com/datasets/andrewmvd/steam-reviews)]
[[스팀 홈페이지](https://store.steampowered.com/)]
* 데이터 구성

| app_id | app_name | review_text | review_score | review_votes |
|--------|----------|-------------|--------------|--------------|
| 게임 아이디 | 게임 이름    | 리뷰 문장       | 평점           | 게임 추천 여부     |
2017년도까지 스팀게임의 리뷰를 담은 데이터이다. 총 9364게임에 640만개의 리뷰 데이터가 있다.

### 2.2 추출한 데이터
FPS장르의 스팀 인기게임중 상위 10개 게임에 각 5000건씩 추출
### 2.3 추출한 데이터에 대한 탐색적 데이터 분석
* 게임 설명

| 게임 이름  | 설명                                                                                  |
|--------|-------------------------------------------------------------------------------------|
| Counter-Strike | 플레이어가 테러리스트, 대테러리스트 두 팀으로 나누어져 한 팀은 사이트에 폭탄을 설치하고 반대 팀에서는 그 폭탄이 폭발하기 전에 해체하는 방식의 게임 |
| Grand Theft Auto V | 열린 세계에서의 자유로운 탐험과 범죄를 중심으로 한 액션 어드벤처 비디오 게임                                |
| Tom Clancy's Rainbow Six Siege | 팀으로 나뉘어 전략적인 근접전과 팀원들과의 협력이 필요한 강렬한 멀티플레이 액션 FPS 게임                                 |
| Fallout 4 | 후원되지 않은 보급창고로부터 나온 생존자로서, 핵전쟁으로 인해 파괴된 보스턴을 탐험하고 건축하며 생존하는 오픈월드 RPG 게임              |
| Team Fortress 2 | 다양한 클래스와 능력을 활용하여 팀원들과 함께 전투하는 팀 기반 멀티플레이 셔터 액션 게임                                  |
| Left 4 Dead 2 | 좀비 대재앙에 맞서 팀원들과 함께 생존을 위해 싸우는 쿼드 플레이어 공포 생존 게임                                      |
| Arma 3 | 현실적인 전투 시뮬레이션과 엄청난 자유도를 가진 군사 시뮬레이션 게임으로, 거대한 오픈월드에서 팀원들과 함께 전쟁을 경험하는 게임            |
| Garry's Mod | 창의력을 발휘하여 자신만의 게임 모드나 콘텐츠를 만들고 공유할 수 있는 무한한 창작성을 지닌 모드 기반의 생존 게임                 |
| PAYDAY 2 | 현대의 범죄를 소재로 한 4인 협력 FPS 게임으로, 팀원들과 함께 각종 습격과 감시 임무를 수행하고 돈을 벌어 자신의 무기와 장비를 업그레이드하는 게임             |
| BioShock Infinite | 스팀펑크 세계를 배경으로 한 대담한 모험과 인종 문제와 종교적 상징을 다루는 색다른 스토리를 풀어나가는 인상적인 액션 어드벤처 게임                        |

* 평점 분포
<img src="스팀 리뷰 평점 평균.png" />
* 리뷰 문장의 길이
 
| 최솟값 | 최대값  | 평균  |
|-----|------|-----|
| nan | 8013 | 318 |

연도별, 장소별 등등 데이터의 부가정보를 바탕으로 데이터를 탐색(pandas, matplotlib)

# 여기까지가 중간 과제 점검
## 3. 학습 데이터

## 4. MobileBert 학습 결과

## 5. 느낀점 및 배운점
