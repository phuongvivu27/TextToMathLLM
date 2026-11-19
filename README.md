# Math Formula Generator AI

Ứng dụng web chuyển mô tả toán học bằng **tiếng Việt dưới dạng văn bản hoặc giọng nói hoặc hình ảnh** thành:

- Biểu thức toán chuẩn hóa
- LaTeX
- Cấu trúc AST
- Giải thích công thức
- Tài liệu Microsoft Word (`.docx`)

Dự án kết hợp Whisper, RAG, Gemini hoặc Ollama/Qwen, tokenizer và parser tự xây dựng để tạo công thức toán có kiểm tra cú pháp.

## Tính năng

- Nhập công thức bằng văn bản
- Tải file âm thanh hoặc ghi âm trực tiếp trên trình duyệt
- Nhận dạng giọng nói tiếng Việt bằng Faster-Whisper
- Sửa lỗi thuật ngữ toán học bằng fuzzy matching
- Chuẩn hóa mô tả thành biểu thức toán bằng LLM
- Tìm kiếm tri thức toán học bằng Sentence Transformers và RAG
- Phân tách công thức, tokenization, parsing và xây dựng AST
- Xuất LaTeX, hiển thị công thức bằng MathJax
- Thêm nhiều công thức vào tài liệu và xuất `.docx`

## Công nghệ

- Backend: Python, FastAPI
- LLM: Google Gemini hoặc Ollama/Qwen
- Speech-to-text: Faster-Whisper
- RAG: `sentence-transformers`, NumPy, pandas
- NLP: Fuzzy matching với RapidFuzz
- Parser: custom tokenizer, parser và AST
- Frontend: HTML, CSS, JavaScript, MathJax
- Export: python-docx, MathML và OMML

## Yêu cầu hệ thống

- Python 3.10 trở lên
- Kết nối internet cho API Gemini và khi tải mô hình lần đầu
- Khoảng 5 GB RAM trở lên được khuyến nghị khi dùng Whisper `medium`
- FFmpeg có thể cần cho một số định dạng audio
- Gemini API key nếu dùng provider mặc định

## Cài đặt

### 1. Clone repository

```powershell
git clone https://github.com/PhuongViVu/SpeechToMath.git
cd SpeechToMath
```

### 2. Tạo môi trường Python

```powershell
py -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Nếu PowerShell chặn kích hoạt môi trường:

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
```

### 3. Cấu hình Gemini

Tạo file `.env` từ mẫu:

```powershell
Copy-Item .env.example .env
```

Sau đó mở `.env` và thêm API key:

```env
GEMINI_API_KEY=your_gemini_api_key
```

### 4. Chọn provider LLM

Provider mặc định trong `config.py` là Gemini:

```python
LLM_PROVIDER = "gemini"
MODEL_NAME = "models/gemini-3.6-flash"
```

Để dùng Ollama/Qwen, cài Ollama, tải model và chỉnh cấu hình:

```python
LLM_PROVIDER = "ollama"
OLLAMA_URL = "http://localhost:11434/api/generate"
MODEL_NAME = "qwen2.5:3b"
```

Sau đó kiểm tra Ollama:

```powershell
ollama pull qwen2.5:3b
ollama run qwen2.5:3b
```

## Khởi động ứng dụng

```powershell
uvicorn app:app --reload --host 127.0.0.1 --port 8000
```

Mở trình duyệt tại <http://127.0.0.1:8000>.

API tài liệu tự động có tại:

- Swagger UI: <http://127.0.0.1:8000/docs>
- ReDoc: <http://127.0.0.1:8000/redoc>


## Cấu trúc thư mục

```text
.
├── app.py                  # FastAPI entry point và REST API
├── pipeline.py             # Pipeline tổng hợp toàn bộ các bước
├── config.py               # Cấu hình LLM, Whisper và RAG
├── build_embeddings.py     # Tạo lại embedding cho knowledge base
├── requirements.txt
├── data/                   # Công thức, dataset và file audio mẫu
├── generator/              # LaTeX, MathML, OMML và export DOCX
├── llm/                    # Gemini, Ollama/Qwen, RAG và prompt
├── parser/                 # Tokenizer, parser và precedence rules
├── rag/                    # Knowledge loader, embedder và vector retrieval
├── speech/                 # Whisper, fuzzy correction và normalization
├── syntax_tree/            # AST node và visitor
├── templates/              # Giao diện Jinja2
├── static/                 # CSS và JavaScript
├── tests/                  # Unit và integration tests
└── utils/                  # Tiện ích xử lý công thức
```

