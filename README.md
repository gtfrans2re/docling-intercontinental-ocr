# Fedora issue #123: Assignment submission document for the _processing multilingual documents_ phase 2 task.

As an Outreachy contributor at the [Fedora project](https://fedoraproject.org/), I completed this assigned task that consisted of using [Docling](https://www.docling.ai/), an open-source optical character recognition (OCR) library, to convert scanned non-English documents (PDFs in my case) into machine-readable and editable digital text. Optionally, I completed the task using another OCR engine called [Surya](https://github.com/datalab-to/surya) with the intent of analyzing and comparing their results through performance and output evaluation.

---

## Task completion environment setup

I chose a Fedora-powered VM over a local Python virtual environment, firstly because it's Fedora, so I had to use a Fedora Linux server to work. Secondly, I have a 32GB laptop with 2T of storage, so there is enough room to host a Fedora VM on my local Ubuntu to isolate it. The setup was done with multipass canonical CLI VM creator with a downloaded Fedora 43 Cloud Image of x86_64 Intel kernel with 4 CPUs, 8GB RAM, and 50GB Disk.

Here is how I did that, and yours may vary based on your choice of environment :

```bash
multipass launch file:///home/gtfrans2re/Downloads/Fedora-Cloud-Base-Generic-43-1.6.x86_64.qcow2 \
  --name fedora-om26 \
  --cpus 4 \
  --memory 8G \
  --disk 50G
```

verified successful installation:
```bash
multipass list | grep -E "fedora"
```

And logged into the Fedora VM:
```bash
multipass shell fedora-om26
```
![Fedora VM Setup](<assets/Screenshot from 2026-03-26 02-10-35.png>)

Updated and installed Base Build tools:
```bash
sudo dnf update -y
sudo dnf install -y gcc gcc-c++ python3-devel libjpeg-devel zlib-devel tesseract tesseract-devel
```

installed language packages with Tesserract to match the intercontinental context (North America, Europe, Africa, and Asia):
```bash
# Installing  packs for French, Hindi, African languages, and Indigenous scripts
sudo dnf install -y tesseract-langpack-fra tesseract-langpack-hin \
  tesseract-langpack-swa tesseract-langpack-yor tesseract-langpack-amh \
  tesseract-langpack-zul tesseract-langpack-wol tesseract-langpack-swa \
  tesseract-langpack-cre
```
This ensured my Fedora VM was ready to get me started on the task.

---

## The core task: Getting started with Docling (and the optional Surya) to perform task completion

Although I preferred a Fedora VM over a local virtual environment, I had to set up the same in the VM to isolate it from upcoming Fedora-related work.
```bash
python3 -m venv venv-ocr
source venv-ocr/bin/activate
pip install --upgrade pip
```
![Virtual Environment setup](<assets/Screenshot from 2026-03-26 02-51-07.png>)

Now that the system-level dependencies are there, here is how I performed the following steps:

1. Installing Docling (and Surya OCR)
```bash
pip install "docling[ocr]" surya-ocr
```
![OCR installation](<assets/Screenshot from 2026-03-26 02-55-11.png>)

If you ever face this ERROR: Failed building wheel for _pillow_, installing Python 3.13 fixed it for me. It's a required package that may not be provided 
by your current running version of Python.
```bash
sudo dnf install python3.13 python3.13-devel
```
This may differ based on your (Linux) OS. Repeat the installation process afterwards, and it shall work fine for you.

2. Displaying the Docling (and Surya-OCR) version

- For Docling:
```bash
docling --version
```
![Docling version](<assets/Screenshot from 2026-03-26 03-07-09.png>)

- Optionally, Surya OCR for the comparison
```bash
pip show surya-ocr
```
![Surya version](<assets/Screenshot from 2026-03-26 03-16-25.png>)

3. Displaying the OCR engine version

- For the OCR Engine, which is Tesseract in this case:
```bash
tesseract --version
```
![OCR Engine version](<assets/Screenshot from 2026-03-26 03-09-20.png>)

4. The Intercontinental Dataset (UDHR)
For this evaluation, I curated a dataset from the **Universal Declaration of Human Rights (UDHR)** provided by the UN Office of the High Commissioner for Human Rights (OHCHR). These documents were chosen because they contain high-quality scans of complex scripts. All files are stored in the `data/` directory.

| Language | Script Type | Filename in `data/` |
| :--- | :--- | :--- |
| **French** | Latin | `frn.pdf` |
| **Hindi** | Devanagari | `hnd.pdf` |
| **Swahili** | Latin | `swa.pdf` |
| **Yoruba** | Latin (Extended) | `yor.pdf` |
| **Amharic** | Ge'ez | `amh.pdf` |
| **Zulu** | Latin | `zuu.pdf` |
| **Wolof** | Latin | `wol.pdf` |
| **Inuktitut** | Syllabics | `iku.pdf` |
| **Plains Cree** | Syllabics | `crm.pdf` |

5. OCR Output Directories
The processed results are organized into separate directories based on the OCR engine used. This allows for a clear side-by-side comparison of the structured Markdown and the layout detection images.

| Engine | Directory Path | Output Type |
| :--- | :--- | :--- |
| **Docling** | `output/docling/` | Structured Markdown (`.md`) |
| **Surya** | `output/surya/` | Layout JSON and Bounding Box Images (`.json`, `.png`) |

- First Docling conversion (same command with next filenames)
```bash
docling --ocr --ocr-lang fra --to md --output output/docling data/frn.pdf
```
![French Docling](<assets/Screenshot from 2026-03-26 03-31-23.png>)

- First Surya conversion (same command with next filenames)
```bash
surya_ocr data/frn.pdf --output_dir output/surya --images
```
![French Surya](<assets/Screenshot from 2026-03-26 03-36-44.png>)

To verify the generated outputs:
# List Docling Markdown results
ls output/docling/

# List Surya layout detection results
ls output/surya/
