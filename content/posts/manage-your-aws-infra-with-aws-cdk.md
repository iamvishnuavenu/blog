---
title: "Manage Your Aws Infra With Aws Cdk"
date: 2023-02-22T22:01:40+05:30
draft: true
---

![alt text](/images/aws-cdk.webp "Title")

Recently, I started working on AWS CDK. CDK helps you to write Infrastructure Provisioning Code in your preferred language, it supports mostly all the popular languages Python, Java, and many more. AWS CDK will ultimately generate the Cloudformation template, so CDK will have all the shortcomings and advantages of AWS Cloudformation. I tried to create a simple old backend service that has the following AWS Resources:

1. Route53
2. ALB
3. ASG
4. EC2

Soon, I encountered some terminologies like Construct, Stack, and App. Let me try to explain you in simple words.

Construct is the first and main building block, it may denote a single resource ( e.g. ALB, EC2 ) or we can create our custom higher-level construct through Composition pattern which consists of multiple AWS resources acting as a single unit.
Stack: Stack consists of multiple constructs or some much Higher level constructs that will combine to create the whole stack.
App: Application is the main class where we initialize the Stack and kickstart the provisioning.
So, [here](https://github.com/iamvishnuavenu/sample-cdk-stack) is our simple Application Stack:

![alt text](/images/aws-cdk-2.webp "Overview")

Let’s code with CDK, for rushers please fetch the code from here.
Others can follow along with me. So I will show you how I organized the code for this stack, because of my previous experience in Terraform, I like to create DRY Reusable modules that can be reused to create a different environment by passing variables.

So the following are the main files:

**lib/sample-cdk.ts** — Blueprint of the stack used composition pattern to create custom.
stack/<dev, stag, prod≥-sample-cdk.ts → Separate configuration for each Stack.
bin/app → Stacks are initialized by the App class and start the provisioning.
Let me explain each of them one by one:

lib/sample-cdk.ts : [code](https://github.com/iamvishnuavenu/sample-cdk-stack/tree/main/lib)

1. **SampleApp** extends the **Construct class**, it can be a single resource or a group of resources. I have defined it as a Composite construct that has all the necessary resources in a single module. But it requires parameters or props to be defined. The props define the environment whether it is for the dev or prod environment.
2. **SampleAppProps**: This is used to initialize the Construct, provides character, and helps in resource labeling.
3. Some helper functions like **getInstanceType**, **getInstanceType** and **getSelectedSubnets** are used to improve code readability.

stack/<dev, stag, prod≥-sample-cdk.ts: code

**DevSampleAppStack** extends the Stack, we are passing the environment props here. As the name suggests, we used it to create Dev Environment.

**bin/sample-CDK-stack.ts**
![alt text](/images/aws-cdk-3.webp "Final component")

This is a simple code file where we define the stacks ie DevSampleAppStack for provisioning.

#### How to invoke it?
![alt text](/images/aws-cdk-4.webp "cmd")

Thank you for reading this! I hope it will help you in some way or another. Working with it I got the chance to brush up on my OOPS concepts and the added advantage we get is, we are using full-blown general-purpose language instead of YAML or DSL that is why it’s much flexible and lots of inbuilt constructs like loops, operators, and intuitive string interpolation are available to exploit. Fortunately, Terraform also coming up with CDK support, well that’s mean it’s easy for us as DevOps to continue to use our favorite language to define our infrastructure.
