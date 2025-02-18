# Overview

The code of paper "xJailbreak: Representation Space Guided Reinforcement Learning for Interpretable LLM Jailbreaking".

## Requirement

### Environment

Linux Ubuntu; Python 3.10; A800 (80G).

### Necessary packages

torch, transformers, numpy, pandas, sklearn, tqdm, openai

### LLM configuration

We deploy [Llama3-8B-Instruct-JB](https://huggingface.co/cooperleong00/Meta-Llama-3-8B-Instruct-Jailbroken) locally And **this is necessary** because we need a LLM without safety alignment. If you have other similar LLMs, you can replace it.

We deploy safety aligned LLM like [Qwen2.5-7B-Instruct](https://huggingface.co/Qwen/Qwen2.5-7B-Instruct) locally as attacking target. You can replace it with any other local or API LLM. We only have chat templates for Llama and Qwen if there is a local model loaded.

We assume that you have a local model like llama, you can load it by

```python
from agent.LLM_agent import Llm_manager
helper_api = {
    'model_name': 'Meta-Llama-3-8B-Instruct-Jailbroken',  # Not necessary for local model
    'model_path': 'huggingface/hub/llama/Meta-Llama-3-8B-Instruct-Jailbroken/',
    'cuda': 0,
}
helpLLM = Llm(helper_api)
helpLLM.load_model()
```

You can call a LLM from API by:

```python
from agent.LLM_agent import Llm_manager
helper_api = {
    'model_name': 'qwen',  # necessary for API model
    'api': 'sk-2233...',
    'url': 'https:// ...'
}
helpLLM = Llm(helper_api)
helpLLM.load_model()
```

## Run

You must define the LLM source using the method mentioned above before running `train.py` and `test.py`, and modify the relevant parts in these two files directly.

At first you need to train a RL agent, run:

```shell
python train.py --special_place "train/" -w 1 --cuda 0
```

We recommend that you modify the `--cuda` parameters and set them to the GPU number that you currently have available. After training, the weights and training data of the RL agent will be saved in `log/train/`.

Now you can execute attack, run:

``````shell
python text.py --special_place "test/" -w 1 --cuda 0 --target "qwen" --weight_path "{your rl-agent model file, such as "log/train/ckpt/42_PPO_linux.pt"}"
``````

We also recommend that you modify the `--cuda` parameter. After the attack is completed, the statistical results are in `log/test`.
