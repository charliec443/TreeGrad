# TreeGrad

`TreeGrad` implements a naive approach to converting a Gradient Boosted Tree Model to an Online trainable model. It does this by creating differentiable tree models which can be learned via auto-differentiable frameworks. `TreeGrad` is in essence an implementation of Kontschieder, Peter, et al. "Deep neural decision forests." with extensions.

To install

```
python setup.py install
```

To do: put this on `pypi`

Please cite 

```
@article{siu2019treegrad,
  title={TreeGrad: Transferring Tree Ensembles to Neural Networks},
  author={Siu, Chapman},
  journal={arXiv preprint arXiv:submit/2665625},
  year={2019}
}
```


# Usage

```py
from sklearn.
import treegrad as tgd

mod = tgd.TGDClassifier(num_leaves=31, max_depth=-1, learning_rate=0.1, n_estimators=100, autograd_config={'refit_splits':False})
mod.fit(X, y)
mod.partial_fit(X, y)
```

# Requirments

The requirements for this package are:

*  lightgbm
*  scikit-learn
*  autograd

Future plans:

*  Add implementation for Neural Architecture search for decision boundary splits (requires a bit of clean up - TBA)
   *  Implementation can be done quite trivially using objects residing in `tree_utils.py` - Challenge is getting this working in a sane manner with `scikit-learn` interface.
*  gpu enabled auto differentiation framework
*  support xgboost/lightgbm additional features such as monotone constraints
*  Support `RegressorMixin`


# Implementation

<!-- insert link to arxiv paper -->

To understand the implementation of `TreeGrad`, we interpret a decision tree algorithm to be a three layer neural network, where the layers are as follows:

1.  Node layer, which determines the decision boundaries
2.  Routing layer, which determines which nodes are used to route to the final leaf nodes
3.  Leaf layer, the layer which determines the final predictions

In the node layer, the decision boundaries can be interpreted as _axis-parallel_ decision boundaries from your typical Linear Classifier; i.e. a fully connected dense layer

The routing layer requires a binary routing matrix to which essentially the global product routing is applied

The leaf layer is your typical fully connected dense layer.

This approach is the same as the one taken by Kontschieder, Peter, et al. "Deep neural decision forests."

