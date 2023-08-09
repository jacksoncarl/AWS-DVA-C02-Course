# AWS Fundamentals

## Public vs Private Services

When you hear the term AWS private service and public service, it is referring to networking only.

- **Public AWS service**: Accessed using public endpoints, such as S3. However, it's essential to understand that not all AWS services have the same accessibility. Just because you are connected to a public service doesn't imply that you have the necessary permissions to access it. Proper access control and permissions are still required to interact with the service effectively.

- **Private AWS service**: Operates within a VPC, ensuring that access is limited to resources within that VPC or those directly connected to it. By default, there is no direct connection between the private service in the VPC and the public cloud. However, for specific services, you can configure them to allow public internet connectivity. This is achieved by exposing a segment of the private service and projecting it into the AWS public zone, enabling inbound or outbound connections from the public internet.

## AWS Global Infrastructure

**Resources**: [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/)

### Regions

- AWS Regions are geographical areas defined by AWS, each representing a specific location in the world where AWS has set up a full deployment of its services.
- AWS can only deploy regions as fast as their planning allows. Regions are often not near their customers.
- An AWS region can be identified in two ways: through its region name or its region code. e.g Region Name: Asia Pacific (Sydney) Region Code: ap-southeast-2.
- Regions are connected with high-speed networking. Certain services, like EC2, need to be provisioned in a specific region. On the other hand, some services, like IAM, are global in scope and are not tied to any specific region.

#### Benefits of Regions

- **Geographical Separation - isolated fault domain**
  - Useful for mitigating the impact of natural disasters.
  - Provides isolated fault domains, enhancing resilience.
  - Regions are 100% isolated from each other.
  
- **Geopolitical Separation - different governance**
  - Different laws in various regions may affect how services are accessed and managed.
  - Offers stability and protection from political events in specific regions.
  
- **Location Control - Performance**
  - Allows fine-tuning of the architecture to optimize performance for specific geographic areas.
  - Facilitates duplicating infrastructure closer to customers, reducing latency and improving user experience.

### Availability Zones

- AZs are isolated in terms of compute, storage, networking, power, and facilities.
- You can design solutions that distribute services across multiple availability zones.
- An availability zone can be part of one or more data centers.

### Resilience

- **Globally Resilient**: Services like IAM or Route 53 have a global presence, ensuring high availability with no possibility of going down. They replicate data across multiple regions, providing robust resilience.
- **Region Resilient**: These services operate independently in each region. They typically replicate data to multiple AZs within that region, enhancing their resilience against failures.
- **AZ Resilient**: These services run from a single AZ. Although they may have redundant equipment to handle hardware failures, they should not be solely relied upon for high availability.

### AWS Default Virtual Private Cloud (VPCs)

- VPC is the service you will use to create private networks inside AWS, where other private services will run.
- VPCs are also used to connect your AWS private networks to your on-premises networks when creating a hybrid environment. Additionally, VPCs enable you to connect to other cloud platforms when setting up a multi-cloud deployment.
- ❗ You'll get lots of networking and VPC-related questions in the exam.

#### VPC Basics

- A VPC = A Virtual Network Inside AWS.
- A VPC is within **1 account** & **1 region**.
- A VPC by default is **private** and **isolated** unless you decide otherwise.
- There are two types of VPC available:
  - Default VPC
  - Custom VPCs
- ❗ There can only be **one** default VPC per region.
- You can have many custom VPCs in a region.

#### Default VPC

- VPC CIDR - defines the start and end ranges of the VPC.
- The IP CIDR of a default VPC is **always**: 172.31.0.0/16.
- The higher that the CIDR range slash number is, the smaller the network is e.g. a /17 is half the size of a /16.
- A strength of the default VPC is its consistent and predictable configuration.
- Each (/20) subnet inside a VPC is located in one availability zone. This is set on creation and can never be changed.
- Subnets are assigned a specific section of the IP ranges from the VPC.
- These ranges cannot be the same as other subnets in the VPC, and they can't overlap with any subnets inside the VPC.
- They are configured to provide anything deployed inside those subnets with public IPv4 addresses.
- A default VPC comes with an Internet Gateway (IGW), Security Group (SG), and Network Access Control List (NACL) pre-configured.

### Elastic Compute Cloud (EC2)

