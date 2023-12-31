How do I track the Duration and Memory usage of my lambda? Create an example using terraform
ChatGPT

To track the Duration and Memory usage of a Lambda function using Terraform, you can configure CloudWatch Logs and CloudWatch Metrics. Here's an example configuration using Terraform:

    Configure CloudWatch Logs:

hcl

resource "aws_cloudwatch_log_group" "lambda_log_group" {
  name              = "/aws/lambda/your-lambda-function"
  retention_in_days = 30
}

resource "aws_lambda_function" "your_lambda_function" {
  // Lambda function configuration

  // Link the CloudWatch Log Group to your Lambda function
  tracing_config {
    mode = "Active"
  }

  environment {
    // Environment variables for your Lambda function
  }

  // Add a CloudWatch Log Group permission to allow Lambda to write logs
  depends_on = [aws_cloudwatch_log_group.lambda_log_group]

  resource "aws_lambda_permission" "lambda_log_group_permission" {
    statement_id  = "AllowExecutionFromCloudWatch"
    action        = "lambda:InvokeFunction"
    function_name = aws_lambda_function.your_lambda_function.function_name
    principal     = "logs.${data.aws_region.current.name}.amazonaws.com"

    source_arn = aws_cloudwatch_log_group.lambda_log_group.arn
  }
}

    Configure CloudWatch Metrics:

hcl

resource "aws_cloudwatch_metric_alarm" "lambda_duration_alarm" {
  alarm_name          = "lambda_duration_alarm"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  evaluation_periods  = "1"
  metric_name         = "Duration"
  namespace           = "AWS/Lambda"
  period              = "60"
  statistic           = "Average"
  threshold           = "300" // Set your desired threshold here
  alarm_description   = "Lambda function duration exceeds threshold"
  alarm_actions       = [] // Specify any actions to be taken when the alarm state is triggered
  dimensions = {
    FunctionName = aws_lambda_function.your_lambda_function.function_name
  }
}

resource "aws_cloudwatch_metric_alarm" "lambda_memory_alarm" {
  alarm_name          = "lambda_memory_alarm"
  comparison_operator = "GreaterThanOrEqualToThreshold"
  evaluation_periods  = "1"
  metric_name         = "MemoryUsed"
  namespace           = "AWS/Lambda"
  period              = "60"
  statistic           = "Average"
  threshold           = "512" // Set your desired threshold here
  alarm_description   = "Lambda function memory usage exceeds threshold"
  alarm_actions       = [] // Specify any actions to be taken when the alarm state is triggered
  dimensions = {
    FunctionName = aws_lambda_function.your_lambda_function.function_name
  }
}

In the above example, we configure a CloudWatch Log Group to capture the logs generated by your Lambda function. The Lambda function is associated with the CloudWatch Log Group using the aws_lambda_permission resource.

Additionally, we define CloudWatch Metric Alarms to track the Duration and Memory usage of the Lambda function. The alarms are triggered when the specified thresholds are exceeded, and you can define alarm actions accordingly.

Make sure to replace your-lambda-function with the actual name or ARN of your Lambda function in the Terraform configuration.

By deploying this Terraform configuration, you'll be able to track the Duration and Memory usage of your Lambda function using CloudWatch Logs and Metrics.