{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "private_outputs": true,
      "provenance": [],
      "collapsed_sections": [
        "kbRwbwqSctxf",
        "AKF4Z-HYYMf2",
        "IZQKThVlmTeb",
        "kD8iHVMkYCkO"
      ]
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    },
    "accelerator": "GPU",
    "gpuClass": "standard"
  },
  "cells": [
    {
      "cell_type": "markdown",
      "source": [
        "## **SETUP**"
      ],
      "metadata": {
        "id": "kbRwbwqSctxf"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "%pip install --quiet --upgrade diffusers transformers accelerate invisible_watermark mediapy omegaconf"
      ],
      "metadata": {
        "id": "ufD_d64nr08H"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "!pip install omegaconf\n",
        "\n",
        "#AKAN MERESTART RUNTIME, KLIK RESTART\n",
        "#WILL RESTART RUNTIME, JUST CLICK RESTART"
      ],
      "metadata": {
        "id": "tpHSiSaGVEKL"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "use_refiner = False"
      ],
      "metadata": {
        "id": "hRVsA7iYxpMj"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "## **MODELS**"
      ],
      "metadata": {
        "id": "AKF4Z-HYYMf2"
      }
    },
    {
      "cell_type": "markdown",
      "source": [
        "**EN : SELECT ONE, IF YOU WANT TO REPLACE OTHER MODELS, SELECT ONE OF THE CELL, AND CLICK RUN CELL**\n",
        "\n",
        "**ID :PILIH SALAH SATU, JIKA MAU MENGGANTI MODEL LAIN, PILIH SALAH SATU SEL, DAN KLIK RUN SEL**"
      ],
      "metadata": {
        "id": "rcMaeB3Diqxr"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#@title Realisian Model\n",
        "# @markdown **Realisian Model** [OUTPUT EXAMPLE](https://image.civitai.com/xG1nkqKTMzGDvpLrqFT7WA/ab5b700f-1cc6-408a-bac2-1d3cc38be280/width=450/00001-20230712095453.jpeg)\n",
        "\n",
        "import mediapy as media\n",
        "import random\n",
        "import sys\n",
        "import torch\n",
        "\n",
        "from diffusers import DiffusionPipeline\n",
        "\n",
        "pipe = DiffusionPipeline.from_pretrained(\n",
        "    \"digiplay/Realisian_v5\",\n",
        "    torch_dtype=torch.float16,\n",
        "    use_safetensors=True,\n",
        "    variant=\"fp16\",\n",
        "    safety_checker = None,\n",
        "    requires_safety_checker = False\n",
        "    )\n",
        "\n",
        "pipe = pipe.to(\"cuda\")"
      ],
      "metadata": {
        "id": "bG2hkmSEvByV"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "#@title WAIFU DIFFUSION MODEL\n",
        "# @markdown **WAIFU DIFFUSION MODEL** [OUTPUT EXAMPLE](https://user-images.githubusercontent.com/26317155/210155933-db3a5f1a-1ec3-4777-915c-6deff2841ce9.png)\n",
        "import mediapy as media\n",
        "import random\n",
        "import sys\n",
        "import torch\n",
        "\n",
        "from diffusers import DiffusionPipeline\n",
        "\n",
        "pipe = DiffusionPipeline.from_pretrained(\n",
        "    \"hakurei/waifu-diffusion\",\n",
        "    torch_dtype=torch.float16,\n",
        "    use_safetensors=True,\n",
        "    variant=\"fp16\",\n",
        "    safety_checker = None,\n",
        "    requires_safety_checker = False\n",
        "    )\n",
        "\n",
        "pipe = pipe.to(\"cuda\")"
      ],
      "metadata": {
        "id": "d86QlXcsegBo"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "source": [
        "#@title ORANGEMIX MODEL\n",
        "#@markdown **ORANGE MIX MODEL** [OUTPUT EXAMPLE](https://raw.githubusercontent.com/WarriorMama777/imgup/2c840982550fab41f45ba4b5aedbd3d84ddf2390/img/AOM3/img_sanmples_AOM3_01_comp001.webp)\n",
        "\n",
        "import mediapy as media\n",
        "import random\n",
        "import sys\n",
        "import torch\n",
        "\n",
        "from diffusers import StableDiffusionPipeline\n",
        "\n",
        "pipe = StableDiffusionPipeline.from_single_file(\n",
        "    \"https://huggingface.co/WarriorMama777/OrangeMixs/blob/main/Models/AbyssOrangeMix3/AOM3_orangemixs.safetensors\",\n",
        "    torch_dtype=torch.float16,\n",
        "    use_safetensors=True,\n",
        "    variant=\"fp16\",\n",
        "    safety_checker = None,\n",
        "    requires_safety_checker = False\n",
        "    )\n",
        "\n",
        "pipe = pipe.to(\"cuda\")"
      ],
      "metadata": {
        "id": "hV58WCxSTlwz"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "##**PROMPT**##"
      ],
      "metadata": {
        "id": "IZQKThVlmTeb"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "# @title PROMPT, GUNAKAN BAHASA INGGRIS / USE ENGLISH { display-mode: \"form\" }\n",
        "import os\n",
        "prompt = \"\" #@param {type:\"string\"}\n",
        "seed = random.randint(0, sys.maxsize)\n",
        "\n",
        "negative_prompt = \"bad-picture-chill-75v, ng_deepnegative_v1_75t, badhandv4, (worst quality:2), (low quality:2), (normal quality:2), (lowres:2), (bad anatomy:2), (bad hands:2), (watermark:2), (mole:1.5), (freckles:1.5)\" #@param {type:\"string\"}\n",
        "\n",
        "width = 512  #@param {type:\"integer\"}\n",
        "height = 1024  #@param {type:\"integer\"}\n",
        "\n",
        "# Mengambil nilai dari widget param\n",
        "width = int(width)\n",
        "height = int(height)\n",
        "\n",
        "images = pipe(\n",
        "    prompt=prompt,\n",
        "    width=width,\n",
        "    height=height,\n",
        "    negative_prompt=negative_prompt,\n",
        "    output_type=\"latent\" if use_refiner else \"pil\",\n",
        "    generator=torch.Generator().manual_seed(seed)\n",
        ").images\n",
        "\n",
        "\n",
        "if use_refiner:\n",
        "  images = refiner(\n",
        "      prompt = prompt,\n",
        "      negative_prompt = negative_prompt,\n",
        "      image = images,\n",
        "      ).images\n",
        "\n",
        "print(f\"Prompt:\\t{prompt}\\nSeed:\\t{seed}\")\n",
        "\n",
        "# Nama file yang akan digunakan\n",
        "base_filename = \"output.jpg\"\n",
        "new_filename = base_filename\n",
        "\n",
        "# Cek apakah file dengan nama yang sama sudah ada\n",
        "if os.path.exists(base_filename):\n",
        "    # Jika sudah ada, tambahkan angka di belakangnya\n",
        "    index = 1\n",
        "    while True:\n",
        "        new_filename = f\"output_{index}.jpg\"\n",
        "        if not os.path.exists(new_filename):\n",
        "            break\n",
        "        index += 1\n",
        "\n",
        "# Simpan gambar dengan nama yang telah ditentukan\n",
        "images[0].save(new_filename)\n",
        "\n",
        "# Menampilkan gambar\n",
        "media.show_images(images)\n"
      ],
      "metadata": {
        "id": "AUc4QJfE-uR9"
      },
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "source": [
        "## **TOOLS**"
      ],
      "metadata": {
        "id": "kD8iHVMkYCkO"
      }
    },
    {
      "cell_type": "code",
      "source": [
        "#@title FIX FALSE NSFW\n",
        "pipe.safety_checker = None\n",
        "pipe.requires_safety_checker = False\n",
        "\n",
        "#JIKA RUNTIME RESTART, JALANKAN KODE INI LAGI\n",
        "#IF THE RUNTIME RESTARTS, RUN THIS CODE AGAIN"
      ],
      "metadata": {
        "cellView": "form",
        "id": "xvpkelwnWQ1d"
      },
      "execution_count": null,
      "outputs": []
    }
  ]
}
