1. Reinforcement learning creates data, game engines play better than humans and make strategies we never taught them.

Note that the model is copied using copy.deepcopy() `from copy import deepcopy`,
because it copies both the model’s hyperparameters and the learned parameters. In
contrast, sklearn.base.clone() only copies the model’s hyperparameters.
