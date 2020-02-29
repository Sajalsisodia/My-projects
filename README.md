# My-projects
#text editor using tkinter
from tkinter import *
from tkinter.messagebox import showinfo
from tkinter.filedialog import asksaveasfilename , askopenfilename
import os

def Newfile():
    global file
    root.title("Untitle notpad")
    file = None
    TextArea.delete(1.0, END)

def Openfile():
    global file
    file = askopenfilename(defaultextension = " .txt", filetypes =[("All Files","*.*"),("Text Documents","*.txt")])
    if file == "":
        file = None
    else:
        root.title(os.path.basename(file)+ " -  Notpad")
        TextArea.delete(1.0,END)
        f =open(file,"r")
        TextArea.insert(1.0 , f.read())
        f.close()


def Savefile():
    global file
    if file  == None:
        file = asksaveasfilename(initialfile = 'Untitled.txt' ,defaultextension = " .txt", filetypes =[("All Files","*.*"),("Text Documents","*.txt")] )
        if file =="":
            file = None
        else:
            f = open(file,"w")
            f.write(TextArea.get(1.0,END))
            f.close()
            root.title(os.path.basename(file)+ " -  Notpad")
            print("File save ")

    else:
        #save the file 
        f = open(file,"w")
        f.write(TextArea.get(1.0,END))
        f.close()
       







def quitapp():
    root.destroy()

def cut():
    TextArea.event_generate(("<<Cut>>"))


def copy():
    TextArea.event_generate(("<<Copy>>"))


def paste():
    TextArea.event_generate(("<<Paste>>"))


def about():
     showinfo("NOTPAD",''' This Notpad is made with help of tkinter ''')


if __name__== '__main__':

    #basic kinter setup
    root=Tk()
    root.title("Untitie Notpad")
    #root.wm_iconbitmap("1.ico")
    root.geometry("644x788")

    #add text 
    TextArea = Text(root , font = " lucida  14")
    file = None 
    TextArea .pack(fill = BOTH, expand = TRUE)

    #main menu 
    Menubar = Menu(root)
    Filemenu = Menu(Menubar , tearoff= 0)
    # file menu      
    Filemenu.add_command(label = "New " , command = Newfile)
    Filemenu.add_command(label = "open",  command = Openfile )
    Filemenu.add_command(label = " Save " , command = Savefile )
    Filemenu.add_command(label = " Exit " , command = quit)
    Menubar.add_cascade(label = " FIle" ,menu = Filemenu)
        


    # Edit menu 
    Editmenu = Menu(Menubar , tearoff = 0)
    Editmenu.add_command(label = " Cut " , command = cut )
    Editmenu.add_command(label = " Copy " , command = copy)
    Editmenu.add_command(label = " paste " , command = paste )
    Menubar.add_cascade(label = " Edit " , menu= Editmenu )

    #help menu
    Helpmenu = Menu(Menubar , tearoff= 0)
    Helpmenu.add_command(label = "About Notpad" , command = about) 
    Menubar.add_cascade(label = "Help", menu= Helpmenu)

    #feedback 
    Exit = Menu(Menubar )
    Menubar.add_cascade(label = "Exit", command = quit)


    root.config(menu = Menubar)
    
    #add scrollbar
    Scroll = Scrollbar(TextArea)
    Scroll.pack(side = RIGHT , fill = Y)
    Scroll.config(command = TextArea.yview)
    TextArea.config(yscrollcommand = Scroll.set)

    
    root.mainloop()
