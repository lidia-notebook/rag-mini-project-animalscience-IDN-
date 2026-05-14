# RAG Animal Science Chatbot

Aplikasi chatbot berbasis Retrieval Augmented Generation (RAG) dengan knowledge base seputar Animal Science (Ilmu Peternakan). Chatbot ini dapat menjawab pertanyaan teknis tentang anatomi hewan ternak, nutrisi pakan, reproduksi, genetika, kesehatan hewan, dan manajemen produksi peternakan.

## Latar Belakang

Animal Science adalah cabang ilmu yang mempelajari berbagai aspek tentang hewan ternak. Di Indonesia, sektor peternakan memiliki peran penting dalam ketahanan pangan, namun informasi teknis seringkali tersebar di berbagai literatur akademik, jurnal, dan laporan yang tidak mudah diakses cepat.

Project ini membangun solusi berupa chatbot yang menggabungkan kekuatan Large Language Model (LLM) dengan sistem pencarian dokumen berbasis embedding, sehingga jawaban yang dihasilkan bersumber langsung dari knowledge base yang terstruktur dan dapat diverifikasi.

## Fitur Utama

- Tanya jawab berbasis dokumen dalam bahasa Indonesia
- Knowledge base dapat diperbarui secara dinamis (tambah dokumen atau ganti seluruhnya)
- Source attribution — setiap jawaban menyertakan dokumen sumber
- Anti-halusinasi — sistem menolak menjawab pertanyaan di luar knowledge base
- Multilingual retrieval — embedding mendukung bahasa Indonesia dan Inggris
- Cepat — menggunakan Groq API dengan LPU untuk inferensi LLM sub-detik

## Ruang Lingkup Knowledge Base

Knowledge base mencakup sub-bidang utama Animal Science:

1. Anatomi dan fisiologi hewan ternak (ruminansia vs non-ruminansia)
2. Nutrisi dan pakan ternak (sapi perah, sapi potong, unggas)
3. Reproduksi hewan (siklus estrus, inseminasi buatan, kebuntingan)
4. Genetika dan pemuliaan ternak (heritabilitas, persilangan, EBV)
5. Kesehatan hewan dan penyakit (PMK, vaksinasi, biosekuriti)
6. Manajemen produksi (broiler, kambing, domba, sapi Bali)
7. Kesejahteraan hewan (animal welfare)
8. Teknologi pakan (silase, fermentasi)

## Tech Stack

| Komponen | Pilihan | Alasan |
|----------|---------|--------|
| LLM | Llama 3.3 70B via Groq API | Free tier, sangat cepat (LPU), bahasa Indonesia bagus |
| Embedding | paraphrase-multilingual-MiniLM-L12-v2 | Multilingual, mendukung bahasa Indonesia, gratis |
| Vector DB | ChromaDB (in-memory) | Ringan, mudah di-setup, tidak butuh server eksternal |
| Framework | LangChain (LCEL) | Standar industri untuk RAG, modular, future-proof |
| Platform | Google Colab | Gratis, tidak butuh setup environment lokal |

## Cara Menjalankan

### Prasyarat

1. Akun Google (untuk akses Google Colab)
2. API key dari Groq — daftar gratis di https://console.groq.com

### Langkah-Langkah

1. Buka notebook di Google Colab
   - Download file `RAG_Animal_Science.ipynb` dari repo ini
   - Buka https://colab.research.google.com
   - File → Upload notebook → pilih file yang sudah di-download

2. Setup API Key
   - Di Colab, klik ikon kunci di sidebar kiri (Secrets)
   - Klik Add new secret
   - Name: `GROQ_API_KEY`
   - Value: paste API key dari Groq (dimulai dengan `gsk_...`)
   - Aktifkan toggle Notebook access

3. Jalankan Semua Cell
   - Klik Runtime → Run all
   - Tunggu instalasi library dan loading model (sekitar 3-5 menit)

4. Mulai Bertanya
   - Setelah semua cell ter-run, gunakan fungsi `ask()` untuk bertanya:
```python
   ask("Berapa lama masa kebuntingan sapi?")
```

## Contoh Penggunaan

### Pertanyaan Faktual
```python
ask("Berapa kebutuhan protein kasar untuk sapi perah laktasi?")
```
Output: "Kebutuhan Protein Kasar (PK) sapi perah laktasi adalah 16-18% dari Bahan Kering (BK) ransum..."

### Update Knowledge Base
```python
add_document(
    title="Sapi Bali",
    content="Sapi Bali (Bos javanicus) adalah sapi asli Indonesia..."
)

vectorstore = rebuild_vectorstore()
qa_chain = build_rag_chain(vectorstore)

ask("Apa keunggulan sapi Bali?")
```

### Pertanyaan di Luar Knowledge Base
```python
ask("Bagaimana cara membuat brownies?")
```
Output: "Maaf, informasi tersebut tidak tersedia dalam knowledge base saya."

Author: Lidia Priskila
