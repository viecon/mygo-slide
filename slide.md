---
marp: true
_class: lead
paginate: true
backgroundImage: url('https://marp.app/assets/hero-background.svg')
---
<style>
/* Custom CSS for Marp Slides Readability */
section {
  font-size: 22px; /* 稍微加大基礎字體 */
  line-height: 1.65; /* 增加行距 */
  padding: 50px 70px; /* 增加頁面內邊距 */
  text-align: left; /* 預設文字靠左對齊 */
}

h1, h2, h3 {
  margin-top: 1em;
  margin-bottom: 0.7em; /* 標題下方間距 */
  line-height: 1.3;
  text-align: left; /* 標題也靠左 */
}

h1 { font-size: 2.0em; color: #333; } /* 調整 H1 大小和顏色 */
h2 { font-size: 1.6em; color: #444; } /* 調整 H2 大小和顏色 */
h3 { font-size: 1.3em; color: #555; } /* 調整 H3 大小和顏色 */

p {
  margin-bottom: 0.8em; /* 段落間距 */
}

ul, ol {
  padding-left: 1.8em; /* 列表縮排 */
  margin-bottom: 0.8em;
}

li {
  margin-bottom: 0.5em; /* 列表項間距 */
}

pre {
  padding: 1em 1.2em; /* 程式碼區塊內邊距 */
  margin: 0.5em 0 1em 0; /* 程式碼區塊上下邊距 */
  border-radius: 5px; /* 圓角 */
  font-size: 0.85em; /* 程式碼字體稍小 */
  line-height: 1.45; /* 程式碼行距 */
  background-color: #f5f5f5; /* 淺灰色背景 */
  border: 1px solid #e0e0e0; /* 細邊框 */
  overflow-x: auto; /* 長行自動出現滾動條 */
  max-width: 100%;
}

/* Marp 的預設主題通常會處理行內程式碼，如果效果不佳可以取消註解並調整 */
/*
code {
  background-color: #eee;
  padding: 0.2em 0.4em;
  border-radius: 3px;
  font-size: 0.9em;
}
*/

img {
  max-width: 85%; /* 限制圖片最大寬度 */
  height: auto;
  display: block;
  margin: 1.2em auto; /* 圖片上下邊距，並水平置中 */
  border: 1px solid #ddd; /* 可選：為圖片加上細邊框 */
  border-radius: 4px; /* 可選：圖片圓角 */
}

blockquote {
  margin: 1em 0 1em 1.5em; /* 引用區塊邊距 */
  padding-left: 1.2em; /* 引用區塊左內邊距 */
  border-left: 4px solid #ccc; /* 左側引用線 */
  color: #666; /* 引用文字顏色 */
  font-style: italic; /* 可選：斜體 */
}

a {
  color: #007bff; /* 連結顏色 */
  text-decoration: none; /* 移除底線 */
}

a:hover {
  text-decoration: underline; /* 滑鼠懸停時加底線 */
}

/* Lead class for title slide */
section.lead h1 {
  text-align: center;
  font-size: 2.5em;
  margin-bottom: 0.5em;
}
section.lead {
  display: flex;
  flex-direction: column;
  justify-content: center;
  text-align: center;
}

/* Helper class for centering content if needed */
.center {
  text-align: center;
}
</style>

<section data-marp-fitting-header="false">

# 陽明交大創客俱樂部社課 - AI 梗圖翻頁機

</section>

---

## Content

1. 架構簡介
2. Prompt engineering
3. API 串接
4. ESP32 程式撰寫
5. Docker (有時間的話)
6. 這篇簡報 (真的有時間的話)

---

## 架構簡介

----

## 架構簡介

![專案整體架構圖](pics/whole_structure.png)
[專案程式碼連結 (Code)](https://github.com/godempty/MyGo_Flipper)

---

## Prompt Engineering

----

### 什麼是大型語言模型?

----

### 什麼是大型語言模型?

> it’s a prediction engine. The model takes sequential text as an input and then predicts what the following token should be, based on the data it was trained on. The LLM is operationalized to do this over and over again, adding the previously predicted token to the end of the sequential text for predicting the following token. The next token prediction is based on the relationship between what’s in the previous tokens and what the LLM has seen during its training.

**簡單來說：** LLM 就像一個超強的文字接龍大師，它根據你給它的文字序列（輸入），預測下一個最可能出現的字詞是什麼，這個預測是基於它在大量資料中學習到的模式。

----

### 什麼是 Prompt Engineering？

* 是設計與優化提示語 (`prompts`) 以引導 **大型語言模型 (Large Language Models, LLMs)** 產生期望輸出的技術。
* 透過精心設計的提示詞，提高模型在各種任務上的表現與可靠性。
* ~~賽博巫術~~

[參考資料：Prompt Engineering - Lee Boonstra](https://www.kaggle.com/whitepaper-prompt-engineering)

----

### Zero-Shot

定義： 直接給予模型任務指令，無需提供例子。

```text
請將以下句子翻譯成法語：
"The book is on the table."
```

----

<!-- backgroundImage: url("https://top1cdn.top1health.com/cdn/am/20080/63980.jpg") -->

### One-Shot 

定義： 提供一個例子，幫助模型理解任務格式與期望輸出。

```text
英文：Hello → 法文：Bonjour
英文：Goodbye → 法文：
```

----
<!-- backgroundImage: url('https://marp.app/assets/hero-background.svg') -->

### Few-Shot

定義： 提供多個例子，幫助模型理解任務格式與期望輸出。

```text
英文：I love you → 法文：Je t'aime
英文：Good morning → 法文：Bonjour
英文：Thank you → 法文：
```

----

### Chain-of-Thought (CoT)

定義： 引導模型逐步推理，透過中間步驟來達成最終答案。

```text
There are 12 cookies. You eat 4 and give away 3. How many are left?  
Let's think step by step.
```

----

### Role/Persona Prompting

定義： 指定模型扮演特定角色，以影響其語氣與回應方式。

```text
你是一位歷史學家，請解釋羅馬帝國的衰落原因。
```

----

### Contextual Prompting


定義： 給模型一些背景知識，引導他產出想要的結果。

```text
The user is traveling to Japan in winter and is allergic to seafood.  
Suggest 2 Japanese meals.
```

----

### Step-Back Prompting

定義：先問廣泛問題以啟動知識，再解任務。

Step-Back Prompt:
```text
What makes a job interview successful?
```

Answer:
> Good communication, confidence, and understanding of the company.


**Main Prompt (using above as context):**

```text
Write a checklist for preparing for a job interview using the contents above.
```

----

### Automatic Prompt Engineering

定義：用 AI 產生 Prompt。

```text
Generate 5 different ways to ask:  
"Show me the weather forecast for Tokyo."
```

----

### 攻擊

- Jail Breaking (越獄)： 繞過模型的安全限制，使其產生不當內容。
- Prompt Injection (提示注入)： 將惡意指令注入提示中，操控模型行為。
- ...等等


[OWASP Top 10 for Large Language Model Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
[SITCON 2025 R2｜開發者的暗黑小紅帽：大野狼與 LLM｜講者 slasho](https://youtu.be/ElhRVHl7xAc)

----

### 怎麼辦

- 使用較新的模型： 新模型通常有更好的安全防護。
- 權限最小化： 不要給予 LLM 過高的系統或資料存取權限。
- 限制速率 (Rate Limiting)： 防止惡意使用者大量發送請求。
- 輸入/輸出過濾： 檢查使用者輸入與模型輸出，過濾潛在惡意內容。
- 多代理架構 (Multi-agent)： 讓不同功能的 AI 互相監督檢查。

---

## API 串接

----

先去 [Google AI Studio](https://aistudio.google.com/app/apikey?hl=zh-tw) 取得 API key

----

使用套件：

```sh
pip install google-genai
```

----

```py
import google.generativeai as genai

# 請將 "YOUR_API_KEY" 替換成你自己的 API Key
genai.configure(api_key="YOUR_API_KEY", transport="rest")

# 選擇要使用的模型 (這裡使用 gemini-1.5-flash)
model = genai.GenerativeModel("gemini-2.0-flash")
response = model.generate_content("講個冷笑話")

print(response.text)
```

----

### API rate

![alt text](pics/gemini_API_rate.png)

----

### 回顧上次社課

![last_slide](pics/last_slide.png)

----

### 這次要做的事情： 在後端 API (/api/transcribe) 接收一個 `wav` 聲音檔，讓 Gemini 理解語音內容後，根據內容選擇一張最適合的梗圖，最後回傳該圖片的編號。

----

### 語音辨識

```py
import google.generativeai as genai

def transcribe_audio(audio_content):

    model = genai.GenerativeModel("gemini-1.5-flash")
    result = model.generate_content(
        [
            "請將以下語音轉文字並直接輸出，如果有雜音可以忽略，如果全都是雜音或是無法分辨，請回覆「&$%$hu#did」",
            {"mime_type": "audio/wav", "data": audio_content},
        ]
    )
    app.logger.info(f"{result.text=}")
    return result.text
```

----

### Prompt

```py
import json

# 假設梗圖台詞與編號儲存在 words.json
# 格式應為 {"0": "台詞一", "1": "台詞二", ...}
json_data = open("words.json", "r", encoding="utf-8")
words = json.loads(json_data.read())

prompt = f"""
你現在是一個名為 "MyGO!!!!! Gemini" 的虛擬對話夥伴，你的回答方式會完全採用動畫「Bang Dream! It's my GO!!!!!」中的台詞。

你的主要任務是：
1.  **理解我的對話內容。**
2.  **根據對話內容，從以下提供的台詞中選擇一句最符合情境的台詞。**
3.  **直接回傳所選語句對應的編號，不需要回覆其他文字。**

**以下是你可以選擇的台詞:**
{words}
**舉例：**
如果我的對話是 "早安"，你應該選擇「貴安」或是「早安喵姆喵姆」這句台詞回覆。

必要時可以選擇最有趣的台詞回覆，但請確保回覆的內容與對話內容有關，可能是諧音或是反諷等等。
但你也需要注意，這些台詞是來自動畫中的角色，所以有些台詞可能不適合用在所有情境中。
**舉例：**
如果我的對話是 "你為甚麼不理我"，你可以選擇「是這樣嗎」，或是「我還是會繼續下去」回覆。
**現在，開始吧！**
"""
```

----

### API

```py
@app.route("/api/transcribe", methods=["POST"])                   # 定義一個路由，處理 POST 請求，路徑為 /api/transcribe
def transcribe():                                                 # 定義一個名為 transcribe 的函式
    if "audio" not in request.files:                              # 檢查請求中是否包含名為 "audio" 的檔案
        return jsonify({"error": "No audio file provided"}), 400  # 如果沒有提供音訊檔案，回傳錯誤訊息和 400 狀態碼
```

----

### API

```py
@app.route("/api/transcribe", methods=["POST"])                   # 定義一個路由，處理 POST 請求，路徑為 /api/transcribe
def transcribe():                                                 # 定義一個名為 transcribe 的函式
    if "audio" not in request.files:                              # 檢查請求中是否包含名為 "audio" 的檔案
        return jsonify({"error": "No audio file provided"}), 400  # 如果沒有提供音訊檔案，回傳錯誤訊息和 400 狀態碼
    
    audio_file = request.files["audio"]                           # 從 body 把檔案拿出來
    audio_content = audio_file.read()                             # 讀檔案
    transcript = transcribe_audio(audio_content)                  # 呼叫 transcribe_audio 辨識語音
```

----

### API

```py
@app.route("/api/transcribe", methods=["POST"])                   # 定義一個路由，處理 POST 請求，路徑為 /api/transcribe
def transcribe():                                                 # 定義一個名為 transcribe 的函式
    if "audio" not in request.files:                              # 檢查請求中是否包含名為 "audio" 的檔案
        return jsonify({"error": "No audio file provided"}), 400  # 如果沒有提供音訊檔案，回傳錯誤訊息和 400 狀態碼
    
    audio_file = request.files["audio"]                           # 從 body 把檔案拿出來
    audio_content = audio_file.read()                             # 讀檔案
    transcript = transcribe_audio(audio_content)                  # 呼叫 transcribe_audio 辨識語音

    # 將上面的 prompt 作為 system prompt
    model = genai.GenerativeModel(model_name="models/gemini-1.5-flash", system_instruction=prompt)
    response = model.generate_content(f"回覆以下句子:{transcript}")
    generated_text = response.text[:-1]
    app.logger.info(words[generated_text])
    return jsonify({"text": int(generated_text), "pic": words[generated_text]})
```

---

## ESP32 程式撰寫

----

### 目標：

1. 在 ESP32 上建立一個簡易的 API Server，接收來自後端 (Flask) 的指令。
2. 根據接收到的指令 (圖片編號)，控制步進馬達旋轉到對應梗圖的位置。

----

### API

- 路由： `/spin`
- Method： `POST`
- Body： `{"position": <轉到第幾張圖>}`

----

### Basic Setting

```cpp
#include <Stepper.h>
#include <WiFi.h>
#include <WebServer.h>
#include <ArduinoJson.h>

// Replace with your network credentials
const char *ssid = "SSID";
const char *password = "PASSWORD";

// Create a WebServer object on port 80
WebServer server(80);

const int stepsPerRevolution = 2048; // change this to fit the number of steps per revolution

// ULN2003 Motor Driver Pins
#define IN1 19
#define IN2 18
#define IN3 5
#define IN4 17

// initialize the stepper library
Stepper myStepper(stepsPerRevolution, IN1, IN3, IN2, IN4);
int position[32] = {...};
int cur_pos = 0;
```

----

### Setup

Connect to WIFI and start the server.

```cpp
void setup()
{
    // Start Serial communication
    Serial.begin(115200);

    // Connect to Wi-Fi
    WiFi.begin(ssid, password);
    while (WiFi.status() != WL_CONNECTED)
    {
        delay(500);
        Serial.print(".");
    }
    Serial.println("\nConnected to WiFi");
    Serial.print("IP Address: ");
    Serial.println(WiFi.localIP());

    // Define routes
    server.on("/spin", HTTP_POST, handlePostData);
    // Start the server
    server.begin();
    // set the speed at 5 rpm
    myStepper.setSpeed(5);
}
```

----

### Control stepper

```cpp
void move_to(int tar)
{
    int diff = position[tar] - cur_pos;
    if (diff > 0)
        diff = diff - 2048;
    myStepper.step(diff);
    cur_pos = position[tar];
}
```

----

### Handle request

```cpp
void handlePostData()
{
    if (server.method() != HTTP_POST)
    {
        server.send(405, "application/json", "{\"error\":\"Method Not Allowed\"}");
        return;
    }
    String body = server.arg("plain"); // Get the raw body as a string

    Serial.println("Received Body:");
    Serial.println(body);

    // Parse JSON using ArduinoJson
    StaticJsonDocument<200> doc;
    DeserializationError error = deserializeJson(doc, body);

    if (error)
    {
        Serial.print("JSON Parse Error: ");
        Serial.println(error.c_str());
        server.send(400, "application/json", "{\"error\":\"Invalid JSON\"}");
        return;
    }

    int position = doc["position"];
    Serial.println(position);
    // Respond with the received data
    String jsonResponse = "{\"received\":\"received\"}";
    server.send(200, "application/json", jsonResponse);
    move_to(position - 1);
    Serial.println(cur_pos);
}
```

----

### 回到 Python

----

建立一個檔案 esp32_control.py 來處理與 ESP32 的通訊：

```py
import requests

ESP_IP = "192.168.50.214"
ESP_PORT = 80
ESP_API_URL = f"http://{ESP_IP}:{ESP_PORT}/spin"


def control_esp(value):
    data = {"position": value}
    response = requests.post(f"{ESP_API_URL}", json=data)
    return response.json()
```

----

修改 app.py (Flask 後端)，在得到 Gemini 回應後呼叫 control_esp：

```py
import esp32_control as esp32_control

@app.route("/api/transcribe", methods=["POST"])
def transcribe():
    if "audio" not in request.files:
        return jsonify({"error": "No audio file provided"}), 400

    # 語音轉文字
    audio_file = request.files["audio"]
    audio_content = audio_file.read()
    transcript = transcribe_audio(audio_content)

    model = genai.GenerativeModel(model_name="models/gemini-1.5-flash", system_instruction=prompt)
    response = model.generate_content(f"回覆以下句子:{transcript}")
    generated_text = response.text[:-1]
    app.logger.info(words[generated_text])
    response = esp32_control.control_esp(int(generated_text))     # 加這行
    app.logger.info(f"send {int(generated_text)} to esp32, {response=}")
    return jsonify({"text": int(generated_text), "pic": words[generated_text]})
```

---

## Docker

----

## 什麼是 Docker？

- Docker 是一個**容器化平台 (Containerization Platform)**。
- 可以將你的應用程式與其所有需要的**依賴環境 (例如 Python 版本、特定函式庫) 打包**在一起，形成一個標準化的**容器 (Container)**。
- 容器是**輕量級、可攜式**的，確保應用程式在任何地方都能**一致地**運行。

----

## 為什麼要使用 Docker？

- 環境一致性：解決「在我電腦上可以跑，在你電腦上就不行」的問題。開發、測試、生產環境完全一致。
- 快速部署：容器啟動非常快 (秒級)，方便快速建置、測試與部署。
- 資源隔離：每個容器有自己獨立的運行環境，互不影響。
- 易於擴展：可以輕鬆複製容器來擴展服務能力。
- 支援多平台：可在不同作業系統上運行。

----

## Docker 核心概念

- Image (映像檔)：相當於容器的**模板 或 藍圖**。它是一個唯讀檔案，包含了執行應用程式所需的所有內容 (程式碼、函式庫、環境變數、設定檔)。
- Container (容器)：是映像檔的**運行實例**。你可以把它想像成一個輕量級的虛擬機，但它共享主機的操作系統核心，所以更節省資源。容器可以被啟動、停止、刪除。
- Dockerfile：是一個**文本文件**，裡面包含了一系列的指令，用來告訴 Docker 如何自動建構 (build) 一個映像檔。
- Docker Hub / Registry (倉庫/註冊中心)：是用來**儲存和分享**映像檔的地方。Docker Hub 是官方的公共倉庫，也有私有倉庫可用。

----

## 安裝 Docker

### Windows / macOS 使用者：
- 安裝 [Docker Desktop](https://www.docker.com/products/docker-desktop)

### Linux:
- 參考 [官方文件](https://docs.docker.com/engine/install/)

----

## Dockerfile

```Dockerfile
FROM python:3.10

WORKDIR /app

RUN pip install --upgrade pip && \
    pip install --no-cache-dir Flask google-generativeai

COPY . .

EXPOSE 5000

CMD ["python3", "/app/app.py"]
```

然後建構映像檔並執行：

```sh
docker build -t my-flask-app .
docker run -p 5000:5000 my-flask-app
```

----


## 什麼是 Docker Compose？

- Docker Compose 是用來**定義與管理多容器應用程式**的工具
- 使用 `docker-compose.yml` 檔案描述服務、網路、掛載等設定

----

## Docker Compose

```yaml
services:
  web:
    image: nginx:latest
    ports: ["8080:80"]
    volumes: ["./nginx.conf:/etc/nginx/nginx.conf:ro"]
    depends_on: [api]
  api:
    build: ./api-service # 假設 API 服務在 api-service 資料夾
    ports: ["5000:5000"]
    environment: { DATABASE_URL: postgresql://user:password@db:5432/mydb }
    depends_on: [db]
  db:
    image: postgres:15
    environment: { POSTGRES_DB: mydb, POSTGRES_USER: user, POSTGRES_PASSWORD: password }
    volumes: ["postgres_data:/var/lib/postgresql/data"]
volumes: { postgres_data: }
```

----

只要一行指令即可啟動全部服務：

```sh
docker compose up
```

----

- Docker 解決了環境不一致和部署困難的問題。
- Dockerfile 用來定義如何打包你的應用程式。
- Docker Compose 用來管理多個容器的應用程式，特別適合本地開發和測試。
- 安裝和入門相對簡單，對開發和部署非常有幫助。

---

## 這篇簡報

----

[簡報 Repo](https://github.com/viecon/mygo-slide)

----

### 架構

- 簡報內容： 使用 Markdown 語法撰寫
- Markdown 轉 HTML： 使用 Marp CLI 工具將 Markdown 轉換成可以發佈的 HTML 投影片。
- 網站託管：GitHub Pages 免費託管生成的 HTML 檔案

----

## Github Actions

----

## 什麼是 GitHub Actions？

- GitHub 推出的 CI/CD（持續整合／持續部署）工具  
- 可以讓你 自動化 軟體開發中的各種 工作流程，例如：程式碼檢查、測試、建構、部署等。
- 工作流程定義在專案根目錄下的 `.github/workflows/` 資料夾內的 YAML 檔案中。

----

## 核心概念

- Workflow (工作流程)：定義自動化流程的 YAML 檔案。可以由一個或多個 Job 組成。
- Event (事件)：觸發 Workflow 運行的動作，例如 `push`、`pull_request`、`schedule` (定時執行) 等。
- Job (工作)：Workflow 中的一個執行單元，包含一個或多個 Step。同一個 Job 中的所有 Step 會在同一個 Runner 上執行。
- Step (步驟)：Job 中的最小執行單位，可以是一個 Shell 指令，或者是一個可重複使用的 Action。
- Action (動作)：可重複使用的程式碼單元，用來執行常見的自動化任務 (例如：checkout 程式碼、設定 Node.js 環境、部署到 AWS 等)。可以自己撰寫，也可以使用市集上別人寫好的 Action。
- Runner (執行器)：實際執行 Job 的虛擬機器。GitHub 提供免費的 Linux, Windows, macOS Runner，也可以自己架設 Runner。

----

## 範例

在專案根目錄建立 `.github/workflows/hello.yml`

```yaml
name: Say Hello                            # Workflow 的名稱

on: [push]                                 # 觸發條件：當有 push 事件發生時

jobs:
  build:                                   # 定義一個名為 'build' 的 Job
    runs-on: ubuntu-latest                 # 指定執行環境為最新的 Ubuntu
    steps:                                 # 這個 Job 包含的步驟
      - uses: actions/checkout@v4          # 拉取程式碼
      - name: Run a one-line script        # 執行 Shell 指令
        run: echo "Hello, GitHub Actions!"
```

當你 push 程式碼到 GitHub 時，這個 Workflow 就會自動執行。

----

## 為什麼要使用 GitHub Actions？

- 與 GitHub 原生整合  
- 輕鬆自動化測試與部署流程  
- 擁有豐富的可重複使用 actions 生態系  
- 公開儲存庫免費使用  

----

## 應用情境

- 程式碼檢查：自動檢查程式碼風格是否符合規範。
- 單元測試：每次 push 或 PR 時自動執行測試。
- 建構與打包：自動編譯程式碼、建構 Docker Image。
- 部署：自動將應用程式部署到伺服器、雲平台 (AWS, GCP, Azure) 或 GitHub Pages。

----

### 我拿來做什麼

當 push 時用 `Marp` 把 Markdown 轉成 HTML，並部署到 `GitHub Pages`

```yaml
- name: Install Marp CLI and build slide
  run: |
    mkdir -p dist
    npx @marp-team/marp-cli@latest slide.md --html --output dist/index.html
    cp -r pics dist/

- name: Setup Pages
  uses: actions/configure-pages@v5

- name: Upload artifact
  uses: actions/upload-pages-artifact@v3
  with:
    path: dist

- name: Deploy to GitHub Pages
  id: deployment
  uses: actions/deploy-pages@v4
```

----

- 官方文件：[https://docs.github.com/actions](https://docs.github.com/actions)  
- Action 市集：[https://github.com/marketplace/actions](https://github.com/marketplace/actions)

---

# END