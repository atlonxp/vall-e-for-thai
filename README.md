<p align="center">
<img src="./vall-e.png" width="500px"></img>
</p>

# VALL-E for Thai

An implementation VALL-E for Thai. This source code is unofficial based on the PyTorch implementation of [VALL-E](https://valle-demo.github.io/), based on the [EnCodec](https://github.com/facebookresearch/encodec) tokenizer.

## Get Started

> A toy Google Colab example: [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1LNWhEyxkHotPS5H52DPm9tstXOAbTFYK?usp=sharing).
> Please note that this example overfits a single utterance under the `data/test` and is not usable.
> The pretrained model is yet to come. 

### Requirements

Since the trainer is based on [DeepSpeed](https://github.com/microsoft/DeepSpeed#requirements), you will need to have a GPU that DeepSpeed has developed and tested against, as well as a CUDA or ROCm compiler pre-installed to install this package.

### Install

```
pip install git+https://github.com/atlonxp/vall-e-for-thai
```

Or you may clone by:

```
git clone --recurse-submodules https://github.com/atlonxp/vall-e-for-thai.git
```

Note that the code is only tested under `Python 3.10.x`.

### Train

1. Put your data into a folder, e.g. `data/your_data`. Audio files should be named with the suffix `.wav` and text files with `.normalized.txt`.

2. Quantize the data:

```
python -m vall_e.emb.qnt data/your_data
```

3. Generate phonemes based on the text:

```
python -m vall_e.emb.g2p data/your_data
```

4. Customize your configuration by creating `config/your_data/ar.yml` and `config/your_data/nar.yml`. Refer to the example configs in `config/test` and `vall_e/config.py` for details. You may choose different model presets, check `vall_e/vall_e/__init__.py`.

5. Train the AR or NAR model using the following scripts:

```
python -m vall_e.train yaml=config/your_data/ar_or_nar.yml
```

You may quit your training any time by just typing `quit` in your CLI. The latest checkpoint will be automatically saved.

### Export

Both trained models need to be exported to a certain path. To export either of them, run:

```
python -m vall_e.export zoo/ar_or_nar.pt yaml=config/your_data/ar_or_nar.yml
```

This will export the latest checkpoint.

### Synthesis

```
python -m vall_e <text> <ref_path> <out_path> --ar-ckpt zoo/ar.pt --nar-ckpt zoo/nar.pt
```

## TODO

- [x] AR model for the first quantizer
- [x] Audio decoding from tokens
- [x] NAR model for the rest quantizers
- [x] Trainers for both models
- [x] Implement AdaLN for NAR model.
- [x] Sample-wise quantization level sampling for NAR training.
- [ ] Pre-trained checkpoint and demos on LibriTTS
- [x] Synthesis CLI

## Notice

- [EnCodec](https://github.com/facebookresearch/encodec) is licensed under CC-BY-NC 4.0. If you use the code to generate audio quantization or perform decoding, it is important to adhere to the terms of their license.