Model Playground Class
======================

Deployed models receive an individual Model Playground. Each Model Playground uses a personal REST API to provide a fully functional prediction dashboard that allows end-users to input text, tabular, or image data and receive live predictions.

Moreover, users can build on Model Playgrounds by:

   1) Creating ML model competitions
   2) Uploading Jupyter notebooks to share code
   3) Sharing model architectures, and 
   4) Sharing data... with all shared artifacts automatically creating a data science user portfolio. 


.. _activate:

activate
--------

Launches a live model playground to the www.modelshare.org website. The playground can optionally include a live prediction REST API for deploying ML models using model parameters and user credentials, provided by the user.

.. py:function:: ModelPlayground.activate(model_filepat, preprocessor_filepath, y_train, example_data, custom_libraries, image, reproducibility_env_filepath, memory, timeout, onnx_timeout, pyspark_support, model_input)

    :param model_filepath: Absolute path to model file. .onnx is the only accepted model file extension. "example_model.onnx" filename for file in directory. "/User/xyz/model/example_model.onnx" absolute path to model file from local directory. If no value is set the playground will be launched with only a placeholder prediction API.
    :type model_filepath: string
    :param preprocessor_filepath:  Absolute path to preprocessor file. Should be: "./preprocessor.zip". searches for an exported zip preprocessor file in the current directory. File is generated using export_preprocessor function from the AI Modelshare library. If no value is set the playground will be launched with only a placeholder prediction API.
    :type preprocessor_filepath: string
    :param y_train: training labels for classification models. 
    :type y_train: pandas dataframe of one hot encoded y train data for classification, or list of values for regression
    :param example_data: Example of X data that will be shown on the online Playground page. if no example data is submitted, certain functionalities may be limited, including the deployment of live prediction APIs. Example data can be updated at a later stage, using the update_example_data() method.` For tabular data - provide pandas DataFrame in same structure expected by preprocessor function. Other data types - absolute path to folder containing example data (first five files with relevant file extensions will be accepted). 
    :type example_data: pandas DataFrame (for tabular & text data) OR filepath as string (image, audio, video data)
    :param custom_libraries: "TRUE" if user wants to load custom Python libraries to their prediction runtime. "FALSE" if user wishes to use AI Model Share base libraries including latest versions of most common ML libs.
    :type custom_libraries: string
    :param onnx_timeout:  Time in seconds after which ONNX conversion should be interrupted.
            		 Set to False if you want to force ONNX conversion.
    :type onnx_timeout: int
    :param model_input: Required only when framework="pytorch" 
            	       One example of X training data in correct format.
    :type model_input: array_like 


    :return: prints statements with generated model playground page and live prediction API details
                        also prints steps to update the model submissions by the user/team 



.. _deploy:

deploy
------

Launches a live prediction REST API for deploying ML models using model parameters and user credentials.

.. py:function:: ModelPlayground.deploy(model_filepath, preprocessor_filepath, y_train, example_data=None, custom_libraries = "FALSE", image="", reproducibility_env_filepath=None, memory=None, timeout=None)

   :param model_filepath: Absolute path to model file. \
                          .onnx is the only accepted model file extension. 
                          "example_model.onnx" filename for file in directory. 
                          "/User/xyz/model/example_model.onnx" absolute path to model file from local directory   
   :type model_filepath: string
   :param preprocessor_filepath:  absolute path to preprocessor file. [REQUIRED] to be set by the user. Should be: "./preprocessor.zip". searches for an exported zip preprocessor file in the current directory. File is generated using export_preprocessor function from the AI Modelshare library  
   :type preprocessor_filepath: string
   :param y_train: training labels for classification models. [REQUIRED] for classification type models
   :type y_train: pandas dataframe of one hot encoded y train data for classification, or list of values for regression
   :param custom_libraries: "TRUE" if user wants to load custom Python libraries to their prediction runtime. "FALSE" if user wishes to use AI Model Share base libraries including latest versions of most common ML libs.
   :type custom_libraries: string
   :param example_data: Tabular data - pandas DataFrame in same structure expected by preprocessor function. Other data types - absolute path to folder containing example data (first five files with relevant file extensions will be accepted). REQUIRED for tabular data
   :type example_data: pandas DataFrame (for tabular & text data) OR filepath as string (image, audio, video data)
   :param reproducibility_env_filepath: [OPTIONAL] to be set by the user- absolute path to environment environment json file. Example:  "./reproducibility.json". File is generated using export_reproducibility_env function from the AI Modelshare library
   :type example_data: String
   :param memory: The amount of memory (in megabytes) to be allocated to the lambda function (default = 1024).
   :type memory: Int
   :param timeout: Number of seconds before the lambda times out (default = 30).
   :type timeout: Int

             
   :return: Prints statements with generated live prediction API details
            also prints steps to update the model submissions by the user/team

Example ::

	from aimodelshare import ModelPlayground

	#Instantiate ModelPlayground() Class as placeholder 
	myplayground=ModelPlayground(model_type="image", classification=True, private=True)

	# Create Model Playground (generates live rest api and web-app for your model/preprocessor)
	myplayground.deploy(model_filepath = "model.onnx", preprocessor_filepath = "preprocessor.zip", y_train_labels, exampledata) 


Example :: 

	# To instantiate a Model Playground that already exists: 
	
	myplayground=ModelPlayground(playground_url = "https://exampleapiurl.execute-api.us-east-1.amazonaws.com/prod/m")
	
	#You can find your Model Playground url under the "Programmatic" sub tab of the "Predict" page


.. _create_competition:

create_competition
------------------

Creates a model competition for a deployed prediction REST API

.. py:function:: ModelPlayground.create_competition(data_directory, y_test, eval_metric_filepath=None, email_list = [], public=False)


   :param y_test: y values for test data used to generate metrics from predicted values from X test data submitted via the submit_model() function. [REQUIRED] to generate eval metrics in competition leaderboard
   :type y_test: list
   :param data_directory: Path to folder storing training data and test data (excluding Y test data)
   :type data_directory: string
   :param eval_metric_filepath: [OPTIONAL] Filepath pointing to a zipfile containing custom evaluation functions exported with export_eval_metric()
   :type eval_metric_filepath: string
   :param email_list: [OPTIONAL] list of comma separated emails for users who are allowed to submit models to competition.
   :type email_list: List of strings
   :param public: [REQUIRED] True/false. Defaults to False.  If True, competition is public and ANY AIModelShare user can submit models.  Use with caution because one model and one preprocessor file will be be saved to your AWS S3 folder for each model submission.
   :type public: Boolean

   :return: Information about how to submit models to competition

Example ::

	# Create Competition
	# Note -- Make competition public (allow any AI Model Share user to submit models) 
	# .... by excluding the email_list argument and including the 'public=True' argument 
	myplayground.create_competition(data_directory='my_competition_data_folder', 
                                y_test = y_test_labels, 
                                email_list=emaillist)
                                public=True) 


.. create_experiment:

create_experiment
-----------------

Creates an experiment for a deployed prediction REST API

.. py:function:: create_experiment(data_directory, y_test, eval_metric_filepath, email_list, public, public_private_split, input_dict)


   :param y_test: y values for test data used to generate metrics from predicted values from X test data submitted via the submit_model() function. [REQUIRED] to generate eval metrics in experiment leaderboard
   :type y_test: list
   :param data_directory: Path to folder storing training data and test data (excluding Y test data)
   :type data_directory: string
   :param eval_metric_filepath: [OPTIONAL] Filepath pointing to a zipfile containing custom evaluation functions exported with export_eval_metric()
   :type eval_metric_filepath: string
   :param email_list: [OPTIONAL] list of comma separated emails for users who are allowed to submit models to competition.
   :type email_list: List of strings
   :param public: [REQUIRED] True/false. Defaults to False.  If True, competition is public and ANY AIModelShare user can submit models.  Use with caution because one model and one preprocessor file will be be saved to your AWS S3 folder for each model submission.
   :type public: Boolean
   :param public_private_split: [REQUIRED] Defaults to 0. Proportion of test data that is allocated to private hold-out set.
   :type public_private_split: Float between 0 and 1.

   :return: Information about how to submit models to experiment

Example ::

	# Create Experiment
	# Note -- Make experiment public (allow any AI Model Share user to submit models) 
	# .... by excluding the email_list argument and including the 'public=True' argument 
	myplayground.create_experiment(data_directory='my_experiment_data_folder', 
                                y_test = y_test_labels, 
                                email_list=emaillist)
                                public=True) 

.. quick_submit:

quick_submit
------------

Submits model/preprocessor to machine learning experiment leaderboard and model architecture database using live prediction API url generated by AI Modelshare library. The submitted model gets evaluated and compared with all existing models and a leaderboard can be generated 

.. py:function:: quick_submit(model_filepath, preprocessor_filepath, prediction_submission, y_test, data_directory, eval_metric_filepath, email_list, public, public_private_split, model_input, timeout, onnx_timeout, example_data)

    :param model_filepath: Absolute path to model file. \
                          .onnx is the only accepted model file extension. 
                          "example_model.onnx" filename for file in directory. 
                          "/User/xyz/model/example_model.onnx" absolute path to model file from local directory   
			If no value is set the playground will be launched with only a placeholder prediction API.
    :type model_filepath: string
    :param preprocessor_filepath:  Absolute path to preprocessor file. Should be: "./preprocessor.zip". searches for an exported zip preprocessor file in the current directory. File is generated using export_preprocessor function from the AI Modelshare library. If no value is set the playground will be launched with only a placeholder prediction API.
    :type preprocessor_filepath: string
    :param y_test: y values for test data used to generate metrics from predicted values from X test data submitted via the submit_model() function. [REQUIRED] to generate eval metrics in experiment leaderboard
    :type y_test: list
    :param data_directory: Path to folder storing training data and test data (excluding Y test data)
    :type data_directory: string
    :param eval_metric_filepath: [OPTIONAL] Filepath pointing to a zipfile containing custom evaluation functions exported with export_eval_metric()
    :type eval_metric_filepath: string
    :param email_list: [OPTIONAL] list of comma separated emails for users who are allowed to submit models to competition.
    :type email_list: List of strings
    :param public: [REQUIRED] True/false. Defaults to False.  If True, competition is public and ANY AIModelShare user can submit models.  Use with caution because one model and one preprocessor file will be be saved to your AWS S3 folder for each model submission.
    :type public: Boolean
    :param public_private_split: [REQUIRED] Defaults to 0. Proportion of test data that is allocated to private hold-out set.
    :type public_private_split: Float between 0 and 1.
    :param onnx_timeout:  Time in seconds after which ONNX conversion should be interrupted.
            		 Set to False if you want to force ONNX conversion.
    :type onnx_timeout: int
    :param model_input: Required only when framework="pytorch" 
            	       One example of X training data in correct format.
    :type model_input: array_like 
    :param example_data: Example of X data that will be shown on the online Playground page. if no example data is submitted, certain functionalities may be limited, including the deployment of live prediction APIs. Example data can be updated at a later stage, using the update_example_data() method.` For tabular data - provide pandas DataFrame in same structure expected by preprocessor function. Other data types - absolute path to folder containing example data (first five files with relevant file extensions will be accepted). 
    :type example_data: pandas DataFrame (for tabular & text data) OR filepath as string (image, audio, video data)

    :return: Model version if the model is submitted sucessfully
             error  if there is any error while submitting models

