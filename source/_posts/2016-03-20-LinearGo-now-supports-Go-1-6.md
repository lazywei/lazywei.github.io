title: LinearGo now supports Go 1.6
tags:
  - dev
  - golang
  - machine learning
  - open source
date: 2016-03-20 17:11:09
---


Last summer, I built [LinearGo](https://github.com/lazywei/lineargo), a Go wrapper for the famous machine learning library [LIBLINEAR](http://www.csie.ntu.edu.tw/~cjlin/liblinear/). Basically, I used CGO to bridge LIBLINEAR’s C library and Go. At first, I tried to build a 1-to-1 mapping for all of the data structure defined in C liblinear to hide the implementation details of C behind Go code.

However, things become complicated after Go 1.6 as they change their pointer policy for CGO. We can no longer pass something that contains a Go pointer which points to a Go pointer (WTF I just said!?). For example, a `Parameter` contains a pointer to a `FeatureNode` (an array of FeatureNode in fact). Before Go 1.6, I simply pass the pointer of `Parameter` to C, contains a pointer to a `FeatureNode` (a Go struct). Things like this, which a Go pointer contains a Go pointer, is not allowed anymore in Go 1.6. Therefore, I'm forced to figure out some other way to get around this.

The solution I came up with is passing plain C data type only (things like `int`, `double`, or pointers to them), and then re-construct the struct in C. Therefore, I removed the corresponding Go structure and wrote some C helper function which I can call from Go.

For example, I have a `call_train` helper function, the signature of which is

```c
struct model* call_train(
    double* x, double* y, int n_rows, int n_cols,
    double bias, int solver_type, double C, double p, double eps,
    int nr_weight, int* weight_label, double* weight);
```

And the real `train` function looks like this

```c
struct model* train(const struct problem *prob, const struct parameter *param);
```

In `call_train`, I manually construct `prob` and `param` and then call the real `train` with these arguments.


Luckily, after some monkey coding, LinearGo is now supporting Go 1.6. Moreover, I believe the current approach is more stable than previous one because we only work on simple data type when bridging while leave the complicated structure in pure C.
