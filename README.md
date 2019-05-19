# Computer-Vision-Weightlifting-Coach

Download `pose_iter_160000.caffemodel` online (http://posefs1.perception.cs.cmu.edu/OpenPose/models/pose/mpi/pose_iter_160000.caffemodel) and put it in `pose/mpi/pose_iter_160000.caffemodel`

Run the jupyter notebook and enjoy! Below is an analysis on how this project was created.

![deadliftgif](https://github.com/SravB/Computer-Vision-Weightlifting-Coach/blob/master/anim.gif)

This project analyzes videos/images of the deadlift (one of the most fundamental weightlifting exercises) and scores the posture of the person performing the deadlift from a range of 0 to 1. 

This process can be simplified into a few main parts:
 1. Analyze videos/gifs frame by frame (for high fps videos we can analyze every X frames since frames will be very repititive and tune X accordingly to satisfy our needs). We will use the OpenCV object `VideoWriter`.
 2. For each frame we score the exercise posture.
 3. First we feed our image into an open source OpenPose keypoint detection model for human pose detection (this model has been pre-trained with ~40 000 examples using the MPII dataset). This gives us locations of specific joints (right shoulder, left elbow, etc) and then using the locations of these joints we can feed this data to a classification model to predict whether the performer of the exercise is in correct posture or not.
 
 Some examples below from images at this stage of our pipeline:
 
![deadlift](https://github.com/SravB/Computer-Vision-Weightlifting-Coach/blob/master/deadlift_example.jpg)
![deadlift2](https://github.com/SravB/Computer-Vision-Weightlifting-Coach/blob/master/deadlift_example2.jpg)

 4. Our classification step involves using a machine learning model to classify the posture of the individual performing the deadlift. Various models were experimented with such as Random Forest, Elastic Net, Lasso, Ridge, and Logistic Regression (and ensemble learning methods) but Ridge seemed to perform best on the given dataset.
 5. The final step involves taking each posture score and printing this information onto the frame of the video (along with the time taken to classify the frame) and write each frame to our output `VideoWriter` object. Once all desired frames have been scored, we release our `VideoWriter` object.

## Next Steps
Overall we are able to analyze our deadlifting videos with some help with computer vision. The next step in this project would be expanding the data set for posture classification (step 4). The main challenge is finding a large enough data set to build a model that can generalize well and avoid overfitting to the small data set. However if a sufficient amount of exercise posture data is aggregated, our model can become extremely useful for fitness enthusiasts.

Built using pre-trained weights for OpenPose keypoint detection using the MPII pose estimation dataset (see https://github.com/spmallick/learnopencv for OpenPose example and other computer vision examples), Python, OpenCV, Scikit-Learn, Jupyter Notebook.
