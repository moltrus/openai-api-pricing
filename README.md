# OpenAI API Pricing

A simple Python script to track the amount spent based on usage parameters for all OpenAI APIs.

## Features

- **Token-Based Pricing**: Calculates costs based on input and output tokens for models like GPT and others.
- **Image-Based Pricing**: Computes costs based on the dimensions and number of images generated using DALL-E models.
- **Time-Based Pricing**: Calculates costs for voice model usage based on minutes.
- **Image Processing Pricing**: Computes costs for specific image processing models based on image dimensions.

## Usage

### Input Parameters

- **model**: Name of the AI model being used.
- **token_input**: Number of input tokens (if applicable).
- **token_output**: Number of output tokens (if applicable).
- **img_num**: Number of images generated (if applicable).
- **minutes**: Duration of usage in minutes (if applicable).
- **img_w** and **img_h**: Width and height of the image being processed (if applicable).

### Example Usage

**GPT**
```
from pricing_calculator import calculate_pricing
from openai import OpenAI

client = OpenAI()
completion = client.chat.completions.create(
    model="gpt-4o",
    messages=[
        {"role": "user", "content": "write a haiku about ai"}
    ]
)

amount = calculate_pricing(
    model = "gpt-4o",
    token_input = completion.usage.prompt_tokens,
    token_output = completion.usage.completion_tokens
)

print(amount)
```

**DALLE 3**
```
from pricing_calculator import calculate_pricing
from openai import OpenAI

client = OpenAI()

model = "dall-e-3"
size = "1024x1024"
quality = "standard"
n = 1

response = client.images.generate(
    model = model,
    prompt = "a white siamese cat",
    size = size,
    quality = quality,
    n = 1,
)

amount = calculate_pricing(
    model = model+"-"+quality+"-"+size,
    img_num = n
)

print(amount)

image_url = response.data[0].url
```

**TTS**
```
from pricing_calculator import calculate_pricing
from openai import OpenAI
client = OpenAI()

audio_file = open("speech.mp3", "rb")
transcript = client.audio.transcriptions.create(
    file=audio_file,
    model="whisper-1",
    response_format="verbose_json",
    timestamp_granularities=["word"]
)

amount = calculate_pricing(
    model = "whisper-1",
    minutes = transcript.duration
)

print(amount)

print(transcript.words)
```

## Note

- Ensure the correct model name and parameters are provided to get accurate pricing.
- Image processing pricing considers the size and complexity of the image in addition to token-based costs for some models.
