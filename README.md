See https://pi.dev/

## Dockerfile.pi 

```
FROM node:24-bookworm-slim

RUN apt-get update \
  && apt-get install -y --no-install-recommends bash ca-certificates git ripgrep \
  && rm -rf /var/lib/apt/lists/*
RUN npm install -g --ignore-scripts @earendil-works/pi-coding-agent

WORKDIR /workspace
ENTRYPOINT ["pi"]
```

## Bash Functions (Podman)

```

# rebuild image
function :build_pi() {
	podman build --no-cache -t pi-sandbox -f Dockerfile.pi .
}

# enter pi
function :pi() {
  podman run --rm -it \
    -e FEATHERLESS_API_KEY="$FEATHERLESS_API_KEY" \
    -v "$PWD:/workspace" \
    -v "$HOME/Pi/agent:/root/.pi/agent" \
    pi-sandbox
}

## enter terminal
function :pash() {
  podman run --rm -it \
    --entrypoint /bin/bash \
    -e FEATHERLESS_API_KEY="$FEATHERLESS_API_KEY" \
    -v "$PWD:/workspace" \
    -v "$HOME/Pi/agent:/root/.pi/agent" \
    pi-sandbox \
}
```

## Custom Model Providers (models.json)

```
{
  "providers": {
    "featherless": {
      "baseUrl": "https://api.featherless.ai/v1",
      "api": "openai-completions",
      "apiKey": "$FEATHERLESS_API_KEY",
      "models": [
        {
          "id": "<MODEL_ID>", # see https://featherless.ai/models
          "name": "<USER_NAME>", # User Label
          "reasoning": true, # Adjust as neccessary
          "input": [
            "text"
          ],
          "contextWindow": 262144,
          "maxTokens": 32768
        },
      ]
    }
  }
}
```
