# Deploying Warm Transfers in AWS Connect
The following document outlines how to deploy warm transfers from an Amazon Connect flow to an AI Agent. 

## Step 1
First, you will need to download a .zip containing the lambda needed to deploy on AWS. You can download that [here](https://customersuccesspublic.s3.us-east-1.amazonaws.com/lambda_package.zip).

## Step 2

Open AWS and navigate to the Lambda console (by typing in Lambda in the navigation console)

![AWS Lambda Console](/public/lambda_search.png)

## Step 3

Next, you will need to navigate to the top right corner and click "Create Function"

![Create Function](/public/create_function.png)

## Step 4

After clicking create function, you will see this page open up. Stay on "Author from scratch", give you function a title (in this case, we will call it ai_agent_warm_transfer, but it doesn't matter the title, just remember the name for use in the future.)

Also, ensure you are using a Python 3.13 runtime.

![Create](/public/settings.png)

Then click "Create function" on the bottom right corner.

## Step 5

You will be brought to this screen, and you will need to upload a the zip file you downloaded at the beginning. Click "Upload from":

![Create](/public/console12.png)

And choose ".zip file":
![zip](/public/zip.png)

and upload the zip file you downloaded at the beginning of the tutorial. Hit save.

And choose ".zip file". Upload the zip file you downloaded previously and click save.

## Step 6

Scroll down and navigate to "Runtime settings". Click Edit: 
![runtime](/public/runtime_settings.png)

Change the "Handler" name to handler.lambda_handler

![handler](/public/handler.png)

## Step 7

Navigate to the Configuration tab and go down to Environment Variables on the left column: 
![handler](/public/environment_variables.png)

Click Edit --> Add environment variable and add the Brainbase API Key we will have provided you:
![handler](/public/api_key.png)


Great! Now your lambda is all set up and ready to go. 

Ok - now navigate to Amazon Connect, and click on the instance alias for your Amazon Connect deployment.

Once here, navigate down to Flows on the left hand column: 
![handler](/public/left.png)

Under flows, scroll down to AWS Lambda, and select the lambda function you just deployed from the lambda function dropdown. 

Then click Add Lambda Function. 

Great! Now we are ready to go into our Amazon Connect instance. Navigate to your Flows page inside of Amazon connect and select the flow you would like to add the warm transfer to. In this case, I will be adding it to a new flow: 
![handler](/public/new_flow.png)

In the Block Library on the left, search "lambda" and drag/drop the Invoke AWS Lambda function into the flow: 
![handler](/public/block.png)

Click the three dots on the upper right of the block, and click "edit settings": 
![handler](/public/three_dots.png)

First, set the Function ARN manually, and select the lambda function you added earlier: 
![handler](/public/manual.png)

Then, click Add Parameter under Function input parameters, and select Set JSON. In your implementation, you will have to set it dynamically depending on where you recieve these two data points, but the schema and key will be the same: 
![handler](/public/customData.png)

And then you're done! You can route this node to a transfer call node to our AI agent, and now our AI agent will have context about this caller. You can also add, in customData, any additional fields you would like the AI to know. 