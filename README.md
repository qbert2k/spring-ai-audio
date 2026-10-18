# Spring AI Audio

A small Spring Boot application that turns text into spoken audio using
OpenAI's text-to-speech API through Spring AI. Send a question or any other
text to the REST endpoint and the application returns an MP3 response.

## Features

- Spring Boot 3.3.6 REST application
- Spring AI OpenAI integration
- OpenAI `tts-1` text-to-speech model
- MP3 output using the Alloy voice at normal speed
- Simple JSON request and binary audio response

## Requirements

- Java 21
- An OpenAI API key
- Internet access to call the OpenAI API

## Configuration

The application reads the API key from the `OPENAI_API_KEY` environment
variable. Set it before starting the application:

```bash
export OPENAI_API_KEY=your-api-key
```

The setting is defined in
`src/main/resources/application.yaml`. Do not commit the API key to the
repository.

## Run locally

Start the application with the Maven wrapper:

```bash
./mvnw spring-boot:run
```

On Windows, use:

```bat
mvnw.cmd spring-boot:run
```

The server listens on `http://localhost:8080` by default.

## API

### `POST /talk`

Request body:

```json
{
  "question": "Explain dependency injection in one sentence."
}
```

Example using `curl`:

```bash
curl -X POST http://localhost:8080/talk \
  -H 'Content-Type: application/json' \
  -d '{"question":"Explain dependency injection in one sentence."}' \
  --output response.mp3
```

The response has content type `audio/mpeg`. Open `response.mp3` with any
standard audio player.

## Project structure

```text
src/main/java/guru/springframework/springaiaudio/
├── SpringAiAudioApplication.java       # Application entry point
├── controllers/QuestionController.java # POST /talk endpoint
├── model/Question.java                  # Request DTO
└── services/
    ├── OpenAIService.java               # Speech service contract
    └── OpenAIServiceImpl.java           # Spring AI/OpenAI implementation
```

`QuestionController` accepts the JSON request and delegates to
`OpenAIService`. `OpenAIServiceImpl` configures the voice, speed, response
format, and model, then returns the generated audio bytes.

## Build and test

Run the test suite:

```bash
./mvnw test
```

Create an executable JAR:

```bash
./mvnw clean package
java -jar target/spring-ai-audio-0.0.1-SNAPSHOT.jar
```

## Technology

- Java 21
- Spring Boot 3.3.6
- Spring AI 1.0.0-M5
- Maven
- OpenAI text-to-speech API

## Learning resources

This repository is part of the Spring Framework Guru examples. More Spring
courses and resources are available at
[springframework.guru](https://springframework.guru/).
