## Talking Dictionary
Talking Dictionary is a Python-based application that provides word meanings along with the ability to listen to the word and its meaning. This project uses the tkinter library for the GUI, pyttsx3 for text-to-speech functionality, and json to handle the dictionary data. This application is useful for those who want to improve their vocabulary and pronunciation.

## Prerequisites
Before you begin, ensure you have met the following requirements:

* Python 3.7+ installed on your system.
* Required libraries: tkinter, pyttsx3, json, difflib

## Problem Description
1.	Lack of Text-to-Speech for Pronunciation
2.	Absence of Text-to-Speech for Meanings
3.	Complex User Interfaces
4.	Poor Error Handling for Misspelled Words
5.	Inaccessibility for Users with Disabilities

## Solution Provided
1.	Text-to-Speech for Word Pronunciation and meanings
2.	User-Friendly Interface
3.	Suggest Close Matches for Misspelled Words
4.	Accessibility Features

## Features
* Search for word meanings from a predefined dictionary.
* Listen to the pronunciation of the word.
* Listen to the meaning of the word.
* Clear the input fields.
* Confirm exit from the application.

## Working
* Please refer to the included screenshot for an overview of how the GUI is :
  ![flowchart](flowchart.jpg)

## Getting Started
Follow these steps to set up and run the project on your local machine.

1. Git clone the repository on your local machine:
  ```
  git clone https://github.com/kittu-122/talking-dictionary.git
  cd talking-dictionary
  ```

2. Install the Python libraries in your Python virtual environment:
  ```
  pip install library_name
  ```

## Usage:

1. Run the main.py file to start the application:
   ```
   python main.py
   ```

2. The main window will open, displaying the interface of the Talking Dictionary.

## File Structure
* main.py: The main Python script containing the application code.
* data.json: A JSON file containing the dictionary data.
* bg.png: Background image for the application.
* search.png: Image for the search button.
* mic.png: Image for the mic button.
* microphone.png: Image for the meaning audio button.
* clear.png: Image for the clear button.
* exit.png: Image for the exit button.

## Detailed Code Explanation
1. Imports
   ```
    from tkinter import *
    from tkinter import messagebox
    import json
    import pyttsx3
    from difflib import get_close_matches
   ```
   * tkinter: Used for creating the graphical user interface.
   * messagebox: Used for displaying message boxes.
   * json: Used for loading the dictionary data from a JSON file.
   * pyttsx3: Used for text-to-speech conversion.
   * get_close_matches: Used for finding close matches of the input word.
    
2. Text-to-Speech Initialization
   ```
     engine = pyttsx3.init()
   ```
   * Initialize the text-to-speech engine.

