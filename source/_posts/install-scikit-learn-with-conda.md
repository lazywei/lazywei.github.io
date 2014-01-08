title: Install scikit-learn with Conda
tags: ["Python", "Machine Learning"]
date: 2014-01-08 22:10:55
---

[Conda](http://docs.continuum.io/conda/) is a powerful package manager tool for python, while [scikit-learn](http://scikit-learn.org/) is a awesome machine learning libraries written in python.

- Download Conda: https://store.continuum.io/cshop/anaconda/

After installing it, there will be a `anaconda` folder in your directory. You can choose add that directory into your `PATH`. I don't, however, because I don't want to override my brewed python.

Remember to update your conda:
```
~/anaconda/bin/conda update conda
```

- Create environment:

```
~/anaconda/bin/conda create -n py3k python=3.3 anaconda
```

You can, of course, use `conda` directly if you added `anaconda/bin` into your `PATH`.

- Activate the environment:

```
source ~/anaconda/bin/activate py3k
```

- Install `scikit-learn`:

```
~/anaconda/bin/conda install scikit-learn
```

I'm confused with whether should I use this approach to install `scikit-learn` or not? Should I use `pip` instead?
