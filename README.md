# CreateML
Create ML model for use in Core ML enabled applications
Create ML ( IOS 12 )

At WWDC 2018, Apple announced CoreML 2 and CreateML. 

- CoreML 2 is focused on optimizations on model size, model performance, and model flexibility. 
- CreateML is a new library designed for Swift playgrounds that allows data scientists to create models for iOS.


Use :
Create machine learning models for use in your app.

1. Use  with familiar tools like Swift & macOS playgrounds to create and train custom machine learning models on your Mac
2. Train models to perform tasks like :
- recognising images, 
- extracting meaning from text, 
- finding relationships between numerical values.

Advantages of create ML :
1. Create ML leverages the machine learning infrastructure built in to Apple products like Photos and Siri.
2. This means your image classification and natural language models are 
- smaller 
- take much less time to train.


TEXT CLASSIFICATION:

Use :
Train a machine learning model to classify natural language text.

Description :
You train a text classifier by showing it lots of examples of text you’ve already labeled—for example, movie reviews that you’ve already labeled as positive, negative, or neutral.
A text classifier is a machine learning model that’s been trained to recognize patterns in natural language text, like the sentiment expressed by a sentence.
￼

I.  PREPARE YOUR DATA


Step 1:
- Start by gathering textual data and importing it into an MLDataTable instance. 
- You can create a data table from JSON and CSV formats. 
- Or, if your textual data is in a collection of files, you can sort them into folders, using the folder names as labels

Eg: Consider the json file :

[
    {
        "text": "The movie was fantastic!",
        "label": "positive"
    }, {
        "text": "Very boring. Fell asleep.",
        "label": "negative"
    }, {
        "text": "It was just OK.",
        "label": "neutral"
    } ...
]

Each entry contains a pair of keys :
- text
- label
The values of those keys are the input samples used to train your model.


2.  PREPARE THE PLAYGROUND
In a macOS playground, create the data table using the init(contentsOf:options:) method of MLDataTable.
The resulting data table has two columns, named text and label, derived from the keys in the JSON file.

3.  PREPARE YOUR DATA FOR TRAINING AND EVALUATION
Use the randomSplit(by:seed:) method of MLDataTable to split your data into two tables
* Training data : The training data table contains the majority of your data.
* Testing data :  contains the remaining 10 to 20 percent.

4. CREATE AND TRAIN THE TEXT CLASSIFIER
Create an instance of MLTextClassifier with your training data table and the names of your columns.

VALIDATION:
During training, Create ML puts aside a small percentage of the training data to use for validating the model’s progress during the training phase.
The validation data allows the training process to gauge the model’s performance on examples the model hasn’t been trained on.

CALCULATE TRAINING AND VALIDATION ACCURACIES :
To see how accurately the model performed on the training and validation data, use the classificationError properties of the model’s trainingMetrics and validationMetrics properties.

5. EVALUATE THE TEXT CLASSIFIER:

Pass your testing data table to the evaluation(on:) method, which returns an MLClassifierMetrics instance.

CALCULATE EVALUATION ACCURACY :
To get the evaluation accuracy, use the classificationError property of the returned MLClassifierMetrics instance.


6. SAVE THE CORE ML MODEL 
- Use the write(to:metadata:) method to write the Core ML model file  to disk.
- Provide any information about the model, like its author, version, or description in an MLModelMetadata instance.

7. ADD THE MODEL TO YOUR EXISTING  CORE ML ENABLED APP
- With your app open in Xcode, drag the SentimentClassifier.mlmodel file into the navigation pane. 
- Xcode compiles the model and generates a SentimentClassifier class for use in your app. 
- Select the SentimentClassifier.mlmodel file in Xcode to view additional information about the model.

8. HOW TO USE?
- Create an NLModel in the Natural Language framework from the created classifier.
- Use predictedLabel(for:) to generate predictions on new text inputs.


IMAGE CLASSIFICATION:

Use :
Train a machine learning model to classify images.


Description :
You train an image classifier by showing it lots of examples of images you’ve already labeled. For example, you can train an image classifier to recognize safari animals by showing it a variety of photos of elephants, giraffes, lions, and so on.
When you give it an image, it responds with a label for that image.
￼


I.  PREPARE YOUR DATA


Step 1:
Start by preparing the data that you’ll use to train and evaluate the classifier. 
* Training data set :    Create a training data set from about 80% of the images you have for each label. 
* Testing data set :     Create a testing data set from the remaining 20% images. 
Make sure that any given image appears in only one of these two sets. ( a intersect b = {} )

Step 2:
Organise your data on disk to be compatible with one of the MLImageClassifier.DataSource types
Eg:
- create a folder called Training Data, 
- create another called Testing Data. 
- In each folder, create subfolders using your labels as names. 
- Then sort the images into the appropriate subfolders for each data set.


2.  SHOW AN IMAGE CLASSIFIER BUILDER IN PLAYGROUND

CreateMLUI
CreateMLUI is a framework just like CreateML but has a UI to it. 
( Import CreateMLUI to train the image classifier in the UI. For other Create ML tasks, import CreateML instead.)

- make a new Xcode playground with a macOS target
- create an MLImageClassifierBuilder instance
- show it in the live view
- Show the assistant editor in Xcode, and then run the playground.
       The live view presents an image classifier:


3.  TRAIN THE IMAGE CLASSIFIER
Take the Training Data folder from finder and drop the entire folder onto indicated location in the live view.
In the console, you’ll see the number of images processed in what time and how much percentage of your data was trained!
take around 30 seconds (depending on your device)

You’ll see a card with three labels:
-  Training
-  Validation
-  Evaluation

Training :  
Training refers to the percentage of training data Xcode was successfully able to train. This should read 100%.
While training, Xcode distributes the training data into 80-20
After training 80% of training data, Xcode runs the classifier on the remaining 20%.

Validation :
The percentage of training images the classifier was able to get right. 
Usually, this can vary because Xcode may not always split the same data. 

Evaluation :
Evaluation is empty because we did not give the classifier any testing data.



4. EVALUATE THE IMAGE CLASSIFIER
 Use the test data set that you created before you started training. Drag the Test Data folder into the live view
- The model processes all of the images, making predictions for each.
- As this is labeled data, the model can check its own predictions. 
- It then adds the overall evaluation accuracy as the final metric in the UI.

5. SAVE THE CORE ML MODEL
- Click on the arrow next to the Image Classifier title. 
- A dropdown menu should appear displaying all the metadata. 
- Change your metadata to how you would like it 
- save it to where you want to ( .mlmodel file is created at specified location )


6. ADD THE MODEL TO YOUR EXISTING  CORE ML ENABLED APP
- Open the sample code project in Xcode 
- Drag your model file into the navigation pane. 
- Once the model is part of the project, Xcode shows you the model metadata, along with other information, like the model class.

7. HOW TO USE?
    The models are interchangeable because they take an image as input and output a label. 
     let model = try VNCoreMLModel(for: MobileNet().model)
     Change it to  use the new model class instead




Things to remember:
* Use at least 10 images per label for the training set, but more is always better. 
* Also, balance the number of images for each label. For example, don’t use 10 images for Cheetah and 1000 images for Elephant.
* The images can be in any format whose uniform type identifier conforms to public.image. This includes common formats like JPEG and PNG
* The images don’t have to be the same size as each other, nor do they have to be any particular size, although it’s best to use images that are at least 299x299 pixels
* If possible, train with images collected in a way that’s similar to how images will be collected for prediction.
* Provide images with variety. For example, use images that show animals from many different angles and in different lighting conditions
