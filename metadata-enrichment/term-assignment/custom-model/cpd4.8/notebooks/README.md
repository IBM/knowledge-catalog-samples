# Assign terms based on a custom ML model

## Overview

This notebook shows how to use Watson Machine Learning and Watson Knowledge Catalog APIs of your Cloud Pak for Data instance to create a scikit-learn model and scoring function in support of automatic term assignment.
Metadata Enrichments can then be configured to use the model instead of the built-in Machine Learning model.

The steps and sample code in the notebook show how to complete the following tasks:

 - Create a model for automatic term assignment
 - Train the model based on the terms in the Watson Knowledge Catalog glossary and on the data assets and columns in a catalog or project in Watson Knowledge Catalog.
 - Create a custom scoring function in support of term prediction.
 - Configure a Watson Knowledge Catalog project such that metadata enrichment on this project uses the model and custom scoring function for automatic term assignment.

### Things to consider

- This sample notebook will not create a model that is equivalent to the existing ML-based term assignment method. It is meant as guidance if you want to create your own custom term assignment based on Watson Machine Learning. A large portion of the code provides _templates_ for the actual logic to be used. The actual logic depends on the metadata to be processed.

  To give an example: The function `_convert_asset_name_to_sequence_of_words(..)`, which converts the name of a table or column to a list of words, passes its input back to the invoking function unchanged. A name might be split into words in various ways, for example, depending on the database management system (mixed case, length restrictions, and so on) or on conventions used when creating the database (use of prefixes, custom abbreviations, and so on). The function in the sample notebook does not implement a real conversion function but shows where you can plug in such preprocessing.

The classifier and scoring function used by this notebook are simple and straightforward implementations of some of scikit-learn capabilities. A full replacement of the built-in ML-based term assignment requires a more thorough approach that should include these steps:

- Careful evaluation of the available metadata and the methods that should be used to process it.
- Selection of test and training sets to evaluate the quality of predictions.
- Selection of an appropriate classifier supporting true multi-label classification. The sample notebook simply emulates this by using multi-class classification because this helps to keep the code simple and straightforward.
- Multiple iterations of evaluation and parameter refinement to find a good combination of the components involved (preprocessing, training, scoring).

Part 1 and 2 describe how to set up your Watson Knowledge Catalog deployment to assign terms based on the sample notebook. After you complete these steps successfully, consider refining and extending the sample notebook such that it works for your data and delivers the expected results.


## Part 1: Check the prerequisites for running the sample notebook

Complete the following steps in the Cloud Pak for Data user interface. Use the same user ID that you plan to use for the notebook.

1. Verify that your Cloud Pak for Data instance has a deployment space.

   1. From the main navigation menu, select **Deployments**.
   2. Go to the **Spaces** tab. 
   3. Verify that a deployment space is shown. If not, create one by clicking **New deployment space** and following the instructions on the screen.

2. Verify that your user has access to published business terms in Watson Knowledge Catalog.

   1. From the main navigation menu, select **Governance > Business terms**.
   2. Verify that terms are listed on the **Published** tab.

3. Select a catalog or project for training.

   As a minimum requirement for training, an empty catalog or project is sufficient. However, if the catalog or project is empty, the model is trained only on term metadata. Better results can be achieved if the catalog or project has data assets (and columns) that have terms assigned. Usually, the quality of the trained model increases with the number of data assets and columns that have terms assigned. Note that by default the following applies to projects: Automatically assigned terms are only considered for training if the respective data asset or column was reviewed. This is the recommended setting because it guarantees some degree of curation. However, you can change that behavior by setting the training parameter `reviewed_columns_only` to False.

4. Select or create a project for testing the prediction function.

   This project must contain at least one metadata enrichment asset to be configured for term prediction.


## <a id="p2">Part 2:</a> Load and run the sample notebook 

Make sure you downloaded the sample notebook file [`term_prediction_model.ipynb`](term_prediction_model.ipynb) from the repository.

Complete the following steps in the Cloud Pak for Data user interface. Use the same user ID that you used for checking the prerequisites.

**Tip:** You might want to create a dedicated (empty) project for this purpose.

To load and run the sample notebook with Watson Studio:

1. Add an asset of type **Jupyter notebook editor** to a project. 
2. Select **From file** and enter a name for the notebook. If you don't specify a name, the name of the notebook file is used. 
3. Add the notebook. You can drag it to the **Notebook file** box, or you can click the **Drag and drop files or upload** and browse to the notebook. 
4. Click **Create**.

When the kernel is started, you can, in principle, select **Cell > Run All** from the menu to run all steps on the notebook. However, it's strongly recommended to carefully read the comments, run the cells, and review their output step by step rather than running them all at once.

Though not technically required, it's recommended to run also the sections marked as optional.

When all steps of the notebook are completed, you can configure and run metadata enrichment in a project and check the results of your custom ML-based term assignment. Due to the way in which confidences are computed, the values might be small compared to the results of the other term assignment methods. Therefore, terms might be suggested because their confidence is lower than the threshold although you would expect them to be assigned.


## References

- Watson Data API: https://cloud.ibm.com/apidocs/watson-data-api-cpd
- Watson Machine Learning API: https://cloud.ibm.com/apidocs/machine-learning
- Watson Machine Learning Python Client API: https://wml-api-pyclient-dev-v4.mybluemix.net/
- scikit-learn: https://scikit-learn.org/
