# Propensity Score Matching
  
  
  일반적으로 어떠한 서비스(treatment)가 효과가 있는지를 알고싶을 때, 서비스를 이용한 집단과 이용하지 않은 집단의 단순 비교를 통해서는 정확한 효과를 알 수 없다. 비교하는 두 집단의 특징이 달라 해당 서비스를 이용할 확률이 다른 경우, 다른 요소들(covariate)로 인한 효과가 함께 나타나기 때문이다.    

따라서 확인하고자 하는 서비스의 순효과를 알기 위해서는 서비스를 이용한 집단(실험군)과 서비스를 이용하지 않은 집단(대조군)의 특성이 유사하도록 구성해야 한다. 하지만 실험연구와 달리 관찰연구의 특징 상 실험군과 유사한 대조군을 구성하기란 매우 어렵다.  
   
   
 이런 경우 이용할 수 있는 방법 중 하나가 PSM 방법이다.
 
 
 #### Propensity Score의 정의
 Propensity Score란 공변량이 주어졌을 때 특정 Treatment에 속할 확률이다.  
 즉, 'Treatment=1'을 기준으로 집단의 특성을 요약한 지표로써 점수가 동일하다면 실험군에 속할 확률이 같다고 볼 수 있고, 실험군과 대조군의 PS 분포가 비슷하게 만들어줌으로써 두 그룹의 특성 차이를 줄여줄 수 있다.   
 
 이를 통해 Treatment에 영향을 주는 covariate의 효과를 모두 상쇄하고 난 후, 정확히 treatment가 outcome에 미치는 영향을 측정할 수 있다.  
 또한 몇가지 조건을 추가적으로 만족하면 인과관계로도 설명이 가능해진다.
  
  
 #### 분석 요소
| 구분 |    Covariate(x)   |                  Treatment(y)                  |           Outcome           |
|:--------:|:--------------------------:|:-------------------------------------------------------:|:----------------------------------:|
| 정의 | 통제하고자 하는 변수들     | 순효과를 알고싶은 변수.  실험군과 대조군을 구분하는 변수 | 효과성을 판단하는 기준이 되는 변수 |
| 시점 | Treatment 이전 시점 데이터 |                                                         | Treatment 이후 시점 데이터         |
|  예  | 나이, 성별                       | 이벤트 참여 여부                                        | 매출                               |

- 분석 요소의 데이터 발생 시점이 매우 중요한데 *Covariate -> Treatment -> Outcome*의 순으로 구성되어야 한다. 
  

 #### PS를 구하는 방법
PS를 구하는 방법은 어렵지 않다. `공변량이 주어졌을 때 Treatment에 속할 확률`이므로 Treatment를 y로, Covariate을 x로 두고 모델링을 하면 된다.
  
  
 #### PS를 이용하여 대조군을 구성하는 방법
  |                 방법                |                                                                개념                                                               |                                                                                                      기타                                                                                                      |
|:-----------------------------------:|:---------------------------------------------------------------------------------------------------------------------------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|
|           매칭 (Matching)           | 처리군의 객체와 성향점수가 유사한 대조군의 객체를 매칭하고, 매칭 결과로 얻어진 새로운 표본을 이용하여 처리의 효과를 예측하는 방법 | 4가지 방법 중 bias reduction 측면에서 가장 좋은것으로 알려져 있음(Austin, 2008) 처리 속도 이슈가 존재하며, 많은 객체가 분석에 고려되지 않는 경우 두 집단간 불균형이 더 심해질 수도 있음(King and Neisen, 2015) |
|       계층화 (Stratification)       | 처리군에 속하는 객체들의 성향점수에 따라 4-5개의 그룹으로 나누고,  대조군에 속하는 객체들을 각 그룹에 할당하는 방법.              | 계층화의 극단적인 경우가 매칭 방법                                                                                                                                                                             |
| 확률 가중치 (Probability Weighting) | 성향점수를 가중치로 활용하여 처리 효과를 보정하는 방법.                                                                           | 가중치의 분산이 큰 경우 효과 추정치의 분산도 커짐 모든 객체를 사용하는 장점이 있지만, 이상값에 민감                                                                                                            |
|  공변량 조정 (Covariate adjustment) | 성향점수와 결과 사이의 관계를 가정하고  회귀식을 이용하여 처리효과를 예측하는 방법                                                | 성향점수와 결과 사이에 가정된 관계가 만족되지 않은 경우 효과 추정치가 부정확함                                                                                                                                 |

  
    
#### 참고 사이트
* https://sejdemyr.github.io/r-tutorials/statistics/tutorial8.html
