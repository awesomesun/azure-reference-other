---
title: "Import Images | Microsoft Docs"
ms.custom: ""
ms.date: 02/21/2017
ms.reviewer: ""
ms.service: "machine-learning"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "reference"
ms.assetid: 893f8c57-1d36-456d-a47b-d29ae67f5d84
caps.latest.revision: 20
author: "jeannt"
ms.author: "jeannt"
manager: "jhubbard"
---
# Import Images
*Loads images from Azure BLOB Storage into a dataset*  
  
 Category: [OpenCV Library Modules](opencv-library-modules.md)  
  
##  <a name="Remarks"></a> Module Overview  
 You can use the [Import Images](import-images.md) module to get multiple images from Azure Blob storage and create an image dataset from them.  
  
 As you read each image from blob storage into your workspace using [Import Images](import-images.md), the image is represented as a series of numeric values for the red, green, and blue channels, together with the image file name.  A dataset of such images consist of multiple rows in a table, each with a different set of RGB values and corresponding image file names. You can then pass this dataset to the [Score Model](score-model.md) module, and connect a pre-trained image classification model to predict the image type.  
  
 You can import any kind of images used for machine learning. For instructions about how to prepare your images and connect to blob storage, see [How to Import Images](#HowToImportImages). For other limitations, including the types and size of images that can be processed, see the [Technical Notes](#bkmk_Notes) section.
 

  
## <a name="HowToImportImages"></a>How to Configure [Import Images](import-images.md)  
 This example assumes that you have uploaded multiple images to your account in Azure blob storage. The images are in a container designated for that purpose only.  As a rule, each image must be fairly small and have the same dimensions and color channels. 
 > [!IMPORTANT]
>  See the [Technical Notes](#bkmk_Notes) section for a detailed list of requirements that apply to images.   
  
1.  Add the [Import Images](import-images.md) module to your experiment.  
  
2.  Add the [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md) and the [Score Model](score-model.md) module.  
  
3.  Double-click the [Import Images](import-images.md) module and configure the location of the images, as well as the authentication method, private or public:  

    - If the image set is in a blob that has been configured for public access through [Shared Access Signatures](https://azure.microsoft.com/documentation/articles/storage-dotnet-shared-access-signature-part-1/)(SAS), type the URL to the container that holds the images.  

    - If the images are stored in a private account in Azure storage, select **Account**, and then type the account name as it appears in the management portal.  Then, paste in the primary or secondary account key.

    - For **Path to container**, type just the container name, and no other path elements.  
  
4.  Connect the output of [Import Images](import-images.md) to the [Score Model](score-model.md) module.  
  
5.  Run the experiment.  
  

### Results

Each row of the output dataset contains data from one image. The rows are sorted alphabetically by image name, and the columns contain the following information, in this order:  
  
-   The first column contains image names.
-   All other columns contain flattened data from the red, green, and blue color channels, in that order.
-   The transparency channel is ignored.  
  
> [!TIP]
>  Depending on the color depth of the image and the image format, there could be many thousands of columns for a single image.  
>   
>  Therefore, to view the results of the experiment, we recommend that you add the [Select Columns in Dataset](select-columns-in-dataset.md) module, and select only these columns:  
>   
>  -   Image Name  
> -   Scored Labels  
> -   Scored Probabilities  
  
##  <a name="bkmk_Notes"></a> Technical Notes  

This section contains details about how images are processed, as well as requirements for images.
 

### Supported Image Formats  

 [Import Images](import-images.md) determines the type of an image by reading the first few bytes of the content, not by the file extension. Based on that information, it determines whether the image is one of the supported image formats. 
   
+ Windows bitmap files: *.bmp, \*.dib  
+ JPEG files: *.jpeg, \*.jpg, \*.jpe  
+ JPEG 2000 files: *.jp2  
+ Portable Network Graphics: *.png  
+ Portable image format: *.pbm, \*.pgm, \*.ppm  
+ Sun Raster: *.sr, \*.ras  
+ TIFF files: *.tiff, \*.tif  
  
### Image Requirements
 
  There are strict requirements that apply to images processed by the [Import Images](import-images.md) module:  
  
+ All images must be the same shape.  
  
+ All images must have the same color channels. For example, you cannot mix grayscale images with RBG images.  
  
+ There is a limit of 65536 pixels per image. However, the number of images is not limited.  
  
+ If you specify a blob container as the source, the container must not contain other types of data. Ensure that the container contains only images before running the module.  
  
### Other Restrictions
  
+ If you intend to use the [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md) module, be aware that it currently supports only recognition of faces in frontal view; other image classifiers are not yet available.  
  
+ You cannot use image datasets with these modules:  [Train](machine-learning-train.md) and [Cross-Validate Model](cross-validate-model.md). 
  
 
  
##  <a name="parameters"></a> Module Parameters  
  
|Name|Range|Type|Default|Description|  
|----------|-----------|----------|-------------|-----------------|  
|Please specify authentication type|List|AuthenticationType|Account|Public or Shared Access Signature (SAS) URI or user credentials|  
|URI|Any|String|none|Uniform Resource Identifier with SAS or public access|  
|Account name|Any|String|none|Name of the Azure Storage account|  
|Account key|Any|SecureString|none|Key associated with the Azure Storage account|  
|Path to container, directory or blob|Any|String|none|Path to blob or name of table|  
  
##  <a name="Outputs"></a> Output  
  
|Name|Type|Description|  
|----------|----------|-----------------|  
|Results dataset|[Data Table](data-table.md)|Dataset with downloaded images|  
  
##  <a name="exceptions"></a> Exceptions  
 For a list of all error codes, see [Module Error Codes](machine-learning-module-error-codes.md).  
  
|Exception|Description|  
|---------------|-----------------|  
|[Error 0003](errors/error-0003.md)|Exception occurs if one or more inputs are null or empty.|  
|[Error 0029](errors/error-0029.md)|Exception occurs when invalid URI is passed.|  
|[Error 0009](errors/error-0009.md)|Exception occurs if the Azure storage account name or container name is specified incorrectly.|  
|[Error 0015](errors/error-0015.md)|Exception occurs if the database connection has failed.|  
|[Error 0030](errors/error-0030.md)|Exception occurs when it is not possible to download a file.|  
|[Error 0049](errors/error-0049.md)|Exception occurs when it is not possible to parse a file.|  
|[Error 0048](errors/error-0048.md)|Exception occurs when it is not possible to open a file.|  
  
## See Also  
 [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md)   
 [A-Z Module List](a-z-module-list.md)