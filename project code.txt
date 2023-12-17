import tkinter as ts
from tkinter import *
from tkinter import filedialog
from tkinter.ttk import Combobox
from tkinter import ttk
import tkinter as tk
import pyttsx3
import gtts as gt
import webbrowser
import os


root=Tk()
root.title("Text2Speech By TechMagazine")
root.geometry("900x450+200+200")
root.resizable(False,False)
root.configure(bg="#305065")


engine = pyttsx3.init()

def speaknow():
    text = text_area.get(1.0, END)
    gender = gender_combobox.get()
    speed = speed_combobox.get()
    voices = engine.getProperty('voices')


    def setvoice():
        if (gender == 'Male'):
            engine.setProperty('voice', voices[0].id)
            engine.say(text)
            engine.runAndWait()
        else:
           engine.setProperty('voice', voices[1].id)
           engine.say(text)
           engine.runAndWait()

    if (text):
        if (speed =="Fast"):
           engine.setProperty('rate',250)
           setvoice()
        elif (speed == 'Normal'):
            engine.setProperty('rate',150)
            setvoice()
        else:
             engine.setProperty('rate', 60)
             setvoice()

def download():
    text = text_area.get(1.0, END)
    gender = gender_combobox.get()
    speed = speed_combobox.get()
    voices = engine.getProperty('voices')


    def setvoice():
        if (gender == 'Male'):
            engine.setProperty('voice', voices[0].id)
            path=filedialog.askdirectory()
            os.chdir(path)
            engine.save_to_file(text , 'text.mp3')
            engine.runAndWait()
        else:
            engine.setProperty('voice', voices[1].id)
            path=filedialog.askdirectory()
            os.chdir(path)
            engine.save_to_file(text , 'text.mp3')
            engine.runAndWait()

    if (text):
        if (speed =="Fast"):
           engine.setProperty('rate',250)
           setvoice()
        elif (speed == 'Normal'):
            engine.setProperty('rate',150)
            setvoice()
        else:
             engine.setProperty('rate', 60)
             setvoice()



#icon
image_icon=PhotoImage(file="C:/Users/yogesh/Documents/python projects/New folder (2)/20220225_155358.png")
root.iconphoto(False,image_icon)


#Top Frame
Top_frame=Frame(root,bg="white",width=900,height=100)
Top_frame.place(x=0,y=0)


Logo=PhotoImage(file="C:/Users/yogesh/Documents/python projects/New folder (2)/speaker logo.png")
Label(Top_frame,image=Logo,bg="white").place(x=10,y=5)


Label(Top_frame,text="TEXT 2 SPEECH By TechMagazine",font="LemonMilk",bg="white",fg="black").place(x=100,y=40)


###################

text_area=Text(root,font="Robote 20",bg="white",relief=GROOVE,wrap=WORD)
text_area.place(x=10,y=150,width=500,height=250)


Label(root,text="VOICE",font="arial 15 bold",bg="#305065",fg="white").place(x=580,y=160)
Label(root,text="SPEED",font="arial 15 bold",bg="#305065",fg="white").place(x=760,y=160)

###################
#Main Buttons


gender_combobox=Combobox(root,values=['Male','Female'],font="arial 14",state='r',width=10)
gender_combobox.place(x=550,y=200)
gender_combobox.set('Male')

speed_combobox=Combobox(root,values=['Fast','Normal','Slow'],font="arial 14",state='r',width=10)
speed_combobox.place(x=730,y=200)
speed_combobox.set('Normal')



imageicon=PhotoImage(file="speak.png")
btn=Button(root,text="Speak",compound=LEFT,image=imageicon,width=130,font="arial 14 bold",command=speaknow)
btn.place(x=550,y=280)

imageicon2=PhotoImage(file="download.png")
save=Button(root,text="Save",compound=LEFT,image=imageicon2,width=130,bg="#39c790",font="arial 14 bold",command=download)
save.place(x=730,y=280)

#############################
#Toogle switch


button_mode=True

def customize():

    global button_mode

    if button_mode:
        button.config(image=off,bg="white",activebackground="white")
        root.config(bg="#26242f")
        button_mode=False
    else:
        button.config(image=on,bg="white",activebackground="white")
        root.config(bg="#305065")
        button_mode=True

on=PhotoImage(file="20220226_104552.png")
off=PhotoImage(file="20220226_104628.png")


button=Button(root,image=on,bd=0,bg="white",activebackground="white",height=50, width=160 , command=customize)
button.pack(padx=0,pady=10)
button.place(x=650,y=30)

#############################
#custom file upload button

def open_text_file(): 
    tf = filedialog.askopenfilename(
        initialdir="C:/Users/MainFrame/Desktop/", 
        title="Open Text file", 
        filetypes=(("Text Files", "*.txt"),)
        )
    tf = open(tf)  # or tf = open(tf, 'r')
    data = tf.read()
    text_area.insert(END, data)
    tf.close()

#open file button
open_button = ttk.Button(
    root,
    text='Open a File',
    command=open_text_file
)

open_button.grid(column=0, row=1, sticky='w', padx=10, pady=120)

#############################

m = Menu(root, tearoff = 0)
m.add_command(label ="Cut")
m.add_command(label ="Copy")
m.add_command(label ="Paste")




# define function to cut 
# the selected text
def cut_text():
        text.event_generate(("<<Cut>>"))
# define function to copy 
# the selected text
def copy_text():
        text.event_generate(("<<Copy>>"))
# define function to paste 
# the previously copied text
def paste_text():
       text.event_generate(("<<Paste>>"))

# create menubar
menu = Menu(root, tearoff = 0) 
menu.add_command(label="Cut", command=cut_text) 
menu.add_command(label="Copy", command=copy_text) 
menu.add_command(label="Paste", command=paste_text)
menu.add_separator()

       
# define function to popup the
# context menu on right button click         
def do_popup(event):
	try:
		menu.tk_popup(event.x_root, event.y_root)
	finally:
		menu.grab_release()

root.bind("<Button-3>", do_popup)



root.mainloop()
