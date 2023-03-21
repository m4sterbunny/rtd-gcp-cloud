=====================
Shared security model
=====================

Security and compliance is only effective once the AWS customer understands their role. AWS operates the operating system (OS) and virtualization layer along with securing the physical drives. AWS also offers updates and security patches as supplied by the various OS.

The AWS customer must: 
- manage the OS
- update and patch the OS
- configure the firewall (aka security group)
- integrate AWS services into any existing environment
- abide by applicable laws and regulations
- apply the relevant IAM tools

Data 
-----

Handling and encrypting data is a massive security consideration. The AWS customer must understand that they are responsible for all:

- Client-side data encryption
- Data integrity authentication
- Server-side encryption 
- Encrypting networking traffic

IT controls
-----------

The above help separate the responsibilities that may otherwise remain unclear. For example:

Patch management
****************

AWS will patch and fix flaws relevant to the infrastructure but will never update the OS or any other application software that customers install on that infrastructure.

Configuration management
************************

AWS configures its infrastructure's hardware and software; while the customer configures OS, self-managed databases, and applications.

Training
********

Both AWS and the customer have responsibility for training their own employees. This often means having a good level of understanding of standards and frameworks such as:

- NIST Cybersecurity framework
- AWS Cloud Adoption Framework



