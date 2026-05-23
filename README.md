# SFT + LoRA: Fine-Tuning Qwen2.5-0.5B untuk Bahasa Daerah Indonesia

Fine-tuning model bahasa menggunakan **Supervised Fine-Tuning (SFT)** dan **LoRA**
agar mampu merespons instruksi dalam bahasa daerah Indonesia.

---

## Spesifikasi

| Item | Detail |
|------|--------|
| Model | `Qwen/Qwen2.5-0.5B` (base model) |
| Dataset | `indonlp/cendol_collection_v2` (1.439 contoh aktif) |
| Platform | Google Colab — Tesla T4 GPU (15.6 GB VRAM) |
| Stack | `transformers==4.47.0` · `peft==0.13.2` · `trl==0.12.0` · `accelerate==1.1.1` |
| LoRA | r=8 · alpha=16 · 7 target modules · 0.88% parameter dilatih |
| Hasil | Eval loss 2.7727 → 2.7193 · Training ~9.6 menit · Adapter 33.5 MB |

---

## Cara Pakai

**1. Upload notebook ke Google Colab**

```
https://colab.research.google.com → File → Upload notebook
```

**2. Aktifkan GPU T4**

```
Runtime → Change runtime type → T4 GPU → Save
```

**3. Jalankan Cell 0** untuk install dependencies, lalu **Restart runtime**

```
Runtime → Restart session
```

**4. Jalankan Cell 1 hingga Cell 11 secara berurutan**

---

## Struktur Notebook

| Cell | Isi |
|------|-----|
| Cell 0 | Install semua dependencies |
| Cell 1 | Import library dan cek GPU |
| Cell 2 | Konfigurasi global: model, dataset, hyperparameter |
| Cell 3 | Load tokenizer dan base model Qwen2.5-0.5B |
| Cell 4 | Eksplorasi respons base model sebelum fine-tuning |
| Cell 5 | Load dataset cendol, filter, dan format ke chat template |
| Cell 6 | Konfigurasi LoRA dan hitung parameter trainable |
| Cell 7 | Training dengan SFTTrainer |
| Cell 8 | Visualisasi training loss curve |
| Cell 9 | Evaluasi kualitatif: perbandingan sebelum vs sesudah |
| Cell 10 | (Opsional) Merge adapter LoRA ke base model |
| Cell 11 | Pertanyaan analisis |

---

## Hasil Training

```
Parameter LoRA  : 4.399.104 dari 498.431.872 total  (0.88%)
Epoch 1         : Train Loss 10.4256  |  Eval Loss 2.7727
Epoch 2         : Train Loss  9.4515  |  Eval Loss 2.7193
Adapter size    : 33.5 MB  (hemat 96.6% vs model penuh 988 MB)
Waktu training  : 575.8 detik (~9.6 menit)
```

---

## Troubleshooting

| Masalah | Solusi |
|---------|--------|
| Out of Memory | Turunkan `BATCH_SIZE = 1` |
| Tidak ada internet | Ubah `DATASET_CHOICE = 'synthetic'` |
| Training terlalu lambat | Pastikan GPU T4 aktif, bukan CPU |
| Import error | Jalankan Cell 0 ulang lalu restart runtime |

---

## Konfigurasi yang Dapat Diubah

Semua konfigurasi ada di **Cell 2**, tidak perlu menyentuh cell lain:

```python
MODEL_ID          = 'Qwen/Qwen2.5-0.5B'   # ganti model di sini
DATASET_CHOICE    = 'cendol'               # 'cendol' | 'bryandts' | 'synthetic'
MAX_TRAIN_SAMPLES = 1500                   # jumlah data yang digunakan
NUM_EPOCHS        = 2                      # jumlah epoch
LORA_R            = 8                      # rank LoRA
```

---

## Referensi

- [LoRA Paper](https://arxiv.org/abs/2106.09685) — Hu et al., 2021
- [Cendol Dataset](https://huggingface.co/datasets/indonlp/cendol_collection_v2) — IndoNLP
- [Qwen2.5-0.5B](https://huggingface.co/Qwen/Qwen2.5-0.5B) — Alibaba Cloud
- [PEFT Docs](https://huggingface.co/docs/peft) — HuggingFace
- [TRL Docs](https://huggingface.co/docs/trl) — HuggingFace

---

*Ima Mahadma — Universitas Gadjah Mada — 2026*
