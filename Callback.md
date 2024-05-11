```python
def costly_iterative_func(num_iterations,
						  function_callback = None,
						  iteration_callback = None
						  ):
	for i in range(num_iterations):
		code
		if not iteration_callback is None:
			iteration_callback() 
	if not function_callback is None:
		function_callback()

def iteration_callback():
	print("im showing some things relevant to the iteration")
	# save checkpoints, do things whatever

#same with func
```

in keras its done with classes:

```python
class PrintValTrainRatioCallback(tf.keras.callbacks.Callback):
	def on_epoch_end(self, epoch, logs):
	ratio = logs["val_loss"] / logs["loss"]
	print(f"Epoch={epoch}, val/train={ratio:.2f}")
```
>[!info]
>As you might expect, you can implement on_train_begin(), on_train_end(), on_epoch_begin(), on_epoch_end(), on_batch_begin(), and on_batch_end(). Call‐ backs can also be used during evaluation and predictions, should you ever need them (e.g., for debugging). For evaluation, you should implement on_test_begin(), on_test_end(), on_test_batch_begin(), or on_test_batch_end(), which are called by evaluate(). For prediction, you should implement on_predict_begin(), on_predict_end(), on_predict_batch_begin(), or on_predict_batch_end(), which are called by predict().

Also uses it for early stopping, getting best model and TensorBoard:

The good news is that Keras provides a convenient TensorBoard() callback that will
take care of creating the log directory for you (along with its parent directories if
needed), and it will create event files and write summaries to them during training. It
will measure your model’s training and validation loss and metrics (in this case, the
MSE and RMSE), and it will also profile your neural network. It is straightforward to
use

```python
tensorboard_cb = tf.keras.callbacks.TensorBoard(run_logdir,
	profile_batch=(100, 200))
history = model.fit([...], callbacks=[tensorboard_cb])
```

