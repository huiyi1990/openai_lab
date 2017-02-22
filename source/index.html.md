---
title: OpenAI Lab Doc

language_tabs:
  - python

toc_footers:
  - <a href='https://github.com/kengz/openai_lab'>OpenAI Lab Github</a>
  - <a href='https://github.com/openai/gym'>OpenAI Gym Github</a>

includes:
  - INSTALLATION
  - USAGE
  - FEATURES
  - DEVELOPMENT
  - ROADMAP
  - CONTRIBUTING

search: true
---

# OpenAI Lab [![CircleCI](https://circleci.com/gh/kengz/openai_lab.svg?style=shield)](https://circleci.com/gh/kengz/openai_lab) [![Codacy Badge](https://api.codacy.com/project/badge/Grade/a0e6bbbb6c4845ccaab2db9aecfecbb0)](https://www.codacy.com/app/kengzwl/openai_lab?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=kengz/openai_lab&amp;utm_campaign=Badge_Grade)

_An experimentation system for Reinforcement Learning using OpenAI and Keras._

This lab is created to let us do Reinforcement Learning (RL) like science - _theorize, experiment_. We can theorize as fast as we think, and experiment as fast as the computers can run.

With the _OpenAI Lab_, we had solved a few OpenAI environments by running dozens of experiments, each with hundreds of trials. Each new experiment takes minimal effort to setup and run - we will show by example below.


## Lab Demo

Each experiment involves:
- a problem - an [OpenAI Gym environment](https://gym.openai.com/envs)
- a RL agent with modular components `agent, memory, optimizer, policy, preprocessor`, each of which is an experimental variable.

We specify input parameters for the experimental variable, run the experiment, record and analyze the data, conclude if the agent solves the problem with high rewards.

### Specify Experiment

The example below is fully specified in `rl/asset/experiment_specs.json` under `dqn`:

```json
{
  "dqn": {
    "problem": "CartPole-v0",
    "Agent": "DQN",
    "HyperOptimizer": "GridSearch",
    "Memory": "LinearMemoryWithForgetting",
    "Optimizer": "AdamOptimizer",
    "Policy": "BoltzmannPolicy",
    "PreProcessor": "NoPreProcessor",
    "param": {
      "train_per_n_new_exp": 1,
      "lr": 0.001,
      "gamma": 0.96,
      "hidden_layers_shape": [16],
      "hidden_layers_activation": "sigmoid",
      "exploration_anneal_episodes": 20
    },
    "param_range": {
      "lr": [0.001, 0.01, 0.02, 0.05],
      "gamma": [0.95, 0.96, 0.97, 0.99],
      "hidden_layers_shape": [
        [8],
        [16],
        [32]
      ],
      "exploration_anneal_episodes": [10, 20]
    }
  }
}
```

- *experiment*: `dqn`
- *problem*: [CartPole-v0](https://gym.openai.com/envs/CartPole-v0)
- *variable agent component*: `Boltzmann` policy
- *control agent variables*:
    - `DQN` agent
    - `LinearMemoryWithForgetting`
    - `AdamOptimizer`
    - `NoPreProcessor`
- *parameter variables values*: the `"param_range"` JSON


### Lab Workflow

The workflow to setup this experiment is as follow:

1. Add the new theorized component `Boltzmann` in `rl/policy/boltzmann.py`
2. Specify `dqn` experiment spec in `experiment_spec.json` to include this new variable,  reuse the other existing RL components, and specify the param range.
3. Add this experiment to the lab queue in `config/production.json`
4. Run `grunt -prod`
5. Analyze the graphs and data (live-synced)


### Lab Results

>The `dqn` experiment analytics (zoom/download to see):
<img alt="The dqn experiment analytics" src="./images/dqn.png" />

On completion, from the analytics (zoom on the graph), we conclude that the experiment is a success, and the best agent that solves the problem has parameters:

- *lr*: (pending below)
- *gamma*: 
- *hidden_layers_shape*: 
- *exploration_anneal_episodes*: 
