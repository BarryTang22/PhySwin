# PhySwin: An Efficient and Physically-Informed Foundation Model for Multispectral Earth Observation

**"PhySwin: An Efficient and Physically-Informed Foundation Model for Multispectral Earth Observation"**
*NeurIPS 2025*

## Repository Status

This repository is currently under active development. We apologize for the slow updates as we are managing multiple deadlines. However, if you encounter any issues or have questions while trying to use the code, please don't hesitate to contact us directly. We are happy to arrange a video call or assist you through any communication method that works best for you.

### Roadmap

- [x] Model architecture implementation
- [x] Configuration files
- [ ] Pre-training scripts
- [ ] Fine-tuning scripts
- [ ] Evaluation scripts
- [ ] Pre-trained model weights
- [ ] Dataset (will be released after the main conference)
- [ ] Complete documentation and tutorials

## Overview

PhySwin is a modified implementation of Swin Transformer V2 that incorporates MixMAE (Mixed Masked Autoencoder) pretraining for multi-spectral satellite imagery processing. The model is specifically designed for Sentinel-2 multispectral data with physically-informed architecture choices.

## Key Features

- **Multi-Spectral Support**: Handles 10-channel Sentinel-2 satellite imagery with grouped channel processing
- **MixMAE Pretraining**: Self-supervised learning through image mixing and unmixing
- **Physically-Informed Design**: Spectral band grouping based on physical properties of electromagnetic radiation
- **Group-Based Patch Embedding**: Separate embedding layers for different spectral band groups
- **Efficient Architecture**: Optimized for satellite imagery resolution and multi-spectral processing

## Installation

### From Source

```bash
git clone https://github.com/BarryTang22/PhySwin.git
cd PhySwin
pip install -e .
```

### Dependencies

```bash
pip install -r requirements.txt
```

Required packages:
- `torch>=2.0.0`
- `transformers>=4.30.0`
- `timm>=0.9.0`
- `numpy>=1.20.0`

## Usage

### Configuration

```python
from swinv2 import Swinv2Config

config = Swinv2Config(
    image_size=128,
    num_channels=10,
    group_channel_indices=[[0, 1, 2], [3, 4, 5, 6, 7], [8, 9]],
    decoder_dim=512,
    decoder_depth=8,
    decoder_num_heads=16,
)
```

### MixMAE Pretraining

```python
from swinv2 import Swinv2MixMAE
import torch

model = Swinv2MixMAE(config)
pixel_values = torch.randn(2, 10, 128, 128)

loss, reconstructed_images, mixing_mask = model(pixel_values, mask_ratio=0.5)
print(f"Loss: {loss.item()}")
print(f"Reconstructed shape: {reconstructed_images.shape}")
```

## Model Architecture

### Key Modifications from Standard Swin Transformer V2

1. **Grouped Channel Processing**: Separate convolutional projections for different spectral band groups
2. **MixMAE Integration**: Mixing masks propagated through encoder layers for self-supervised learning
3. **Vision Transformer Decoder**: 8-layer transformer decoder for image reconstruction
4. **Dual-Path Decoding**: Simultaneous reconstruction of both mixed images

### Physically-Informed Spectral Band Grouping (Sentinel-2)

The grouping is based on the physical properties and typical applications of different spectral bands:

- **Group 1 (RGB)**: B02, B03, B04 - Visible spectrum
- **Group 2 (Red Edge + NIR)**: B05, B06, B07, B08, B8A - Vegetation analysis
- **Group 3 (SWIR)**: B11, B12 - Moisture and geological features

## Configuration Parameters

| Parameter | Default | Description |
|-----------|---------|-------------|
| `image_size` | 128 | Input image resolution |
| `num_channels` | 10 | Number of input channels |
| `patch_size` | 4 | Patch size for embedding |
| `embed_dim` | 96 | Base embedding dimension |
| `depths` | [2, 2, 6, 2] | Number of layers per stage |
| `num_heads` | [3, 6, 12, 24] | Attention heads per stage |
| `window_size` | 7 | Window size for attention |
| `decoder_dim` | 512 | Decoder hidden dimension |
| `decoder_depth` | 8 | Number of decoder layers |
| `decoder_num_heads` | 16 | Decoder attention heads |
| `group_channel_indices` | [[0,1,2], [3,4,5,6,7], [8,9]] | Physically-informed channel grouping |

## Citation

If you use this code in your research, please cite our paper.

## License

This project is licensed under the Apache License 2.0.

## Acknowledgments

- Based on [Hugging Face Transformers](https://github.com/huggingface/transformers) Swin Transformer V2 implementation
- MixMAE methodology from [MixMAE: Mixed and Masked Autoencoder for Efficient Pretraining of Hierarchical Vision Transformers](https://arxiv.org/abs/2205.13137)
- Position embeddings adapted from [MAE](https://github.com/facebookresearch/mae) and [MoCo v3](https://github.com/facebookresearch/moco-v3)

## Contact

For questions and feedback, please contact:
- Barry Tang: barrytang19@gmail.com