3. Functions
   
   Word Audio
   ```
    def wordaudio():
      voices = engine.getProperty('voices')
      engine.setProperty('voice', voices[0].id)
      engine.say(enterwordentry.get())
      engine.runAndWait()
   ```
   * This function retrieves the text from the word entry field and uses the first voice in the system to speak the word.

   Meaning Audio
   ```
    def meaningaudio():
      voices = engine.getProperty('voices')
      engine.setProperty('voice', voices[1].id)
      engine.say(textarea.get(1.0, END))
      engine.runAndWait()
   ```
   * This function retrieves the text from the meaning text area and uses the second voice in the system to speak the meaning.

   Exit Application
   ```
   def iexit():
      res = messagebox.askyesno('Confirm', 'Do you want to exit?')
      if res == True:
          root.destroy()
      else:
          pass
   ```
   * This function asks the user to confirm if they want to exit the application.

  Clear Fields
   ```
     def clear():
        textarea.config(state=NORMAL)
        enterwordentry.delete(0, END)
        textarea.delete(1.0, END)
        textarea.config(state=DISABLED)
   ```
   * This function clears the word entry field and the meaning text area.

  Search Word
   ```
     def search():
        data = json.load(open('data.json'))
        word = enterwordentry.get().lower()
    
        if word in data:
            meaning = data[word]
            textarea.config(state=NORMAL)
            textarea.delete(1.0, END)
            for item in meaning:
                textarea.insert(END, u'\u2022' + item + '\n\n')
            textarea.config(state=DISABLED)
        elif len(get_close_matches(word, data.keys())) > 0:
            close_match = get_close_matches(word, data.keys())[0]
            res = messagebox.askyesno('Confirm', 'Did you mean ' + close_match + ' instead?')
            if res == True:
                meaning = data[close_match]
                textarea.delete(1.0, END)
                textarea.config(state=NORMAL)
                for item in meaning:
                    textarea.insert(END, u'\u2022' + item + '\n\n')
                textarea.config(state=DISABLED)
            else:
                textarea.delete(1.0, END)
                messagebox.showinfo('Information', 'Please type a correct word')
                enterwordentry.delete(0, END)
        else:
            messagebox.showerror('Error', 'The word doesnt exist.Please double check it.')
            enterwordentry.delete(0, END)
   ```
   * This function searches for the word in the JSON data. If the word is found, it displays the meaning. If the word is not found but close matches exist, it asks the user if they meant a close match. If no matches are found, it shows an error message.

   GUI Setup
   ```
     root = Tk()
     root.geometry('1000x626+100+50')
     root.title('TALKING DICTIONARY')
     root.resizable(0, 0)
   ```
   * Initialize the main window.

   Background Image
   ```
     bgimage = PhotoImage(file='bg.png')
     bgLabel = Label(root, image=bgimage)
     bgLabel.place(x=0, y=0)
   ```
   * Set the background image.

   Word Entry
   ```
     enterwordLabel = Label(root, text='Enter Word', font=('ALGERIAN', 29, 'bold'), fg='red3', bg='whitesmoke')
     enterwordLabel.place(x=530, y=20)
     enterwordentry = Entry(root, font=('arial', 23, 'bold'), bd=8, relief=GROOVE, justify=CENTER)
     enterwordentry.place(x=510, y=80)
     enterwordentry.focus_set()
   ```
   * Create the label and entry field for entering the word.

   Buttons
   ```
    searchimage = PhotoImage(file='search.png')
    searchButton = Button(root, image=searchimage, bd=0, bg='whitesmoke', activebackground='whitesmoke', cursor='hand2', command=search)
    searchButton.place(x=620, y=150)
    
    micimage = PhotoImage(file='mic.png')
    micButton = Button(root, image=micimage, bd=0, bg='whitesmoke', activebackground='whitesmoke', cursor='hand2', command=wordaudio)
    micButton.place(x=710, y=153)
    
    meaninglabel = Label(root, text='Meaning', font=('ALGERIAN', 29, 'bold'), fg='red3', bg='whitesmoke')
    meaninglabel.place(x=580, y=240)

    textarea = Text(root, font=('arial', 18, 'bold'), height=8, width=34, bd=8, relief=GROOVE, wrap='word')
    textarea.place(x=460, y=300)
    
    audioimage = PhotoImage(file='microphone.png')
    audioButton = Button(root, image=audioimage, bd=0, bg='whitesmoke', activebackground='whitesmoke', cursor='hand2', command=meaningaudio)
    audioButton.place(x=530, y=555)
    
    clearimage = PhotoImage(file='clear.png')
    clearButton = Button(root, image=clearimage, bd=0, bg='whitesmoke', activebackground='whitesmoke', cursor='hand2', command=clear)
    clearButton.place(x=660, y=555)
    
    exitimage = PhotoImage(file='exit.png')
    exitButton = Button(root, image=exitimage, bd=0, bg='whitesmoke', activebackground='whitesmoke', cursor='hand2', command=iexit)
    exitButton.place(x=790, y=555)
   ```
   * Create the buttons for search, mic, meaning audio, clear, and exit functionalities.

   Main Loop
   ```
     root.mainloop()
   ```
   * Run the main loop to keep the application running.

## Contributing
If you would like to contribute to this project, please fork the repository and submit a pull request. Contributions are welcome!

**Thank you for choosing this project. Hoping that this project proves useful and delivers a seamless experience for your needs!**
