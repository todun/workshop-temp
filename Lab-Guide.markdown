<h1>Zombie Microservices Workshop: Lab Guide</h1>

<h3>SMS Integration with Twilio</h3>
<p>In this section, you’ll wire together Twilio with an existing API Gateway endpoint created in the CloudFormation stack, to bring SMS texting functionality into the Zombie Chat application.</p>

1. Sign up for a free trial Twilio account at https://www.twilio.com/try-twilio. ![test link to step 10](1)
2. Once you have created your account, login to the Twilio console and navigate to the **Get Started with Phone Numbers** page as shown below. ![Manage Twilio Phone Number](/Images/Twilio-Step2.png)
3. Select the red **Get your first Twilio phone number** button to assign a phone number to your account. We’re going to generate a 10-digit phone number in this lab, but a short-code would also work if preferred. This number should be enabled for voice and messaging by default.
4. Once you’ve received a phone number, navigate to the **Manage Numbers** page and click on your phone number, which will take you to the properties page for that number.
5. Scroll to the bottom of the properties page, to the messaging section. In the **Configure With** section, select the **URL** radio button option.
6. Now you’ll retrieve your **/mobile** API endpoint from API Gateway and provide it to Twilio to hook up to AWS. Open the AWS Management console in a new tab, and navigate to API Gateway, as illustrated below. Be sure to leave the Twilio tab open as you’ll need it again to finish setup. ![API Gateway in Management Console](/Images/Twilio-Step6.png)
7. In the API Gateway console, select your API, **Zombie Workshop API Gateway**. On the top navigation bar, under Resources, click Stages, shown highlighted in orange below. ![API Gateway Resources Page](/Images/Twilio-Step7.png)
8. On the Stages page, expand the Resources tree on the left pane and select **POST** in the **/mobile** resource. The mobile resource is the endpoint that CloudFormation created for sms functionality. You should see an **Invoke URL** displayed for your /mobile resource, as shown below. ![API Gateway Invoke URL](/Images/Twilio-Step8.png)
9. Copy the Invoke URL and return to Twilio. On the Twilio page you left open, paste the Invoke URL you copied from API Gateway into the **Request URL** field. Ensure that the request type is set to **HTTP POST**. This is illustrated below. ![Twilio Request URL](/Images/Twilio-Step9.png)
10. Finally, click **Save**. You are now ready to test out Twilio integration with your API. Send a text message to your Twilio phone number, you should receive a confirmation response text message and the message you sent should display in the web app chat room. You have successfully integrated Twilio text message functionality with API Gateway.
