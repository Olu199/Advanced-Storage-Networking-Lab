**CloudWatch Basics**

Amazon CloudWatch is a monitoring and management service that collects operational data, such as performance metrics, log data, and event information, from AWS resources, applications, and on-premises systems. It performs three primary functions:

### Key Functions of CloudWatch:

1. **Metrics**: CloudWatch gathers metrics related to AWS services, applications, and on-premises systems. These metrics can include CPU utilization on EC2 instances, disk space usage, the number of visitors to a website, and more. Some metrics are gathered automatically by the AWS services themselves. In other cases, you may need to use the CloudWatch Agent to collect additional data, particularly for on-premises systems or custom application metrics.

2. **Logs**: CloudWatch Logs enables the collection, monitoring, and storage of log data from AWS services, applications, and on-premises systems. For non-AWS systems, you will need to install and configure the CloudWatch Agent to collect log data.

3. **Events (CloudWatch Events / EventBridge)**: CloudWatch Events provides the ability to respond to changes in your AWS environment in near real-time. It can generate events that trigger actions, such as invoking an AWS Lambda function, starting a Step Function, or sending a notification via Amazon SNS. Additionally, it can be set up to perform scheduled actions at specific times.

### Key Concepts in CloudWatch:

- **Namespace**: A container used to organize and separate metrics. For example, AWS service metrics are stored in namespaces like `AWS/EC2`.

- **Metric**: A metric is a time-ordered set of data points that represent the value of a particular resource or application performance metric. Each metric belongs to a specific namespace.

- **Data Point**: A single measurement at a specific time. For example, if CPU utilization is reported as 98.3% at `2019-13-08T08:45:45Z`, this is a data point.

- **Dimension**: A name-value pair that is part of a metric's identity. Dimensions allow you to categorize and filter data points. For example, you could have a dimension with the name `InstanceId` and a value of `i-XXXXX`, or a dimension with the name `InstanceType` and a value of `t3.small`.

### Alarms in CloudWatch:

- **Alarms**: CloudWatch Alarms monitor metrics and perform actions based on predefined conditions. An alarm can have three states:
  - **OK**: The metric is within the defined threshold.
  - **Alarm**: The metric has crossed the threshold, triggering the alarm.
  - **Insufficient Data**: There isn't enough data to determine the state of the alarm.

  Actions can be configured to occur when an alarm enters the "Alarm" state, such as sending a notification via Amazon SNS or executing an Auto Scaling action.
