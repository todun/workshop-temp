<h1>Zombie Microservices Workshop: Lab Guide</h1>

<h3>SMS Integration with Twilio</h3>
<p>In this section, you’ll wire together Twilio with an existing API Gateway endpoint created in the CloudFormation stack, to bring SMS texting functionality into the Zombie Chat application.</p>

1. Sign up for a free trial Twilio account at https://www.twilio.com/try-twilio.
2. Once you have created your account, login to the Twilio console and navigate to the **Get Started with Phone Numbers** page. ![Manage Twilio Phone Number](/Images/Twilio-Step2.png)
3. Select the red **Get your first Twilio phone number** button to assign a phone number to your account. We’re going to generate a 10-digit phone number in this lab, but a short-code would also work if preferred. This number should be enabled for voice and messaging by default.
4. Once you’ve received a phone number, navigate to the **Manage Numbers** page and click on your phone number, which will take you to the properties page for that number.
5. Scroll to the bottom of the properties page, to the messaging section. In the **Configure With** section, select the **URL** radio button option.


