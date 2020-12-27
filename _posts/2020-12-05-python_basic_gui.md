---
layout: post
title:  "파이썬_GUI"
date:   2020-12-05
excerpt: "파이썬 GUI tkinter공부"
tag:
- python
- tkinter
- messagebox
- filedialog
- PIL
comments: true
---
[git 바로가기](https://github.com/rssungjae1/python_basic_gui/tree/main)

## tkinter
python의 gui라이브러리 사용

```python
from tkinter import *
root = Tk()
root.title("SONG Scheduler")
root.geometry("640x480") # 가로 * 세로
root.resizable(False, False) # x, y 값 변경 불가( 창 크기 변경 불가)
root.mainloop()
```

## 버튼
버튼
  - text : 텍스트
  - padx : x축 공백
  - pady : y축 공백
  - width : 넓이
  - height : 높이
  - fg : 폰트 색깔
  - bg : 배경 색깔
  - image : 이미지 삽입

```python
btn1 = Button(root, text="button1",padx = 10, pady = 5, width = 10, height = 3, fg = "red", bg = "yellow")
btn1.pack()

photo = PhotoImage(file="python_gui_basic\img.png")
btn2 = Button(root, image = photo)
btn2.pack()
```

## 라벨
텍스트 표시

```python
label1 = Label(root, text="hi")
label1.pack()
```

## 글자 입력
  - Text() : 여러 줄 가능
  - Entry() : 한줄만 가능
  - .insert(END, "string") : END(글자시작 커서), "string" 입력 할 string
  - .get("1.0", END) : "줄.몇번째 인덱스", END(글자검색 커서)

```python
txt = Text(root, width=30, height=5)
txt.pack()
txt.insert(END, "글자를 입력하세요.")

e = Entry(root, width=30)
e.pack()
e.insert(0, "한 줄만 입력하세요.")
print(txt.get("1.0", END))
print(e.get())
```

## 리스트 박스

  - selectmode : "extended" 여러개 선택가능, "single" 하나만 선택가능
  - height : =n n개만큼 표시
  - .curselection() : 현재 선택된 항목 확인
  - .get(0,2) : 시작 인덱스, 끝 인덱스

```python
listbox = Listbox(root, selectmode="extended", height=0)
listbox.insert(0, "사과")
listbox.insert(1, "딸기")
listbox.insert(END, "수박")
listbox.pack()

print("선택된 항목 : ", listbox.curselection())
```

## 체크 박스

  - variable : 체크박스 맵핑, 체크한 값을 int값으로 저장해서 확인한다.
  - .select : 디폴트 선택값을 체크한다.
  - .deselect : 선택값을 해제한다.

```python
chkvar = IntVar() # chkvar에 int형으로 값을 저장한다
chkbox = Checkbutton(root, text="오늘 하루 보지 않기", variable=chkvar)
# chkbox.select() # 자동 선택 처리
# chkbox.deselect() # 선택 해제 처리
chkbox.pack()
print(chkvar.get()) # 0 : 체크 해제, 1 : 체크
```

## 라디오 버튼
개중 하나만 체크 가능

  - variable : 라디오박스 맵핑, 체크한 값을 int값으로 저장해서 확인한다.
  - .select : 디폴트 선택값을 체크한다.

```python
burger_var = IntVar() # 여기에 int 형으로 값을 저장한다
btn_burger1 = Radiobutton(root, text="햄버거", value=1, variable=burger_var)
btn_burger1.select() # 기본값 선택
btn_burger1.pack()
burger_var.get()
```

## 콤보박스
하나씩 체크할 수 있는 박스

  - values : 항목
  - .state : readonly 읽기전용(수정불가)
  - .current : 현재 값 확인

```python
import tkinter.ttk as ttk
values = [str(i) + "일" for i in range(1, 32)] # 1~31
combobox = ttk.Combobox(root, height=5, values=values)
combobox.pack()
combobox.set("카드 결제일")    

readonly_combobox = ttk.Combobox(root, height=5, values=values, state="readonly") 
readonly_combobox.current(0) # 0번째 인덱스 값 선택
readonly_combobox.pack()
```

## 진행상황
퍼센트바

  - variable : 퍼센트 int값
  - maximum : 최대값
  - length : 표현 길기
  - set : 숫자 맵핑
  - update : ui 업데이트

```python
import tkinter.ttk as ttk
p_var2 = DoubleVar()
progressbar2 = ttk.Progressbar(root, maximum = 100, length=150, variable=p_var2)
progressbar2.pack()

def btncmd():
    for i in range(101): # 1~100
        time.sleep(0.01) # 0.01 초 대기
        
        p_var2.set(i)
        progressbar2.update() # ui 업데이트
        print(p_var2.get())
  
```

## 메뉴
 - label : 텍스트
 - command : 선택 시 실행 함수
 - add_separator : 구분선 
 - state : 활성화/ disable 비활성화
 - add_cascade : 메뉴 항목 추가
 - add_radiobutton : 라디오 버튼 활성화

```python
menu = Menu(root)
menu_file = Menu(menu, tearoff=0)
menu_file.add_command(label="New File", command = create_new_file)
menu_file.add_separator()
menu.add_cascade(label="File", menu=menu_file)
menu_lang.add_radiobutton(label="Python")
```

## 메세지 박스

 - title : 메세지 박스 제목
 - message : 메세지 박스 내용

```python
import tkinter.messagebox as msgbox
msgbox.showinfo("알림", "정상적으로 예매 완료 되었습니다.")
msgbox.showwarning("경고", "해당 좌석은 매진 되었습니다.")
msgbox.showerror("에러", "가능한 접근이 아닙니다.")
msgbox.askokcancel("확인 / 취소", "해당 좌석은 유아동반석입니다. 예매하시겠습니까?")
msgbox.askretrycancel("재시도 / 취소", "일시적인 오류입니다. 다시 시도하시겠습니까?")
msgbox.askyesno("예 / 아니오", "해당 좌석은 역방향입니다. 예매하시겠습니까?")
```

## 프레임
화면 프레임 적용

 - .pack(side="bottom") : bottom, top, left, right
 - .pack(fill="both", expand="True") : fill 채우기, expand 변형가능
 - Frame(root, relief="solid", bd=1) : relief 선, bd 선 두깨
 - LabelFrame(root, text="음료") : 라벨 프레임

```python
frame_burger = Frame(root, relief="solid", bd=1)
frame_burger.pack(side="left", fill="both", expand=True)
Button(frame_burger, text="햄버거").pack()

frame_drink = LabelFrame(root, text="음료")
frame_drink.pack(side="right", fill="both", expand="True")
Button(frame_drink, text="콜라").pack()
```

## 스크롤 바

 - .pack(side="right", fill="y") : y y축 채우기
 - .config : 맵핑

```python
scrollbar = Scrollbar(frame)
scrollbar.pack(side="right", fill="y")
listbox = Listbox(frame, selectmode="extended", height=10, yscrollcommand=scrollbar.set)
scrollbar.config(command=listbox.yview) # 맵핑
```

## 그리드
파티션처럼 나눠서 프레임을 맞추는 것

```python
btn_1 = Button(root, text="1", width=5, height=2) # padx, pady 버튼 자체 크기
btn_2 = Button(root, text="2", width=5, height=2)
btn_3 = Button(root, text="3", width=5, height=2)
btn_enter = Button(root, text="enter", width=5, height=2) # 세로로 합쳐짐

btn_1.grid(row=4, column=0, stick=N+E+W+S, padx=3, pady=3) # stick : 지정한 방향으로 채우기
btn_2.grid(row=4, column=1, stick=N+E+W+S, padx=3, pady=3)
btn_3.grid(row=4, column=2, stick=N+E+W+S, padx=3, pady=3)
btn_enter.grid(row=4, column=3, rowspan=2, stick=N+E+W+S) # 현재 위치로부터 아래쪽으로 합쳐짐
```

## 파일 다이알로그

```python
from tkinter import filedialog 
files = filedialog.askopenfilenames(title="이미지 파일을 선택하세요", \
    filetypes=(("PNG 파일","*.png"),("모든 파일", "*.*")),\
    initialdir=r"C:\Users\rssun\Desktop\PythonWorkspace\python_gui_basic\gui_project") 
```

## 이미지 파일 편짐

```python
from PIL import Image
images = [Image.open(x) for x in list_file.get(0, END)]
Image.new("RGB", (max_width, total_height),(255,255,255))
```