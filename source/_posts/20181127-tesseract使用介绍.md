---
title: tesseract使用介绍
date: 2018-11-27 15:35:57
tags: ocr
---

## 0.前言
&nbsp;&nbsp;Tesseract是一款开源的OCR引擎，2005年被惠普实验室开发，2006年被谷歌开发者及其他开源组织者维护开发。Tesseract3.x基于传统的计算机视觉算法，在Tesseract4中，Tesseract 实现了一个基于LSTM的识别引擎。识别图像中单个的字符，我们通常使用CNN网络。对于任意长度的字符串的识别，我们通常使用LSTM.Tesseract4中包含了Tesseract3中的OCR引擎，但是默认使用LSTM引擎.

## 1.How to install Tesseract on Ubuntu and macOS
- Tesseract library (libtesseract)
- Command line Tesseract tool (tesseract-ocr)
- Python wrapper for tesseract (pytesseract)

### 1.1. Install Tesseract 4.0 on Ubuntu 18.04

```shell
sudo apt install tesseract-ocr
sudo apt install libtesseract-dev
sudo pip install pytesseract

```

### 1.2. Install Tesseract 4.0 on Ubuntu 14.04, 16.04, 17.04, 17.10

```shell
sudo add-apt-repository ppa:alex-p/tesseract-ocr
sudo apt-get update
sudo apt install tesseract-ocr
sudo apt install libtesseract-dev
sudo pip install pytesseract
```

### 1.3. Install Tesseract 4.0 on macOS

```shell
# If you have tesseract 3 installed, unlink first by uncommenting the line below 
# brew unlink tesseract 
brew install tesseract --HEAD
pip install pytesseract
```

### 1.4. Checking Tesseract version

`tesseract --version`

## 2.Tesseract Basic Usage
- Input filename: 待识别的图像
- OCR language:　识别图像中字体中的语言，在命令行和pytesseract，使用-l 选项
- OCR Engine Mode(oem):tesseract4有2个ocr引擎(legacy,lstm),用--oem选项去设置
   0    Legacy engine only.
   1    Neural nets LSTM engine only.
   2    Legacy + LSTM engines.
   3    Default, based on what is available.
- Page Segmentation Mode(psm): psm 或许是非常有用的，对于结构化文本有额外的信息对于python和命令行工具默认是3,c++　API默认是6.
   0    只有方向和脚本检测（OSD）。
   1    使用OSD自动分页。
   2    自动分页，但没有OSD或OCR。
   3    全自动页面分割，但没有OSD。（默认）
   4    假设一列可变大小的文本。
   5    假设一个统一的垂直排列文本块。
   6    假设一个统一的文本块。
   7    将图像作为单个文本行处理。
   8    将图像视为一个单词。
   9    将图像视为一个圆圈中的单个单词。
   10   将图像视为单个字符。

### 2.1 Command Line Usage
```shell
# Output to terminal
tesseract image.jpg stdout -l eng --oem 1 --psm 3
# Output to output.txt 
tesseract image.jpg output -l eng --oem 1 --psm 3
```
### 2.2  Using pytesseract
```python
import cv2
import sys
import pytesseract
 
if __name__ == '__main__':
 
  if len(sys.argv) < 2:
    print('Usage: python ocr_simple.py image.jpg')
    sys.exit(1)
   
  # Read image path from command line
  imPath = sys.argv[1]
     
  # Uncomment the line below to provide path to tesseract manually
  # pytesseract.pytesseract.tesseract_cmd = '/usr/bin/tesseract'
 
  # Define config parameters.
  # '-l eng'  for using the English language
  # '--oem 1' for using LSTM OCR Engine
  config = ('-l eng --oem 1 --psm 3')
 
  # Read image from disk
  im = cv2.imread(imPath, cv2.IMREAD_COLOR)
 
  # Run tesseract OCR on image
  text = pytesseract.image_to_string(im, config=config)
 
  # Print recognized text
  print(text)
```

