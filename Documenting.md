# Documenting Gensim - A checklist

This is a checklist for some frequent and easy to miss mistakes while documenting.
Before you push a new commit, please make sure your docstrings are checked against every item in this list
as some of them will not be caught by [CircleCi](https://circleci.com/).

*This is not an exhaustive list (yet) so please feel free to expand or modify it.*


1. All parameters must exist in the docstring, including their type and explanation
	```
	s : list of float
		Eigenvalues of the original matrix.
	```

2. Parameters with a default argument are tagged with type, optional
	```
	def f(num_topics=500, ...)
		"""Function summary.

		Parameters
		----------
		num_topics : int, optional
			Number of requested factors (latent dimensions)

		"""
	```

3. Functions and methods that return something only include type and explanation, not the variable's name.
	```
	Returns
	-------
	list of (int, int)
		Topic distribution matrix of shape [num_docs, num_topics]
	```

4. References to methods, functions or classes of gensim are sphinx compliant using an absolute path (`:func:`, `:meth:`, `:class:`).
	
	```
	The projection can be later updated by merging it with another 
	:class:`~gensim.models.lsimodel.Projection` via :meth:`~gensim.models.lsimodel.Projection.merge`.
	```

5. Each module comes with an example of 5 to 20 lines. Sample data from `gensim.test.utils` can be imported.
	```
	Examples
	--------
	Integrate with sklearn Pipelines:

	>>> from sklearn.pipeline import Pipeline
	>>> from sklearn import linear_model
	>>> from gensim.test.utils import common_corpus, common_dictionary
	>>> from gensim.sklearn_api import LsiTransformer
	>>>
	>>> # Create stages for our pipeline (including gensim and sklearn models alike).
	>>> model = LsiTransformer(num_topics=15, id2word=common_dictionary)
	>>> clf = linear_model.LogisticRegression(penalty='l2', C=0.1)
	>>> pipe = Pipeline([('features', model,), ('classifier', clf)])
	>>>
	>>> # Create some random binary labels for our documents.
	>>> labels = np.random.choice([0, 1], len(common_corpus))
	>>>
	>>> # How well does our pipeline perform on the training set?
	>>> score = pipe.fit(common_corpus, labels).score(common_corpus, labels)
	```

6. In case parameters can be of type A OR B, use this notation: my_param : {A, B}
	```
	docs : {iterable of list of (int, float), scipy.sparse.csc}
		Corpus in BoW format or as sparse matrix.
	```

7. In case of scripts, do not duplicate command line arg documentation, call it instead with `:program-output`. See file `~gensim.models.lsiworker.py`

8. When documenting container types, mention their shape (if known) like: shape is (a, b).
	```
	docs : iterable of iterable of (int, float)
		Stream of document vectors or sparse matrix of shape: (`num_terms`, num_documents).
	```

9. When referring to variables in the code use the `` ` `` (backtick) symbol.
See example above where `num_terms` is method argument.

10. When referencing a paper use the `` `authors: "title, journal" <link>`_`` syntax.
	```
	Explained in `Gerlof Bouma: "Normalized (Pointwise) Mutual Information in Collocation Extraction"
	<https://svn.spraakdata.gu.se/repos/gerlof/pub/www/Docs/npmi-pfd.pdf>`_.
	```
	
	Alternatively define it once and reuse it later.
	```
	References
    ----------
    .. [1] https://en.wikipedia.org/wiki/SMART_Information_Retrieval_System


	def function(x):
	""" Applies the transformation explained in [1]_. """
		pass
	```









