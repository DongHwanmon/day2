# 2018년 12월 18일

## 오늘 배운 내용 및 소스코드

### 다음웹툰

import requests
import time
import json
from bs4 import BeautifulSoup as bs

today = time.strftime("%a").lower()#오늘 날짜 요일
#print(today)
'''
1.내가 원하는 정보를 얻을 수 있는 주소를 url이라고 하는 변수에 담는다.
2.해당 url에 요청을 보내 응답을 받아 저장한다.
3.구글신에게 python으로 어떻게 json을 파싱(딕셔너리 형으로 변환)하는지 물어본다.
4.파싱한다.(변환한다)
5.내가 원하는 데이터를 꺼내서 조합한다.
'''

daum_url = 'http://webtoon.daum.net/data/pc/webtoon/list_serialized/thu'
response = requests.get(daum_url).text
document = json.loads(response)
print(document)
data = document["data"]

for toon in data:
  print(toon["title"])
  print(toon["pcThumbnailImage"]["url"])
  print("http://webtoon.daum.net/webtoon/view/{}".format(toon["nickname"]))

#찾을떄는 클래스 또는 각각의이름으로 찾는다?
'''toons = [{"title":?,
​         "url":?,
​         "img_url":?}
]'''

'''
for item in li:
  toon = {"title":item.select('dt a')[0].text,#같은의미 "title":item.select_one('dt a').text
​          "url":item.select('dt a')[0]["href"],
​          "img_url":item.select('.thumb img')[0]["src"]#src라는 키값으로 값을 구한다는 의미
  }
  '''
'''
for item in li:
  toon = {"title":item.select('.thumb a')[0]["title"],#같은의미 "title":item.select_one('dt a').text
​          "url":item.select('.thumb a')[0]["href"],
​          "img_url":item.select('.thumb img')[0]["src"]#src라는 키값으로 값을 구한다는 의미
  }
  toons.append(toon)
'''

### 로또번호 추출기 및 번호 맞추기

import requests
import random

from bs4 import BeautifulSoup as bs

#as를 쓰면 BeautifulSoup를 bs로 사용가능

url = 'https://m.dhlottery.co.kr/common.do?method=main'
response = requests.get(url).text
#print(response)
soup = bs(response, 'html.parser')
document = soup.select('.prizeresult')[0]
numbers = document.select('span')
ns = []
for number in numbers:
  ns.append(int(number.text))

print(ns)

numbers = range(1,46)
lotto_num=random.sample(numbers,6)
lotto_num.sort()#오름차순

print(lotto_num)
lotto_real = ns
#지난주 로또 번호와 추출된 랜덤 로또 번호가
#한번씩 순회 하면서 몇개가 맞았는지 카운트하기
cnt = 0
'''
for a in range(0,6) : 
  for b in range(0,6) :
​    if(lotto_num[a] == lotto_real[b]):
​      cnt+=1
​      #lotto_real[b] = 0
'''   
for num in ns:
  if num in lotto_num: # 배열에서 어떤 요소가 있는지 확인하는 방법
​    cnt=cnt+1
​      


cnt_2nd = 0
​      
print("맞춘숫자의 개수는 : {} ". format(cnt))

if cnt == 1:
  print("꼴등입니다..")
elif cnt == 2:
  print("꼴등입니다..")
elif cnt == 3:
  print("6등 5000원에 당첨되셧어요~")
elif cnt == 4:
  print("5등에 당첨되셧어요~")
elif cnt == 5:
​    for b in range(0,6) :
​      if(lotto_num[b] == lotto_real[6]):
​        cnt_2nd+=1
​        #lotto_real[b] = 0
​      if(cnt_2nd==1):
​        print("2등에 당첨되셧어요~ 축하드립니다~")
​      else : 
​        print("3등에 당첨되셧어요~ 축하드립니다~")
elif cnt == 6:
​      print("1등에 당첨되셧어요~!!!!!!!!!!!!")
else:
​    print("꼴등입니다..")
​      
​    

# 네이버웹툰

import requests
import time
from datetime import date
from bs4 import BeautifulSoup as bs

#today = date.today()
#today_day = today.weekday()
#print(today_day)
today = time.strftime("%a").lower()#오늘 날짜 요일
print(today)
#1.네이버 웹툰을 가져올수 있는 주소를 찾는다 파악한다 url변수에 저장하고
#2.해당주소로 요청을 보내 정보를 가져온다.
#3.받은 정보를 뷰티플스프bs를 이용해서 검색하기 좋게 만든다.
#4.네이버웹툰페이지로가서 내가 원하는 정보가 어디에 있는지 파악한다
#5.내가 원하는 정보는 웹툰을 볼 수 있는 링크
#6.오늘자 업데이트 된 웹툰들의 각각 리스트 페이지 , 그리고 웹툰의 제목+해당웹툰의 썸네일까지
#7.3번에서 저장한 정보를 이용해서 4번에서 파악한 정보의 위치를 찾는다.
naver_url = 'https://comic.naver.com/webtoon/weekdayList.nhn?week='+today
response = requests.get(naver_url).text
soup = bs(response, 'html.parser')
'''toons = [{"title":?,
​         "url":?,
​         "img_url":?}
]'''
li = soup.select('.img_list li')

toons = []
'''
for item in li:
  toon = {"title":item.select('dt a')[0].text,#같은의미 "title":item.select_one('dt a').text
​          "url":item.select('dt a')[0]["href"],
​          "img_url":item.select('.thumb img')[0]["src"]#src라는 키값으로 값을 구한다는 의미
  }
  '''
for item in li:
  toon = {"title":item.select('.thumb a')[0]["title"],#같은의미 "title":item.select_one('dt a').text
​          "url":item.select('.thumb a')[0]["href"],
​          "img_url":item.select('.thumb img')[0]["src"]#src라는 키값으로 값을 구한다는 의미
  }
  #print(toon)
  toons.append(toon)


print(toons)#별명이 없을떄는 그냥 쓰면된다



# 오늘 배운 내용

### json,html,xml 등을 가져와서 텍스트로 가져와서 그곳에 있는 data들을 원하는것을 찾아서 보여준다.

### html에서 클래스 별명일때는 .을 붙여서 검색
ID로 검색할떄는 #아이디로 검색
빈칸을 띄우면 그 클래스 안에 있는것에서 띄우고 쓴 것을 검색한다는 의미

```
git init
처음 폴더 만들고 폴더에 git.파일 없을때 만드는 명령어
git add README.md
README.md파일을 생성하는 명령어
git commit -m "first commit"
첫번째 커밋이라는 것을 언급하고 깃에 생성한다.

git remote add origin https://github.com/DongHwanmon/day2.git
원격으로 어디에 저장을 할건지 연결을 시키는 명령어
git push -u origin master
등록한 허브에 올린다

mkdir day2
day2폴더를 생성한다.

touch add.py
add.py파일을 생성한다.
```