- The default compute service within AWS.
- EC2 is IAAS (Infrastructure as a service). It provides access to virtual machines known as EC2 instances.
- The unit of consumption is an instance.
- EC2 is a private service by default. An EC2 instance is configured to launch into a single VPC subnet. Public access can also be configured.
- If you want to allow public access for an EC2 instance, the VPC that it's running within needs to support public access. The default VPC handles this for you. However, if you use a custom VPC, then you must handle the networking on your own.
- EC2 is AZ resilient. EC2 deploys into one AZ. If it fails, the instance fails.
- EC2 has different instance sizes and capabilities.
- EC2 offers on-demand billing (per second or per hour).
  - Pricing based on:
    - CPU
    - Memory
    - Storage
    - Networking
    - Commercial software (extra cost)
- EC2 has Local on-host storage or Elastic Block Storage (EBS).

#### Instance Lifecycle

- EC2 instance has an attribute called a **state**. The state of an instance provides an indication into its condition.
- Instance states:
  - Running
  - Stopped
  - Terminated
- When you launch an EC2 instance, after it finishes provisioning, it moves into the **running** state. An instance can then be transitioned from **running** to **stopped** when you shut it down. Once **stopped**, an instance can be moved back to **running** when you start it up again.
- You can **terminate** an instance when its **running** or **stopped**. ❗This is a one-way change (non-reversible action). The instance is fully deleted.

##### Running state

- When an instance is in a **running** state, you are charged for all four categories:
  - CPU (consumes CPU capacity even while idle)
  - Memory (uses memory even when no processing occurs)
  - Storage (OS and its data are stored on disk)
  - Networking (networking is generally always used)

##### Stopped state

- When an instance is in a **stopped** state, you are only charged for storage (OS and its data are stored on disk).

##### Terminated state

- When an instance is **terminated**, it stops the resource usage for all categories, and deletes any allocated storage.

#### AMI (Amazon Machine Image)

- An AMI can be used to create an EC2 instance, or can be created from an EC2 instance.
- An AMI contains:
  - Attached permissions. These permissions control which accounts can and can't use the AMI:
      - Public - Everyone is allowed
      - Owner - Implicit allow, only the owner can use it to spin up new instances
      - Explicitly - Owner grants access to AMI for specific AWS accounts
  - Root Volume. This contains the boot volume on the instance.
  - Block Device Mapping. This is a configuration which links the volumes that the AMI has and how they're presented to the operating system. Determines which volume is a boot volume and which volume is a data volume.

#### Connecting to EC2

- EC2 instances can run different operating systems:
  - They can run a distribution or a version of Linux
  - They can also run various different versions of Windows

- You connect to Windows instances using RDP (Remote Desktop Protocol). 
  - This runs on port 3389.

- You connect to Linux instances using the SSH protocol.
  - This runs on port 22.
  - You login or authenticate to that instance using what's known as an SSH key pair. 

### Simple Storage Service (S3)

- S3 is a global storage platform.
  - It's global because it runs from all of the AWS regions and can be accessed from anywhere with an internet connection.
  - It's a public service.
  - It's regionally based because your data is stored in a specific AWS region at rest. Data can only leave this region if you explicitly configure it to.
  - It's regionally resilient.
  - It can cope with unlimited data amounts.
  - It's designed for multi-user usage of that data.
  - You should think of S3 as the default storage service of AWS.
  - Accessible via UI, CLI, API, and HTTP.
  - Great for large scale data storage, distribution or upload. 
  - It can input and/or output to many AWS products. 

#### S3 Objects

- An object in S3 is made up of two main components:
  - An object key (e.g. Koala.jpg - think of it as a file name in the bucket)
    - The key identifies the object in a bucket.
  - The value of the object (e.g. the data or contents)
    - ❗ An object can range from zero bytes to 5 terabytes in size.

- An object also have the following properties:
  - Version Id
  - Metadata
  - Access Control
  - Subresources

#### S3 Buckets

- Created in a specific AWS region.
  - Data that is inside the bucket has a primary home region. Data will not leave this region unless configured to. 
- ❗Buckets are identified by their name and need to be globally unique.  
- Buckets can hold an unlimited number of objects.
- Buckets have no complex structure. It has a flat structure (all objects are stored at the same level). 
   - When an object has a name such as `/old/Koala3.jpg`, the UI presents the object as being in a folder called `old`. 
- Bucket names need to be between 3-63 characters, all lower case and no underscores.
  - They must start with a lowercase letter or number.
- ❗100 bucket soft limit, 1000 bucket hard limit per account.




