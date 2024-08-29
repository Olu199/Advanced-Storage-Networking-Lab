**EC2 Basics**

Amazon EC2 (Elastic Compute Cloud) provides access to virtual machines known as **instances**.

### Features of EC2:

- **Infrastructure as a Service (IaaS)**: EC2 offers virtual machines as a service, where the instance is the unit of consumption. It is a private AWS service that operates within the AWS private zone.
  
- **Deployment**: EC2 instances are always deployed in an Availability Zone (AZ). The instance exists within a subnet of that AZ, making it AZ-resilient. However, if the AZ fails, the instance will likely fail as well.

- **Billing**: EC2 offers on-demand billing, meaning you only pay for what you consume. Charges include:
  - **Instance Charge**: The cost of running the instance.
  - **Storage Charge**: The cost of the storage used by the instance.
  - **Software Charge**: Additional charges for any software used by the instance at launch.

- **Storage Options**: EC2 instances can use two types of storage:
  - **Local Host Storage**: Storage on the physical host where the instance is launched.
  - **Elastic Block Store (EBS)**: Network-attached block storage that persists independently of the life of the instance.

### EC2 Instance States:

- **Running**: Once an instance has completed provisioning, it enters the running state. All resources (CPU, memory, disk, and networking) are being consumed, and you are billed accordingly, even if the instance is idle.

- **Stopped**: When an instance is shut down, it moves to the stopped state. While in this state, EBS storage is still consumed, generating storage charges.

- **Terminated**: When an instance is terminated, it is deleted. Termination is the only state where you stop incurring any charges related to the instance.

### Amazon Machine Image (AMI):

- **Purpose**: An AMI is used to create an EC2 instance or can be created from an existing EC2 instance.

- **Permissions**: AMIs come with attached permissions. They can be set as a public AMI (e.g., for Linux or Windows), where the owner has implicit permissions. Explicit permissions can also be granted to specific AWS accounts.

- **Components**:
  - **Boot Volume**: The AMI includes the boot volume of the instance (e.g., the C: drive for Windows or the root volume for Linux).
  - **Block Device Mapping**: This configuration defines how the volumes attached to the AMI are presented to the operating system.

### Connecting to an EC2 Instance:

- **Operating Systems**: EC2 instances can run various operating systems, including Windows and Linux.

  - **Windows**: Connect using Remote Desktop Protocol (RDP) on port 3389. The private key is used to connect to the local administrator login, after which you provide the password.

  - **Linux**: Connect using the SSH protocol on port 22. A key pair, which includes a public and private key, is typically created. The public key is stored on the instance, while the private key is used to authenticate to the instance.
  - 
  - 
  -EC2 Instance Key pair
![[A4L.pem]]