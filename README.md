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
