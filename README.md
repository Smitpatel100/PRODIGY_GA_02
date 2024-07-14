# PRODIGY_GA_2
from diffusers import StableDiffusionPipeline
import torch

# Load the model
model_id = "CompVis/stable-diffusion-v1-4"
pipe = StableDiffusionPipeline.from_pretrained(model_id)
pipe = pipe.to("cuda")

# Generate the image
prompt = "Aurora Night"
image = pipe(prompt, guidance_scale=7.5).images[0]

# Save the image
image.save("aurora_night.png")


# Exploring AI-Powered Image Creation with Stable Diffusion
I use Stable Diffusion software to turn text descriptions into high-quality images. By applying advanced machine learning techniques, I aim to create images that are both accurate and visually impressive.

I've been learning how to develop methods to make the image creation process faster and more efficient. I help integrate these capabilities into various projects, enhancing the use of AI in creative technology.

Looking ahead, I plan to improve my skills for even better image quality and explore new applications for Stable Diffusion. I'm dedicated to advancing AI-driven image creation and contributing to the growth of creative technologies.

For those interested in creating stunning images from their creative ideas, you can explore Stable Diffusion here:  https://stabledifffusion.com/generate
