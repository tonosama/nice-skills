---

name: nano-banana-pro

description: |

  Google の Nano Banana Pro（Gemini 3 Pro Image）で画像生成。Gemini API 経由の AI 画像、プロ品質ビジュアル、画像生成機能実装時に使用。トリガー語：Nano Banana Pro, Gemini 3 Pro Image, gemini-3-pro-image-preview, Google image generation.



  Google の Nano Banana Pro（Gemini 3 Pro Image）で画像生成。Gemini API 経由の AI 画像、プロ品質ビジュアル、画像生成機能実装時に使用。トリガー語：Nano Banana Pro, Gemini 3 Pro Image, gemini-3-pro-image-preview, Google image generation.



---



# Nano Banana Pro（Gemini 3 Pro Image）



Google の Gemini 3 Pro Image API で高品質画像を生成する。



## 概要



**Nano Banana Pro** は **Gemini 3 Pro Image**（`gemini-3-pro-image-preview`）のマーケティング名。Gemini 3 Pro 上の画像生成・編集モデル。



## クイックスタート



### API Key の取得

1. [Google AI Studio](https://aistudio.google.com) を開く

2. 「Get API Key」をクリック

3. 環境変数として安全に保存



### 基本的な画像生成（Python）

```python

from google import genai

from google.genai import types



client = genai.Client(api_key="YOUR_GEMINI_API_KEY")



response = client.models.generate_content(

    model="gemini-3-pro-image-preview",

    contents="A serene Japanese garden with cherry blossoms and a koi pond",

    config=types.GenerateContentConfig(

        response_modalities=['TEXT', 'IMAGE']

    )

)



# Process response

for part in response.candidates[0].content.parts:

    if hasattr(part, 'text'):

        print(f"Description: {part.text}")

    elif hasattr(part, 'inline_data'):

        # Save image

        image_data = part.inline_data.data  # Base64 encoded

        mime_type = part.inline_data.mime_type  # image/png

        

        import base64

        with open("output.png", "wb") as f:

            f.write(base64.b64decode(image_data))

```



### REST API（cURL）

```bash

curl -s -X POST \

  "https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent" \

  -H "x-goog-api-key: $GEMINI_API_KEY" \

  -H "Content-Type: application/json" \

  -d '{

    "contents": [{

      "role": "user",

      "parts": [{"text": "Create a vibrant infographic about photosynthesis"}]

    }],

    "generationConfig": {

      "responseModalities": ["TEXT", "IMAGE"]

    }

  }'

```



### TypeScript/JavaScript

```typescript

const GEMINI_API_KEY = process.env.GEMINI_API_KEY;



async function generateImage(prompt: string) {

  const response = await fetch(

    'https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent',

    {

      method: 'POST',

      headers: {

        'x-goog-api-key': GEMINI_API_KEY!,

        'Content-Type': 'application/json',

      },

      body: JSON.stringify({

        contents: [{ 

          role: 'user', 

          parts: [{ text: prompt }] 

        }],

        generationConfig: {

          responseModalities: ['TEXT', 'IMAGE'],

        },

      }),

    }

  );



  const data = await response.json();

  return data;

}

```



## 設定オプション



### Image Configuration

```python

response = client.models.generate_content(

    model="gemini-3-pro-image-preview",

    contents="Professional product photo of a coffee mug",

    config=types.GenerateContentConfig(

        response_modalities=['TEXT', 'IMAGE'],

        image_config=types.ImageConfig(

            aspect_ratio="16:9",  # Options: 1:1, 3:2, 16:9, 9:16, 21:9

            image_size="2K"       # Options: 1K, 2K, 4K

        )

    )

)

```



### Google Search Grounding 付き

```python

response = client.models.generate_content(

    model="gemini-3-pro-image-preview",

    contents="Create an infographic showing today's stock market trends",

    config=types.GenerateContentConfig(

        response_modalities=['TEXT', 'IMAGE'],

        tools=[{"google_search": {}}]  # Enable search grounding

    )

)

```



## マルチターン会話（反復編集）



```python

# Create a chat session

chat = client.chats.create(

    model="gemini-3-pro-image-preview",

    config=types.GenerateContentConfig(

        response_modalities=['TEXT', 'IMAGE'],

        tools=[{"google_search": {}}]

    )

)



# Initial generation

response1 = chat.send_message(

    "Create a vibrant infographic explaining photosynthesis"

)



# Edit the image

response2 = chat.send_message(

    "Update this infographic to be in Spanish. Keep all other elements the same."

)

```



## 主要機能



### 1. Superior Text Rendering

```python

response = client.models.generate_content(

    model="gemini-3-pro-image-preview",

    contents="""Create a professional poster with:

    - Title: "Annual Tech Summit 2025"

    - Date: March 15-17, 2025

    - Location: San Francisco Convention Center

    """,

    config=types.GenerateContentConfig(

        response_modalities=['TEXT', 'IMAGE']

    )

)

```



### 2. Character Consistency（最大 5 主体）

```python

import base64



def load_image(path: str) -> str:

    with open(path, "rb") as f:

        return base64.b64encode(f.read()).decode()



character_ref = load_image("character.png")



response = client.models.generate_content(

    model="gemini-3-pro-image-preview",

    contents=[

        {"text": "Generate an image of this person at a tech conference"},

        {"inline_data": {"mime_type": "image/png", "data": character_ref}}

    ],

    config=types.GenerateContentConfig(

        response_modalities=['TEXT', 'IMAGE']

    )

)

```



## Next.js API Route



```typescript

// app/api/generate-image/route.ts

import { NextRequest, NextResponse } from 'next/server';



export async function POST(request: NextRequest) {

  const { prompt, aspectRatio = '1:1', imageSize = '2K' } = await request.json();



  try {

    const response = await fetch(

      'https://generativelanguage.googleapis.com/v1beta/models/gemini-3-pro-image-preview:generateContent',

      {

        method: 'POST',

        headers: {

          'x-goog-api-key': process.env.GEMINI_API_KEY!,

          'Content-Type': 'application/json',

        },

        body: JSON.stringify({

          contents: [{ role: 'user', parts: [{ text: prompt }] }],

          generationConfig: {

            responseModalities: ['TEXT', 'IMAGE'],

            imageConfig: { aspectRatio, imageSize },

          },

        }),

      }

    );



    const data = await response.json();

    const parts = data.candidates?.[0]?.content?.parts || [];

    const imagePart = parts.find((p: any) => p.inline_data);



    return NextResponse.json({

      image: imagePart ? {

        data: imagePart.inline_data.data,

        mimeType: imagePart.inline_data.mime_type,

        url: `data:${imagePart.inline_data.mime_type};base64,${imagePart.inline_data.data}`,

      } : null,

    });

  } catch (error) {

    return NextResponse.json({ error: 'Generation failed' }, { status: 500 });

  }

}

```



## モデル比較



| Feature | Nano Banana (2.5 Flash) | Nano Banana Pro (3 Pro Image) |

|---------|-------------------------|-------------------------------|

| Model ID | gemini-2.5-flash-image | gemini-3-pro-image-preview |

| Quality | Good | Best |

| Speed | Faster | Slower |

| Cost | Lower | Higher |

| Best For | Previews, high-volume | Production, professional |



## リソース



- **Documentation**: https://ai.google.dev/gemini-api/docs/image-generation

- **Prompt Guide**: https://ai.google.dev/gemini-api/docs/prompting-intro



## Anti-Patterns & Safety Warnings



| Avoid | Why | Instead |

|-------|-----|---------|

| Base64 エラーハンドリングの省略 | API バッファ欠落で破損画像を書き込む | base64 デコードループを `try/except` で囲む |

| レイアウトの固定 aspect ratio | デバイスでレスポンシブが崩れる | デバイスから次元を導出（例：mobile = `9:16`） |



## リファレンスと読込トリガー



**必須 — LOADING TRIGGERS**:

- スクリプト API やマルチターン会話編集の前に、prompt guides から API 設定値を**必ず**読み込む。

- 単純な Python 端末生成のみの場合は Next.js route リファレンスを**読み込まない**。