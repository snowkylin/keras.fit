# Keras Model Training API

## `fit` method

```python
fit(
    x=None,
    y=None,
    batch_size=None,
    epochs=1,
    verbose='auto',
    callbacks=None,
    validation_split=0.0,
    validation_data=None,
    shuffle=True,
    class_weight=None,
    sample_weight=None,
    initial_epoch=0,
    steps_per_epoch=None,
    validation_steps=None,
    validation_batch_size=None,
    validation_freq=1
)
```

Trains the model for a fixed number of epochs (dataset iterations).

Args:

<style>
td, th {
    border: 0;
    margin: 0;
    text-align: left;
    vertical-align: top;
}
/* thead {
    background-color: #feefe3;
}
td:first-child {
    background-color: #fff9f4;
} */
table {
    font-size: 14px;
}
</style>

:::{list-table}
:widths: 10 30
:header-rows: 1
:stub-columns: 1

*   - Args
    - 
*   - `x`
    - Input data. It could be:

         - A NumPy array (or array-like), or a list of arrays
         (in case the model has multiple inputs).
         - A tensor, or a list of tensors
         (in case the model has multiple inputs).
         - A dict mapping input names to the corresponding array/tensors,
         if the model has named inputs.
         - A `tf.data.Dataset`. Should return a tuple
         of either `(inputs, targets)` or
         `(inputs, targets, sample_weights)`.
         - A `keras.utils.PyDataset` returning `(inputs,
         targets)` or `(inputs, targets, sample_weights)`.
*   - `y`
    - Target data. Like the input data `x`,
         it could be either NumPy array(s) or backend-native tensor(s).
         If `x` is a dataset, generator,
         or `keras.utils.PyDataset` instance, `y` should
         not be specified (since targets will be obtained from `x`).
*   - `batch_size`
    - Integer or `None`.
         Number of samples per gradient update.
         If unspecified, `batch_size` will default to 32.
         Do not specify the `batch_size` if your data is in the
         form of datasets, generators, or `keras.utils.PyDataset`
         instances (since they generate batches).
*   - `epochs`
    - Integer. Number of epochs to train the model.
         An epoch is an iteration over the entire `x` and `y`
         data provided
         (unless the `steps_per_epoch` flag is set to
         something other than None).
         Note that in conjunction with `initial_epoch`,
         `epochs` is to be understood as "final epoch".
         The model is not trained for a number of iterations
         given by `epochs`, but merely until the epoch
         of index `epochs` is reached.
*   - `verbose`
    - `"auto"`, 0, 1, or 2. Verbosity mode.
         0 = silent, 1 = progress bar, 2 = one line per epoch.
         "auto" becomes 1 for most cases.
         Note that the progress bar is not
         particularly useful when logged to a file,
         so `verbose=2` is recommended when not running interactively
         (e.g., in a production environment). Defaults to `"auto"`.
*   - `callbacks`
    - List of `keras.callbacks.Callback` instances.
         List of callbacks to apply during training.
         See `keras.callbacks`. Note
         `keras.callbacks.ProgbarLogger` and
         `keras.callbacks.History` callbacks are created
         automatically and need not be passed to `model.fit()`.
         `keras.callbacks.ProgbarLogger` is created
         or not based on the `verbose` argument in `model.fit()`.
*   - `validation_split`
    - Float between 0 and 1.
         Fraction of the training data to be used as validation data.
         The model will set apart this fraction of the training data,
         will not train on it, and will evaluate
         the loss and any model metrics
         on this data at the end of each epoch.
         The validation data is selected from the last samples
         in the `x` and `y` data provided, before shuffling. This
         argument is not supported when `x` is a dataset, generator or
         `keras.utils.PyDataset` instance.
         If both `validation_data` and `validation_split` are provided,
         `validation_data` will override `validation_split`.
*   - `validation_data`
    - Data on which to evaluate
         the loss and any model metrics at the end of each epoch.
         The model will not be trained on this data. Thus, note the fact
         that the validation loss of data provided using
         `validation_split` or `validation_data` is not affected by
         regularization layers like noise and dropout.
         `validation_data` will override `validation_split`.
         It could be:
         - A tuple `(x_val, y_val)` of NumPy arrays or tensors.
         - A tuple `(x_val, y_val, val_sample_weights)` of NumPy
         arrays.
         - A `tf.data.Dataset`.
         - A Python generator or `keras.utils.PyDataset` returning
         `(inputs, targets)` or `(inputs, targets, sample_weights)`.
*   - `shuffle`
    - Boolean, whether to shuffle the training data
         before each epoch. This argument is
         ignored when `x` is a generator or a `tf.data.Dataset`.
*   - `class_weight`
    - Optional dictionary mapping class indices (integers)
         to a weight (float) value, used for weighting the loss function
         (during training only).
         This can be useful to tell the model to
         "pay more attention" to samples from
         an under-represented class. When `class_weight` is specified
         and targets have a rank of 2 or greater, either `y` must be
         one-hot encoded, or an explicit final dimension of `1` must
         be included for sparse class labels.
