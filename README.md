 
# AI 여름학교 빅데이터 기초 실습 (이 자료는 datasicencebox.org의 자료를 활용하였다)

RStudio Cloud에 대한 소개와 간단한 실습

## Example 1 : COVID-19 사망율 자료

### 자료소개

다음자료는 R package **coronavirus** 에서 추출한 코로나-19 사망율 자료이다. 이 자료은 원래 존스홉킨스 대학 Center for Systems Science and Engineering Coronavirus repository에서 추출한 것이다. 

 
 
## Visualisation

다음 시각화는 각나라별로 10번째 확진자가 나온후 날짜별 누적확진자의 숫자를 보여준다. 
 

```{r visualise, warning=FALSE}
ggplot(data = country_data,
       mapping = aes(x = days_elapsed, 
                     y = cumulative_cases, 
                     color = country, 
                     label = end_label)) +
  # represent cumulative cases with lines
  geom_line(size = 0.7, alpha = 0.8) +
  # add points to line endings
  geom_point(data = country_data %>% filter(end_date)) +
  # add country labels, nudged above the lines
  geom_label_repel(nudge_y = 1, direction = "y", hjust = 1) + 
  # turn off legend
  guides(color = FALSE) +
  # use pretty colors
  scale_color_viridis_d() +
  # better formatting for y-axis
  scale_y_continuous(labels = label_comma()) +
  # use minimal theme
  theme_minimal() +
  # customize labels
  labs(
    x = "Days since 10th confirmed death",
    y = "Cumulative number of deaths",
    title = "Cumulative deaths from COVID-19, selected countries",
    subtitle = glue("Data as of", as_of_date_formatted, .sep = " "),
    caption = "Source: github.com/RamiKrispin/coronavirus"
  )
```

## 연습문제 1: UN 국가별 투표

### 자료소개

1946년부터 2015년까지 UN 총회에서 6가지 종류의 안건에 대한 년도별 각 국가의 투표자료로 R package **unvote** 를 이용하면 얻을 수 있다. 

### 분석목표

1. 데이터 시각화를 통해 주요 안건별 각 나라의 투표성향이 시간이 흘러감에 따라 어떻게 변하고 있는지 알아보자.
2. 선진국과 개발도상국사이의 투표패턴을 비교해보자.
3. 주어진 코드를 변형해서 관심이 있는 국가들의 패턴을 알아보자. 이때 국가이름은 마지막에 있는 

### R code

아래 코드에서 Country 부분의 이름를 바꾸어 가면서 코드를 실행해 보자.

```{r plot-yearly-yes-issue, fig.width=10, fig.height=6, message=FALSE}
unvotes %>%
  filter(country %in% c("United Kingdom", "United States", "Turkey")) %>%
  mutate(year = year(date)) %>%
  group_by(country, year, issue) %>%
  summarize(percent_yes = mean(vote == "yes")) %>%
  ggplot(mapping = aes(x = year, y = percent_yes, color = country)) +
  geom_point(alpha = 0.4) +
  geom_smooth(method = "loess", se = FALSE) +
  facet_wrap(~issue) +
  scale_y_continuous(labels = percent) +
  labs(
    title = "Percentage of 'Yes' votes in the UN General Assembly",
    subtitle = "1946 to 2019",
    y = "% Yes",
    x = "Year",
    color = "Country"
  )
```

### References

1.  David Robinson (2017). [unvotes](https://CRAN.R-project.org/package=unvotes): United Nations General Assembly Voting Data. R package version 0.2.0.
2.  Erik Voeten "Data and Analyses of Voting in the UN General Assembly" Routledge Handbook of International Organization, edited by Bob Reinalda (published May 27, 2013).
3.  Much of the analysis has been modeled on the examples presented in the [unvotes package vignette](https://cran.r-project.org/web/packages/unvotes/vignettes/unvotes.html).



## Exampe 2 : Shiny 소개

## 연습문제 2 : Shiny 실습
