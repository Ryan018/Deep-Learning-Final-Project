# Deep-Learning-Final-Project
Final Project Repo for CSE 490g1
Ryan Whitaker, Tadeusz Pforte

# Abstract
- in a paragraph or two, summarize the project

We are trying to see if we can train a model to predict if a Reddit post will be successful (ie. have a large karma score) by training our model on the title text, subreddit, time posted, and karma of posts across the prior "default" subreddits. In order to do so we wanted a binary classifier to assign a label of successful or unsuccessful to text titles. Using a pre-trained NLP model with a fully connected layer, we trained the network on a dataset of Reddit posts from default subreddit communities from the past year obtained via Reddit PushShift.

<Model Resultas>

# Problem statement 
- what are you trying to solve/do

We are trying to see if we can train a model to predict if a Reddit post will be successful where success is measured as having a karma score greater than 100.

# Related work 
- what papers/ideas inspired you, what datasets did you use, etc

We were inspired from the work in homework 2 using RNNs to train our model on a corpus and generate text. We wanted to see if it would be possible to analyze Reddit posts rather than a standard corpus as well as apply some form of linear/binary predictor. We used the Reddit PushShift archive which contains an archive of all Reddit posts in order to bypass the Reddit API restrictions for collecting large datasets. We used Google's pre-trained BERT model which is a masked language model using next sentence prediction in order to have an english language basis for analyzing post text. 

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

Using the Reddit PushShift archive, we downloaded data from the previous year across these default subreddits with posts of score (karma) greater than 10. This was to filter out spam posts yet keep ones that did poorly for purpose of training. This data however was not complete and was missing many posts upon closer inspection due to PushShift's API being partially down however, we still were able to download 173,627 posts with only 2 posts containing null data that were then filtered out. From the post metadata we decided to filter out only the title text, score (karma), subreddit, and post time to obtain the most relevant features for our question and reduce the file size dramatically. 

Originally we wanted to create a model that was able to predict an exact karma score for a given Reddit post as a regression task however, this proved to be extremely innacurate upon first attempts and our problem statement seemed suited better for a classification task of assigning a successfull/unsuccessful label. 


Network Architecture: 

We used Google's pre-trained BERT NLP model and applied a fully connected layer afterward. Our network was trained over 4 epochs.

# Experiments/evaluation 
- how are you evaluating your results

Cross Entropy Loss on predictors
# Results 
- How well did you do

Inconclusive
# Examples 
- images/text/live demo, anything to show off your work
# Video 
- a 2-3 minute long video where you explain your project and the above information
