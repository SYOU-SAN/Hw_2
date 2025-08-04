Hazelnut Anomaly Detection using Autoencoders
這份專案旨在使用深度學習模型來檢測榛果影像中的異常。專案中實現了兩種不同的自動編碼器（Autoencoder）模型：一個是基於 TensorFlow 和 Alibi Detect 的變分自動編碼器（VAE），另一個是基於 PyTorch 的卷積自動編碼器。

專案描述
本專案透過訓練自動編碼器來學習正常榛果影像的特徵。當模型看到異常影像時，由於無法有效地重建影像，將產生較高的重建誤差。藉由設定一個合理的閾值，我們可以區分出正常的榛果與異常的榛果。

此專案可偵測的異常類型包含：

孔洞 (hole)

斑點 (print)

切割 (cut)

裂縫 (crack)

目錄
環境建置

如何使用

模型介紹

結果

環境建置
前置需求:

Python 3.x

Google Colab 或已安裝 Jupyter Notebook 的本機環境

掛載 Google Drive (如果您在 Colab 中運行並將資料集存儲在雲端硬碟)

安裝相依套件:
在您的環境中，透過 pip 安裝以下套件：

Bash

pip install alibi-detect numpy==1.26 tensorflow torch torchvision Pillow opencv-python matplotlib tqdm
資料集:

請將榛果影像資料集放置在指定的目錄結構下。根據程式碼，預設路徑為 /content/drive/MyDrive/HW03Rawdata/hazelnut。

如果您使用不同的路徑，請務必在程式碼中修改 base_dir 變數。

資料夾結構應如下：

hazelnut/
├── train/
│   └── good/
│       ├── 000.png
│       └── ...
└── test/
    ├── good/
    ├── hole/
    ├── print/
    ├── cut/
    └── crack/
如何使用
開啟 Notebook:
使用 Google Colab 或 Jupyter Notebook 開啟 Alibi_Detect.ipynb 檔案。

設定路徑:
確認程式碼中的 base_dir 變數指向您的資料集所在的正確路徑。

執行程式碼:
依照 Notebook 中的儲存格順序，依序執行。程式將會自動完成以下步驟：

安裝必要的套件。

載入並前處理影像資料。

建立並訓練 VAE 模型（使用 TensorFlow）。

評估 VAE 模型的異常檢測準確率。

視覺化 VAE 模型的重建結果與異常偵測。

建立並訓練 Autoencoder 模型（使用 PyTorch）。

評估 PyTorch Autoencoder 模型的異常檢測準確率。

視覺化 PyTorch Autoencoder 模型的重建結果與異常偵測。

模型介紹
1. TensorFlow Variational Autoencoder (VAE)
框架: TensorFlow, Keras, Alibi Detect

模型架構:

編碼器 (Encoder): 由四個卷積層組成，將 64x64 的影像壓縮至 64 維的潛在空間。

解碼器 (Decoder): 由四個轉置卷積層組成，將潛在空間的向量重建回 64x64 的影像。

訓練:

使用 alibi_detect.models.tensorflow.VAE 進行 VAE 模型的建立與訓練。

模型在 100 個 epochs 下進行訓練，並儲存驗證損失最低的模型權重。

2. PyTorch Autoencoder
框架: PyTorch

模型架構:

編碼器 (Encoder): 由三個卷積層組成。

解碼器 (Decoder): 由三個轉置卷積層組成。

訓練:

模型在 20 個 epochs 下進行訓練。

透過計算正常樣本重建誤差的平均值與標準差，來動態設定異常閾值。

結果
根據 Notebook 的執行結果，兩個模型在檢測特定類型的異常時表現良好：

VAE (TensorFlow):

good: 100.00%

print: 88.24%

crack: 50.00%

hole: 0.00%

cut: 0.00%

Autoencoder (PyTorch):

good: 100.00%

hole: 77.78%

print: 100.00%

cut: 11.76%

crack: 66.67%

視覺化結果顯示了模型如何重建輸入影像，以及標示出異常區域的殘差圖。
