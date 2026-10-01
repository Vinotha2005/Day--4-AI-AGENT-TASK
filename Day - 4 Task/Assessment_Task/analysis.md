# Will It Fit, and May I Use It?

## 3.1 Scenario and Concept Explanation
**Scenario:** Running an open model locally on a 16GB laptop to act as a coding helper for myself. The memory budget is 16GB RAM. The licensing should permit personal use.

* **Model weights:** The weights are fixed by the number of parameters and the precision used; quantization lowers the bytes per parameter, saving memory at some cost in quality. In my scenario, this controls the base VRAM needed. If ignored, the model simply will not fit into memory.
* **Quantization:** Reduces the size of model weights. This is critical for fitting a capable 8B model into a 16GB machine. If misjudged, the model will run out of memory or quality will be too poor.
* **KV cache and context length:** The KV cache grows with context length, so a model that fits at a short context can stop fitting at a long one — which matters for anything that keeps adding to the conversation, such as an agent collecting tool results. I used this to calculate maximum token capacity.
* **The memory formula:** Gives an estimate for deciding fit; real usage differs with architecture, runtime defaults and build. I used this to pre-calculate if Llama 3 8B would fit.
* **Model card & Licensing:** Open-weight is not the same as open-source; the exact licence name and its conditions decide commercial use and redistribution, and a model card is where you find them. Essential to ensure the model allows personal/coding use.

## 3.2 Estimate table and comparison table

### (a) Memory Estimate Table (Available Memory: 16 GB)

| Model | Params (B) | Precision | Context (K) | Weights (GB) | KV cache (GB) | Total (GB) | Fits in 16 GB? |
|---|---|---|---|---|---|---|---|
| Qwen small | 1.5 | Q4_K_M | 8 | 0.85 | 0.24 | 1.20 | fits comfortably |
| Granite/Qwen mid | 8.0 | Q4_K_M | 8 | 4.56 | 1.28 | 6.42 | fits comfortably |
| Mid at FP16 | 8.0 | FP16 | 8 | 16.00 | 1.28 | 19.01 | does NOT fit |
| Large local | 30.0 | Q4_K_M | 8 | 17.10 | 4.80 | 24.09 | does NOT fit |

### (b) Comparison Table

| Basis for comparison | Model 1 (Qwen2.5) | Model 2 (Mistral) | Model 3 (Granite) |
|---|---|---|---|
| Full model name and version | Qwen2.5-7B-Instruct | Mistral-7B-Instruct-v0.3 | granite-7b-instruct |
| Publisher | Alibaba | Mistral AI | IBM |
| Total / active parameters | 7B | 7B | 7B |
| Context window | 128K | 32K | 4K |
| Licence (exact name) | Apache 2.0 | Apache 2.0 | Apache 2.0 |
| Commercial use allowed? | Yes | Yes | Yes |
| Any extra conditions? | No | No | No |
| Tool calling stated on the card? | Yes | Yes | Yes |
| GGUF / Ollama build available? | Yes | Yes | Yes |
| Download size at Q4 | 4.7 GB | 4.1 GB | 4.1 GB |
| Your memory estimate (total) | 6.42 GB | 6.42 GB | 6.42 GB |
| Fits your scenario’s machine? | Yes | Yes | Yes |
| Date you checked the card | Oct 1, 2026 | Oct 1, 2026 | Oct 1, 2026 |

## 3.3 Context length and quantization observation

| Setting changed | Value used | Weights (GB) | KV cache (GB) | Total (GB) | Fits? |
|---|---|---|---|---|---|
| Context length | 4K (Q4_K_M) | 4.56 | 0.64 | 5.72 | fits comfortably |
| Context length | 8K (Q4_K_M) | 4.56 | 1.28 | 6.42 | fits comfortably |
| Context length | 32K (Q4_K_M) | 4.56 | 5.12 | 10.65 | fits comfortably |
| Context length | 128K (Q4_K_M) | 4.56 | 20.48 | 27.54 | does NOT fit |
| Quantization | Q3_K_M (8K ctx) | 3.44 | 1.28 | 5.19 | fits comfortably |
| Quantization | Q4_K_M (8K ctx) | 4.56 | 1.28 | 6.42 | fits comfortably |
| Quantization | Q5_K_M (8K ctx) | 5.44 | 1.28 | 7.39 | fits comfortably |
| Quantization | Q8_0 (8K ctx) | 8.00 | 1.28 | 10.21 | fits comfortably |

**Observation:** When context length grew, the KV cache increased, pushing the total memory required significantly up. The weights changed when quantization was altered. The largest context this model can use on my machine at Q4_K_M is around 32K. I would choose Q4_K_M quantization to balance memory size and model quality, giving up a small amount of accuracy compared to FP16.

## 3.4 Estimate versus reality

| Model | ollama list size | ollama ps size | Processor | Your estimate (weights / total) |
|---|---|---|---|---|
| qwen2.5:7b | 4.7 GB | 5.1 GB | CPU/GPU | 4.56 GB / 6.42 GB |

The estimate was close. The weights were accurately approximated, but the total in reality was slightly less because Ollama loads a default, often smaller context window than the theoretical maximum we assumed.

## 3.5 Suitability analysis

I recommend **Qwen2.5-7B-Instruct** at Q4_K_M for this scenario. It fits comfortably in the 16GB memory (using about 6.4GB), provides excellent context length up to 32K practically, and operates under an Apache 2.0 license. The runner-up is Mistral-7B, but Qwen2.5 has generally shown stronger coding capabilities recently and explicitly supports tool calling well. If I needed an open model for a commercial product, the Apache 2.0 license remains perfect, but if I had more memory (e.g., 32GB), I might scale up to a larger parameter model (e.g. 14B or 30B) for better reasoning accuracy.

## 3.6 Conclusion
Size and quantization determine the base fit for any local setup. Context length should be the deciding factor when deploying agents or tasks requiring extensive document ingestion (RAG). Licensing is the absolute deciding factor when taking a project to production for commercial purposes or public distribution, even if the model perfectly fits memory requirements.
