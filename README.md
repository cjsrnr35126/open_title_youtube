# open_title_youtube
유튜브제목 모으기

<h3>'간결하고 매혹적인' 제목을 찾아서</h3>

<p>우리가 웹에서 정보를 얻을수 있는 수단은 크게 '텍스트'와 '영상'으로 나눌수 있다. 유튜브가 발흥하고 난 후 현재까지의 행보를 보면 오늘날 사람들은 '텍스트'형식의 정보 보다는 '영상'을 선호한다고 추측해 볼 수 있다.<br>
  이와 같은 흐름 속에서 사람들은 유튜브를 통한 수익창출에 많은 관심을 보인다. 동영상 플랫폼 시장의 모든것은 조회수로 귀결된다. 조회수와 가장 연관성 높은 변수는 영상의 '질'이지만, 영상의 '제목'과 '섬네일'또한 못지 않게 중요한 변수라고 할 수 있다. <br>
   "어떠한 영상을 올려야 조회수가 잘 나올까?" <br>
  영상의 '질'에 대한 문제는 논외로 한다면 '제목'과 '섬네일'이 남는다. 그중 제목의 경우, 이미 많은 사람들이 어떻게 해야 좋은 글과 좋은 제목을 지을 수 있을지 고민해 왔다. 이를 '영상'에도 접목시킨다면 아주 쉽게 문제가 해결될 것이다.
</p>
<p>실제로 플랫폼 인기탭에 오르내리는 영상들의 제목을 얻어와서 '<a href='https://brunch.co.kr/@oms1225/104'>매력적인 제목 짓기'</a>매뉴얼이 유의미한지 검증해 볼 것이다.</p>

## R-code

<pre><code>
#install.packages("rvest")
library(rvest)
#유튜브 인기탭 제목 크롤링
y_url="https://www.youtube.com/feed/trending"
html=read_html(y_url)
html1=html %>% html_nodes("div") %>% html_nodes('h3') %>% html_nodes('a') %>% html_text()
html
#네이버tv 인기탭 제목 크롤링
n_url="https://tv.naver.com/i/"
html=read_html(n_url)
inner=html %>% html_nodes("div") %>% html_nodes("dt");#inner
inner_title=inner %>% html_nodes('strong') %>% html_text();#inner_title

content=html %>% html_nodes("div") %>% html_nodes("dt");#content
content_title=content %>% html_nodes("tooltip") %>% html_text(); #content_title

ndf=c(inner_title,content_title);ndf
</code></pre>

-------------------------------------

하루치 정보로는 턱없이 부족하다. 이제 위 코드를 매일 특정시간에 실행시켜서 데이터를 축적해야한다. <br>
자동화를 위해 taskscheduleR 패키지를 사용한다. 그전에 system.file(package= "taskschedulR")를 사용해서 패키지가 저장된 주소를 찾는다.

![캡처](https://user-images.githubusercontent.com/49007889/56267485-eb417600-6129-11e9-97fb-5768211a5fbe.PNG)

이제 script를 taskschduelR의 extdata 폴더에 옮긴다음 아래 코드를 실행한다.

--------------------------

<pre><code>
if(!require(taskscheduleR))install.packages("taskscheduleR")
library(taskscheduleR)
myscript <- system.file("extdata", "exp.R", package = "taskscheduleR")
taskscheduler_create(taskname = "dailyload", rscript = myscript, 
                     schedule = "DAILY", starttime = "10:30", startdate = format(Sys.Date()+1, "%Y/%m/%d"))

</code></pre>

-----------------------------

