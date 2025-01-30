# Setup Fabric on top of Ollama

[fabric](https://github.com/danielmiessler/fabric) looks really nice.

I created this little playground to play with it and maybe let others do the same.

The container image we are using for our devcontainer environment is installing and configuring fabric with ollama.
We still need to download the model we want to use.

## Finalize setup

```shell
export MODEL=llama3.2:latest
ollama pull $MODEL
alias fabric='/usr/bin/fabric -m ${MODEL}'
```

## Play time

```shell
echo "Create a prompt that generates an outline for an article based on the input provided to it" | fabric -p improve_prompt
```