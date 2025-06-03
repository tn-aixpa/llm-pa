# LLM for PA

## Usage Scenario
The main purpose of the model is to support the creation of chatbot dialogs around public services. The model is an LLM adapter for the 
[meta-llama/Llama-3.1-8B-Instruct](https://huggingface.co/meta-llama/Llama-3.1-8B-Instruct) model.

## Prerequisities

To use the model, several requirements should be satisfied:

- Being an Adapter, the LLM should be deployed together with the main Llama-3.1-8B-Instruct model. The corresponding environment relying on vLLM or its derivatives (e.g., KubeAI) may apply.
  Alternatively, may be deployed using the Huggingface libraries directly.
- The model requires appropriate GPU for serving (e.g., NVIDIA A100). 

## Usage scenarios

Typical usage of the model is to support the RAG-baseds dialog creation for public services. See e.g., the [FamilyAudit Chatbot](https://github.com/tn-aixpa/faudit-chatbot) application that relies on this model. 

The way the model can be deployed and exposed using the AIxPA platform is described [here](howto/expose.md).
