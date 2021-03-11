## Alerts with Amazon EventBridge

**Introduction**
- EventBridge bridges your application based on events. You can plug your own AWS platform with REST API, any SaaS applications, and custom apps as event sources that publish events to an event bus. You can state a filtering rule to refine events and target events to different services and REST endpoint destinations.
- EventBridge reduces the process of constructing an event-driven platform. EventBridge, decouple the destination from the source of the event, and this way you can refine and notify directly to EventBridge. There is no configuration needed. 

**Tutorial**
- This tutorial, will leverage Amazon EventBridge to track and notify when an IAM policy is attached to an IAM user. The EventBridge Rule configured will track for a specific event name in CloudTrail, and will use an SNS message to notify regarding an event, when it occurs.

- Step 1: Deploy SNS and register an email-based Subscription 

    1. Navigate to Amazon SNS  

       <img src="images/image1.png" class="inline"/>

    2. On the right side provide the topic name and click Next step

       <img src="images/image2.png" class="inline"/>

    3. Provide SNS topic name and type as Standard, leave rest as default

        <img src="images/image3.png" class="inline"/>

    4. Scroll down the page and select Create topic

        <img src="images/image4.png" class="inline"/>

    5. Next let's register subscription under the SNS topic created above, Select Create Subscription

        <img src="images/image5.png" class="inline"/>

    6. Select Email as an option from Protocol drop-down list and provide your email address 

         <img src="images/image6.png" class="inline"/>

    7. Leave rest as default, scroll down and click Create Subscription

         <img src="images/image7.png" class="inline"/>

    8. Post that confirms your subscription via subscription email you would have received on the email id provided in Step 6

        <img src="images/image8.png" class="inline"/>

        <img src="images/image9.png" class="inline"/>


- Step 2: Deploy Amazon EventBridge and define the rule

    1. Navigate to Amazon EventBridge  

       <img src="images/image10.png" class="inline"/>

    2. Select Rules in the left pane or Select Create rule button on the right side from the Amazon EventBridge home screen

       <img src="images/image11.png" class="inline"/>

    3. Set up the Rule using the below settings
        - Enter a Name for the rule (e.g. AttachUserPolicy_Event)

            <img src="images/image12.png" class="inline"/>

        - Event Pattern:
            - Pre-defined pattern by service
            - Service provider: AWS
            - Service Name: IAM
            - Event Type: AWS API Call via CloudTrail
            - Specific operation(s): AttachUserPolicy

            <img src="images/image13.png" class="inline"/>

    4. Targets: SNS topic - Topic: demo-tutorial

       <img src="images/image14.png" class="inline"/>
    
    5. Click Create and you will find the rule configured.

       <img src="images/image15.png" class="inline"/>

       <img src="images/image16.png" class="inline"/>

- Step 3: Generate AttachUserPolicy Notification
    In this step we will create an IAM user and attach a user policy to this user to generate the notification

    1. Navigate to IAM console 

    <img src="images/image17.png" class="inline"/>
    
    2. Select User in the left pane

       <img src="images/image18.png" class="inline"/>

    3. Click on Add user 

    <img src="images/image19.png" class="inline"/>

    4. Provide the below details for the user:

        - User name: demo
        - Select AWS access type: AWS Management Console access
        - Console password: Autogeneratted password
        - Requrie password reset: Check the checkbox

    <img src="images/image20.png" class="inline"/>

    4. Select Next: Permissions

    5. On the Set permissions page, we will select:
        
        - Attach existing policies directly
        - Check the checkbox for the AdministratorAccess policy

    <img src="images/image21.png" class="inline"/>

    6. Select on Next: Tags.

    7. Select Next: Review on the Add tags page.
    
     <img src="images/image22.png" class="inline"/>

    8. Select Create user and Close

    <img src="images/image23.png" class="inline"/>

    After the user is registered and the policy is attached, the CloudWatch Event will be triggered and an e-mail will be sent to the e-mail address defined in the SNS topic.

    <img src="images/image24.png" class="inline"/>
