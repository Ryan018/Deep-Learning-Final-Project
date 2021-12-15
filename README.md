# Deep-Learning-Final-Project
Final Project Repo for CSE 490g1

Ryan Whitaker, Tadeusz Pforte

# Abstract
- in a paragraph or two, summarize the project

We are trying to see if we can train a model to predict if a Reddit post will be successful (ie. have a large karma score) by training our model on the title text and karma of posts across the prior "default" subreddits. In order to do so we wanted a binary classifier to assign a label of successful or unsuccessful to text titles. Using a pre-trained NLP model with a fully connected layer for binary classification, we trained the network on a dataset of Reddit posts from default subreddit communities from 2020 obtained via Reddit PushShift.

We found our original idea of predicting a karma score for given Reddit posts was too challenging/ambitious and our data was hard to train on given the variable nature of Reddit posts across several subreddits. We refined our question to allow for a binary classification problem in which our model was able to trainn to 88% accuracy which is significant for the limitations of the data. Our data was also very large and the model took a long time to train so in order to speed up the process we used a buffer such that we were only using <20% of the total post data (x thousand posts).

# Problem statement 
- what are you trying to solve/do

We are trying to see if we can train a model to predict if a Reddit post will be successful where success is measured as having a karma score greater than 100.

# Related work 
- what papers/ideas inspired you, what datasets did you use, etc

We were inspired from the work in homework 2 using RNNs to train our model on a corpus and generate text. We wanted to see if it would be possible to analyze Reddit posts rather than a standard corpus as well as apply some form of linear/binary predictor. We used the Reddit PushShift archive which contains an archive of all Reddit posts in order to bypass the Reddit API restrictions for collecting large datasets. We used Google's pre-trained BERT model which is a masked language model using next sentence prediction in order to have an english language basis for further training on post text. 

# Methodology 
- what is your approach/solution/what did you do?

To meet our problem statement we wanted to use data that contained both successful and unsuccessful Reddit posts which meant that taking data from r/all or r/popular would not work as those contain only the popular/hot posts at the time. We instead opted to pull data from the prior default subreddits which would then contain both sucessfull and unsuccesful posts. Although default subreddits are no longer a Reddit feature currently, the most previous defauls were:

<table>
    <tr>
      <td>AskReddit</td>
      <td>announcements</td>
      <td>funny</td>
      <td>pics</td>
      <td>todayilearned</td>
      <td>science</td>
      <td>TwoXChromosomes</td>
    </tr>
      <td>IAmA</td>
      <td>blog</td>
      <td>videos</td>
      <td>worldnews</td>
      <td>gaming</td>
      <td>movies</td>
      <td>Music</td>
    </tr>
    <tr>
      <td>aww</td>
      <td>news</td>
      <td>gifs</td>
      <td>askscience</td>
      <td>explaintdkeimfive</td>
      <td>EarthPorn</td>
      <td>books</td>
    </tr>
    <tr>  
      <td>television</td>
      <td>tdfeProTips</td>
      <td>sports</td>
      <td>DIY</td>
      <td>Showerthoughts</td>
      <td>space</td>
      <td>Jokes</td>
    </tr>
    <tr>
      <td>tifu</td>
      <td>food</td>
      <td>photoshopbattles</td>
      <td>Art</td>
      <td>InternetIsBeautiful</td>
      <td>mildlyinteresting</td>
      <td>GetMotivated</td>
    </tr>
    <tr>
      <td>history</td>
      <td>nottheonion</td>
      <td>gadgets</td>
      <td>dataisbeautiful</td>
      <td>Futurology</td>
      <td>Documentaries</td>
      <td>tdstentothis</td>
    </tr>
    <tr>
      <td>personalfinance</td>
      <td>philosophy</td>
      <td>nosleep</td>
      <td>creepy</td>
      <td>OldSchoolCool</td>
      <td>UptdftingNews</td>
      <td>WritingPrompts</td>
    </tr>
  </table>

Using the Reddit PushShift archive, we downloaded data from 2020 across these default subreddits with posts of score (karma) greater than 10. This was to filter out spam posts yet keep ones that did poorly for purpose of training. This data however was not complete and was missing many posts upon closer inspection due to PushShift's API being partially down however, we still were able to download 280,509 posts with only 2 posts containing null data that were then filtered out. From the post metadata we decided to filter out only the title text, score (karma), subreddit, and post time to obtain the most relevant features for our question and reduce the file size dramatically. Additionally, we introduced a buffer to reduce the total number of posts to that we were training on ~<20% as our dataset was very large and took a long time to train (couple minutes per batch), time which we did not have freely available for testing and further iteration. 

Originally we wanted to create a model that was able to predict an exact karma score for a given Reddit post as a regression task however, this proved to be extremely innacurate upon first attempts and our problem statement seemed suited better for a classification task of assigning a successfull/unsuccessful label. 

Network Architecture: 

We used Google's pre-trained BERT NLP model with a fully connected layer with softmax afterward for binary classification. BERT utilizes bidirectional transformer application with masked language modeling to better process sentence structure and flow. The base BERT model consists of 12 tranformer layers with hidden size of 768. Our network was trained over 4 epochs with a learning rate of 2e-5.

# Experiments/evaluation 
- how are you evaluating your results

We used cross entropy loss on predictors to determine our loss at each batch/epoch and a binary logits calcualtion for validation/test accuracy. We additionally calculated F1 scores for our classifications.

One of our models trained was a multiclass classifier that predicted posts to be within 5 classes of <100, <1,000, <10,000, <100,000, >=100,000. This model trained on 10% of the data across 4 epochs with a learning rate of 5e-5. 

Another model we trained was a binary classifier 

# Results 
- How well did you do

We cahieved 88% test accuracy on trained model. This is significant given the poor quality of data that was trained on and shows how well the model can adapt to such. 

On our 5 classifier model, we achieved a final test accuracy of 83% with intermediate epoch accuracies of 87.4%, 86%, 83.7%, 84.7%. Our loss steadily decreased which showed us that our model was picking up on the data and demonstrated the power of the BERT model. You can see our loss and accuracy results in the figures below within Examples. The calculated F1 scores were 0.93, 0.014, 0.0, 0.0, 0.0. These results show us that our model was overtraining and in the end essentially predicted 0 every time. This could be due to the data having skewed classifications for which 87% of the data was class 1, with lower and lower percentages for the other 4 classes as rates of higher scoring posts drop off. As there was very little data of higher classes, it did not make sense for the model to give them importance.

# Examples 
- images/text/live demo, anything to show off your work

5 Class Multiclassification Model Results: 
![5 class results step loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/step%20loss.png)
![5 class results val loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/step%20val%20loss.png)
![5 class results val loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/accuracy.png)

# Video 
- a 2-3 minute long video where you explain your project and the above information
