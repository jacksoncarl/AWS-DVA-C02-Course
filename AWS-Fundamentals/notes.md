## AWS Fundamentals

### Public vs Private Services

When you hear the term AWS private service and public service, it is referring to networking only.

- A public AWS service is something which is accessed using public endpoints, such as S3. However, it's essential to understand that not all AWS services have the same accessibility. Just because you are connected to a public service doesn't imply that you have the necessary permissions to access it. Proper access control and permissions are still required to interact with the service effectively.

- A private AWS service operates within a VPC, ensuring that access is limited to resources within that VPC or those directly connected to it. By default, there is no direct connection between the private service in the VPC and the public cloud. However, for specific services, you can configure them to allow public internet connectivity. This is achieved by exposing a segment of the private service and projecting it into the AWS public zone, enabling inbound or outbound connections from the public internet.

### AWS Global Infrastructure

#### Regions

- AWS Regions are geographical areas defined by AWS, each representing a specific location in the world where AWS has set up a full deployment of its services. 

- AWS can only deploy regions as fast as their planning allows. Regions are often not near their customers.

-  An AWS region can be identified in two ways: through its region name or its region code. e.g Region Name: Asia Pacific (Sydney) Region Code: ap-southeast-2



- Regions are connected with high-speed networking. Certain services, like EC2, need to be provisioned in a specific region. On the other hand, some services, like IAM, are global in scope and are not tied to any specific region. 

- Geographical Separation - isolated fault domain
  - Useful for mitigating the impact of natural disasters.
  - Provides isolated fault domains, enhancing resilience.
  - Regions are 100% isolated from each other.
- Geopolitical Separation - different governance 
  - Different laws in various regions may affect how services are accessed and managed.
  - Offers stability and protection from political events in specific regions.
- Location Control - Performance
  - Allows fine-tuning of the architecture to optimize performance for specific geographic areas.
  - Facilitates duplicating infrastructure closer to customers, reducing latency and improving user experience.

#### Availability Zones

- AZs are isolated in terms of compute, storage, networking, power, and facilities.
- You can design solutions that distribute services across multiple availability zones.
- An availability zone can be part of one or more data centers.

#### Resilience
- Globally Resilient: Services like IAM or Route 53 have a global presence, ensuring high availability with no possibility of going down. They replicate data across multiple regions, providing robust resilience.

- Region Resilient: These services operate independently in each region. They typically replicate data to multiple AZs within that region, enhancing their resilience against failures.

- AZ Resilient: These services run from a single AZ. Although they may have redundant equipment to handle hardware failures, they should not be solely relied upon for high availability.