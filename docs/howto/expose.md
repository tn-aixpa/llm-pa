# Expose LLM as OpenAI-compatible service

The model may be exposed as OpenAI text generation API using the appropriate tools. Specifically, it is possible to use KubeAI deployment for this purpose.

### Standalone KubeAI Deployment

To deploy the model in a standalone KubeAI environment, use the following Model spec:

```yaml
apiVersion: kubeai.org/v1
kind: Model
metadata:
  name: llm-pa
spec:
  features: [TextGeneration]
  owner: meta-llama
  url: hf://unsloth/Meta-Llama-3.1-8B-Instruct-bnb-4bit
  adapters: # <--
  - name: famiglia
    url: hf://LanD-FBK/aixpa
  engine: VLLM
  args:
  - --load-format=bitsandbytes
  - --max-lora-rank=32
  - --enable-prefix-caching
  - --max-model-len=8192
  resourceProfile: 1xa100:1
  minReplicas: 1
  maxReplicas: 1
```

Change the ``resourceProfile`` value with the values that correspond to the environment.

Once deployed, the model API is available under the following endpoint: ``http://$KUBEAI_ENDPOINT/openai/v1``. Use OpenAI ``/completions`` or ``/chat/completions`` method to interact with the model API.

### Deploy in AIxPA platform

To deploy the model in AIxPA platform, use the ``kubeai-text`` runtime. Specifically,

1. Define the KubeAI-based function:

```python
llm_function = project.new_function("famigliallm",
                                    kind="kubeai-text",
                                    model_name="llm-pa",
                                    url="hf://unsloth/Meta-Llama-3.1-8B-Instruct-bnb-4bit",
                                    engine="VLLM",
                                    features=["TextGeneration"],
                                    adapters=[{"name": "famiglia", "url": "hf://LanD-FBK/aixpa"}])
```

2. Deploy the model:
```python
llm_run = llm_function.run(action="serve",
                           profile="1xa100",
                           args=["--load-format=bitsandbytes", "--max-lora-rank=32", "--enable-prefix-caching", "--max-model-len=8192"])
```
Once deployed, the model API is available under the following endpoint: ``http://$KUBEAI_ENDPOINT/openai/v1``. Use OpenAI ``/completions`` or ``/chat/completions`` method to interact with the model API.
