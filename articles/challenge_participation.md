---
title: Participating in a Challenge
layout: article
excerpt: Learn how to participate in challenges.
category: howto
---

<style>
#image {
    width: 100%;
}
#toobig {
    width: 45%;
}
</style>

This tutorial will teach you the steps of participating in a challenge.

## Challenge Registration
If you do not have a Synapse account, please go to our [getting started guide](http://docs.synapse.org/articles/getting_started.html#becoming-a-certified-user) to become a certified Synapse user. Participants **must** be registered for the challenge if they want to submit and participate.  The registration button can be found on the home page or `How to Participate` page for every challenge.  *In order to be fully registered for any challenge, you must (1) have a Synapse account and become a [certified user](http://docs.synapse.org/articles/getting_started.html#becoming-a-certified-user); (2) agree to the DREAM Rules of participation, and (3) agree to the Terms of Use to work with the Challenge data.*


## Join or Create a Team
We encourage you to form a team with other participants for the challenge. You can either join a team or create your own team of collaborators. See instructions on how to form a team [here](http://docs.synapse.org/articles/teams.html).  It is important to note that you **cannot** be on more than one team.  Once you have submitted as a team or individual, you will not be able to submit as another team.  If you decide to be part of a team, please register your team to the challenge - there will be a place to do this in every challenge wiki.

## Accessing Challenge Data
The data stored on the challenge Synapse site can be accessed using the Synapse website or programmatically using the Synapse R or Python clients. Instructions on using Synapse are provided in the [Synapse User Guide](http://docs.synapse.org/).  File descriptions are provided on the Data Description page in each challenge wiki.


## Run your Algorithms and Submit to the Challenge

There are multiple ways you can submit to a challenge queue by using the R, python or web client.  The most conventional way is to submit through the web.  All submissions must be uploaded onto a private synapse page.  Follow instructions [here](http://docs.synapse.org/articles/getting_started.html#project-and-data-management-on-synapse) on how to upload to a project. Most challenge queues will be labeled by `challengename-subchallenge#` as a challenge may have different questions that it may want you to answer.

In the R and python examples, you will have to know the evaluation id of the subchallenge you are trying to submit to.  The examples below will show you the process of uploading a file to an example project then submitting that file to the challenge. The submission function takes two optional parameters: name and team.  Name can be provided to serve as a custom name of the submission, if name isn't provided the name of the entity being submitted will be used.  Teams can optionally be provided to give credit to members of the team that contributed to the submission. 

{% tabs %}
	{% tab Python %}
		{% highlight python %}
import synapseclient
from synapseclient import File
syn = synapseclient.login()
mySubmission = File("/path/to/submission.csv",parent = "syn12345")
mySub = syn.store(mySubmission)
submission = syn.submit(evaluationId, mySub, name='Our Final Answer', team='Blue Team')
		{% endhighlight %}
	{% endtab %}

	{% tab R %}
		{% highlight r %}
library(synapseClient)
mySubmission <- File("/path/to/submission.csv",parentId="syn12345")
mySub <- synStore(mySubmission)
submission <- submit(evaluation = "evaluationId", entity = mySub, submissionName='Our Final Answer', teamName='Blue Team') #The evaluationId HAS to be a string here or there will be an error
		{%endhighlight %}
	{% endtab %}

	{% tab Web %}
Navigate to an uploaded file in synapse and click on tools and click submit to challenge.

<img id="image" src="/assets/images/howtosubmit.png">
After doing so, pick the challenge you want to submit to, in this case (My Example Challenge). Click Next and follow the steps to complete your submission.

<img id="toobig" src="/assets/images/submitToChallenge.png">
	{% endtab %}

{% endtabs %}

## Share Ideas and Ask Questions

Every challenge has a discussion forum for participants (the 'Discussion' tab on the Challenge Project page).  The forum is a space for participants to ask any questions and raise ideas.  

Instructions on how to use the discussion forum can be found [here](http://docs.synapse.org/articles/discussion.html)
