import requests
from bs4 import BeautifulSoup
import json
import tkinter


url = "https://fanyi.baidu.com/sug"

headers = {
   "User-Agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:96.0) Gecko/20100101 Firefox/96.0"     #UA伪装
}


win = tkinter.Tk()
win.title('小小翻译')
win.geometry('400x360+300+300')

count = 0
def start():
    global count
    global url
    global headers
    dict_data = {}
    if count != 0:
        text1.delete(1.0,tkinter.END)
        text2.delete(1.0, tkinter.END)
    wordStr = eword.get()
    data = {
        'kw':wordStr
    }
    request = requests.post(url=url,data=data,headers=headers)
    dic_obj = request.json()
    x = 0
    for i in dic_obj['data']:
        if x == 0:
            dict_data = i
        x += 1
    text1.insert(tkinter.INSERT,"{}\n{}\n".format(dict_data.get('k'),dict_data.get('v')) + '\n')
    if x != 0:
        for i in dic_obj['data']:
            dict_data = i
            text2.insert(tkinter.INSERT, "{}:\n{}\n".format(dict_data.get('k'), dict_data.get('v')) + '\n')
    count += 1


label1 = tkinter.Label(win,text='输入单词').grid(row=1,column=0)
eword = tkinter.Variable()
entryWord = tkinter.Entry(win,textvariable=eword,width=20).grid(row=1,column=1)
button = tkinter.Button(win, text="翻译", command=start).grid(row=1, column=2)
label2 = tkinter.Label(win,text='翻译结果').grid(row=3,column=0)
b = tkinter.Label(win,text="\n").grid(row=4,column=1)
text1 = tkinter.Text(win,height=5,width=40)
label3 = tkinter.Label(win,text='类似单词').grid(row=5,column=0)
text2 = tkinter.Text(win,height=15,width=40)
text1.grid(row=3,column=1)
text2.grid(row=5,column=1)


win.mainloop()
