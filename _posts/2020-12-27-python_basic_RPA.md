---
layout : post
title : "파이썬_업무자동화"
date : 2020-12-27
excerpt : "파이썬 업무자동화 공부"
tag :
- post
- python
- openpyxl
- pyautogui
- selenium
- account
- imap_tools
comments : true
---
[git 바로가기](https://github.com/rssungjae1/python_rpa_basic/tree/dev)

## 엑셀
python의 openpyxl라이브러리 사용
pip install openpyxl

(wb : 워크북)
.active
.create_sheet("sheet1")
.save("sample.xlsx")
.close()
.copy_worksheet(new_ws)
.sheetnames
wb = load_workbook("sample.xlsx")

(ws : 워크시트)
.title
.sheet_properties.tabColor

(셀에 값 넣기)
```python
ws["A3"] = 3
ws["B1"] = 4
ws["A1"].value
ws.cell(column=3, row=1, value=10)
```

(셀에서 값 가져오기)
```python
print(ws.cell(row=1, column=1).value)
```

(셀 범위, 이동)
col_range = ws["B:C"] # column
row_range = ws[2:6] # row
```python
for row in ws.iter_rows(min_row = 2, max_row=11, min_col=2, max_col=3): # row 2~11, column 2~3 범위
  print(row[0].value, row[1].value) # 수학, 영어

ws.move_range("B1:C11", rows=0, cols=1) # (a,b,c) a범위를 기준으로 b,c만큼 이동
```

(셀 정보 출력)
```python
xy = coordinate_from_string(cell.coordinate) # A/10 AZ/250
```

(셀 스타일)
```python
# A열의 너비를 5로 설정
ws.column_dimensions["A"].width = 5
# 1행의 높이를 5로 설정
ws.row_dimensions[1].height = 50
# 스타일 적용
a1.font = Font(color="FF0000", italic=True, bold=True)
b1.font = Font(color="CC33FF", name="Arial", strike=True)
c1.font = Font(color="0000FF", size=20, underline="single")
# 테두리 적용
thin_border = Border(left=Side(style="thin"), right=Side(style="thin"), top=Side(style="thin"), bottom=Side(style="thin"))
a1.border = thin_border
b1.border = thin_border
c1.border = thin_border
```

(셀 연산)
```python
ws["A2"] = "=SUM(1,2,3)"
ws["A3"] = "=AVERAGE(1,2,3)"
# point
# 수식이 아닌 실제 데이터 가져오기
# evaluate 되지 않은 상태의 데이터는 None
# 엑셀 파일을 다시 열었다가 다시 저장하고 하는걸로 ㄱ
# a파일 열고 저장하고 a파일을 가지고 b파일을 가지고 수식연산하려면 불가
# 사람이 열고 저장해야한다.
```

(셀 합치기)
```python
ws.merge_cells("B2:D2")
```

(row, column 추가, 삭제)
```python
ws.insert_rows(8, 5) # 8번째 줄 위치에 5줄 추가
ws.insert_cols(2, 4) 
ws.delete_rows(8,2)
ws.delete_cols(2)
```

(차트)
엑셀 업무에서 꼭 필요한 차트
```python
from openpyxl.chart import BarChart, Reference, LineChart
bar_value = Reference(ws,min_row=2, max_row=11, min_col=2, max_col=3) # 차트 데이터 지정
bar_chart = BarChart() # 차트 종류 설정(Bar, Line, Pie...)
bar_chart.add_data(bar_value)
ws.add_chart(bar_chart, "E1") # 차트 넣을 위치 정의

line_chart.style  =20 # 미리 정의된 스타일을 적용(스타일은 구글링)
```

(이미지 추가)
```python
from openpyxl.drawing.image import Image
wb = Workbook()
ws = wb.active

img = Image("img.png")
ws.add_image(img, "C3")
```

```python
pip install openpyxl
from openpyxl import Workbook
wb = Workbook() # 새 워크북 생성

ws = wb.active # 현재 활성화 된 sheet 가져옴
ws.title = "SongSheet" # 워크시트 이름변경
ws.sheet_properties.tabColor = "ccffff" # RGB형태로 값을 넣어 색 변경
wb.save("sample.xlsx") # 워크북 저장
wb.close()
```

---

## desktop
window전용
python의 pyautogui라이브러리 사용
pip install pyautogui

(화면)
화면 사이즈 좌표로 주로 이용
```python
pyautogui.size()
```

(마우스)
duration 이동시간
```python
# 절대좌표
pyautogui.moveTo(100,200, duration= 1)
# 상대좌표
pyautogui.move(100,100, duration= 1)
# 현재 마우스 위치
pyautogui.position()
```

(마우스 액션)
```python
# 클릭
pyautogui.click(1321, 19, duration=1)
# 클릭 수
pyautogui.doubleClick()
pyautogui.click(clicks=2)
# 마우스 클릭(다운) 버튼떼기(업)
pyautogui.mouseDown()
pyautogui.mouseUp()
# 마우스 드래그
pyautogui.drag(100,100, duration = 0.25)
# 마우스 스크롤
pyautogui.scroll(300) # 양수이면 윗방향, 음수이면 아래방향으로
# 마우스 정보 출력
pyautogui.mouseInfo()
```

(스크린샷)
```python
img = pyautogui.screenshot()
img.save("screenshot.png")
# 해당 픽셀의 컬러가 맞는지 확인
print(pyautogui.pixelMatchesColor(1294,16, (34, 166, 242)))
```

(이미지 인식)
locateOnScreen
png파일 인식
```python
file_menu = pyautogui.locateOnScreen("file_menu.png")
# 흑백버전
trash_icon = pyautogui.locateOnScreen("trash_icon.png", grayscale=True)
# 범위 지정
trash_icon = pyautogui.locateOnScreen("trash_icon.png", region=(2292, 522, 200, 300))
# 정확도 지정 70퍼센트
trash_icon = pyautogui.locateOnScreen("run_btn.png", confidence=0.7)
```

(윈도우)
```python
# 현재 활성화된 창
fw = pyautogui.getActiveWindow() 
# 창의 제목 정보
print(fw.title) 
# 창의 크기 정보 (width, height)
print(fw.size) 
# 창의 좌표 정보
print(fw.left, fw.top, fw.right, fw.bottom) 
# 모든 윈도우 가져오기
for w in pyautogui.getAllWindows():
    print(w) 
# 최소화
w.minimize() 
# 최대화
w.maximize() 
# 윈도우 닫기
w.close() 
```

(키보드)
```python
pyautogui.write("NadoCoding", interval=0.25)
# 한글일 경우, copy paste해서 출력
import pyperclip
pyperclip.copy("ㄱㄴㄷㄹ")
# 조합키 (Hot Key)
pyautogui.hotkey("ctrl", "alt", "shift", "a")
```

(메세지 박스)
```python
# 메세지 박스
pyautogui.alert("자동화 수행에 실패하였습니다.", "경고")
# 확인창
result = pyautogui.confirm("계속 진행하시겠습니까?", "확인")
# 입력창
result = pyautogui.prompt("파일명을 무엇으로 하시겠습니까?", "입력")
# 패스워드 입력착
result = pyautogui.password("암호를 입력하세요")
```

(로그)
```python
import logging
from datetime import datetime
# 시간 [로그레벨] 메시지 형태로 로그를 작성
logFormatter = logging.Formatter("%(asctime)s [%(levelname)s] %(message)s") 
logger = logging.getLogger()
# 로그 레벨 설정
logger.setLevel(logging.DEBUG)
# 스트림 (터미널)
streamHandler = logging.StreamHandler()
streamHandler.setFormatter(logFormatter)
logger.addHandler(streamHandler)
# 파일
filename = datetime.now().strftime("./2_desktop/log/mylogfile_%Y%m%d%H%M%S.log") # mylogfile_20201010141011.log
fileHandler = logging.FileHandler(filename, encoding="utf-8")
fileHandler.setFormatter(logFormatter)
logger.addHandler(fileHandler)
```

(파일 cmd)
```python
# https://github.com/rssungjae1/python_rpa_basic/blob/dev/2_desktop/11_file_system.py 참조
```

---

## web
selenium 데이터 크롤링
https://rssungjae1.github.io/rssungjae1_kr//python_basic_webscraping/

---

## email
gmail
python의 smtplib, EmailMessage, imap_tools라이브러리 사용
pip install imap_tools

(account)
gmail의 어카운트를 사용한다. 비밀번호는 구글 개인 계정 정보보호설정이 필요함
참조 : https://cpuu.postype.com/post/23066
```python
EMAIL_ADDRESS = "" # 주소
EMAIL_PASSWORD = "" # 앱 비밀번호
```

(send)
```python
import smtplib
from account import *

with smtplib.SMTP("smtp.gmail.com", 587) as smtp:
    smtp.ehlo() # 연결이 잘 수립되는지 확인
    smtp.starttls() # 모든 내용이 암호화되어 전송
    smtp.login(EMAIL_ADDRESS, EMAIL_PASSWORD) # 로그인

    subject = "test mail" # 메일 제목
    body = "mail body" # 메일 본문

    msg = f"Subject: {subject}\n{body}"

    # 발신자, 수신자, 정해진 형식의 메시지
    smtp.sendmail(EMAIL_ADDRESS, "nadocoding@gmail.com", msg) 
```

(recive)
```python
from imap_tools import MailBox
from account import *

mailbox = MailBox("imap.gmail.com", 993)
mailbox.login(EMAIL_ADDRESS, EMAIL_PASSWORD, initial_folder="INBOX")

# limit : 최대 메일 갯수
# reverse : True 일 경우 최근 메일부터, False 일 경우 과거 메일부터
for msg in mailbox.fetch(limit=1, reverse=True):
    print("제목", msg.subject)
    print("발신자", msg.from_)
    print("수신자", msg.to)
    #print("참조자", msg.cc)
    #print("비밀참조자", msg.bcc)
    print("날짜", msg.date)
    print("본문", msg.text)
    print("HTML 메시지", msg.html)
    print("=" * 100)
    # 첨부 파일 없으면 list len[] 0 값
    print(msg.attachments)
    # 첨부 파일
    for att in msg.attachments:
        print("첨부파일 이름", att.filename)
        print("타입", att.content_type)
        print("크기", att.size)

        # 파일 다운로드
        with open("download_" + att.filename, "wb") as f:
            f.write(att.payload)
            print("첨부 파일 {} 다운로드 완료".format(att.filename))

mailbox.logout()
```

