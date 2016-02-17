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
<b>Please note that this section requires purchasing equipment.</b>
<b>Items Required</b><br/>
    * 1 Intel® Edison and Grove IoT Starter Kit Powered by AWS. This can be purchased [here](http://www.amazon.com/gp/product/B0168KU5FK?*Version*=1&*entries*=0). <br/>
    * Within this start kit you will be using the following components for this exercise:
        1. Intel® Edison for Arduino
        2. Base Shield
        3. USB Cable; 480mm-Black x1
        4. USB Wall Power Supply x1
        5. Grove - PIR Motion Sensor