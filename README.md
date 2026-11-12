# 🔍 CLIP Text-to-Image Search

A text-to-image search engine built with OpenAI's **CLIP** and **FAISS**. Type a description like *"a dog running on the beach"* and get the most relevant images.

[

![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)

](https://colab.research.google.com/github/KamandNajari/clip-text-to-image-search/blob/main/text_to_image_search_clip.ipynb)


![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)




![PyTorch](https://img.shields.io/badge/PyTorch-DeepLearning-ee4c2c.svg)




![License](https://img.shields.io/badge/License-MIT-green.svg)



---

## ⭐ Two Main Features

1. **Search a built-in dataset:** search the 1,000 images of the Flickr8k test split with natural-language queries, and evaluate retrieval quality with Recall@1 / Recall@5 / Recall@10.
2. **Search your own images:** upload your own photos in the Gradio demo, build an index in seconds, and search through them with text.

Both features use the same pretrained CLIP model (zero-shot, no training required).

## ✨ Highlights

- Pretrained **CLIP ViT-B/32** maps images and text into one shared 512-dimensional space
- **FAISS** index over L2-normalized embeddings (cosine similarity)
- Quantitative evaluation on Flickr8k (1,000 images, 5,000 captions)
- Interactive **Gradio** demo with two tabs, one for each feature

## 💡 Example Queries

- `a dog running on the beach`
- `children playing in the snow`
- `a man riding a bike on a street`
- `people sitting on a bench`

## 🧠 How It Works

1. **Embedding:** CLIP has an image encoder and a text encoder that map their inputs into the same vector space. Matching images and captions end up close together.
2. **Indexing:** All images are embedded once, L2-normalized, and stored in a FAISS `IndexFlatIP` index (inner product = cosine similarity on normalized vectors). For your own images, the embeddings are kept in memory for the current session.
3. **Search:** The text query is embedded with the text encoder, and the top-k images with the highest cosine similarity are returned.

```
Text query ──► CLIP text encoder ──► query vector ─┐
                                                   ├─► cosine similarity ──► Top-k images
Images ──► CLIP image encoder ──► image vectors ───┘
```

## 🖥️ Gradio Demo

The last notebook cell launches a Gradio app with two tabs.

**Tab 1: Flickr8k dataset**
1. Type an English description.
2. Choose the number of results.
3. Click **Search** to see the best matching images with their similarity scores.

**Tab 2: Your own images**
1. Upload your images.
2. Click **Build index** and wait for the "Indexed N images" message.
3. Type an English description and click **Search**.

The index of your uploaded images is built in memory for the current session and is not stored anywhere.

> Note: the "Your own images" tab is newer than the rest of the project and has been tested less. Please open an issue if you run into a problem.

## 📊 Results

Evaluated on the **Flickr8k test split** (1,000 images, 5,000 captions). For each caption, the model must retrieve its ground-truth image among all 1,000 images.

| Model | Dataset | Recall@1 | Recall@5 | Recall@10 |
|-------|---------|:--------:|:--------:|:---------:|
| CLIP ViT-B/32 (zero-shot) | Flickr8k test | 53.76 | 80.80 | 88.86 |

> Recall@K = percentage of captions whose correct image appears in the top K results.

## 🚀 Quick Start

```bash
git clone https://github.com/KamandNajari/clip-text-to-image-search.git
cd clip-text-to-image-search
pip install -r requirements.txt
jupyter notebook text_to_image_search_clip.ipynb
```

A GPU is recommended but not required.

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

Thanks for visiting 🤍
