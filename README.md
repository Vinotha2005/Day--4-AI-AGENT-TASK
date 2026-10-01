# VRAM Estimation Results

**Memory available:** 16.0 GB

## Model Variations

| Model | Quantization | Parameters | Context | Weights VRAM | KV VRAM | Total VRAM | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Qwen small | Q4_K_M | 1.5B | 8K | 0.85 GB | 0.24 GB | 1.20 GB | fits comfortably |
| Granite / Qwen mid | Q4_K_M | 8.0B | 8K | 4.56 GB | 1.28 GB | 6.42 GB | fits comfortably |
| Mid at FP16 | FP16 | 8.0B | 8K | 16.00 GB | 1.28 GB | 19.01 GB | does NOT fit |
| Large local | Q4_K_M | 30.0B | 8K | 17.10 GB | 4.80 GB | 24.09 GB | does NOT fit |
| Server class | Q4_K_M | 70.0B | 8K | 39.90 GB | 11.20 GB | 56.21 GB | does NOT fit |

## Same 8B model, different context lengths

| Model | Quantization | Parameters | Context | Weights VRAM | KV VRAM | Total VRAM | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 8B agent | Q4_K_M | 8.0B | 4K | 4.56 GB | 0.64 GB | 5.72 GB | fits comfortably |
| 8B agent | Q4_K_M | 8.0B | 8K | 4.56 GB | 1.28 GB | 6.42 GB | fits comfortably |
| 8B agent | Q4_K_M | 8.0B | 32K | 4.56 GB | 5.12 GB | 10.65 GB | fits comfortably |
| 8B agent | Q4_K_M | 8.0B | 128K | 4.56 GB | 20.48 GB | 27.54 GB | does NOT fit |

## Same 8B model, different quantizations

| Model | Quantization | Parameters | Context | Weights VRAM | KV VRAM | Total VRAM | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 8B agent | Q3_K_M | 8.0B | 8K | 3.44 GB | 1.28 GB | 5.19 GB | fits comfortably |
| 8B agent | Q4_K_M | 8.0B | 8K | 4.56 GB | 1.28 GB | 6.42 GB | fits comfortably |
| 8B agent | Q5_K_M | 8.0B | 8K | 5.44 GB | 1.28 GB | 7.39 GB | fits comfortably |
| 8B agent | Q8_0 | 8.0B | 8K | 8.00 GB | 1.28 GB | 10.21 GB | fits comfortably |
| 8B agent | FP16 | 8.0B | 8K | 16.00 GB | 1.28 GB | 19.01 GB | does NOT fit |
