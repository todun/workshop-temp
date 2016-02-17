<h1>Zombie Microservices Workshop: Lab Guide</h1>
<h2>All Labs must be performed in us-west-2</h2>
<h2>Lab Overview</h2>
  <ul>
    <li><b>Typing Indicator</b><br/>
        This exercise already has the UI and backend implemented, and focuses on how to setup the API gatway to provide a RESTful endpoint.</li>
    <li><b>SMS Integration with Twilio</b><br/>
        This exercise wires together Twilio to an existing API gateway stack.  It shows how you can leverage templates in API Gateway to transform form posted data into JSON format for the backend lambda function.</li>
    <li><b>Search over the chat messages</b><br/>
        This exercise adds an elasticsearch cluster, which is used to index chat messages streamed from a dynamodb table.</li>
    <li><b>Slack Integration</b><br/>
        This exercise integrates Slack into the chat application.</li>
    <li><b>Intel Edison Zombie Motion Sensor</b>
        This exercise integrates motion sensor detection of zombies to the chat system using an Intel Edison board and a Grove PIR Motion Sensor.</li>
  </ul>
<hr/>
<h3>1. Typing Indicator</h3>
<p>The typing indicator shows up in the web chat client.  It's a section above the post message input that shows when other survivors are typing.  The UI and backend lambda functions have been implemented, and this lab focuses on how to enable the feature in API gateway.</p>
<p> The application uses CORS in order to query the API gateway.  The lab will both wire up the backend lambda function as well as perform the necessary steps to enable CORS</p>

1. Select the API Gateway Service from the main console page
![API Gateway in Management Console](/Images/Typing-Step1.png)
2. Select the Zombie Workshop API Gateway
3. Go into the /zombie/talkers/GET method flow
![GET Method](/Images/Typing-Step3.png)
4. Select the Integration Request component in the flow
5. Under Integration Type, Select Lambda Function
6. Select the us-west-2 region
7. Select the <b><i>[CloudformationTemplateName]</i></b>-GetTalkersFromDynamoDB-<b><i>[XXXXXXXXXX]</i></b> Function
8. Select Save and Grant access for API Gateway to invoke the Lambda function.
9. Click the Method Response section of the Method Execution Flow
10. Add a 200 HTTP Status response
![Method Response](/Images/Typing-Step10.png)
11. Go to the /zombie/talkers/POST method
![POST Method](/Images/Typing-Step11.png)
12. Perform Steps 4-10, but instead select the <b><i>[CloudformationTemplateName]</i></b>-WriteTalkersToDynamoDB-<b><i>[XXXXXXXXXX]</i></b> Lambda Function
13. Go to the /zombie/talkers/OPTIONS method
14. Select the Method Response
15. Add a 200 method response
16. Go back to the OPTIONS method flow and select the Integration Response
17. Select the Integration Response
18. Add a new Integration response with a method response status of 200 (leaving the regex blank)
19. Select the /zombie/talkers resource
![talker resource](/Images/Typing-Step19.png)
20. Select "Enable CORS" in the top right
21. Select Enable and Yes to replace the existing values
![talker resource](/Images/Typing-Step21.png)
22. Select Deploy API <br/>
![talker resource](/Images/Typing-Step22.png)
23. Select the ZombieWorkshopStage deployment and hit the Deploy button. The typing indicator should now show when survivors are typing.<br/>
![talker resource](/Images/Typing-Done.png)

<hr/>
<h3>2. SMS Integration with Twilio</h3>
<p>In this section, you’ll wire together Twilio with an existing API Gateway endpoint created in the CloudFormation stack, to bring SMS texting functionality into the Zombie Chat application.</p>

