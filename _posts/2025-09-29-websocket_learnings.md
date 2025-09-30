---
layout: single
title: "AWS Learnings"
excerpt: "This post was created in high school to document my self-guided learnings in AWS!"
---

| This post was created in high school to document my self-guided learnings in AWS! I later implemented these processes into my project (as I outlined in my plans).

# Service Descriptions:

## Lambda

### What is lambda?

AWS Lambda is essentially a service that allows you to create and run functions for your backend. It basically just runs code when called. Thats kinda the simple way to put it. The code is run on Amazon's computer infrastructure, and it automatically scales with use. 

### How will I use Lambda?

I'm planning on using Lambda to communicate between computers. I will either use a dynamo table to keep track of all of the players current positions or just send periodic updates to all of the players. The benefit of using Lambdas is that a call to adds something to a database can trigger other clients to pull from the database. That's why this alternative is better than what I've done in previous projects, as I just used continuous api calls to check if it had changed. 

## API Gateway

### What is API Gateway?

API Gateway is another AWS service that can be used in combination with many other services. It creates RESTful APIs that are HTTP based and enable stateless client-server communication. This is essentially a way of managing concurrent API calls.

### How will I use API Gateway>

I will be using it with AWS Lambda, though it can be used with many other AWS services. It will essentially act as a manager for my connecting and disconecting from the serverless websocket. 


# First Steps

The first thing I wanted to do was create a functional websocket on AWS using the method I described above. To do this I followed a tutorial. [Here](https://www.youtube.com/watch?v=FIrzkt7kH80) is the video I used, it is very concise and well made. For anyone watching the video, he left out one important thing. Your send message lambda function must also be given special permissions like the connect function, specifically it needs the "AmazonAPIGatwayInvokeFullAccess" policy. 

## The Process

1. The first thing I did was create the lambda functions. This part was not shown in the video, so I skipped ahead to where he showed the functions and I just copied them down. I used python 3.9 because that's what he used. Then I went back to the beginning of the video. 
2. The next thing I did was follow his guide pretty much exactly. If you would like to see how to do it, I would just recomend watching the guide (and leaving a like!). 

## What went wrong
- Permissions bug: I didn't know initially why I was getting the bug, but I read the error message and it said something about permission being denied. The video had already told us to add a permission to our connection lambda function, so I just assumed that might be the issue and added every permission related to APIGateways and Lambdas. After that I systematically removed permissions until I found the one permission that fixed my errors. I actually just got really lucky and my first guess was the correct one, so I didn't really have to test that much. That fixed that error.
- Testing: I was trying to test it with my friend on seperate computers and initially it worked fine. However when I tried to join it appeared to break for some reason. The only way to find someones connectionID for now was to go to the CloudWatch logs and find it in the statement that got printed. However when I looked, I didn't see a new connection. Then I checked the event my friend had logged and my connection outputs were in that same event. I wasn't expecting this, I thought it would create a seperate event for each instance it's called, but apparently it does not. 


### Calling a Lambda from a Lambda

This was something I needed to do after running into an error. Essentially, I wanted the connectionId to be sent back to the client as soon as they connect to the websocket. However when I tried to add this functionality into the connection lambda, it tried to send it before it had finished connecting and it didn't work the way I wanted it to. My solution was to just call a second lambda from within the first lambda. I found another [youtube video](https://www.youtube.com/watch?v=Lf98s3NczBE) about it and followed his step by step, including creating my own invoke lambda IAM role to give the lambda the right permissions. It worked first try. 

![]({{ site.baseurl }}/images/LambdaFlowchart.jpg)