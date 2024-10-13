### The Twenty-Four Challenge

One of my favorite games growing up was a game called the [24 Game](https://www.amazon.com/24-Game-Single-Digit-cards/dp/B002AODZFQ/) where a group of friends could take a group of numbers from a card and use the order of operations and basic arithmatic to add, subtract, multiply, and divide the numbers on the card to get to a value of 24.

Combined with the [Lambda Calculator](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/dotnetv3/Lambda/LambdaCalculator/LambdaCalculator) from the AWS C# SDK, I thought it might be a neat way to demonstrate [Amazon Step Functions](https://aws.amazon.com/step-functions/)'s basic capabilites.

The lambda calculator takes an input payload in JSON and performs basic math on these inputs, directed by the "action" property in the JSON.  We can re-use this same Lambda in a few places in our ASL to accomplish the math we need.  Here's an initial ASL definition to get us started, doing some basic arithmatic.

```
{
  "Comment": "An example of the Amazon States Language for using our 24 Game example",
  "StartAt": "InitialPayload",
  "TimeoutSeconds": 3600,
  "States": {
    "InitialPayload": {
      "Type": "Pass",
      "Result": {
        "a": "2",
        "b": "4",
        "c": "6",
        "d": "8"
      },
      "ResultPath": "$.payload",
      "Next": "Math1Prep"
    },
    "Math1Prep": {
      "Type": "Pass",
      "Parameters": {
        "action": "multiply",
        "x.$": "$.payload.a",
        "y.$": "$.payload.c"
      },
      "ResultPath": "$.payload.math1in",
      "Next": "Math1"
    },
    "Math1": {
      "Type": "Task",
      "Resource": "arn:aws:lambda:us-west-2:123456789011:function:LambdaCalculator:$LATEST",
      "InputPath": "$.payload.math1in",
      "ResultPath": "$.payload.math1out",
      "End": true
    }
  }
}
```
This ASL will take an initial payload (the four numbers from the card), run it through a pre-state preparation (essentially variable substitution), and execute the Lambda that we need to do the math.  The output of the state machine looks like this:
```
{
  "payload": {
    "a": "2",
    "b": "4",
    "c": "6",
    "d": "8",
    "math1in": {
      "action": "multiply",
      "x": "2",
      "y": "6"
    },
    "math1out": 12
  }
}
```
