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
{
  "providers": {
    "featherless": {
      "baseUrl": "https://api.featherless.ai/v1",
      "api": "openai-completions",
      "apiKey": "$FEATHERLESS_API_KEY",
      "models": [
        {
          "id": "<FEATHER_ID>",
          "name": "<NAME>",
          "reasoning": true,
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
