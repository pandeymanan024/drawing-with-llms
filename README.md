An operational system for text-to-SVG conversion achieved a 0.81 mean fidelity score on Kaggle's benchmark. The system uses a hybrid neural-symbolic approach, combining Stable Diffusion v2 with computer vision techniques to generate vector graphics from text prompts.

## System Overview

The system employs a three-stage pipeline to convert text into SVG images. It was validated on NVIDIA T4 GPUs, processing 500 prompts and achieving 98.6% SVG validity.

***

### Generation Pipeline
The workflow consists of three main stages:
* **Bitmap Synthesis**: An initial image is generated from the text prompt.
* **Vector Conversion**: The bitmap image is converted into a vector format.
* **Scoring**: The final output is evaluated for quality.



***

### Key Components
Different models and libraries are used for each stage of the process, each with specific resource requirements.

| Component | Model/Version | Key Parameters | Resource Use |
| :--- | :--- | :--- | :--- |
| **Image Generation** | SDv2-base | Steps: 25, Guidance: 20 | 8.3GB VRAM |
| **Vectorization** | OpenCV 4.8 | Colors: 16, Epsilon: 0.02 | 1.4GB RAM |
| **VQA Evaluation** | PaliGemma-10B | 4bit-Quant | 4.1GB VRAM |
| **Aesthetic Scoring** | CLIP-VIT-L/14 | Linear MSE | 1.8GB VRAM |

## Performance and Analysis

The system's performance was evaluated based on runtime, resource usage, and comparison against baseline models.

***

### Runtime and Validation
The average generation time is approximately 54 seconds, with the system optimized for performance using FP16 precision. The safety checker was disabled to increase speed.

| Category | Avg Score | Time (s) | SVG Size (KB) |
| :--- | :--- | :--- | :--- |
| Landscapes | 0.83 | 52.4 | 9.2 |
| Abstract | 0.77 | 58.1 | 8.7 |
| Fashion | 0.79 | 55.3 | 9.1 |
| Text-heavy | 0.68 | 61.7 | 9.8 |

***

### Resource Utilization
During operation, the system's peak GPU memory usage was 14.2GB (89% of a 16GB NVIDIA T4 GPU).
* **VRAM Allocation**: Stable Diffusion v2 used 58% of the VRAM, PaliGemma used 29%, and CLIP used 13%.
* **Thermal Performance**: The GPU reached a maximum temperature of 84°C without any performance throttling.

***

### Benchmark Comparison
The system's output was compared against both a direct Large Language Model (LLM) approach and human expert-generated images, showing a significant improvement in quality.

| Method | Fidelity Score | Time (s) | Validity |
| :--- | :--- | :--- | :--- |
| Direct LLM | 0.52 | 38 | 89% |
| **This Work** | **0.81** | **54** | **98.6%** |
| Human Expert | 0.91 | N/A | 100% |
