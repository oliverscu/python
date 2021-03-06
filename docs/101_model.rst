BigML Bindings: 101 - Using a Decision Tree Model
=================================================

Following the schema described in the `prediction workflow <api_sketch.html>`_,
document, this is the code snippet that shows the minimal workflow to
create a decision tree model and produce a single prediction.

.. code-block:: python

    from bigml.api import BigML
    # step 0: creating a connection to the service (default credentials)
    api = BigML()
    # step 1: creating a source from the data in your local "data/iris.csv" file
    source = api.create_source("data/iris.csv")
    # waiting for the source to be finished. Results will be stored in `source`
    api.ok(source)
    # step 3: creating a dataset from the previously created `source`
    dataset = api.create_dataset(source)
    # waiting for the dataset to be finished
    api.ok(dataset)
    # step 5: creating a decision tree model
    model = api.create_model(dataset)
    # waiting for the model to be finished
    api.ok(model)
    # the new input data to predict for
    input_data = {"petal length": 4, "sepal length": 2}
    # creating a single prediction
    prediction = api.create_prediction(model, input_data)

If you want to create predictions for many new inputs, you can do so creating
a `batch_prediction` resource. First, you will need to upload to the platform
all the input data that you want to predict for and create the corresponding
`source` and `dataset` resources. In the example, we'll be assuming you already
created a `model` following the steps 0 to 5 in the previous snippet.

.. code-block:: python

    # step 6: creating a source from the data in your local "data/test_iris.csv" file
    test_source = api.create_source("data/test_iris.csv")
    # waiting for the source to be finished. Results will be stored in `source`
    api.ok(test_source)
    # step 8: creating a dataset from the previously created `source`
    test_dataset = api.create_dataset(test_source)
    # waiting for the dataset to be finished
    api.ok(test_dataset)
    # step 10: creating a batch prediction
    batch_prediction = api.create_batch_prediction(model, test_dataset)
    # waiting for the batch_prediction to be finished
    api.ok(batch_prediction)
    # downloading the results to your computer
    api.download_batch_prediction(batch_prediction,
                                  filename='my_dir/my_predictions.csv')

The batch prediction output (as well as any of the resources created)
can be configured using additional arguments in the corresponding create calls.
Check the `API documentation <https://bigml.com/api/>`_ to learn about the
available configuration options for any BigML resource.

You can also predict locally using the `Model`
class in the `model` module. A simple example of that is:

.. code-block:: python

    from bigml.model import Model
    local_model = Model("model/5968ec46983efc21b000001b")
    # predicting for some input data
    local_time_series.predict({"petal length": 4, "sepal length": 2,
                               "petal width": 1, "sepal witdh": 3})

Every modeling resource in BigML has its corresponding local class. Check
the `Local resources <index.html#local_resources>`_ section of the
documentation to learn more about them.
