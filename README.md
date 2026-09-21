# Prodigy-GA-2
# 🎨 Task-02: Image Generation with Pre-trained Models

Generating images from text prompts using **Stable Diffusion v1.5**, a pre-trained latent diffusion model, through the Hugging Face `diffusers` library.

Completed as part of the **Generative AI Internship at Prodigy InfoTech**.


## 🎯 Objective
Use a pre-trained generative model (DALL-E mini or Stable Diffusion) to create images from text prompts.

I chose **Stable Diffusion v1.5** because it produces higher-quality images than DALL-E mini, runs on a free Colab T4 GPU, and is well supported by the `diffusers` library.

## 🧠 How It Works
Stable Diffusion is a **latent diffusion model**. Instead of working on full-size pixels, it works in a compressed latent space:

1. **Text encoder (CLIP)** turns the prompt into embeddings.
2. **U-Net** starts from random noise in latent space and removes noise step by step, guided by the text embeddings.
3. **VAE decoder** converts the final latent back into a 512x512 image.

## 🛠️ Implementation
| Component | Choice |
|---|---|
| Model | `stable-diffusion-v1-5/stable-diffusion-v1-5` |
| Library | Hugging Face `diffusers`, `transformers`, `accelerate` |
| Precision | float16 for speed and lower GPU memory |
| Hardware | Google Colab, NVIDIA T4 GPU |
| Steps | 30 inference steps |
| Guidance scale | 7.5 (default), varied in experiments |
| Reproducibility | Fixed random seeds via `torch.Generator` |
| Negative prompt | `blurry, low quality, distorted, deformed, text, watermark` |

## 🖼️ Generated Images
| Prompt | Output |
|---|---|
| a photo of an astronaut riding a horse on mars, cinematic lighting, highly detailed | ![astronaut](outputs/astronaut.png) |
| a futuristic city at sunset with flying cars, digital art, vibrant colors | ![city](outputs/city.png) |
| a cute orange cat wearing sunglasses on a beach, realistic photo | ![cat](outputs/cat.png) |
| a medieval castle on a mountain in the fog, fantasy painting | ![castle](outputs/castle.png) |

## 🔬 Experiments

### 1. Prompt detail
Same seed, a simple prompt vs a detailed prompt.

- **Left:** `a dog`
- **Right:** `a golden retriever puppy sitting in a field of flowers, golden hour lighting, shallow depth of field, professional photography, highly detailed`

![prompt detail comparison](outputs/prompt_detail_comparison.png)

### 2. Guidance scale
Same prompt and seed with guidance scale **3, 7.5 and 15** (left to right).

Prompt: `a lighthouse on a rocky coast during a storm, dramatic sky, oil painting`

![guidance comparison](outputs/guidance_comparison.png)

## 💡 Key Learnings
- **Prompt detail matters.** Adding subject, style, lighting and quality descriptors gives the model far more to work with than a bare noun.
- **Guidance scale is a trade-off.** Low values give the model more freedom, while high values follow the prompt more strictly but can reduce variety and produce over-saturated results.
- **Seeds make results reproducible.** Fixing the seed lets you change one variable at a time and compare fairly.
- **Negative prompts help** steer the model away from common artifacts such as blur and distortion.

## ⚠️ Limitations
- The base resolution is 512x512, so fine details can look soft.
- Stable Diffusion 1.5 can struggle with hands, readable text and complex multi-object scenes.
- The model reflects biases in its training data (LAION), so outputs should be reviewed before real-world use.
- The built-in safety checker may occasionally replace an image with a blank one.

## ▶️ How to Run
1. Open `stable_diffusion_image_generation.ipynb` in [Google Colab](https://colab.research.google.com).
2. Set **Runtime → Change runtime type → T4 GPU**.
3. Run all cells in order. The first run downloads the model weights, which takes a few minutes.
4. Generated images are saved to the `outputs/` folder.

To try your own idea, edit the `prompts` dictionary in section 4 of the notebook.

## 📁 Project Structure
```
├── stable_diffusion_image_generation.ipynb
├── outputs/
│   ├── astronaut.png
│   ├── city.png
│   ├── cat.png
│   ├── castle.png
│   ├── prompt_detail_comparison.png
│   └── guidance_comparison.png
└── README.md
```

## 📚 References
- [Stable Diffusion v1.5 on Hugging Face](https://huggingface.co/stable-diffusion-v1-5/stable-diffusion-v1-5)
- [Hugging Face Diffusers documentation](https://huggingface.co/docs/diffusers)
- Rombach et al., *High-Resolution Image Synthesis with Latent Diffusion Models* (2022)
