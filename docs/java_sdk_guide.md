# Java SDK Quickstart

Get started with the Camb.ai Java SDK in minutes

## Overview

The Camb.ai Java SDK provides a simple interface to integrate high-quality text-to-speech into your applications. This quickstart will have you generating speech in under 5 minutes.

## Installation

### Gradle

Add the dependency to your `build.gradle`:

```groovy
dependencies {
    implementation 'ai.camb:cambai-java-sdk:0.0.1'
}
```

### Maven

Add the dependency to your `pom.xml`:

```xml
<dependency>
    <groupId>ai.camb</groupId>
    <artifactId>cambai-java-sdk</artifactId>
    <version>0.0.1</version>
</dependency>
```

## Authentication

Get your API key from [CAMB.AI Studio](https://studio.camb.ai/) and set it as an environment variable:

```bash
export CAMB_API_KEY=your_api_key_here
```

## Quick Start

### Streaming Text-to-Speech

Generate and stream speech in real-time. The SDK returns an `InputStream` for the audio data:

```java
import CambApiClient;
import resources.texttospeech.requests.CreateStreamTtsRequestPayload;
import resources.texttospeech.types.CreateStreamTtsRequestPayloadLanguage;
import resources.texttospeech.types.CreateStreamTtsRequestPayloadSpeechModel;
import java.io.InputStream;
import java.io.File;
import java.nio.file.Files;
import java.nio.file.StandardCopyOption;

public class Main {
    public static void main(String[] args) throws Exception {
        // Initialize the client
        CambApiClient client = CambApiClient.builder()
            .apiKey(System.getenv("CAMB_API_KEY"))
            .build();

        // Stream TTS audio
        InputStream audioStream = client.textToSpeech().tts(CreateStreamTtsRequestPayload.builder()
            .text("Hello! Welcome to Camb.ai text-to-speech.")
            .voiceId(147320)
            .language(CreateStreamTtsRequestPayloadLanguage.EN_US)
            .speechModel(CreateStreamTtsRequestPayloadSpeechModel.MARS_FLASH)
            .build());

        // Save to file
        File outputFile = new File("output.wav");
        Files.copy(audioStream, outputFile.toPath(), StandardCopyOption.REPLACE_EXISTING);

        System.out.println("Success! Audio saved to output.wav");
    }
}
```

### Using the Helper Function

You can easily wrap the stream saving into a helper method:

```java
import java.io.InputStream;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardCopyOption;

public static void saveStreamToFile(InputStream stream, String filename) throws IOException {
    Files.copy(stream, Path.of(filename), StandardCopyOption.REPLACE_EXISTING);
}

// Usage:
InputStream stream = client.textToSpeech().tts(payload);
saveStreamToFile(stream, "output.wav");
```

## Choosing a Model

Camb.ai offers three MARS models optimized for different use cases:

### MARS Flash

```java
.speechModel(CreateStreamTtsRequestPayloadSpeechModel.MARS_FLASH)
```

**Best for**: Real-time voice agents, low-latency applications  
**Sample rate**: 22.05kHz

### MARS Pro

```java
.speechModel(CreateStreamTtsRequestPayloadSpeechModel.MARS_PRO)
```

**Best for**: Audio production, high-quality content  
**Sample rate**: 48kHz

### MARS Instruct

```java
.speechModel(CreateStreamTtsRequestPayloadSpeechModel.MARS_INSTRUCT)
.userInstructions("Speak in a warm, friendly tone")
```

**Best for**: Fine-grained control over tone and style  
**Sample rate**: 22.05kHz

## Listing Available Voices

Discover available voices for your application:

```java
var voices = client.voiceCloning().listVoices();

for (var voice : voices) {
    System.out.println("ID: " + voice.getId() + ", Name: " + voice.getVoiceName());
}
```

## Language Support

Camb.ai supports 140+ languages. Specify the language using the `CreateStreamTtsRequestPayloadLanguage` enum:
Languages supported by each model mentioned at [MARS Models](https://docs.camb.ai/models).

```java
// English (US)
.language(CreateStreamTtsRequestPayloadLanguage.EN_US)

// Spanish
.language(CreateStreamTtsRequestPayloadLanguage.ES_ES)

// French
.language(CreateStreamTtsRequestPayloadLanguage.FR_FR)
```

## Error Handling

Handle common errors gracefully:

```java
try {
    InputStream stream = client.textToSpeech().tts(payload);
} catch (Exception e) {
    System.err.println("Error generating speech: " + e.getMessage());
}
```

## Using Custom Provider

For more details check this guide [Custom Cloud Providers](https://docs.camb.ai/custom-cloud-providers)

### Baseten Deployment

Initialize the client with your custom provider implementation. [Baseten Provider Example](https://github.com/Camb-ai/cambai-java-sdk/blob/master/src/main/java/examples/BasetenExample.java)

```java
import providers.BasetenProvider;

ITtsProvider ttsProvider = new BasetenProvider(
    "YOUR_BASETEN_API_KEY",
    "YOUR_BASETEN_URL"
);
```

## Next Steps

| | |
| --- | --- |
| **🎙️ Voice Agents** <br> Build real-time voice agents with Pipecat <br> [Learn more](/tutorials/pipecat) | **🔗 LiveKit Integration** <br> Create voice agents with LiveKit <br> [Learn more](/tutorials/livekit) |
| **📄 API Reference** <br> Explore the full TTS API <br> [Learn more](/api-reference/endpoint/create-tts-stream) | **🔊 Voice Library** <br> Browse available voices <br> [Learn more](/api-reference/endpoint/list-voices) |

## Resources

*   [GitHub: camb-ai/cambai-java-sdk](https://github.com/Camb-ai/cambai-java-sdk)
*   [SDK Examples](https://github.com/Camb-ai/cambai-java-sdk/tree/master/src/main/java/examples)
*   [API Reference](https://docs.camb.ai/api-reference/endpoint/create-tts-stream)
