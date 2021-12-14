# Deep-Learning-Final-Project
Final Project Repo for CSE 490g1
Ryan Whitaker, Tadeusz Pforte

# Abstract
- in a paragraph or two, summarize the project


We are trying to see if we can train a model to predict if a Reddit post will be successful (ie. have a large karma score) by training our model on the title text, subreddit, time posted, and karma of posts across the prior "default" subreddits. 

# Problem statement 
- what are you trying to solve/do

We are trying to see if we can train a model to predict if a Reddit post will be successful (ie. have a large karma score).

# Related work 
- what papers/ideas inspired you, what datasets did you use, etc

We were inspired from the work in homework 2 using RNNs to train our model on a corpus and generate text. We wanted to see if it would be possible to analyze Reddit posts rather than a standard corpus. We used the Reddit PushShift archive which contains an archive of all Reddit posts in order to bypass the Reddit API restrictions for collecting large datasets. We used Google's pre-trained BERT model which is a masked language model using next sentence prediction in order to have an english language basis for analyzing post text. 

# Methodology 
- what is your approach/solution/what did you do?

To meet our problem statement we wanted to use data that contained both successful and unsuccessful Reddit posts which meant that taking data from r/all or r/popular would not work as those contain only the popular/hot posts at the time. We instead opted to pull data from the prior default subreddits which would then contain both sucessfull and unsuccesful posts. Although default subreddits are no longer a Reddit feature currently, the most previous defauls were 
<ul class="columns" data-columns="4">  
      <li>AskReddit</li>
      <li>announcements</li>
      <li>funny</li>
      <li>pics</li>
      <li>todayilearned</li>
      <li>science</li>
      <li>IAmA</li><li>blog</li>
      <li>videos</li>
      <li>worldnews</li>
      <li>gaming</li>
      <li>movies</li>
      <li>Music</li>
      <li>aww</li>
      <li>news</li>
      <li>gifs</li>
      <li>askscience</li>
      <li>explainlikeimfive</li>
      <li>EarthPorn</li>
      <li>books</li>
      <li>television</li>
      <li>LifeProTips</li>
      <li>sports</li>
      <li>DIY</li>
      <li>Showerthoughts</li>
      <li>space"</li>
      <li>Jokes</li>
      <li>tifu</li>
      <li>"food</li>
      <li>photoshopbattles</li>
      <li>Art</li>
      <li>InternetIsBeautiful</li>
      <li>mildlyinteresting</li>
      <li>GetMotivated</li>
      <li>history</li>
      <li>nottheonion</li>
      <li>gadgets</li>
      <li>dataisbeautiful</li>
      <li>Futurology</li>
      <li>Documentaries</li>
      <li>listentothis</li>
      <li>personalfinance</li>
      <li>philosophy</li>
      <li>nosleep</li>
      <li>creepy</li>
      <li>OldSchoolCool</li>
      <li>UpliftingNews</li>
      <li>WritingPrompts</li>
      <li>TwoXChromosomes"
</ul>

Using the Reddit PushShift archive, we downloaded data from the previous year across these default subreddits with posts of score (karma) greater than 10. This was to filter out spam posts yet keep ones that did poorly for purpose of training. This data however was not complete and was missing many posts upon closer inspection due to PushShift's API being partially down however, we still were able to download 173,627 posts with only 2 posts containing null data that were then filtered out. From the post metadata we decided to filter out only the title text, score (karma), subreddit, and post time to obtain the most relevant features for our question and reduce the file size dramatically. 

Network Architecture: 

We used Google's pre-trained BERT NLP model and applied a fully connected layer afterward. Our network was trained over 4 epochs.

# Experiments/evaluation 
- how are you evaluating your results
# Results 
- How well did you do
# Examples 
- images/text/live demo, anything to show off your work
# Video 
- a 2-3 minute long video where you explain your project and the above information
