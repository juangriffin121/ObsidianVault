1. Reinforcement learning creates data, game engines play better than humans and make strategies we never taught them.

Note that the model is copied using copy.deepcopy() `from copy import deepcopy`,
because it copies both the model’s hyperparameters and the learned parameters. In
contrast, sklearn.base.clone() only copies the model’s hyperparameters.

Windows 64-bit packages of scikit-learn can be accelerated using scikit-learn-intelex.
    More details are available here: https://intel.github.io/scikit-learn-intelex

    For example:

        $ conda install scikit-learn-intelex
        $ python -m sklearnex my_application.py