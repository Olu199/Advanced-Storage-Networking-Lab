**AWS Global Infrastructure: Types of Deployments**

**AWS Regions**: An AWS Region is not directly mapped to a physical location, such as a specific continent or country. Instead, it represents a comprehensive deployment of AWS infrastructure, including databases, AI services, analytics, storage, and compute resources. While some countries may have multiple regions, others may only have one. These regions are geographically distributed, which allows for the design of globally resilient architectures by leveraging this spread.

**AWS Edge Locations**: These are smaller than regions and typically serve as local content distribution points, often supporting edge computing as well. Edge locations are crucial for services like Netflix, which needs to store content closer to its users to deliver it at faster speeds. Edge locations help when an AWS region is too far away, resulting in slower content delivery.

**Visualizing AWS Global Locations**:

**Benefits of AWS Regions**:

- **Geographical Separation**: AWS regions are geographically isolated, providing distinct fault domains. This isolation ensures that a failure in one region does not impact others.
- **Geopolitical Separation**: Regions are governed by different legal and regulatory frameworks, allowing customers to choose regions based on governance requirements.
- **Location Control**: You can optimize your deployment for performance by choosing regions closer to your users.

**Additional Granularity**:

- **Region Code**: This is the identifier used in the AWS CLI, such as `ap-southeast-2`.
- **Region Name**: This is how the region appears in the AWS Management Console GUI, such as "Asia Pacific (Sydney)."

**Inside Regions**:

- **Availability Zones (AZs)**: An Availability Zone is a logical unit within an AWS region. It can consist of a single data center or multiple data centers with redundant power, networking, and connectivity. AZs are isolated infrastructure units within a region. Examples include `ap-southeast-2a`, `ap-southeast-2b`, and `ap-southeast-2c`.

**Defining the Resilience of AWS**:

- **Globally Resilient**: Services that are globally resilient are replicated across all AWS regions. These services would require a global catastrophe to fail. An example is AWS Identity and Access Management (IAM).
- **Regionally Resilient**: Services that are regionally resilient operate within a single region with one set of data. They function as separate services in each region and replicate data across multiple availability zones within that region.
- **Availability Zone Resilient**: Services that are resilient at the availability zone level run within a single AZ. These services are more prone to failure because they rely on the infrastructure of a single zone.
