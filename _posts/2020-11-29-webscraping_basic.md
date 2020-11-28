---
layout: post
title:  "webscraping_basic"
date:   2020-11-29
excerpt: ""
tag:
- python
- Requests
- re
- Selenium
comments: true
---
[git 바로가기](https://github.com/rssungjae1/webscraping_basic/tree/master)

## <정규식>
python 기본 라이브러리
import re
  - .(ca.e) # 하나의 문자
  - &#94;(^de) # 문자열의 시작
  - $(se$) # 문자열의 끝

  - compile() # 정규식 패턴을 입력으로 받아들여 정규식 객체를 리턴
  - match() # 처음부터 일치하는지
  - search() # 일치하는 게 있는지
  - findall() # 일치하는 것 모두 리스트로

---

## group() 
  - 매치객체.group(그룹숫자)
```python
m = re.match('([0-9]+) ([0-9]+)', '10 295')
m.group(1)    # 첫 번째 그룹(그룹 1)에 매칭된 문자열을 반환
# '10'
m.group(2)    # 두 번째 그룹(그룹 2)에 매칭된 문자열을 반환
# '295'
m.group()     # 매칭된 문자열을 한꺼번에 반환
# '10 295'
m.group(0)    # 매칭된 문자열을 한꺼번에 반환
# '10 295'
```

---

## <Requests>
웹 페이지 읽어오기
빠르다, 동적 웹 페이지 불가
주어진 url을 통해 받아온 html에 원하는 정보가 있을때

Requests를 통해 받아온 html이 정상인지 확인
```python
res.raise_for_status()
```

---

## <Selenium>
웹 페이지 자동화
느리다, 동적 웹 페이지 가능
로그인, 어떤 결과에 대한 필터링 등 어떤 동작을 해야하는 경우
chromedriver.exe 가 반드시 있어야 한다.
```python
find_element(s)_by_tag_name("a")
find_element(s)_by_id("id")
find_element(s)_by_class_name()
find_element(s)_by_link_text()
find_element(s)_by_xpath()

click() # 클릭
send_keys() clear # 글자 입력

# 로딩될때까지 기다려야 할때,
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

try:
    # 검색 결과 대기
    elem = WebDriverWait(browser, 10)
    .until(EC.presence_of_element_located((By.XPATH, "copy한 xpath")))
    # 성공했을 때 동작 수행
finally:
    browser.quit()

# 스크롤 다운
import time
interval = 2 # 2초에 한번씩 스크롤 내림
# 현재 문서 높이를 가져와서 저장
prev_height = browser.execute_script("return document.body.scrollHeight")

# 반복 수행
while True:
    # 화면 가장 아래로 스크롤 내리기
    browser.execute_script("window.scrollTo(0, document.body.scrollHeight)")

    # 페이지 로딩 대기
    time.sleep(interval)

    # 현재 문서 높이를 가져와서 저장
    curr_height = browser.execute_script("return document.body.scrollHeight")
    if curr_height == prev_height:
        break
    
    prev_height = curr_height
```

---

## <Headless chrome>
브라우저를 띄우지 않고 동작
때로는 User-Agent 정의 필요

---

## <BeautifulSoup>
추출
```python
find # 조건에 맞는 첫 번째 element
find_all # 조건에 맞는 모든 element 리스트로
find_next_sibling(s) # 다음 형제 찾기
find_previous_sibling(s) # 이전 형제 찾기

soup["href"] # 속성
soup.get_text() # 텍스트
```

---

## <이미지 다운로드>
```python
with open("파일명", "wb") as f:
    f.write(res.content)
```

---

## <csv 다운로드>
```python
import csv
f = open(filename, "w", encoding("utf-8-sig", newline=""))
```

---
