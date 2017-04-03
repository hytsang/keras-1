keras for R
================

Getting started: 30 seconds to Keras
------------------------------------

The core data structure of Keras is a model, a way to organize layers. The simplest type of model is the Sequential model, a linear stack of layers. For more complex architectures, you should use the Keras functional API, which allows to build arbitrary graphs of layers.

Here is the Sequential model:

``` r
library(keras)

model <- model_sequential() %>% 
  layer_dense(units = 64, input_shape = 100) %>% 
  layer_activation(activation = 'relu') %>% 
  layer_dense(units = 10) %>% 
  layer_activation(activation = 'softmax')
```

Once your model looks good, configure its learning process with `compile()`:

``` r
model <- model %>% 
  compile(loss='categorical_crossentropy',
          optimizer='sgd',
          metrics=c('accuracy'))
```

If you need to, you can further configure your optimizer. A core principle of Keras is to make things reasonably simple, while allowing the user to be fully in control when they need to (the ultimate control being the easy extensibility of the source code).

``` r
model <- model %>% 
  compile(loss='categorical_crossentropy',
          optimizer=optimizer_sgd(lr=0.01, momentum=0.9, nesterov=TRUE),
          metrics=c('accuracy'))
```

You can now iterate on your training data in batches:

``` r
# x_train and y_train are R matrices
model <- model %>% 
  fit(x_train, y_train, epochs=5, batch_size=32)
```

Evaluate your performance in one line:

``` r
loss_and_metrics <- model %>%  
  evaluate(x_test, y_test, batch_size=128)
```

Or generate predictions on new data:

``` r
classes <- model %>% 
  predict(x_test, batch_size=128)
```

Building a question answering system, an image classification model, a Neural Turing Machine, or any other model is just as fast. The ideas behind deep learning are simple, so why should their implementation be painful?
