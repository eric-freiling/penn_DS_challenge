# data-science-challenge

Welcome! The following challenge is to evaluate your strengths as a data scientist. The expectation is not that you would finish all of it, but represents all of the sorts of problems you might expect working on a typical problem. You should limit the exercise to no more than a few hours. When you're finished put your code in a VCS of your choice and share the link!

## Problem Statement

We are creating a machine learning algorithm that will predict the price of Bitcoin in USD in the next second based on the price from the last 60 seconds. The variable `price_high` is the target variable we wish to predict, our *Y*. The rest of the data can be used for your feature set, *X*. I have created a lookback function to expand the feature set and a min-max scaler to standardize the features. You can use this as your feature set, or modify it as you wish. The dataset consists of the first 10,000 seconds of Bitcoin data a single day, January 1, 2020 (until about 8:00 am). The data can be found in `bitcoin.csv`. We have a model in place, trained in `bitcoin-predictor.ipynb`. I started by using a deep neural network, because everyone keeps talking about how cool they are. Problem is, the model is not very good. Actually the model is completely awful and predicts `0` for everything. So, a lot of room for improvement!

The main challenge is this: can you make a model that actually works? You can use a neural network in tensorflow as I've done, or you can scrap it altogether and use regression, random forest, svm, anything you like. No model or package is off the table. Feel free to modify the jupyter notebook directly or put it in whatever workflow suits you best.

The main idea is to get a decent working model up and running quickly, but some other considerations you should think about are listed below. You don't have to answer all of the questions, but pick at least one or two and try to answer them (you can add your answers in this readme if you'd like).

* Does this data set even make sense? What are the limitations of this data set?
    * The first problem with this data set if the data leakage in it. The original code used the information about the current second in the feature set to make a prediciton. The second problem is that the resolution isn't practical. It would be better to make predictions for the next hour/day/week, etc, so that someone can have time to take action given the models prediction.
* Is the lookback window of 60 seconds helpful? What are its limitations? What other features would you want to see in this data set?
    * Again the time resolution isn't practical. Other features I would like to see would be from other data sets. Stock market price of gold etc.
* If you stuck with the neural network, what did you change to make it better? Did you change the architecture, did you change the optimizer? The learning rate? The activation function(s)? Why was the model stuck at `0` with an incredibly high root mean squared error?
    1. Changed the arrays to tensors
    2. Converted train and validation to datasets
    3. Set a random seed so we can have consistency with debugging and testing
    4. Added relu activations at each layer, other wise it's just a linear function
    5. Changed loss to MAE. I dont think in terms of values squared
    6. Reduced the learning rate
    7. Got rid of early stopping callback and replaced with checkpoint to save the model with the best validation
    8. Increased epochs due to smaller learning rate and model checkpoint
    9. The model was stuck at 0 with a high rmse because the inputs were not in the correct tensor format so nothing was happening in the network.
* If you used a different model, why'd you choose this model? What about it made it work for this problem? Is this model complex and if so, is the complexity necessary? Is it intuitive enough to explain it to a lay-person? What was your optimizing metric? What were the hyperparameters and why'd you choose them?
* Did you include any regularization strategies in your model? If so, why'd you choose the one you did?
    * I did not include any regularization strategies because I didnt see a problem with over fitting. I typically like to see if a model is capable of over fitting, then add reguralization techniques to improve the validation score. I would have used drop out layers if I thought it was neccessary. 
* Did you include visualizations? (everyone loves a good graphic)
    * Yes I did, I included plots of the time series and the predictions as well as a the training and validation loss curves
* How do we know the model is good? How understandable are the diagnostics? How will we know how good the model is predicting in production?
    * We know the model is "good" in the sense that it has a low loss in the training and validation sets. It would not be good in production because of reasons mentioned in the first question.
* If we see data for more than a single day's worth of prices, how do expect the model to perform? Will it generalize well to new data? Will retraining with this new data be an issue for this model?
    * If kept at a resolution of seconds, I don't think it will matter because it is only looking very locally. I dont think re-training will be an issue because it is fairly simple network.
* **What question would you ask of the data, or add to this analysis that I haven't thought of?**
    * I think it would be very powerful if we can find other data sets that are also good predictors of the bitcoin price. Also calculate average bitcoin price for a time interval, it might be more stable then high/low. Is it useful to incorporate/compare to other crypto currencies?


## Note on Dependencies

Feel free to install dependencies however you wish (docker, pyenv, conda, etc.). This model was trained in a conda virtual environment, and you can find all of the details in the `spec-file.txt` (the output of `conda list`) file or the explicit packages in `spec-file-explicit.txt` (the output of `conda list --explicit`). You may upgrade/downgrade dependencies as you see fit, as long as it doesn't effect the solution.
