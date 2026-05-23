#SFT + LoRA#

Fine-Tuning Qwen2.5-0.5B untuk Bahasa Daerah Indonesia
Fine-tuning model bahasa menggunakan Supervised Fine-Tuning (SFT) dan LoRA
agar mampu merespons instruksi dalam bahasa daerah Indonesia.

Spesifikasi"
ItemDetailModelQwen/Qwen2.5-0.5B (base model)Datasetindonlp/cendol_collection_v2 (1.439 contoh aktif)PlatformGoogle Colab — Tesla T4 GPU (15.6 GB VRAM)Stacktransformers==4.47.0 · peft==0.13.2 · trl==0.12.0 · accelerate==1.1.1LoRAr=8 · alpha=16 · 7 target modules · 0.88% parameter dilatihHasilEval loss 2.7727 → 2.7193 · Training ~9.6 menit · Adapter 33.5 MB

Cara Pakai:
1. Upload notebook ke Google Colab
https://colab.research.google.com → File → Upload notebook
2. Aktifkan GPU T4
Runtime → Change runtime type → T4 GPU → Save
3. Jalankan Cell 0 untuk install dependencies, lalu Restart runtime
Runtime → Restart session
4. Jalankan Cell 1 hingga Cell 11 secara berurutan

Struktur Notebook
CellIsiCell 0Install semua dependenciesCell 1Import library dan cek GPUCell 2Konfigurasi global: model, dataset, hyperparameterCell 3Load tokenizer dan base model Qwen2.5-0.5BCell 4Eksplorasi respons base model sebelum fine-tuningCell 5Load dataset cendol, filter, dan format ke chat templateCell 6Konfigurasi LoRA dan hitung parameter trainableCell 7Training dengan SFTTrainerCell 8Visualisasi training loss curveCell 9Evaluasi kualitatif: perbandingan sebelum vs sesudahCell 10(Opsional) Merge adapter LoRA ke base modelCell 11Pertanyaan analisis

Hasil Training
Parameter LoRA  : 4.399.104 dari 498.431.872 total  (0.88%)
Epoch 1         : Train Loss 10.4256  |  Eval Loss 2.7727
Epoch 2         : Train Loss  9.4515  |  Eval Loss 2.7193
Adapter size    : 33.5 MB  (hemat 96.6% vs model penuh 988 MB)
Waktu training  : 575.8 detik (~9.6 menit)

Troubleshooting
MasalahSolusiOut of MemoryTurunkan BATCH_SIZE = 1Tidak ada internetUbah DATASET_CHOICE = 'synthetic'Training terlalu lambatPastikan GPU T4 aktif, bukan CPUImport errorJalankan Cell 0 ulang lalu restart runtime

Konfigurasi yang Dapat Diubah
Semua konfigurasi ada di Cell 2, tidak perlu menyentuh cell lain:
pythonMODEL_ID          = 'Qwen/Qwen2.5-0.5B'   # ganti model di sini
DATASET_CHOICE    = 'cendol'               # 'cendol' | 'bryandts' | 'synthetic'
MAX_TRAIN_SAMPLES = 1500                   # jumlah data yang digunakan
NUM_EPOCHS        = 2                      # jumlah epoch
LORA_R            = 8                      # rank LoRA

Referensi

LoRA Paper — Hu et al., 2021
Cendol Dataset — IndoNLP
Qwen2.5-0.5B — Alibaba Cloud
PEFT Docs — HuggingFace
TRL Docs — HuggingFace
