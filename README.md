# Yapay Zekâ Kodlama Ajanlarının Açtığı Pull Request'lerin Kabulünün Tahmini

**CME519 Veri Madenciliği — Vize Projesi** · Musa AL AHMED (26221001014) · İstanbul Topkapı Üniversitesi

Bu proje, yapay zekâ kodlama ajanlarının (OpenAI Codex, Devin, GitHub Copilot, Cursor, Claude Code vb.) açtığı bir pull request'in (PR) **birleştirilip (merge)** birleştirilmeyeceğini, yalnızca PR açıldığı anda bilinen bilgileri kullanarak tahmin eder. Çalışma CRISP-DM sürecini izler.

## Veri Seti

[AIDev](https://huggingface.co/datasets/hao-li/AIDev) (Hugging Face, `hao-li/AIDev`) — 100'den fazla yıldıza sahip depoları içeren seçilmiş alt küme.

| Tablo | Kullanım |
|---|---|
| `pull_request` | hedef değişken (birleştirildi / birleştirilmedi), ajan, başlık, açıklama, oluşturulma zamanı |
| `repository` | programlama dili, lisans, yıldız, fork sayısı, fork bilgisi |
| `all_user` | yazarın takipçi ve takip sayısı, hesap oluşturulma tarihi |
| `pr_commits` | PR başına commit sayısı |
| `pr_commit_details` | dosya düzeyinde kod değişiklikleri (~12,1 milyon satır), PR düzeyine toplanmıştır |

Birleştirme sonrası: **22.357 depodan 332.406 kapatılmış PR** (%74,2 birleştirilmiş, %25,8 reddedilmiş). Ham veri Notebook 1 tarafından otomatik olarak indirilir; bu depoda saklanmaz.

## Notebook'lar

| Notebook | İçerik |
|---|---|
| [`notebooks/01_EDA.ipynb`](notebooks/01_EDA.ipynb) | iş problemini anlama, veri toplama ve birleştirme, keşifsel veri analizi (eksik değerler, çarpıklık, IQR / Z-skor ile aykırı değerler, kategorik ve zamansal örüntüler, korelasyonlar, veri sızıntısı kontrolü) |
| [`notebooks/02_Preprocessing_FeatureSelection.ipynb`](notebooks/02_Preprocessing_FeatureSelection.ipynb) | özellik mühendisliği, tabakalı eğitim/test ayrımı, sızıntıya karşı güvenli ön işleme pipeline'ı (eksik değer doldurma, aykırı değer işleme, one-hot kodlama, ölçekleme), 8 özellik seçimi yönteminin 3 modelle karşılaştırılması |
| `notebooks/03_Modeling_Evaluation.ipynb` | KNN, Lojistik Regresyon, Karar Ağacı ve Random Forest ile 5 katlı tabakalı çapraz doğrulama, test kümesi değerlendirmesi, permutation importance, sızıntı ablasyonu |

## Çalıştırma

1. Notebook'ları [Google Colab](https://colab.research.google.com/) üzerinde sırasıyla açın (01 → 02 → 03).
2. Tüm hücreleri çalıştırın. Her notebook Google Drive'ı bağlar ve ara dosyaları `MyDrive/CME519_AIDev/` klasöründe saklar.

Kullanılan başlıca kütüphaneler: pandas, numpy, scikit-learn, matplotlib, seaborn, pyarrow, huggingface_hub (bkz. `requirements.txt`).

## Değerlendirme

Reddedilen PR'lar azınlık sınıfı olduğu için temel metrik **reddedilen sınıfın PR-AUC değeridir** ("birleştirilmedi" pozitif sınıf olarak alınan average precision). Ayrıca ROC-AUC, makro-F1, recall ve precision raporlanır.
