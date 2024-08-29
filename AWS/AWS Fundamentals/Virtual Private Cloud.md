
**AWS Global Infrastructure: Types of Deployments**

1. **AWS Regions**: AWS Regions are not directly mapped to specific physical locations, such as a country or continent, but are instead a conceptual creation by AWS. Each region represents a fully isolated deployment of AWS infrastructure, including databases, AI, analytics, storage, and compute services. Some countries may have multiple regions, while others may have only one. The geographic distribution of these regions allows for the design of globally resilient architectures.

2. **AWS Edge Locations**: These are smaller than AWS Regions and typically serve as local content distribution points. They may also support edge computing. Edge locations are used by services like Netflix to store content closer to clients, enabling faster delivery, particularly when an AWS Region is too distant to provide optimal speed.

**Visualizing Global AWS Locations:**

**Benefits of AWS Regions:**

- **Geographical Separation**: Each region is geographically isolated, providing fault tolerance and disaster recovery across different geographic areas.
- **Geopolitical Separation**: Different regions operate under different governance and compliance regimes, offering control over data sovereignty.
- **Location Control**: AWS Regions allow you to optimize your deployment for performance based on the geographical proximity of users.

**Additional Granularity:**

- **Region Code**: For example, `ap-southeast-2` represents the Asia Pacific (Sydney) region in the AWS CLI.
- **Region Name**: For instance, "Asia Pacific (Sydney)" as displayed in the AWS Management Console.

**Inside AWS Regions:**

- **Availability Zones (AZs)**: These are logically distinct components within an AWS Region. An AZ can consist of one or more data centers or other infrastructure components. AZs are isolated from each other to enhance fault tolerance within a region. For example, in the `ap-southeast-2` region, AZs are denoted as `ap-southeast-2a`, `ap-southeast-2b`, `ap-southeast-2c`.

**Defining AWS Resilience:**

- **Globally Resilient Services**: These services are available across all AWS Regions and require a global disaster to impact their availability. An example is AWS IAM (Identity and Access Management).
- **Regionally Resilient Services**: These services operate within a single AWS Region and replicate data across multiple Availability Zones within that region. They are independent of services in other regions.
- **Availability Zone Resilient Services**: These services operate within a single Availability Zone. While they are isolated, they are more prone to failure compared to services distributed across multiple AZs.

---

**VPC (Virtual Private Cloud)**

A Virtual Private Cloud (VPC) is a virtual network within a specific AWS region, associated with a particular AWS account. VPCs are region-specific, private, and isolated. They are commonly used to create private networks within AWS, enabling the secure connection of private services, hybrid environments, or multi-cloud networks.

**Key Characteristics of a VPC:**

- **Regional Resilience**: VPCs are regionally resilient and can span multiple Availability Zones (AZs) within the same region.
- **Isolation**: By default, a VPC is isolated from other VPCs and the public zone unless explicitly connected.

**Types of VPCs:**

1. **Default VPC**: Each AWS region has one default VPC, pre-configured by AWS. It has a fixed setup, making it less flexible than custom VPCs.
  
2. **Custom VPCs**: You can create multiple custom VPCs per region. Custom VPCs offer greater flexibility, allowing detailed configuration. They are commonly used in serious or production-grade AWS deployments.

**VPC Architecture:**

- **Creation**: A VPC is created within an AWS account and is specific to a particular AWS region. Each region can have multiple VPCs.
  
- **CIDR Block**: Each VPC is allocated a range of IP addresses, known as a VPC CIDR block (e.g., `172.31.0.0/16`). All communication within the VPC uses this CIDR block.

- **Default VPC**: The default VPC always has the CIDR block `172.31.0.0/16`. It can be divided into subnets, each corresponding to a different AZ. 

- **Subnets**: Subnets are smaller sections of the VPC CIDR block, each associated with an AZ. Subnets are like slices of the overall VPC, partitioning the IP range.

**Facts About VPCs:**

- **Default VPC**: Only one default VPC exists per region. It can be removed and recreated, but it always uses the same CIDR block (`172.31.0.0/16`).
  
- **Subnets**: In each AZ, a smaller `/20` subnet is typically created within the default VPC. The higher the CIDR block notation (e.g., `/16` vs. `/20`), the larger the IP address range.
  
- **Default VPC Components**: The default VPC includes several components:
  - **Internet Gateway**: Provides connectivity between the VPC and the internet.
  - **Security Groups**: Act as virtual firewalls for instances within the VPC.
  - **Network ACLs (NACLs)**: Provide stateless filtering of traffic entering or leaving subnets.

- **Public IP Addresses**: By default, instances launched in the default VPC are assigned a public IPv4 address.