*   - `sample_weight`
    - Optional NumPy array of weights for
         the training samples, used for weighting the loss function
         (during training only). You can either pass a flat (1D)
         NumPy array with the same length as the input samples
         (1:1 mapping between weights and samples),
         or in the case of temporal data,
         you can pass a 2D array with shape
         `(samples, sequence_length)`,
         to apply a different weight to every timestep of every sample.
         This argument is not supported when `x` is a dataset, generator,
         or `keras.utils.PyDataset` instance, instead provide the
         sample_weights as the third element of `x`.
         Note that sample weighting does not apply to metrics specified
         via the `metrics` argument in `compile()`. To apply sample
         weighting to your metrics, you can specify them via the
         `weighted_metrics` in `compile()` instead.
*   - `initial_epoch`
    - Integer.
         Epoch at which to start training
         (useful for resuming a previous training run).
*   - `steps_per_epoch`
    - Integer or `None`.
         Total number of steps (batches of samples)
         before declaring one epoch finished and starting the
         next epoch. When training with input tensors such as
         backend-native tensors, the default `None` is equal to
         the number of samples in your dataset divided by
         the batch size, or 1 if that cannot be determined. If `x` is a
         `tf.data.Dataset`, and `steps_per_epoch`
         is `None`, the epoch will run until the input dataset is
         exhausted.  When passing an infinitely repeating dataset, you
         must specify the `steps_per_epoch` argument. If
         `steps_per_epoch=-1` the training will run indefinitely with an
         infinitely repeating dataset.
*   - `validation_steps`
    - Only relevant if `validation_data` is provided.
         Total number of steps (batches of
         samples) to draw before stopping when performing validation
         at the end of every epoch. If `validation_steps` is `None`,
         validation will run until the `validation_data` dataset is
         exhausted. In the case of an infinitely repeated dataset, it
         will run into an infinite loop. If `validation_steps` is
         specified and only part of the dataset will be consumed, the
         evaluation will start from the beginning of the dataset at each
         epoch. This ensures that the same validation samples are used
         every time.
*   - `validation_batch_size`
    - Integer or `None`.
         Number of samples per validation batch.
         If unspecified, will default to `batch_size`.
         Do not specify the `validation_batch_size` if your data is in
         the form of datasets or `keras.utils.PyDataset`
         instances (since they generate batches).
*   - `validation_freq`
    - Only relevant if validation data is provided.
         Specifies how many training epochs to run
         before a new validation run is performed,
         e.g. `validation_freq=2` runs validation every 2 epochs.
:::

Unpacking behavior for iterator-like inputs:
   A common pattern is to pass an iterator like object such as a
   `tf.data.Dataset` or a `keras.utils.PyDataset` to `fit()`,
   which will in fact yield not only features (`x`)
   but optionally targets (`y`) and sample weights (`sample_weight`).
   Keras requires that the output of such iterator-likes be
   unambiguous. The iterator should return a tuple
   of length 1, 2, or 3, where the optional second and third elements
   will be used for `y` and `sample_weight` respectively.
   Any other type provided will be wrapped in
   a length-one tuple, effectively treating everything as `x`. When
   yielding dicts, they should still adhere to the top-level tuple
   structure,
   e.g. `({"x0": x0, "x1": x1}, y)`. Keras will not attempt to separate
   features, targets, and weights from the keys of a single dict.
   A notable unsupported data type is the `namedtuple`. The reason is
   that it behaves like both an ordered datatype (tuple) and a mapping
   datatype (dict). So given a namedtuple of the form:
   `namedtuple("example_tuple", ["y", "x"])`
   it is ambiguous whether to reverse the order of the elements when
   interpreting the value. Even worse is a tuple of the form:
   `namedtuple("other_tuple", ["x", "y", "z"])`
   where it is unclear if the tuple was intended to be unpacked
   into `x`, `y`, and `sample_weight` or passed through
   as a single element to `x`.

:::{admonition} Returns:

A `History` object. Its `History.history` attribute is
   a record of training loss values and metrics values
   at successive epochs, as well as validation loss values
   and validation metrics values (if applicable).
:::

### References

- TensorFlow `tf.keras.Model` documentation: [[link]](https://www.tensorflow.org/api_docs/python/tf/keras/Model#fit)
- Keras Model training APIs documentation: [[link]](https://keras.io/api/models/model_training_apis/#fit-methodt)


:::{admonition} `.fit` is a top-level domain.
:class: tip
This might be the reason why you find here :-) Details [here](https://icannwiki.org/.fit).

How many guys are before you? ↓

<a href="https://info.flagcounter.com/c86D"><img src="https://s01.flagcounter.com/count2/c86D/bg_FFFFFF/txt_000000/border_CCCCCC/columns_4/maxflags_250/viewers_3/labels_0/pageviews_1/flags_0/percent_0/" alt="Free counters!" border="0"></a>

Maybe you are also interested in:

- A Concise Handbook of TensorFlow 2: <https://tf.wiki/en/>
- My personal webpage: <https://snowkylin.github.io>
:::

<script src="https://utteranc.es/client.js"
        repo="snowkylin/keras.fit"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
