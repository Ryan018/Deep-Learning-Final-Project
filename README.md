# Deep-Learning-Final-Project
Final Project Repo for CSE 490g1

Ryan Whitaker, Tadeusz Pforte

# Abstract
- in a paragraph or two, summarize the project

We are trying to see if we can train a model to predict if a Reddit post will be successful (ie. have a large karma score) by training our model on the title text and karma of posts across the prior "default" subreddits. In order to do so we wanted to assign a label of karma range to text titles looking like <100 or >1,000. Using a pre-trained NLP model with a fully connected layer for binary classification, we trained the network on a dataset of Reddit posts from default subreddit communities from 2020 obtained via Reddit PushShift.

We found our original idea of predicting a karma score for given Reddit posts as a regression task was too challenging/ambitious and our data was hard to train on given the variable nature of Reddit posts across several subreddits. We pivtoed and refined our question to allow for classification problems whereby we tried several models that were able to train to >80% accuracy which is significant for the limitations of the data. Our data was also very large and the model took a long time to train so in order to speed up the process we used a buffer such that we were only using <20% of the total post data (in the order of tens of thousands posts). We finalized a model through trial and error, eventually evenly splitting the training data by class such that we produce a model incentivized to do more than be constant resulting in an accuracy greater than that of random classification.

# Problem statement 
- what are you trying to solve/do

We are trying to see if we can train a model to predict if a Reddit post will be successful where success is measured as having a karma score greater than some threshold x.

# Related work 
- what papers/ideas inspired you, what datasets did you use, etc

We were inspired from the work in homework 2 using RNNs to train our model on a corpus and generate text. We wanted to see if it would be possible to analyze Reddit posts rather than a standard corpus as well as apply some form of linear/binary predictor. We used the Reddit PushShift archive which contains an archive of all Reddit posts in order to bypass the Reddit API restrictions for collecting large datasets. We used Google's pre-trained BERT model which is a masked language model using next sentence prediction in order to have an english language basis for further training on post text. We found other people online successfully used BERT as a base for classification tasks with (we suspect) stronger correlations than our data.

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

We used Google's pre-trained BERT NLP model with a fully connected layer with softmax afterward for binary classification. BERT utilizes bidirectional transformer application with masked language modeling to better process sentence structure and flow. The base BERT model consists of 12 tranformer layers with hidden size of 768. Our final network was trained over 4 epochs with a learning rate of 5e-5 and 2 more with a learning rate 1e-5 for a total of 6 epochs.

# Experiments/evaluation 
- how are you evaluating your results

We used cross entropy loss on predictors to determine our loss at each batch/epoch and a binary logits calcualtion for validation/test accuracy. We additionally calculated F1 scores for our classifications.

One of our models trained was a binary classifier with two classes of <100 and >=100. This mopdel was trained on 20% of the data for 4 epochs with a learning rate of 5e-5. This model was designed to check if we could run a binary classification instead of a more complicated regression problem.

Another model we trained was a multiclass classifier that predicted posts to be within 5 classes of <100, <1,000, <10,000, <100,000, >=100,000. This model trained on 10% of the data across 4 epochs with a learning rate of 5e-5. This model more accurately invisioned our goal of replicating a karma predictor by having bins of log scale to classify posts.

To save time, both of the above models were trained simultaineously on different colab accounts to allow us to gather more models and testing in the same amount of time. As a result, our results for these models will look similar.

A third model we trained was another multiclass classifier that predicts posts to be within 3 classes of <=100, >100, >1000. This model trained on a new, balanced dataset where each class has an equal number of datapoints to train on. The model was trained for 4 epochs with a learning rate of 5e-5, and 2 more with a learning rate of 1e-5. This new data distribution was made in order to better balance the model and prevent it from guessing 0 for each class without actually learning the higher order data. The extra epochs were added to fine tune the model and check for improvement once it had converged on the larger learning rate.

# Results 
- How well did you do

(Disclaimer: The first two model results were trained concurently)

Our binary classifier achieved a final test accuracy of 84%. The F1 scores calculated were 0.93, 0.023. The graphs for these reuslts can be found below un Examples. Our accuracy decreased over time as the loss increased which tells us that this model was overfitting to the data. Our results show that our model was essentially predicting zero for all posts regardless. This is due to the skewed classifications in the data for which the majority of posts scored under 100 karma.

On our 5 classifier model, we achieved a final test accuracy of 83% with intermediate epoch accuracies of 87.4%, 86%, 83.7%, 84.7%. Our loss steadily decreased which showed us that our model was picking up on the data and demonstrated the power of the BERT model. You can see our loss and accuracy results in the figures below within Examples. The calculated F1 scores were 0.93, 0.014, 0.0, 0.0, 0.0. These results show us that our model was overfitting and in the end essentially predicted 0 every time. This could be due to the data having skewed classifications for which 87% of the data was class 1, with lower and lower percentages for the other 4 classes as rates of higher scoring posts drop off. As there was very little data of higher classes, it did not make sense for the model to give them importance.

In our final 3 classifier model, we reached a peak accuracy of 43% which is greater than that of random chance (33%). Our loss steadily decreased and accuracy incresed across the first 4 epochs however, the final 2 epochs only contributed to overfitting and reduced the final accuracy to 41%. These results show that our model was able to slightly learn and classify posts correctly at a higher rate than a random or uniform classifier. This improvement comes from the way we evenly split the train data into classes in order to remove any bias encountered in the previous models due to the sheer volume of low score posts. You can see the graphs in the below Examples section to see how the model improved in the first 4 epochs and quickly fell off in the final 2. 

If we were to create a new model or improve, we would wish to include more post data such as subreddit and time posted into our classification as these features play a large role in actual reddit post success. This is important as currently, taking a fairly uniform and proportional spread of posts across default subreddits will lead to great variance in successful title texts from quality to length. We considered our model a success enough to perform better than random chance alone, given the high variability and relatively low quality of the reddit data we have.

# Examples 
- images/text/live demo, anything to show off your work

Binary Classification Model Results: 

![binary class results step loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/images/binary_steploss.png)
![binary class results epoch loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/images/binary_epochloss.png)
![binary class results accuracy](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/images/binary_epochacc.png)

5 Class Multiclassification Model Results: 

![5 class results step loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/images/step%20loss.png)
![5 class results val loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/images/step%20val%20loss.png)
![5 class results val loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/images/accuracy.png)

3 Class Multiclass Model Results: 

![3 Class step loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/images/final%20step%20loss.png)
![3 Class val loss](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/images/final%20val%20loss.png)
![3 Class results accuracy](https://github.com/Ryan018/Deep-Learning-Final-Project/blob/main/images/final%20accuracy.png)

# Video 
- a 2-3 minute long video where you explain your project and the above information

![Final Project Video](https://youtu.be/L1LuH_1GOMU)
