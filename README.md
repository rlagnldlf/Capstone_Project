## MobileBERT를 활용한 STEAM 게임 리뷰 분석 프로젝트 - 좀비물 FPS 게임을 중심으로
<!--
badge icon 참고 사이트
https://github.com/danmadeira/simple-icon-badges
-->
<img src="https://img.shields.io/badge/python-%233776AB.svg?&style=for-the-badge&logo=python&logoColor=white" /><img src="https://img.shields.io/badge/pytorch-%23EE4C2C.svg?&style=for-the-badge&logo=pytorch&logoColor=white" /><img src="https://img.shields.io/badge/pycharm-%23000000.svg?&style=for-the-badge&logo=pycharm&logoColor=white" />

![스팀 이미지](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/e65af0a2-63d4-4056-a03b-56e586b53ed1)

## 1. 개 요

### 1.1 게임 리뷰의 영향력과 STEAM
1970년대 후반부터 1980년대 초반에는 게임 기업들이 컴퓨터 및 비디오 게임 시장에서 주목할 만한 성과를 거두며 게임 산업을 크게 성장시켰다. 이후 1990년대에는 개인용 컴퓨터와 게임 콘솔의 보급으로 게임 산업이 더욱 확대되었으며, 온라인 게임 및 모바일 게임의 등장으로 시장은 더욱 커져 현재는 전 세계적으로 게임이 큰 산업으로 자리 잡았으며 또한 문화의 일부로 인식되고 있다.[[1](https://news.samsung.com/kr/스페셜-리포트-게임-현대인의-문화가-되다)]
게임 리뷰는 게임 구매 결정에 큰 영향을 미치는 중요한 요소 중 하나로, 사용자 리뷰는 개인적인 경험을 기반으로 한다. 이러한 리뷰는 사용자들 간의 소통을 촉진하고 게임에 대한 정보를 공유하는 데 도움을 준다. 긍정적인 리뷰는 마케팅 효과에도 도움이 되며, 게임 개발자들은 리뷰를 통해 자신의 게임에 대한 피드백을 받아 게임을 개선하고 향상시킬 수 있다.
스팀은 세계 최대 규모의 전자 소프트웨어 유통망으로, 스팀 클라이언트를 통해 게임을 구입하고 관리할 수 있으며, 다양한 커뮤니티 기능을 통해 다른 유저들과 소통할 수 있다. 스팀에는 약 5만 개가 넘는 게임들이 있으며,  [[2](https://www.pcgamesn.com/steam/total-games)] 2019년 발표된 자료에 의하면 가입 계정이 10억 개 [[3](https://www.thegamer.com/steam-1-billion-users/)]를 돌파했고, 2021년에는 월 평균 사용자 1억 3,200만 명을 기록[[4](https://store.steampowered.com/news/group/4145017/view/3133946090937137590)]한 명실상부한 전자 소프트웨어 유통망 업계의 1인자로 게임 리뷰가 가장 활발하게 일어나는 곳이다.
![스팀 리뷰 이미지](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/46490ca0-a904-4589-9007-c9a2a2d9f8e1)

### 1.2 문제정의
FPS는 First Person Shooter의 약자로, 플레이어가 주인공의 시점에서 게임 세계를 바라보며, 총기를 사용하여 적을 격파하고 임무를 수행하는 게임 장르다.
이번 프로젝트에서는 2017년까지 스팀의 FPS 장르 게임 중 리뷰 데이터가 두 번째, 세 번째, 네 번째로 많은 게임들의 리뷰 데이터를 분석하여, 리뷰 내용과 평점을 기반으로 긍정적 또는 부정적인지를 예측하는 인공지능 모델을 개발하는 것이 목표이다.
### 2004년부터 2021년까지 연간 스팀게임 출시 개수
![Graph about number of games on Steam](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/3d01467d-9bba-4586-8dcb-b3fb15956c6d)
스팀은 2010년대 초반까지는 게임 출시량이 적었으나, 2010년대 중반부터 급격히 증가하여 2021년에는 연간 게임 출시량이 1만 개가 넘을 정도로 크게 성장했다.
## 2. 데이터
### 2.1 원시 데이터
[[Steam Reviews 데이터셋](https://www.kaggle.com/datasets/andrewmvd/steam-reviews)]
* 데이터 구성

| app_id | app_name | review_text | review_score | review_votes |
|--------|----------|-------------|--------------|--------------|
| 게임 아이디 | 게임 이름    | 리뷰 문장       | 평점           | 게임 추천 여부     |

2017년도까지 스팀게임의 리뷰를 담은 데이터이다. 평점은 긍정을 1로, 부정을 -1로 표시하며, 총 9364개의 게임에는 640만개의 리뷰 데이터가 있다.

### 2.2 데이터 부가 정보
![상위 50개 게임 리뷰 개수](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/65e53908-627e-42b2-9425-06d4fca89d8f)
리뷰가 많은 순으로 정렬되어 있으며, 초록색으로 표시된 데이터는 FPS 장르에 속하는 게임이다. 이로 인해, 인기 게임 중 FPS 장르의 비율이 상당히 높다는 사실을 확인할 수 있다."
* 리뷰 문장의 길이
  
| 최솟값 | 최대값  | 평균  |
|-----|------|-----|
| 1 | 8000 | 255 |

![리뷰의 길이에 따른 개수(수정)](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/dd809481-2564-4e4d-8a72-762cfc170659)
리뷰 길이가 상대적으로 짧은 리뷰가 대다수를 차지하며, 매우 긴 리뷰는 적어 대부분의 사용자가 짧고 간결한 리뷰를 작성한다는 것을 의미한다. 그러나 몇몇 사용자는 자세하고 긴 리뷰를 작성하는 경우도 있다.

### 2.3 추출한 데이터
스팀의 FPS 좀비 게임 중 리뷰 데이터가 가장 많은 게임(Left 4 Dead 2, Killing Floor, Doom) 3개의 리뷰데이터 중 리뷰 길이가 20자 이상 500자 이하인 데이터 75,409개
![각 게임당 리뷰 개수(수정)](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/580fe958-4c40-4a95-bd50-7508023019a5)
### 2.4 추출한 데이터에 대한 탐색적 데이터 분석
* 게임 설명

| 게임 이름  | 설명                                                                                  |
|--------|-------------------------------------------------------------------------------------|
| Left 4 Dead 2      | 좀비 대재앙에 맞서 팀원들과 함께 생존을 위해 싸우는 쿼드 플레이어 공포 생존 게임                                      |
| Killing Floor  | 플레이어들은 팀을 이루어 각종 무장한 좀비들을 처치하고 라운드를 클리어하며 생존하는 협동적인 게임.            |
| Doom | 인간과 지옥 사이의 전쟁을 배경으로 한 격투를 통해 고통스럽게 공포에 가득 찬 환영적인 액션게임.                                |

* 평점 분포
![긍부정에 따른 리뷰 개수 분포(수정)](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/221ac93e-c409-4ad3-975f-71c815624b13)
긍정과 부정의 개수 분포를 시각화한 표이다. 사용자들이 대체로 긍정적인 리뷰를 작성하는 경향이 있다.

## 3. 학습 데이터
### 3.1 첫 번째 학습 데이터
첫 번째 학습 데이터는 각 게임당 긍정 리뷰 데이터 500개와 부정 리뷰 데이터 500개씩, 총 3000개를 추출하여 만들었다.
* 각 게임당 데이터 개수

| Left 4 Dead 2 | Killing Floor  | Doom  |

| Left 4 Dead 2 |               |  Doom   |       |   Killing Floor |               |
| ------ | ------ | ------ |------ | ------ | ------ |
| 긍정 | 부정 | 긍정 | 부정 | 긍정 | 부정 |
| 500 | 500 | 500 | 500 | 500 | 500 |

* 데이터 비율 분포

![원그1](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/c893d646-2d15-4355-aa12-10fc546aec73)


### 3.2 두 번째 학습 데이터
두 번째 학습 데이터는 데이터 양 비율에 따라 Left 4 Dead 2는 긍정 리뷰 데이터 839개와 부정 리뷰 데이터 839개, Killing Floor는 긍정 리뷰 데이터 361개와 부정 리뷰 데이터 361개, Doom은 긍정 리뷰 데이터 300개와 부정 리뷰 데이터 300개로, 총 3000개를 추출하여 만들었다.
* 각 게임당 데이터 개수

| Left 4 Dead 2 |               |  Doom   |       |   Killing Floor |               |
| ------ | ------ | ------ |------ | ------ | ------ |
| 긍정 | 부정 | 긍정 | 부정 | 긍정 | 부정 |
| 839 | 839 | 300 | 300 | 361 | 361 |

* 데이터 비율 분포
 
![원그2](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/6cac7d1d-8273-4355-b8c5-4a455d6ad256)

## 4. MobileBert 학습 결과
### 개발환경

<img src="https://img.shields.io/badge/python-%233776AB.svg?&style=for-the-badge&logo=python&logoColor=white" /><img src="https://img.shields.io/badge/pycharm-%23000000.svg?&style=for-the-badge&logo=pycharm&logoColor=white" />

### 패키지

<img src="https://img.shields.io/badge/pandas-%23150458.svg?&style=for-the-badge&logo=pandas&logoColor=white" /><img src="https://img.shields.io/badge/pytorch-%23EE4C2C.svg?&style=for-the-badge&logo=pytorch&logoColor=white" /><img src="https://img.shields.io/badge/tensorflow-%23FF6F00.svg?&style=for-the-badge&logo=tensorflow&logoColor=white" /><img src="https://img.shields.io/badge/numpy-%23013243.svg?&style=for-the-badge&logo=numpy&logoColor=white" />


### 4.11 첫 번째 학습 데이터
| epoch | 0  | 1  | 2  | 3  |
|-----|------|-----|------|-----|
| training loss | 7.4573e+4 | 0.59 | 0.25 | 0.23 |
| Validation accuracy | 0.84 | 0.86 | 0.86 | 0.87 |

### 4.12 두 번째 학습 데이터
| epoch | 0  | 1  | 2  | 3  |
|-----|------|-----|------|-----|
| training loss | 1.3910e+4 | 0.32 | 0.23 | 0.36 |
| Validation accuracy | 0.76 | 0.89 | 0.87 | 0.89 |

### 4.13 첫 번째 학습 데이터와 두 번째 학습 데이터 손실값 비교
![TL](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/f8f8d2fe-423e-4f19-81f3-8e0248b3bf9b)
### 4.14 첫 번째 학습 데이터와 두 번째 학습 데이터 정확도 비교
![VA](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/34b30d89-dd8a-4dc8-988d-2bb9e7356cd3)

첫 번째 모델은 초기 Training Loss가 높지만 빠르게 감소하여 안정된 성능을 보이고, Validation Accuracy도 점진적으로 증가한다. 두 번째 모델은 초기 Training Loss가 낮고 빠르게 학습하지만, 후반에 Training Loss가 증가하며 과적합의 징후를 보일 수 있다. 두 모델 모두 좋은 성능을 보이나, 첫 번째 모델이 더 안정적인 학습 과정을 가진다.

### 4.21 첫 번째 학습 모델 원본 데이터에 적용한 결과값
![버트 스팀 결과 1](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/7050c723-57d9-405e-87ea-44471fffbf1c)

### 4.22 두 번째 학습 모델 원본 데이터에 적용한 결과값
![두번째 버트테스트 풀](https://github.com/rlagnldlf/Capstone_Project/assets/136410965/dc28b5e6-3813-4e10-a691-3200f74e2427)

첫 번째 모델은 학습 초기의 높은 Training Loss에도 불구하고, 학습 과정에서 빠르게 안정화되어 높은 Total Accuracy를 달성했다. 반면, 두 번째 모델은 초기 학습이 빠르지만 과적합의 징후를 보이며, 최종 Total Accuracy가 상대적으로 낮았다. 따라서 안정성과 성능을 고려했을 때, 첫 번째 모델이 더 나은 모델로 생각된다.

## 5. 느낀점 및 배운점

학습 데이터가 모델 성능에 큰 영향을 미친다는 것을 알게 되었다. 각 게임당 1000건씩 총 3000건을 균일하게 학습시킨 첫 번째 학습 데이터는 균형 잡힌 데이터로 안정적인 학습 과정을 거쳤다. 반면, 원본 데이터 비율에 맞춰 3000건을 학습시킨 두 번째 학습 데이터는 초기에는 손실값이 더 낮았으나 데이터의 불균형으로 인해 과적합 징후를 보였다. 결과적으로 첫 번째 모델이 더 나은 성능과 안정성을 보여주었다. 이를 통해 학습 데이터의 균형이 중요하며, 초기에는 균형 잡힌 학습 데이터의 정확도가 떨어지더라도 지속적인 학습을 통해 성능이 개선될 수 있다는 것을 알게 되었다. 또한, 데이터를 학습시킬 때 과적합을 방지하기 위한 다양한 기법의 필요성을 느꼈다. 이 프로젝트의 개선점으로는 같은 장르의 다양한 게임 리뷰 데이터를 추가로 수집하고, 과적합을 방지하는 기법을 적용하여 학습시키면 더욱 높은 정확도를 얻을 수 있을 것 같다.
