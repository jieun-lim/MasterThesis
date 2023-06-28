# 다중회귀 분석이 필요한가? 
- 맥락에 따른 변이성 추정(CHAPTER 19.6.6 , p.1106)


```{r intercept}
# 절편만 있는 모델 
interceptOnly <-gls(responserate~1, data = data, method="ML")
summary(interceptOnly)
```

```{r linear mixed model }


# lme - linear mixed model 
randomInterceptOnly <-lme(responserate~1, data = data,random = ~1|uid, method="ML")
summary(randomInterceptOnly)
```

```{r compare }


# 앞의 두모델 비교 
anova(interceptOnly,randomInterceptOnly)
# p가 threshold 보다 작을 때, 유ㅇ의미한 차이가 있다. 
# p<0.0001, therefore, multi-level regression is worth being conducted on this data
```



# 개별 모델 비교

```{r lme}
# before run model, let's select useful factors
randomInterceptOnly <-lme(responserate~1, data = data,random = ~1|uid, method="ML")


randomIntercept_userDemos <- lme(responserate ~ gender
               + Extroversion_score + Agreeableness_score + Conscientiousness_score + Neuroticism_score + Openness_score
               + PHQ9_score + GAD7_score + PSS_score + GHQ_score, data = data, random = ~1|uid, method = "ML")

randomIntercept_day <- lme(responserate ~ dayofweek + weekend  , data = data, random = ~1|uid, method = "ML")
randomIntercept_time <-lme(responserate ~ dawn + morning + daytime + evening   , data = data, random = ~1|uid, method = "ML")
randomIntercept_sensor <-lme(responserate ~ + human + light + co2 + noise , data = data, random = ~1|uid, method = "ML")

#anova(randomInterceptOnly,randomIntercept_userDemos)
#anova(randomInterceptOnly,randomIntercept_day)
anova(randomInterceptOnly,randomIntercept_time) # p<.001
anova(randomInterceptOnly,randomIntercept_sensor) #p<.001
```


```{r lme 2 variables }
# before run model, let's select useful factors
randomInterceptOnly <-lme(responserate~1, data = data,random = ~1|uid, method="ML")


randomIntercept_time <-lme(responserate ~ dawn + morning + daytime + evening   , data = data, random = ~1|uid, method = "ML")
randomIntercept_sensor <-lme(responserate ~ human + light + co2 + noise + (1 | uid), data = data, method = "ML")
randomIntercept_time_sensor <-lme(responserate ~ dawn + morning + daytime + evening+human + light + co2 + noise, data = data, random = ~1|uid, method = "ML")

anova(randomInterceptOnly,randomIntercept_time,randomIntercept_time_sensor)

```