1. Sign up for a free trial Twilio account at https://www.twilio.com/try-twilio.
2. Once you have created your account, login to the Twilio console and navigate to the **Get Started with Phone Numbers** page as shown below. ![Manage Twilio Phone Number](/Images/Twilio-Step2.png)
3. Select the red **Get your first Twilio phone number** button to assign a phone number to your account. We’re going to generate a 10-digit phone number in this lab, but a short-code would also work if preferred. This number should be enabled for voice and messaging by default.
4. Once you’ve received a phone number, navigate to the **Manage Numbers** page and click on your phone number, which will take you to the properties page for that number.
5. Scroll to the bottom of the properties page, to the messaging section. In the **Configure With** section, select the **URL** radio button option.
6. Now you’ll retrieve your **/mobile** API endpoint from API Gateway and provide it to Twilio to hook up to AWS. Open the AWS Management console in a new tab, and navigate to API Gateway, as illustrated below. Be sure to leave the Twilio tab open as you’ll need it again to finish setup. ![API Gateway in Management Console](/Images/Twilio-Step6.png)
7. In the API Gateway console, select your API, **Zombie Workshop API Gateway**. On the top navigation bar, under Resources, click Stages, shown highlighted in orange below. ![API Gateway Resources Page](/Images/Twilio-Step7.png)
8. On the Stages page, expand the Resources tree on the left pane and select **POST** in the **/mobile** resource. The mobile resource is the endpoint that CloudFormation created for sms functionality. You should see an **Invoke URL** displayed for your /mobile resource, as shown below. ![API Gateway Invoke URL](/Images/Twilio-Step8.png)
9. Copy the Invoke URL and return to Twilio. On the Twilio page you left open, paste the Invoke URL you copied from API Gateway into the **Request URL** field. Ensure that the request type is set to **HTTP POST**. This is illustrated below. ![Twilio Request URL](/Images/Twilio-Step9.png)
10. Finally, click **Save**. You are now ready to test out Twilio integration with your API. Send a text message to your Twilio phone number, you should receive a confirmation response text message and the message you sent should display in the web app chat room. You have successfully integrated Twilio text message functionality with API Gateway.

<hr/>
<h3>3. Search over the chat messages</h3>
1. Select the Amazon Elasticsearch from the main console page
2. Create a new Amazon Elasticsearch domain
3. Leave the default cluster settings
4. For access policy, select the allow access from one or more accounts and fill in the account ID
5. Save and Select Next to the domain review page
6. Select Confirm and Create
7. The creation of the ELasticsearch cluster takes approximately 10 minutes
8. Take note of the Endpoint once the cluster starts,  we'll need that for the Lambda function ![API Gateway Invoke URL](/Images/Search-Step8.png)
9. Go into the Lambda Service Page
10. Select Create a Lambda Function
11. Skip the Blueprint section by selecting the Skip button in the bottom right
12. Fill in ZombieWorkshopSearchIndexing
13. Paste in the code from the ZombieWorkshopSearchIndexing.js file
14. On line 7, replace ENDPOINT_HERE with the Elasticsearch endpoint created in step 8.  Make sure it starts with https://
15. Under the Role, create a new Dynamodb event stream role
16. Select Next and Create Function
17. Select the "Event Sources" tab for the new ZombieWorkshopSearchIndexing function
18. Select Add event source
19. Select the DynamoDB Event source type and the messages dynamodb table.  You can leave the rest the default
20. After creation, you should see an event source that looks like this 
![API Gateway Invoke URL](/Images/Search-Step20.png)
21. Now after you post messages, you can see them show up in the elasticsearch indexing 
![API Gateway Invoke URL](/Images/Search-Done.png)

<hr/>
<h3>4. Slack Integration</h3>
<hr/>

<h3>5. Motion Sensor Integration with Intel Edison and Grove</h3>
<p>If you wish to utilize the Zombie Sensor as a part of the workshop, this guide will walk you through the following:</p>
* Items required to create the physical Zombie sensor<br/>
* How to create the AWS backend (Simple Notification Service Topic) for the Zombie detector<br/>
* How to install the code in the repo onto the device<br/>

