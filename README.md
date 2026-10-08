# 🎥 YouTube Video Summarizer

Paste a YouTube link and get a clear, transcript-grounded summary in the language you choose.

The app fetches a video's transcript, cleans it, summarizes it with a Qwen2.5 LLM, and presents the result in a Streamlit web interface. It also flags names and numbers that don't appear in the transcript, so you know what to double-check.

## ✨ Features

- **Three summary lengths:** short, standard, and detailed
- **Multiple output languages**, including Arabic
- **Map-reduce summarization** for long videos: key facts are extracted from each chunk, then combined into the final summary
- **Hallucination checks:** names and numbers in the summary that are missing from the transcript are highlighted for verification
- **Custom transcript cleaning** for messy auto-generated captions
- **Manual transcript fallback** when YouTube blocks transcript access
- **4-bit quantization** (bitsandbytes) to run 7B and 14B models on limited GPU memory

## 🧠 How It Works

```
YouTube URL
   │
   ▼
Fetch transcript (youtube-transcript-api)  ──(blocked?)──▶  Paste transcript manually
   │
   ▼
Clean transcript
   │
   ▼
Split into chunks ──▶ Extract key facts per chunk (map)
   │
   ▼
Generate final summary from the facts (reduce) using Qwen2.5
   │
   ▼
Validate names & numbers against the transcript
   │
   ▼
Display in Streamlit
```

## 🛠️ Tech Stack

Python · Hugging Face Transformers · PyTorch · Streamlit · youtube-transcript-api · Qwen2.5 · bitsandbytes · Cloudflare Tunnel

## 🚀 Getting Started

### Prerequisites

- Python 3.10+
- A CUDA-capable GPU is recommended (required for 4-bit models)

### Installation

```bash
git clone https://github.com/3Mory12/<repo-name>.git
cd <repo-name>
pip install -r requirements.txt
```

### Run

```bash
streamlit run app.py
```

To share the app publicly from your own machine or a notebook, expose it with Cloudflare Tunnel:

```bash
cloudflared tunnel --url http://localhost:8501
```

## 📖 Usage

1. Paste a YouTube link.
2. Choose the summary length and output language.
3. Click **Summarize**.
4. If transcript fetching fails, paste the transcript manually in the fallback box.
5. Review any highlighted names or numbers that weren't found in the transcript.

## 💡 What I Learned

- **BART-CNN wasn't a good fit.** It generated content that wasn't in the video, so I switched to Qwen2.5 with a strict "use only the transcript" instruction.
- **Long videos need structure.** Map-reduce summarization preserved details that single-pass summarization lost.
- **Validation matters.** Simple checks on names and numbers catch many hallucinations.
- **Model choice affects language quality.** Testing 7B and 14B 4-bit Qwen models improved multilingual and Arabic output.
- **Real-world failures are part of the job.** Blocked transcripts and messy captions needed fallbacks and custom preprocessing.

## ⚠️ Limitations

- Summaries can still contain errors. The validation check only covers names and numbers.
- YouTube may block transcript requests, especially from cloud IPs.
- Larger models need a capable GPU.

## 🗺️ Roadmap

- [ ] Whisper-based speech-to-text for videos without captions
- [ ] Export summaries to Markdown/PDF
- [ ] Timestamped summary sections
- [ ] Broader fact-checking beyond names and numbers

## 🙏 Acknowledgments

Thanks to **Tips Hindawi** for the opportunity to work on practical AI projects.

## 📬 Feedback

Suggestions and issues are welcome. Open an issue or reach out via [GitHub](https://github.com/3Mory12).
