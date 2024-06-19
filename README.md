<div align="center">
 <img alt="astra.ai" width="300px" height="auto" src="https://github.com/AgoraIO-Community/ASTRA.ai/assets/471561/20ae3baa-5f84-44b6-8a17-19ca70d37c95">
</div>

# ASTRA.ai
ASTRA.ai is an agent framework that supports the creation of real-time multimodal AI Agents. It enables the rapid orchestration and reuse of the latest large model capabilities, achieving low-latency, real-time multimodal interaction with AI Agents.

ASTRA.ai is the perfect framework for building multimodal AI agents that communicate through text, vision, and audio using the latest AI capabilities, such as those from OpenAI, in real time.

## Concepts
<div align="center">
 <img src="https://github.com/AgoraIO-Community/ASTRA.ai/assets/471561/9fd7fa08-4eff-46b0-bd50-012c8dccfd9a" width="800">
</div>

### Extension
An extension is the fundamental unit of composition. Developers can create extensions in various languages and combine them in different ways to build diverse scenarios and applications. The ASTRA.ai framework emphasizes cross-language collaboration, allowing extensions written in different programming languages to seamlessly work together within the same application or service.

For example, if an application requires real-time communication (RTC) features and advanced AI capabilities, a developer might choose to write RTC-related extensions in C++ for its performance advantages in processing audio and video data. At the same time, they could develop AI extensions in Python to leverage its extensive libraries and frameworks for data analysis and machine learning tasks.

### Graph
A graph is used to describe the data flow between extensions, a graph in ASTRA orchestrates how different extensions interact. For example, the text output from a speech-to-text (STT) extension might be directed to a large language model (LLM) extension. Essentially, a graph defines which extensions are involved and the direction of data flow between them. Developers can customize this flow, directing outputs from one extension, such as an STT, into another, like an LLM.

In ASTRA, there are four main types of data flow between extensions:

- Command
- Data
- Image frame
- PCM frame

By specifying the direction of these data types in the graph, developers can enable mutual invocation and unidirectional data flow between plugins. This is especially useful for PCM and image data types, making audio and video processing simpler and more intuitive.

### Agent App
A runnable server-side participant application compiled to combine multiple **Extensions** following **Graph** rules to accomplish more sophisticated operations.

## Example
This project provides an example Agent App to help you get started.
We'll use following Extensions:
- *agora_rtc* / [Agora](https://docs.agora.io/en) for RTC transport + VAD + Azure speech-to-text (STT)
- *azure_tts* / [Azure](https://azure.microsoft.com/en-us/products/ai-services/text-to-speech) for text-to-speech (TTS)
- *openai_chatgpt* / [OpenAI](https://openai.com/index/openai-api/) for LLM
- *chat_transcriber* / A utility ext to forward chat logs into channel
- *interrupt_detector* / A utility ext to help interrupt agent

<div align="left">
 <img src="https://github.com/AgoraIO-Community/ASTRA.ai/assets/471561/bff35c13-e19b-43f7-ba1f-f9f0d7ec095f" width="600">
</div>

We also provide [a web playground](https://astra-agents.agora.io/) to help you test with the agent you built.


## Running Locally
Currently, the agent we build runs on Linux only, while we have prepared a Docker image so that you can build and run the agent on Windows / MacOS too.

To start, ensure you have [Docker](https://www.docker.com/) installed.

You can either choose to use our prebuilt agent docker image or build from source.

### Start Prebuilt Agent
```
docker run --restart=always -itd -p 8080:8080 \
        -v /tmp:/tmp \
        -e AGORA_APP_ID=<your_agora_appid> \
        -e AGORA_APP_CERTIFICATE=<your_agora_app_certificate> \
        -e AZURE_STT_KEY=<your_azure_stt_key> \
        -e AZURE_STT_REGION=<your_azure_stt_region> \
        -e OPENAI_API_KEY=<your_openai_api_key> \
        -e AZURE_TTS_KEY=<your_azure_tts_key> \
        -e AZURE_TTS_REGION=<your_azure_tts_region> \
        --name astra_agents_server \
        agoraio/astra_agents_server:0.1.0
```

### Build Agent From Source
We need to prepare the proper `manifest.json` file first.

```
# rename manifest example
cp ./agents/manifest.json.example ./agents/manifest.json

# pull the docker image with dev tools and mount your current folder as workspace
docker run -itd -v $(pwd):/app -w /app -p 8080:8080 --name astra_agents_dev agoraio/astra_agents_build:0.1.0

# enter docker image
docker exec -it astra_agents_dev bash

# build agent
make build
```

```
export AGORA_APP_ID=<your_agora_appid>
export AGORA_APP_CERTIFICATE=<your_agora_app_certificate>
export AZURE_STT_KEY=<your_azure_stt_key>
export AZURE_STT_REGION=<your_azure_stt_region>
export OPENAI_API_KEY=<your_openai_api_key>
export AZURE_TTS_KEY=<your_azure_tts_key>
export AZURE_TTS_REGION=<your_azure_tts_region>

# agent is ready to start on port 8080
make run-server
```

## Playground

You can use the playground project to test with the server you just started.

The playground project is built based on Next.js, it requires Node.js 18.17 or above.

```
# set up an .env file with AGORA_APP_ID and AGORA_APP_CERTIFICATE
cp ./playground/.env.example ./playground/.env

cd playground

# install npm dependencies & start
npm i
npm run dev
```

Greetings ASTRA.ai Agent!

## Build & Share Your Extensions with  and Package Manager
### ASTRA Cloud Store
Provides a hub for developers to share their extensions or use extensions from other developers.

### ASTRA Package Manager
Simplifies the process of uploading, sharing, downloading, and installing ASTRA extensions. Extensions can specify dependencies on other extensions and the environment, and the package manager automatically manages these dependencies, making the installation and release of extensions extremely convenient.

## TODO
- [ ] Extension Support: Python
- [ ] Extension: elevenlabs, google, whisper, moondream
- [ ] Example Agent: real-time video agent
- [ ] Extension Store
- [ ] UI Graph Editor
- ...
Stay tuned!

## Code Contributors
Thanks to all contributors!
