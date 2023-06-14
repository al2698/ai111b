## 注意事項

- 程式碼聲明
  本作業的程式碼部分是修改自別人網站的程式碼，該網址為：[Driver Drowsiness Detection Using Mediapipe In Python](https://learnopencv.com/driver-drowsiness-detection-using-mediapipe-in-python/)。
---
# 人工智慧影音辨識於線上教學之輔助應用
## 摘要
線上教學在全球疫情下扮演重要角色，然而傳統教學模式在視訊軟體上，教師難以監控學生的學習狀況和辨識不專心的學生。為解決此問題，本研究提出了人工智慧影音辨識技術的輔助應用。
首先，我們使用視訊分析技術結合臉部辨識和眼睛活動偵測，即時辨識學生的專注度。透過分析表情和眼睛活動，尤其是利用眼睛開合程度（EAR技術），我們能夠判斷學生的專注狀態；同時，透過分析電腦使用情況，即追蹤和識別當前活躍視窗，我們能夠瞭解學生的應用程式使用情況。這提供教師寶貴的學生學習狀況資訊，使其能夠即時提供指導和幫助，提升學習效果和互動性。
其次，我們開發了基於網頁平台的數位教學系統，提供課程開設、學生加入、文件上傳和聊天室等互動功能。系統目標是透過直觀顯示學生的專注狀態，讓教師快速了解學生的專注度，並即時提醒和支援不專心的學生。
本研究的人工智慧影音辨識技術和基於網頁的數位教學系統有效克服教學困難，提升學生的學習效果和互動性。這些創新應用為教育界帶來新的可能性，同時凸顯了人工智慧在疫情和教育領域中的重要作用。線上教學已成為全球疫情下不可或缺的學習方式。


## 眨眼檢測

為了檢測眼睛是否眨眼，我們採用了一個公式，該公式是根據先前由採用Dewi C, Chen RC, Jiang X, Yu H在PeerJ Comput Sci上發表的 "Adjusting eye aspect ratio for strong eye blink detection based on facial landmarks" 的方法[1] 進行開發和改進的。這個方法的主要思想是利用眼睛特徵點的位置來計算眼睛的特徵值，從而判斷眼睛的狀態。
具體而言，我們使用了Mediapipe庫和OpenCV來進行眼睛眨眼檢測。首先，我們使用OpenCV將視訊分解成一個個幀，然後利用Mediapipe庫進行面部特徵檢測，其中包括眼睛。根據先前的研究和實驗，我們選擇了特定的特徵點索引來標定眼睛的位置。
對於左眼，我們選擇了P1、P2、P3、P4、P5和P6六個特徵點，其對應的特徵點索引分別為[362, 385, 387, 263, 373, 380]。同樣地，對於右眼，我們選擇了P1、P2、P3、P4、P5和P6六個特徵點，其對應的特徵點索引分別為[33, 160, 158, 133, 153, 144]。
在每個眼睛區域內，我們繪製了一條水平線和一條垂直線，以幫助我們觀察眼睛的運動情況。眼睛眨眼是指眼睛暫時閉合並伴隨著眼瞼運動的過程。通過計算眼睛縱橫比（EAR）和眼睛特徵的差異值（dEAR），我們可以判斷眼睛的狀態。當眼睛特徵值EAR大於等於dEAR時，表示眼睛處於睜開狀態（EyeOpen）；當EAR小於dEAR時，表示眼睛處於閉合狀態（EyeClose）。

 ![睜開的眼睛和閉上的眼睛示例圖片及面部特徵點](../picture/%E9%9D%A2%E9%83%A8%E7%89%B9%E5%BE%B5%E9%BB%9E.png)

這個眨眼檢測方法的具體計算方式是根據先前研究的方法，我們參考了其他學者的論文和研究成果。透過這個檢測方法，我們能夠準確地檢測學生眼睛是否發生眨眼的動作，這將成為後續疲勞水平評估的重要依據，有助於了解學生的注意力和專注度狀態。同時，這個方法的使用也確保了我們的研究具有可靠性和科學性，並能夠與其他研究成果進行比較和驗證。為了檢測眼睛是否眨眼，我們採用了以下公式：

![模擬學生上課時單次疲倦檢測過程](../picture/EAR%E5%85%AC%E5%BC%8F.png)

因此，EAR作為一種眨眼檢測指標，具有重要的應用價值。它是一種簡單而有效的技術，可靈敏地評估眼睛的開合程度。通過計算眼睛的EAR值，我們可以得知眼睛的狀態，例如睜眼時EAR值保持較穩定，閉眼時EAR值迅速下降，幾乎為零。

![模擬學生上課時單次疲倦檢測過程](../picture/%E6%A8%A1%E6%93%AC%E5%AD%B8%E7%94%9F%E4%B8%8A%E8%AA%B2%E6%99%82%E5%96%AE%E6%AC%A1%E7%96%B2%E5%80%A6%E6%AA%A2%E6%B8%AC%E9%81%8E%E7%A8%8B.png)

## 檢測學生狀態流程圖

在進行學生狀態檢測時，我們按照以下流程進行操作。首先，利用操作系統函式 GetForegroundWindow 獲取學生的前景視窗，以確定學生的操作環境。接著，進行眨眼檢測的初始化設置，包括基準閾值和調整參數的設定。
繼續進行視訊捕捉，使用攝像頭或視訊文件來源獲取學生的面部視訊，並將視訊分解為幀以進一步處理。利用 Mediapipe 库進行面部特徵檢測，定位學生面部的特定特徵點，如左眼和右眼的位置。
根據特徵點的位置，提取出左眼和右眼的區域，以便進行後續的眼睛狀態檢測。在每個眼睛區域內，繪製水平線和垂直線，並計算眼睛特徵的滑動窗口平均值（EAR_mean）。通過計算眼睛特徵的差異值（dEAR = EAR - EAR_mean），判斷眼睛的狀態，如睜開（EyeOpen）或閉合（EyeClose）。
同時，我們需要檢測眼睛閉合的持續時間。當檢測到眼睛閉合的動作時，開始計時閉合的持續時間，如果持續時間超過等於 3 秒，則判定為眼睛閉合狀態。
此外，我們還需要檢測窗口，監測學生前景視窗的停留時間並計算其持續時間。如果窗口停留的持續時間超過等於 3 秒，則判定為對應的窗口處於焦點狀態。
最後，根據眼睛的狀態和窗口的活躍情況，更新學生的疲勞水平和窗口的活躍狀態。這個流程提供了一個詳細的框架，用於監測學生的狀態並進行相關的分析和應用。這些檢測和分析結果可以應用於教育領域，提供有價值的信息，同時也可以應用於驗證系統，例如自動化測驗系統，以檢測學生的注意力和專注度，並提供相應的反饋和調整。

![ 檢測學生的專心狀態架構圖](../picture/%E6%AA%A2%E6%B8%AC%E5%AD%B8%E7%94%9F%E7%9A%84%E5%B0%88%E5%BF%83%E7%8B%80%E6%85%8B%E6%9E%B6%E6%A7%8B%E5%9C%96.png)

在這個部分，這個算法描述了學生狀態檢測的過程。該算法利用眼睛狀態和課程中開啟的活躍窗口觀察學生的行為，並根據觀察到的情況更新學生的狀態，用來即時檢測學生是否表現出懈怠行為。以下是我們算法的pseudocode：

## Algorithm 1: Slacking Checker Algorithm

```
Import necessary libraries and modules

Class SlackingChecker:
    Initialize necessary variables and objects
    
    Define handle_status_change function:
        Check for changes in slacking status
        Perform actions based on the current slacking status
        
    Define check_slacking function:
        Check for slacking behavior
        Update slacking status based on the observations
        Record slacking history and update the database if necessary
        
    Define get_slacking_status function:
        Return the current slacking status
        
    Define _handle_eyes_status function:
        Evaluate the state of eyes (EAR value) and update slacking status
        
    Define _handle_class_related_window function:
        Evaluate the current window and update slacking status
        
    Define _record_history function:
        Record the current slacking status along with the timestamp
        
    Define _update_database function:
        Send the collected slacking data to the database
        
Create an instance of the SlackingChecker class

Execute the main program:
    Set up necessary configurations
    
    Loop:
        Get the latest EAR value and window name
        
        Call the check_slacking function to monitor slacking behavior
        
        Retrieve the current slacking status using get_slacking_status
        
        Perform any required actions based on the slacking status
        
        Pause for a certain interval
```

## 參考文獻

[1] Dewi C, Chen RC, Jiang X, Yu H. Adjusting eye aspect ratio for strong eye blink detection based on facial landmarks. PeerJ Comput Sci. 2022 Apr 18;8:e943. doi: 10.7717/peerj-cs.943. PMID: 35494836; PMCID: PMC9044337.
[2]Driver Drowsiness Detection Using Mediapipe In Python. Retrieved January 18 , 2023, from https://learnopencv.com/driver-drowsiness-detection-using-mediapipe-in-python/

