---
title: "Image to Text NLP with openCV"
date: 2024-07-28
description: "Loading my hand-notes to Joplin"
draft: true
toc: true

codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - coding
tags:
  - python
  - image-to-text
  - NLP
---

## Summary

I rely on a pocket notebook to scribble passing ideas or reminders. Eventually I transfer these
notes to Joplin. I'm finally stopping to write down a small piece of code to parse the text from my
notes into string of text I can just copy to Joplin.

### Left-hand writing

Being left-handed when using pen with ink that take longer to dry. a common problem is smudging. The
text messy, it
stains my hand. After reading Walter Issacson's biography on Leonardo da vinci's mirror
writing, He would write from right-left (<-). I gave it a try, after a while I got the hang of it...
The problem now is that I don't carry a mirror around, and it's damn hard to read from right to left.



## Before I go through the code snippet, few notes

* I do some preparation before passing the image through the script:
  * Make sure the image is not upside-down
  * crop the image to the page only
  * 


## Coding details


```bash
# Import libraries
pip install tesseract
pip install pytesseract
pip install opencv-python
pip install Pillow
#pip install pdf2image If you're playing with PDF

# Install to the system (Linux ) 
apt install tesseract-ocr 
apt install libtesseract-dev
sudo apt install tesseract-ocr

# If you're on MACOS
brew install tesseract 
```

## References

* [PyTesseract](https://pypi.org/project/pytesseract/)
* [OpenCV](https://docs.opencv.org/4.x/)
* [PyTorch](https://pytorch.org/tutorials/beginner/introyt/trainingyt.html?highlight=nn%20crossentropyloss)
* [diogo-pessoa/training_CNN_MNiST_Model](https://github.com/diogo-pessoa/CapturingCharactersSpellChecker/blob/main/notebooks/training_CNN_MNiST_Model.ipynb)
