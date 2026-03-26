# Fedora issue #123: Assignment submission document for the _processing multilingual documents_ phase 2 task.

As an Outreachy contributor at the [Fedora project](https://fedoraproject.org/), I completed this assigned task that consisted of using [Docling](https://www.docling.ai/), an open-source optical character recognition (OCR) library, to convert scanned non-English documents (PDFs in my case) into machine-readable and editable digital text. Optionally, I completed the task using another OCR engine called [Surya](https://github.com/datalab-to/surya) with the intent of analyzing and comparing their results through performance and output evaluation.

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
# Install the packs for French, Hindi, African languages, and Indigenous scripts
sudo dnf install -y tesseract-langpack-fra tesseract-langpack-hin \
  tesseract-langpack-swa tesseract-langpack-yor tesseract-langpack-amh \
  tesseract-langpack-zul tesseract-langpack-wol tesseract-langpack-swa \
  tesseract-langpack-cre
```
This ensured my Fedora VM was ready to get me started on the task.

