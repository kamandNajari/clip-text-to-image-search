# 🔍 CLIP Text-to-Image Search

Search images using natural language. Type a description like *"a dog running on the beach"* and retrieve the most relevant photos, powered by OpenAI's **CLIP** and **FAISS**.

[

![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)

](https://colab.research.google.com/github/KamandNajari/clip-text-to-image-search/blob/main/text_to_image_search_clip.ipynb)


![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)




![PyTorch](https://img.shields.io/badge/PyTorch-DeepLearning-ee4c2c.svg)




![License](https://img.shields.io/badge/License-MIT-green.svg)



---

## ✨ Features

- **Zero-shot text-to-image retrieval** with a pretrained CLIP model (no training required)
- **Fast similarity search** using a FAISS index over normalized embeddings
- **Quantitative evaluation** with Recall@1 / Recall@5 / Recall@10
- **Interactive Gradio demo** to search the Flickr8k test images
- **Search your own images** from the demo (experimental)
- Runs entirely in **Google Colab** on a free T4 GPU

## 💡 Example Queries

- `a dog running on the beach`
- `children playing in the snow`
- `a man riding a bike on a street`
- `people sitting on a bench`

## 🧠 How It Works

1. **Embedding:** CLIP has an image encoder and a text encoder that map their inputs into the same 512-dimensional vector space. Matching images and captions end up close together.
2. **Indexing:** All images are embedded once, L2-normalized, and stored in a FAISS `IndexFlatIP` index (inner product = cosine similarity on normalized vectors).
3. **Search:** The text query is embedded with the text encoder, and the top-k images with the highest cosine similarity are returned.

```
Text query ──► CLIP text encoder ──► query vector ─┐
                                                   ├─► cosine similarity ──► Top-k images
Images ──► CLIP image encoder ──► image vectors ───┘
```

## 📊 Results

Evaluated on the **Flickr8k test split** (1,000 images, 5,000 captions). For each caption, the model must retrieve its ground-truth image among all 1,000 images.

| Model | Dataset | Recall@1 | Recall@5 | Recall@10 |
|-------|---------|:--------:|:--------:|:---------:|
| CLIP ViT-B/32 (zero-shot) | Flickr8k test | 53.76 | 80.80 | 88.86 |

> Recall@K = percentage of captions whose correct image appears in the top K results.

## 🚀 Quick Start

### Option 1: Google Colab (recommended)

1. Click the **Open in Colab** badge above.
2. Go to **Runtime → Change runtime type → T4 GPU**.
3. Run all cells (**Runtime → Run all**).
4. Change the query in the search cell:
   ```python
   show_results("a dog running on the beach")
   ```

### Option 2: Run locally

```bash
git clone https://github.com/KamandNajari/clip-text-to-image-search.git
cd clip-text-to-image-search
pip install -r requirements.txt
jupyter notebook text_to_image_search_clip.ipynb
```

A GPU is recommended but not required.

## 🖥️ Gradio Demo

The last notebook cell launches a Gradio app with two tabs:

1. **Flickr8k dataset:** type a query and get the best matching images from the 1,000 test images.
2. **Your own images (experimental):** upload your photos, click **Build index**, then type a query and click **Search**. This tab has not been fully tested yet. The index is built in memory and is not stored anywhere.

## 🛠️ Tech Stack

- [PyTorch](https://pytorch.org/) and [Hugging Face Transformers](https://huggingface.co/docs/transformers) (CLIP)
- [FAISS](https://github.com/facebookresearch/faiss) for vector search
- [Hugging Face Datasets](https://huggingface.co/docs/datasets) (Flickr8k)
- [Gradio](https://www.gradio.app/) for the web interface
- NumPy, Matplotlib, Pillow

## 📁 Project Structure

```
clip-text-to-image-search/
├── text_to_image_search_clip.ipynb   # Main notebook
├── requirements.txt
├── README.md
├── LICENSE
└── .gitignore
```

## ⚠️ Limitations

- The built-in search covers only the 1,000 Flickr8k test images; queries with no matching content return the closest images anyway.
- The model understands **English** queries only.
- Zero-shot performance; the model is not fine-tuned on this dataset.

## 🔮 Future Work

- [ ] Fully test and polish the "Your own images" tab
- [ ] Multilingual queries (including Persian) with a multilingual CLIP model
- [ ] Compare more models (CLIP ViT-L/14, SigLIP, OpenCLIP)
- [ ] Fine-tune CLIP on a custom dataset and report the improvement
- [ ] Deploy a permanent demo on Hugging Face Spaces
- [ ] Refactor into a reusable `src/` package with `app.py`

## 📚 References

- Radford et al., *Learning Transferable Visual Models From Natural Language Supervision* (CLIP), 2021
- Johnson et al., *Billion-scale similarity search with GPUs* (FAISS), 2017
- Hodosh et al., *Framing Image Description as a Ranking Task* (Flickr8k), 2013

## 📄 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## 👤 Author

**Kamand Najari**
GitHub: [@KamandNajari](https://github.com/KamandNajari)

⭐ If you find this project useful, please consider giving it a star!
