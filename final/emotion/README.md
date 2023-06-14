# TensorFlow 識別人們的情感狀態
## 前言
傳統的線上教學中，教師往往面臨著無法直接觀察學生情感狀態的挑戰。為了改善這一情況並提供更具互動性的教學環境，我在之前的專題研究進一步拓展了人工智慧在教育領域的應用。
本研究著重於使用TensorFlow來識別人們的情感狀態。透過深度學習模型和視覺分析技術，我能夠準確辨識學生在教學過程中所表現出的情感狀態，包括高興、悲傷、憤怒等等。這項功能的引入不僅有助於教師更好地了解學生的情感反應，也能夠提供更加個人化的教學和支援。
此外，我的研究還探索了一些新的延伸主題。我進行了情感狀態和學習成效之間的關聯性研究，以深入了解情感狀態對學習表現的影響。同時，我也研究了如何根據學生的情感狀態調整教學策略，以提高學習效果和學習動機。
這些新功能和延伸主題將為教育界帶來全新的可能性，同時推動線上教學的發展。我相信，透過運用人工智慧技術，特別是TensorFlow在情感識別上的應用，我可以創造一個更具人性化和有效的教學環境，從而提升學生的學習體驗和成效。

## 資料集
資料來源：[kaggle face-expression-recognition-dataset](https://www.kaggle.com/datasets/jonathanoheix/face-expression-recognition-dataset)

在 train 資料集中找到屬於 7 個類的 28821 張圖片。
在驗證資料集中找到屬於 7 個類別的 7066 張圖像。

## 修改模型
- 程式碼聲明
  本作業的程式碼部分是修改自別人kaggle的程式碼，該網址為：[FaceExpressionRecognition](https://www.kaggle.com/code/keerthanas57/faceexpressionrecognition/notebook)。

原版模型參數量：

![原版模型參數量](../picture/%E5%8E%9F%E7%89%88%E6%A8%A1%E5%9E%8B%E5%8F%83%E6%95%B8%E9%87%8F.png)

我的模型參數量：

![我的模型參數量](../picture/%E6%88%91%E7%9A%84%E6%A8%A1%E5%9E%8B%E5%8F%83%E6%95%B8%E9%87%8F.png)

原版模型非常大，導致較長的訓練時間和更高的計算要求。
我對原版模型進行了以下修改：
1. 我減少了卷積層的數量和每個卷積層的特徵圖數量。原版模型中有多個64和512個特徵圖2
的卷積層，我將這些數字分別調整為32和64。
2. 我簡化了卷積層的核大小。原版模型中使用了不同大小的卷積核，我將這些卷積核的大小統一調整為3x3。
3. 我刪除了一些卷積層和池化層。原版模型中有多個卷積層和池化層的堆疊，我刪除了其中一些層。
4. 我減少了全連接層的特徵數量。原版模型中有512個特徵的全連接層，我將這個數字減少為256。
5. 刪減迭代次數。原版模型中迭代57次，我將其減少成27。


## 模型結構
![模型結構](../picture/%E6%A8%A1%E5%9E%8B%E7%B5%90%E6%A7%8B.png)
在我的研究中，我使用了一個基於深度學習的模型來識別人們的情感狀態。現在我將向您展示這個模型的結構，並解釋每個層次的作用。
我的模型採用了一種稱為“Sequential”的CNN 架構，這是一種線性堆疊模型，它可以逐層地建構我的神經網絡。詳情請看右側這個模型的結構表格：
- epochs = 27
- batch_size = 64
- target_size=(48,48)

## 成果展示- Acc和Loss

![我的Acc和Loss](../picture/%E6%88%91%E7%9A%84Acc%E5%92%8CLoss.png)
▲我的Acc和Loss

![原版Acc和Loss](../picture/%E5%8E%9F%E7%89%88Acc%E5%92%8CLoss.png)
▲原版Acc和Loss

## 成果展示-Confusion Matrix
![Confusion Matrix](../picture/Confusion%20Matrix.png)

## 實驗結果
這個程式使用情緒分類模型和人臉檢測器來從攝像頭捕獲的視頻中檢測人臉並識別情緒。

![實驗結果](../picture/%E5%AF%A6%E9%A9%97%E7%B5%90%E6%9E%9C.png)

## 參考資料
- FaceExpressionRecognition, Retrieved from https://www.kaggle.com/code/keerthanas57/faceexpressionrecognition

- Face expression recognition dataset, Retrieved from  https://www.kaggle.com/datasets/jonathanoheix/face-expression-recognition-dataset

- How to use OpenCV Videocapture With Saved Keras Model? , Retrieved from https://stackoverflow.com/questions/66874821/how-to-use-opencv-videocapture-with-saved-keras-model
