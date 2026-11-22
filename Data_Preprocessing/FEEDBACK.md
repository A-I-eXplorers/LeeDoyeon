
## 피드백

데이터셋 처리과정이 다 비슷해서, 
가장 분석할 거리가 많고 복잡하다고 할수있는 타이타닉 데이터셋 기준 피드백


(1) 데이터 불러오는 방식

 >fetch_openml 을 통해 sklearn 에서 fetch 하는 방식으로 가져옴 
 >(전체 데이터, 1309)

이충희 => pandas 라이브러리로 웹에있는 csv 직접 불러오는 방식

심소은 => seaborn 사용해서 load 하는 방식

우유정 => 마찬가지로 seaborn 사용해 load

여러 방식으로 데이터 가져올 수 있음을 배움 (891, 1309로 규모가 다른 데이터셋)



(2) 데이터 처리 및 분석

> 결측치 처리 시 중앙값/최빈값으로 대체하거나 파생변수를 만들었음
> 분석에 큰 의미 없는 컬럼들은 제외 (drop)
> fare 값이 치우쳐 있어 log scale 로 변환

```python
df['age'] = df['age'].fillna(df.groupby(['sex', 'pclass'])['age'].transform('median'))
```
추가로 위와 같이 나이는 성별, 객실별 중앙값으로 채워줌 



이충희 
=> embarked, embarked_town이 nan인 행을 최빈값으로 대체하는 대신 제거함으로써 왜곡이 비교적 적도록 하였으며 학습셋 테스트셋 분리 및 표준화(StandardScaler)하여 로지스틱 회귀까지 실행 및 결과 확인 : 실제 학습과정을 위한 데이터 처리 과정이므로, 데이터 처리의 목표가 확실함

주석: 
```python
#df.dropna(subset=['embarked'])
#df.dropna(subset=['embark_town'])

df = df.dropna(subset=['embarked'])
df = df.dropna(subset=['embark_town'])
```
위와 같이 수정이 필요해 보임  (이전 코드는 dropna가 df에 실제로 적용 안될 것으로 예상)



심소은 => 연령대별 생존 평균을 시각화하여 가시적으로 분석 가능했음

주석: 중복 의미 칼럼과 embarked, deck은 처리하지 않은 것으로 보임.



우유정 => 동일한 방식으로 log scale 변환 방식 채용, 가족 수별 생존율을 시각화하여 더 입체적인 정보를 알 수 있었으며
`is_alone` 으로 개인/단체의 생존율 차이 직관적 분석도 가능했음

주석: sex, sex_encoded,  alone, is_alone 의미적 중복칼럼 (처리 전 칼럼) drop_cols 추가하면 모델에 넣기 더 쉬워질 것으로 보임 