.. _update_runtime_model:

update_runtime_model
--------------------

Updates the prediction API behind the Model Playground with a new model from the leaderboard and verifies Model Playground performance metrics.

.. py:function:: update_runtime_model(model_version=None)

   :param model_version: model version number from competition leaderboard
   :type model_version: integer
        
   :return: Success message when the model and preprocessor are updated successfully.

Example ::

	myplayground.update_runtime_model(model_version=1)


.. _update_example_datal:

update_example_data
-------------------

Updates example data associated with a model playground prediction API.

.. py:function:: update_example_data(example_data)

   :param example_data: Example of X data that will be shown on the online Playground page. If no example data is submitted, certain functionalities may be limited, including the deployment of live prediction APIs. Example data can be updated at a later stage, using the update_example_data() method.` For tabular data - provide pandas DataFrame in same structure expected by preprocessor function. Other data types - absolute path to folder containing example data (first five files with relevant file extensions will be accepted). 
   :type example_data: pandas DataFrame (for tabular & text data) OR filepath as string (image, audio, video data)
        
   :return: "Success" upon successful request

Example ::

	myplayground.update_example_data(example_tabular_df)



delete_deployment
-----------------

Delete all components of a Model Playground, including: AWS s3 bucket & contents, attached competitions, prediction REST API, and interactive Model Playground web dashboard.

.. py:function:: ModelPlayground.delete_deployment(playground_url=None)

   :param playground_url: API URL that the user wishes to delete. WARNING: User must own an API in order to delete it.
   :type playground_url: string

   :return: Success message when deployment is deleted.

Example ::

	myplayground.delete_deployment()
