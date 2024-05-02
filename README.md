## MobileBERT를 활용한 스팀 FPS장르 인기게임 리뷰 분석 프로젝트
<!--
badge icon 참고 사이트
https://github.com/danmadeira/simple-icon-badges
-->
<img src="https://img.shields.io/badge/python-%233776AB.svg?&style=for-the-badge&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/pytorch-%23EE4C2C.svg?&style=for-the-badge&logo=pytorch&logoColor=white" />
<img src="https://img.shields.io/badge/pycharm-%23000000.svg?&style=for-the-badge&logo=pycharm&logoColor=white" />

![스팀 이미지](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/e65af0a2-63d4-4056-a03b-56e586b53ed1)

## 1. 개 요

### 1.1 게임 리뷰의 영향력
1970년대 후반부터 1980년대 초반에는 게임 기업들이 컴퓨터 및 비디오 게임 시장에서 주목할 만한 성과를 거두며 게임 산업을 크게 성장시켰습니다. 이후 1990년대에는 개인용 컴퓨터와 게임 콘솔의 보급으로 게임 산업이 더욱 확대되었으며, 온라인 게임 및 모바일 게임의 등장으로 시장은 더욱 커져 현재는 전 세계적으로 게임이 큰 산업으로 자리 잡았으며 또한 문화의 일부로 인식되고 있습니다.[[1](https://news.samsung.com/kr/스페셜-리포트-게임-현대인의-문화가-되다)]
게임 리뷰는 게임 구매 결정에 대한 큰 영향을 미치는 중요한 요소중 하나로 사용자 리뷰는 보다 개인적이고 실제 경험을 기반으로 합니다. 이러한 리뷰는 사용자들 간의 소통 촉진과 게임에 대한 정보를 공유하는데 도움을 주고 긍정적인 리뷰는 마케팅 효과에도 도움이 됩니다. 또한 게임 개발자들은 리뷰를 통해 자신의 게임에 대한 피드백을 받을 수 있어 게임을 개선하고 향상시킬 수 있습니다.
![스팀 리뷰 이미지](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/46490ca0-a904-4589-9007-c9a2a2d9f8e1)

### 1.2 문제정의
스팀은 세계 최대 규모의 전자 소프트웨어 유통망으로, 스팀 클라이언트를 통해 게임을 구입하고 관리할 수 있으며, 다양한 커뮤니티 기능을 통해 다른 유저들과 소통할 수 있습니다. 스팀에는 약 5만 개가 넘는 게임들이 있으며  [[2](https://www.pcgamesn.com/steam/total-games)] 2019년 발표된 자료에 의하면 가입 계정이 10억 개 [[3](https://www.thegamer.com/steam-1-billion-users/)]를 돌파했고, 2021년에는 월 평균 사용자 1억 3,200만 명을 기록[[4](https://store.steampowered.com/news/group/4145017/view/3133946090937137590)]한 명실상부한 ESD 업계의 1인자입니다.
FPS는 First Person Shooter의 약자로, 플레이어가 주인공의 시점에서 게임 세계를 바라보며, 총기를 사용하여 적을 격파하고 임무를 수행하는 게임 장르입니다.
이번 프로젝트에서는 2017년까지 스팀의 FPS 장르 게임 중 리뷰 데이터가 두 번째, 세 번째, 네 번째로 많은 게임들의 리뷰 데이터를 분석하여, 리뷰 내용과 평점을 기반으로 긍정적 또는 부정적인지를 예측하는 인공지능 모델을 개발하는 것이 목표입니다.
## 2. 데이터
### 2.1 원시 데이터
[[Steam Reviews 데이터셋](https://www.kaggle.com/datasets/andrewmvd/steam-reviews)]
* 데이터 구성

| app_id | app_name | review_text | review_score | review_votes |
|--------|----------|-------------|--------------|--------------|
| 게임 아이디 | 게임 이름    | 리뷰 문장       | 평점           | 게임 추천 여부     |

2017년도까지 스팀게임의 리뷰를 담은 데이터입니다. 총 9364게임에 640만개의 리뷰 데이터가 있습니다.

### 2.11 리뷰 개수 순서대로 게임 50개 (초록색은 FPS장르)
![인기게임 50개 리뷰 순서(초록색은 FPS장르)](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/6cfdf1da-ce33-4f32-b2f2-72e3d2cd0dc4)

### 2.2 추출한 데이터
스팀의 FPS 장르 게임 중 리뷰 데이터가 두 번째, 세 번째, 네 번째로 많은 게임(Left 4 Dead 2, Grand Theft Auto V, Robocraft) 3개의 리뷰데이터 134950개
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
![스팀 리뷰 평점 평균](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/9bfcbe83-4ec9-4353-adb4-2d514842ea77)
* 리뷰 문장의 길이
 
| 최솟값 | 최대값  | 평균  |
|-----|------|-----|
| nan | 8013 | 318 |

## 3. 학습 데이터

## 4. MobileBert 학습 결과

## 5. 느낀점 및 배운점
