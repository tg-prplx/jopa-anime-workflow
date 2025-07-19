# ComfyUI Jopa Anime/NSFW/SFW Workflow
---

## 📋 Description

This workflow is a flexible, production-ready pipeline for generating high-quality anime/NSFW/SFW art. Every generation step is fully parameterized and exposed for easy tweaking.

---

## 🚀 Quick Start

1. **Install ComfyUI:**

   ```bash
   git clone https://github.com/comfyanonymous/ComfyUI.git
   cd ComfyUI
   pip install -r requirements.txt
   ```

2. **Install dependencies:**

   * Python 3.10+
   * PyTorch (CUDA, for your GPU)
   * ComfyUI >= 1.23.4
   * Git
   * Extra nodes:

     * [ComfyUI-Image-Filters](https://github.com/spacepxl/ComfyUI-Image-Filters) (for postprocessing)

       ```bash
       cd custom_nodes
       git clone https://github.com/spacepxl/ComfyUI-Image-Filters.git
       ```

3. **Download models** (see above):

   * checkpoints:

     * `abyssorangemix3AOM3_aom3a1b.safetensors` (final high-detail)
     * `divineanimemix_V2.safetensors` (prepass)
   * lora:

     * `lcm-lora-sd15.safetensors` (detail/consistency)
     * `add_detail.safetensors` (skin/hair/cloth details)
   * vae:

     * `Anime-fp16-vae.pt` (anime colors)
     * `taesd.safetensors` (neutral VAE)
   * upscale\_models:

     * `2x-AniScale.pth` (clean upscale)

4. **Folder structure:**

   ```
   ComfyUI/
   ├─ models/
   │   ├─ checkpoints/
   │   ├─ lora/
   │   ├─ vae/
   │   └─ upscale_models/
   └─ custom_nodes/
       └─ ComfyUI-Image-Filters/
   ```

5. **Run:**

   ```bash
   python main.py --listen 127.0.0.1 --port 8188
   ```

   Open [http://127.0.0.1:8188](http://127.0.0.1:8188) → Load → "jopa-v5.json"

---

## 📦 Architecture & Nodes

* **CheckpointLoader (x2):**

  * Two checkpoints for stages: soft (prepass) and high-detail (final/AOM3)
* **LoraLoader (x2):**

  * Controlled via PrimitiveNode (Combo) for flexible LoRA selection and strength.
* **CLIPTextEncode + PrimitiveStringMultiline:**

  * Long prompts split for positive/negative (bypassing CLIP 77 token limit)
* **ConditioningCombine:**

  * Merges conditionings (stretch prompts, mix styles)
* **KSampler (x2):**

  * One for prepass, one for final. Centralized params (seed, steps etc.)
* **LatentUpscaleBy:**

  * Latent upscaling for clean hi-res images
* **SharpenFilterLatent, EnhanceDetail, BetterFilmGrain** *(from Image-Filters)*:

  * Postprocess: sharpness, detail, film grain (all configurable)
* **ImageUpscaleWithModel:**

  * Final upscale via AniScale (2x)
* **VAELoader:**

  * Select VAE for color/clarity control
* **PreviewImage/SaveImage:**

  * Quick preview and saving

---

## ⚙️ Centralized controls

* **Seed both models:** unified for repeatable results
* **Steps pre-model / end model:** per stage
* **Details power:** LoRA/detail intensity
* **Latent upscale ratio:** scaling before final upscaling
* **LoRA1/LoRA2:** flexible LoRA selection and weights
* **Prompts (positive 1/2, negative):** long positive/negative prompts
* **UpScaler:** easily swap upscale models (AniScale, RealESRGAN etc.)
* **All main params via widgets/input, no graph digging!**

---

## 🛠️ Extra: dependencies & integration

* **ComfyUI-Image-Filters:**

  * Extends ComfyUI, provides EnhanceDetail, BetterFilmGrain, SharpenFilterLatent, etc.
  * Without it, postprocessing nodes won’t work!
* **ControlNet:**

  * Easy to add: connect conditioning after latent at prepass, select module (openpose/canny).
* **Batch rendering:**

  * Add BatchSeeds/BatchPrompt for multi-seed/prompt runs.
* **API/script friendly:**

  * All params exposed for scripts/ComfyUI API

---

## 🧩 Tips & FAQ

* For weak GPUs, lower resolution or steps (RTX 3060/3070 are enough for hires).
* Want more control? Add more LoRA or prompt mixing.
* Download models/LoRA from trusted sources (Civitai, Huggingface).
* Check checkpoint/LoRA licenses before commercial use!
* "randomize" seed = different every time, "fixed" = repeatable.
* Avoid batch>4 on low VRAM (12+ GB VRAM preferred for big batch).
* If you get token errors, split prompt and use ConditioningCombine.

---

## 💾 System requirements

* Python 3.10+
* ComfyUI >=1.23.4
* PyTorch (CUDA)
* NVIDIA GPU 6+ GB (8–12+ GB for hires) OR CPU with 16+ GB RAM
* 8–15 GB free space for models/LoRA

---

## 📜 License

MIT (workflow), models/LoRA — see their own licenses.

---

**Happy comfy prompting! 🧩**

---

## 🇷🇺 Русская версия

### 📋 Описание

Этот воркфлоу — мощный инструмент для создания высококачественных аниме/NSFW/SFW артов с максимальным контролем параметров. Каждый этап генерации прозрачно и гибко настраивается через виджеты.

---

### 🚀 Быстрый старт

1. **Установи ComfyUI:**

   ```bash
   git clone https://github.com/comfyanonymous/ComfyUI.git
   cd ComfyUI
   pip install -r requirements.txt
   ```

2. **Установи зависимости:**

   * Python 3.10+
   * PyTorch с CUDA (для твоей видеокарты)
   * ComfyUI >= 1.23.4
   * Git
   * Внешние ноды:

     * [ComfyUI-Image-Filters](https://github.com/spacepxl/ComfyUI-Image-Filters)  (постпроцессинг/улучшалки)

       ```bash
       cd custom_nodes
       git clone https://github.com/spacepxl/ComfyUI-Image-Filters.git
       ```

3. **Скачай модели** (Civitai, Huggingface, VK и др):

   * checkpoints:

     * `abyssorangemix3AOM3_aom3a1b.safetensors` (основная для финишного пасса)
     * `divineanimemix_V2.safetensors` (prepass, более мягкая генерация)
   * lora:

     * `lcm-lora-sd15.safetensors` (ускоряет/стабилизирует детали)
     * `add_detail.safetensors` (детализация, кожа/одежда/волосы)
   * vae:

     * `Anime-fp16-vae.pt` (аниме-цвета)
     * `taesd.safetensors` (нейтральная VAE, для тестов)
   * upscale\_models:

     * `2x-AniScale.pth` (чистый upscale без каши)

4. **Структура папок:**

   ```
   ComfyUI/
   ├─ models/
   │   ├─ checkpoints/
   │   ├─ lora/
   │   ├─ vae/
   │   └─ upscale_models/
   └─ custom_nodes/
       └─ ComfyUI-Image-Filters/
   ```

5. **Запуск:**

   ```bash
   python main.py --listen 127.0.0.1 --port 8188
   ```

   Открой [http://127.0.0.1:8188](http://127.0.0.1:8188) → Load → "jopa-v5.json"

---

### 📦 Архитектура и ноды

* **CheckpointLoader (x2):**

  * Два чекпоинта для разных стадий: первая “мягкая”, вторая финишная (AOM3).
* **LoraLoader (x2):**

  * Подключены через PrimitiveNode (Combo) для гибкой смены/регулировки веса.
* **CLIPTextEncode + PrimitiveStringMultiline:**

  * Промпты разделены на длинные positive/negative (обход лимита CLIP — 77 токенов на conditioning).
* **ConditioningCombine:**

  * Склейка нескольких conditioning (можно “растягивать” промпт или миксовать разные стили в одной генерации).
* **KSampler (x2):**

  * Один на prepass, другой на финальный pass. Все основные параметры централизованы (seed, steps и пр.)
* **LatentUpscaleBy:**

  * Апскейл внутри latent space — позволяет безопасно увеличивать разрешение без жёстких артефактов.
* **SharpenFilterLatent, EnhanceDetail, BetterFilmGrain** *(из Image-Filters)*:

  * Повышают резкость, добавляют “живости”, стилизацию или текстуру. Всё настраивается по силе эффекта.
* **ImageUpscaleWithModel:**

  * Финальный upscale через AniScale (2x) — итоговая чистота и детализация картинки.
* **VAELoader:**

  * Разные VAE для управления цветами, стилями и четкостью.
* **PreviewImage/SaveImage:**

  * Быстрый просмотр и сохранение результата.

---

### ⚙️ Централизованные параметры (управление)

* **Seed both models:** единый seed для предсказуемых/повторяемых артов
* **Steps pre-model / end model:** шаги для каждой стадии
* **Details power:** интенсивность детализации
* **Latent upscale ratio:** насколько увеличить картинку на этапе latent
* **LoRA1/LoRA2:** гибкий выбор и веса любых лор
* **Prompts (positive 1/2, negative):** длинные описания, анти-теги
* **UpScaler:** легко сменить upscale-модель (например, если хочешь RealESRGAN/4x-UltraMix)
* **Все параметры доступны через input/panel, ничего не надо вручную искать в графе!**

---

### 🛠️ Дополнительно: про dependencies и интеграцию

* **ComfyUI-Image-Filters**:

  * Расширяет стандартный comfy, даёт EnhanceDetail, BetterFilmGrain, SharpenFilterLatent и др.
  * Без этой ноды фильтры postprocessing работать не будут! (Установка: см. выше)
* **ControlNet:**

  * Легко добавить: подключи Conditioning после latent на prepass, выбери модуль (openpose/canny).
* **Batch рендер:**

  * Можно добавить BatchSeeds/BatchPrompt и гонять серию артов с разными seed/промптами.
* **API/Script-friendly:**

  * Весь воркфлоу параметризован, легко подключать через ComfyUI API и скрипты.

---

### 🧩 Советы и FAQ

* Для слабых GPU уменьши разрешение или steps (на RTX 3060/3070 летает стабильно).
* Хочешь больше контроля — добавь дополнительные лоры или промпт-мешалку.
* Все модели/лоры скачивай только с проверенных источников (Civitai, Huggingface).
* Перед коммерцией проверь лицензии чекпоинтов/лор!
* Если “randomize” seed — арты всегда разные, если “fixed” — повторяемость.
* Batch лучше не гнать выше 4 на слабых картах (12+ ГБ VRAM желательно).
* Если возникла ошибка по токенам — разбей промпт и используй ConditioningCombine.

---

### 💾 Системные требования

* Python 3.10+
* ComfyUI >=1.23.4
* PyTorch (с CUDA)
* NVIDIA GPU 6+ GB (лучше 8–12+ ГБ для hires) ИЛИ ЦПУ с 16+ ГБ ОЗУ
* Свободные 8–15 ГБ под модели/лоры

---

### 📜 Лицензия

MIT (граф), модели/LoRA — отдельные лицензии.

---
