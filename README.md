# Medical Document Classifier
## Project Organization Summary
    ├── README.md
    ├── requirements.txt
    ├── LICENSE.txt
    ├── .gitignore
    ├── data
    │   ├── processed
    │   │   ├── test
    |   |   │   ├── communication
    |   |   │   ├── fax
    |   |   │   ├── medical_image
    |   |   │   ├── other
    |   |   │   └── requisition_results
    │   │   ├── train
    |   |   │   ├── communication
    |   |   │   ├── fax
    |   |   │   ├── medical_image
    |   |   │   ├── other
    |   |   │   └── requisition_results
    │   │   └── validate
    |   |       ├── communication
    |   |       ├── fax
    |   |       ├── medical_image
    |   |       ├── other
    |   |       └── requisition_results
    │   └── raw
    │       ├── test
    |       |   ├── communication
    |       |   ├── fax
    |       |   ├── medical_image
    |       |   ├── other
    |       |   └── requisition_results
    │       ├── train
    |       |   ├── communication
    |       |   ├── fax
    |       |   ├── medical_image
    |       |   ├── other
    |       |   └── requisition_results
    │       └── validate
    |           ├── communication
    |           ├── fax
    |           ├── medical_image
    |           ├── other
    |           └── requisition_results
    ├── models
    └── notebooks
        ├── model_training_overview.ipynb
        └── lightning_logs
            └── version_0
                ├── hparams.yaml
                └── metrics.csv


## Description
This notebook outlines the process to fine-tune a deep learning neural network that classifies medical documents into one of five categories. The end goal was to incorporate a document classifying model into a data processing pipeline where the documents could be further processed based on their classification. The five separate categories were as follows:
- communication: Documents that communicate information to subjects (e.g. instructions for colonscopy) contina limited research utility. Therefore, these documents were excluded from further processing and were excluded from the data registry.
- requisition_results: The results from lab tests were already captured in a structured manner. Processing the lab results did add any additional information; therefore, they were expected to not be processed any further.
- fax: Since the final model only evaluates the first page of a document, if it is determined that the first page of the document is a fax cover sheet, our pipeline will look at the second page to determine how the document is processed.
- medical_image: It was anticipated that there would be requests to access medical images in the near future; therefore, we need to identify the medical images to segregate them for further use.
- other: All other documents were considered relevant and would be further processed including extracting the data using optical character recognition and natural language processing.

This project uses Microsoft's LayoutLM model (HuggingFace implementation) to classify the documents and is influenced by this [Venelin Valkov blog](https://www.mlexpert.io/blog/document-classification-with-layoutlmv3). Unlike a traditional convolution neural network, this model incorporates the text elements in the document and the spatial relationship between the text elements. This is in contrast to traditional convolution neural networks which ignores the text elements. Because of the ability to incorporoate text, LayoutLM can produce superior results for document classification than a CNN.

The LayoutLM model was obtained from Hugging Face and the fine-tuning of the model was done using PyTorch/PyTorch Lightning.

The resulting model metrics are presented in the table below.

|          Label          | Train Accuracy | Validation Accuracy | Test Accuracy |
|-------------------------|----------------|---------------------|---------------|
| Document Classification |  99%        | 88%              | 96%        |


## Technologies
Project is created with:
* Python
    * PyTorch/PyTorch Lightning
* LayoutLMv3 (via Hugging Face transformers)

## License
[MIT](LICENSE.txt)

## References
* [Venelin Valkov Blog](https://www.mlexpert.io/blog/document-classification-with-layoutlmv3)

## Acknowledgements
This project would not be possible if not for the support from the Ontario Best Practices Research Initiative Inflammatory Bowel Disease (OBRI-IBD) registry.