<b>Please note that this section requires purchasing equipment.</b>
####Items Required
1. One Intel® Edison and Grove IoT Starter Kit Powered by AWS. This can be purchased [here](http://www.amazon.com/gp/product/B0168KU5FK?*Version*=1&*entries*=0). <br/>
2. Within this start kit you will be using the following components for this exercise:<br/>
    1. Intel® Edison for Arduino<br/>
    2. Base Shield<br/>
    3. USB Cable; 480mm-Black x1<br/>
    4. USB Wall Power Supply x1<br/>
    5. Grove - PIR Motion Sensor

The application code is a very simple app that publishes a message to an Amazon Simple Notification Service (SNS) queue when motion is detected on the Grove PIR Motion Sensor. For the purpose of a workshop, this should be done only once in a central account by the workshop organiser, the topic will be made public so that the various teams are able to subscribe to this topic and make use of it during the workshop. 

```
{"message":"A Zombie has been detected in London!", "value":"1", 
"city":"London", "longtitude":"-0.127758", "lattitude":"51.507351"}
```

A simple workflow of this architecture is:

Intel Edison -> Public SNS topic in central account -> Your AWS Lambda functions subscribed to the topic.

####Creating the AWS Backend
<b>If you are following this guide during a workshop presented by AWS, please ignore the following steps 2-4. An SNS topic should already be configured for the workshop particants to consume messages from.</b>

1. Firstly, you will need to have an AWS Account. If you do not already have one, you can sign up [here](https://aws.amazon.com).
2. We will now create the SNS Topic. Navigate to the SNS product page within the AWS Management Console and click 'Topics' in the left hand menu. Then click on 'Create New Topic'. You will be presented with the following window. Fill in the fields with your desired values and click create topic.
![Create Topic Screenshot](Images/MotionSensor-createTopic.png)
3. You will now need to edit the topic polciy to permit any AWS account to subscribe lambda functions to your SNS topic. Check the check box next to your new topic, and then click Actions -> Edit topic policy. You need to configure these settings presented as per the below screenshot. Then click Update Policy. This step is what allows others (perhaps teammates working on this lab with you, to consume notifications from your SNS topic.
![Edit Topic Policy Screenshot](/Images/MotionSensor-createTopicPolicy.png)
4. You now have your central SNS topic configured and ready to use. Ensure that you make a note of the Topic ARN and region where you have created the topic, you will need it in some of the following steps.

####Installing the application on the Intel Edison
<b>If you are following this guide during a workshop presented by AWS, please ignore this section. An Intel Edison board should already be configured for the workshop particants to consume messages from.</b>

1. First, You will need to get your Edison board set up. You can find a getting started guide for this on the Intel site [here](https://software.intel.com/en-us/articles/assemble-intel-edison-on-the-arduino-board).   
Note that for the purpose of this tutorial, we will be writing our client code for the Edison in node.js and will therefore be using the Intel® XDK for IoT (referred to as 'XDK' from here on) as our IDE.
2. You will need to physically connect the Grove PIR Motion Sensor to pin D6 on the breakout board.  
3. Download all of the code from the 'zombieIntelEdisonCode' folder in this repository and store it in a folder locally on your machine. This simply consists of a main.js file (our application) and our package.json (our app dependencies).  
4. Navigate to the homepage in the XDK and start a new project.  
5. Choose to import an existing Node.js project and select the folder where you stored the code from this repository in the previous step.  
6. Give your project a name. We called ours <b>zombieSensor</b>.
7. You now need to edit the code in main.js to include your AWS credentials and the SNS topic that you have created. Firstly, we'll need some AWS credentials. 
8. You will need to create an IAM user with Access and Secret Access Keys for your Edison to publish messages to your SNS topic. There is a guide on how to create IAM users [here](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html), your IAM policy for the user should look like the following: 
    ```
    {
    "Version": "2012-10-17",
    
    "Statement": [
        {
        "Action": [
            "sns:Publish"
        ],
        "Effect": "Allow",
        "Resource": "ENTER YOUR SNS TOPIC ARN HERE"
        }
    ]
    }
    ```

9. Now let's add your credentials to the client side code. Edit the following line in main.js to include your user access keys and the region where you have set up your SNs topic.
```
AWS.config.update({accessKeyId: 'ENTER ACCESSKEY HERE', secretAccessKey: 'ENTER SECRET ACCESS KEY HERE', region: 'ENTER REGION HERE'});
```
10. Edit the following line in main.js to reflect the region in which you created the SNS topic.  
```
var sns = new AWS.SNS({region: 'ENTER REGION HERE'}); 
```  
11. Edit the following line in main.js to reflect the Amazon resource name (ARN) of the SNS topic that you created earlier.

```
TopicArn: "ENTER YOUR SNS TOPIC ARN HERE"
```  
12. You now need to connect the XDK to your Intel Edison device. There is a guide on the Intel site on how to do this [here](https://software.intel.com/en-us/getting-started-with-the-intel-xdk-iot-edition) under the 'Connect to your Intel® IoT Platform' section.  
13. You now need to build the app and push it to your device. Firstly hit the build/install icon, this looks like a hammer in the XDK. It may take a couple of minutes to install the required packages etc.
14. Once the app has been built succesfully, you can run the app by pressing the run icon, this looks like a circuit board with a green 'play' sign.
15. Your app should now be running on the Edison device and your messages being published to the SNS topic. you can now consume this topic and do something meaningful with the Zombie alerts. You can consume these messages using AWS Lambda. There is some documentation to get you started [here](http://docs.aws.amazon.com/sns/latest/dg/sns-lambda.html). Have fun!!



<hr/>