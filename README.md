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
### 2004년부터 2021년까지 연간 스팀게임 출시 개수
![Graph about number of games on Steam](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/3d01467d-9bba-4586-8dcb-b3fb15956c6d)
스팀은 2010년대 초반까지는 게임 출시량이 적었으나, 2010년대 중반부터 급격히 증가하여 2021년에는 연간 게임 출시량이 1만 개가 넘을 정도로 크게 성장했습니다.
## 2. 데이터
### 2.1 원시 데이터
[[Steam Reviews 데이터셋](https://www.kaggle.com/datasets/andrewmvd/steam-reviews)]
* 데이터 구성

| app_id | app_name | review_text | review_score | review_votes |
|--------|----------|-------------|--------------|--------------|
| 게임 아이디 | 게임 이름    | 리뷰 문장       | 평점           | 게임 추천 여부     |

2017년도까지 스팀게임의 리뷰를 담은 데이터이다. 총 9364게임에 640만개의 리뷰 데이터가 있다.

### 2.1.1 리뷰 개수 순서대로 게임 50개 (초록색은 FPS장르)
![인기게임 50개 리뷰 순서(초록색은 FPS장르)](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/6cfdf1da-ce33-4f32-b2f2-72e3d2cd0dc4)
리뷰가 많은 순으로 정렬되어 있으며, 초록색으로 표시된 데이터는 FPS 장르에 속하는 게임이다. 이로 인해, 인기 게임 중 FPS 장르의 비율이 상당히 높다는 사실을 확인할 수 있다."

### 2.2 추출한 데이터
스팀의 FPS 좀비 게임 중 리뷰 데이터가 가장 많은 게임(Left 4 Dead 2, 7 Days to Die, Killing Floor) 3개의 리뷰데이터 98067개
![각 게임당 리뷰 개수](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/d4b87ccd-fd45-49b2-8f39-be508d31a142)
### 2.3 추출한 데이터에 대한 탐색적 데이터 분석
* 게임 설명

| 게임 이름  | 설명                                                                                  |
|--------|-------------------------------------------------------------------------------------|
| Left 4 Dead 2      | 좀비 대재앙에 맞서 팀원들과 함께 생존을 위해 싸우는 쿼드 플레이어 공포 생존 게임                                      |
| 7 Days to Die | 좀비로 오염된 세계에서 자원을 수집하고 건축하여 7일마다 오는 좀비 공격을 버티며 생존하는 생존 호러 게임                                |
| Killing Floor  | 플레이어가 좀비와 싸워 생존하는 협동적인 게임으로, 플레이어들은 팀을 이루어 각종 무장한 좀비들을 처치하고, 라운드를 클리어하며 생존.             |

* 평점 분포
![평점평균](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/2d99a3cc-53a9-48c2-ad89-9d29d79acede)
리뷰 데이터의 평점은 0점부터 5점까지의 분포를 가지며, 이를 평균화한 표입니다. 사용자들이 대체로 긍정적인 리뷰를 작성하는 경향이 있습니다.
* 리뷰 문장의 길이
 
| 최솟값 | 최대값  | 평균  |
|-----|------|-----|
| 1 | 8000 | 255 |

![리뷰 길이에 따른 개수](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/95c197ad-fa45-4517-a176-ba1c556e3428)

## 3. 학습 데이터

## 4. MobileBert 학습 결과

## 5. 느낀점 및 배운점
