# decades_classifier
Basic transfer learning image classifier, using pre-trained ImageNet model to classify yearbook faces into decades.

Most of the code here is directly from the [TensorFlow tutorial](https://www.tensorflow.org/tutorials/image_retraining), I only made minor edits to their scripts, to instead of recategorizing Inception V3 (the neural net) to catagorize flowers, instead catagorize our face images. The major changes that had to be made were that our images are .png files and not the .jpg the script expects - this led to several errors that I had to install and run ImageMagick (command line image editor tool) to fix.

A step by step on how this script was run is found in "procedure.txt".

My first iteration using this model was running on completely default settings, it took roughly 30 minutes and yielded a final test accuracy of ~59%. Through a 4x increase in the number of steps to 16,000 and setting the flag --validation_batch_size=-1 alone, I managed to increase this final test accuracy to 67.7% (run time was less than 1 hour thanks to caching). This is still not great, however, despite tweaking several other parameters, there was no significant increase in test accuracy worth the increase in time. I also decided to stop at 16000 steps, as I tried 6000 and 8000 steps and the increase yielded by these increases was not worth the computing time (from 64% -> 68%), looking at the tensorboard plots below, we can see that the validation accuracy and cross-entropy curves is starting to plateau. Had this project been an actual research task, we could still probbaly increase the accuracy further (>70%) by further increasing number of steps, as we can see the cross entropy has not completely reached a plateau quite yet.

![Alt text](result_images/accuracy_1.png?raw=true "Accuracy")
![Alt text](result_images/cross_entropy_1.png?raw=true "Cross Entropy")

To try increase accuracy I tried other models such as facenet, which i reasoned would yield better results, moving from an ImageNet classifier trained on objects such as bananas and school buses to a classifier specifically built for faces. However, after implementing and running a facenet classifier on our dataset of images, the resulting test accuracy actually was far lower, at around ~50% only, and had to be run overnight. I hypothesize that there is an insignificant amount of facial differences between images across decades, particularly between decades right before or after one another i.e. 1930 and 1940 or 2000 and 2010. Where ImageNet does better, is it looks at patterns outside the faces i.e. hair and backgrounds, which are more distinctive features for each decade. Thus the original ImageNet classifier (Inception V3) script has been submitted as the best result.

Shown below are screenshots of the results of this classification on the test set, showing examples of some misclassified images. Here we can see that my hypothesis was somewhat correct, as most of the classifications are off by only 1 decade, either up or down. This is a fault in the way we catagorized the data to begin with, as there is more of a substantial difference between an image in 1940 and 1949, than between 1939 and 1940, and yet the latter is catagorized the same, while the former is not.

![Alt text](result_images/ex_res_1.png?raw=true)
![Alt text](result_images/ex_res_2.png?raw=true)
![Alt text](result_images/ex_res_3.png?raw=true)
