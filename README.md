# AWS-Techtips
Quick reference Guide for AWS Solution Architects

EC2
-----------------
You are limited to running up to a total of 20 On-Demand instances across the instance family, purchasing 20 Reserved Instances, and requesting Spot Instances per your dynamic spot limit per region (by default)
AMIs are regional. You can only launch an AMI from the region in which it is stored.
Public IPv4 addresses are lost when the instance is stopped but private addresses (IPv4 and IPv6) are retained. Elastic IPs are retained when the instance is stopped.
All accounts are limited to 5 elastic IP’s per region by default
By default EC2 instances come with a private IP
By default Eth0 is the only Elastic Network Interface (ENI) created with an EC2 instance when launched
Public IP addresses are assigned for instances in public subnets (VPC)
To receive a history of all EC2 API calls (including VPC and EBS) made on your account, you simply turn on CloudTrail 
The data stored on a local instance store will persist only as long as that instance is alive. However, data that is stored on an Amazon EBS volume will persist independently of the life of the instance.
If your applications benefit from high packet-per-second performance and/or low latency networking, Enhanced Networking will provide significantly improved performance, consistence of performance and scalability. There is no additional fee for Enhanced Networking
Amazon EC2 provides enhanced networking capabilities through the Elastic Network Adapter (ENA).To test whether enhanced networking is already enabled, verify that the ena module is installed on your instance and that the enaSupport attribute is set. 
Price Model
RIs provide you with a significant discount (up to 75%) compared to On-Demand instance pricing
You have the flexibility to change families, OS types, and tenancies while benefitting from RI pricing when you use Convertible RIs
The following are a few reasons why an instance might immediately terminate (i.e. while restart a stopped EC2 instance and it immediately changed from a pending state to a terminated state):
You’ve reached your EBS volume limit
An EBS snapshot is corrupt
The root EBS volume is encrypted and you do not have permissions to access the KMS key for decryption
The instance store-backed AMI that you used to launch the instance is missing a required part (an image.part.xx file)
You can modify the Availability Zone, scope, network platform, or instance size of your Reserved Instance as long as it is within the same instance type.
AWS Systems Manager Run Command lets you remotely and securely manage the configuration of your managed instances. A managed instance is any Amazon EC2 instance or on-premises machine in your hybrid environment that has been configured for Systems Manager
Reserved Instance Marketplace is a platform that supports the sale of third-party and AWS customers' unused Standard Reserved Instances, which vary in terms of lengths and pricing options.


AWS Lambda
------------------------------

AWS Lambda allocates CPU power proportional to the memory you specify using the same ratio as a general purpose EC2 instance type
Functions larger than 1536MB are allocated multiple CPU threads, and multi-threaded or multi-process code is needed to take advantage
Any Lambda function invoked asynchronously is retried twice before the event is discarded. If the retries fail and you're unsure why, use Dead Letter Queues (DLQ) to direct unprocessed events to an Amazon SQS queue or an Amazon SNS topic to analyze the failure.
You can update the configuration and request additional memory in 64 MB increments from 128MB to 3008 MB.
There is no limit to the number of environment variables you can create as long as the total size of the set does not exceed 4 KB.
Number of file descriptors 1,024
Number of processes and threads (combined total) 1,024
Concurrent executions (see Managing Concurrency) 1000
AWS Lambda automatically monitors functions on your behalf, reporting metrics through Amazon CloudWatch. These metrics include total requests, latency, and error rates

S3
------------------------------

Amazon S3 automatically scales to high request rates
For example, your application can achieve at least 3,500 PUT/POST/DELETE and 5,500 GET requests per second per prefix in a bucket. There are no limits to the number of prefixes in a bucket
You can setup access control to your buckets using:
Bucket policies
Access Control Lists
S3 static website - Example: http://<bucketname>.s3-website-<region>.amazonaws.com
Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your client and your Amazon S3 bucket.
Individual Amazon S3 objects can range in size from a minimum of 0 bytes to a maximum of 5 terabytes. The largest object that can be uploaded in a single PUT is 5 gigabytes..
Multipart upload is recommended for objects of 100MB or larger:
Can be used for objects from 5MB up to 5TB
Must be used for objects larger than 5GB
CRR is an Amazon S3 feature that automatically replicates data across AWS Regions

Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3 using standard SQL. You can use Athena to process logs, perform ad-hoc analysis, and run interactive queries

Glacier
------------------------------

Data stored in Amazon Glacier is protected by default; only vault owners have access to the Amazon Glacier resources they create.
With Amazon Glacier Select, you can now perform filtering and basic querying using a subset of SQL directly against your data in Amazon Glacier
Glacier provides three retrieval options - Expedited, Standard, and Bulk. 
The total volume of data and number of archives you can store are unlimited. Individual Amazon Glacier archives can range in size from 1 byte to 40 terabytes. The largest archive that can be uploaded in a single Upload request is 4 gigabytes

EBS
------------------------------

There is no direct way to encrypt an existing unencrypted EBS volume, or to remove encryption from an encrypted volume
EBS volume data is replicated across multiple servers in an AZ
EBS volumes must be in the same AZ as the instances they are attached to
You cannot encrypt the EBS volume even if you unmount the volume. Remember that encryption has to be done during volume creation.
You cannot create an encrypted snapshot of an unencrypted EBS volume or change existing volume from unencrypted to encrypted. You have to create new encrypted volume and transfer data to the new volume.
Along with the monitoring features of Amazon CloudWatch Events and AWS CloudTrail, Amazon DLM (Data Lifecycle Manager) provides a complete backup solution for EBS volumes at no additional cost. Automate the creation of snapshots and add life cycle policies for old snapshots
When you create an EBS volume in an Availability Zone, it is automatically replicated within that zone only to prevent data loss due to failure of any single hardware component. After you create a volume, you can attach it to any EC2 instance in the same Availability Zone.
It is the EBS snapshots, not the EBS volume, that has a copy of the data which is stored redundantly in multiple Availability Zones.
To keep a backup copy of your data, you can create a snapshot of an EBS volume, which is stored in Amazon S3
You can increase the size of an EBS volume to as much as 16 TiB, but whether the OS recognizes all of that capacity depends on its own design characteristics and on how the volume is partitioned.

Load Balancer
------------------------------

Elastic Load Balancing provides access logs that capture detailed information about requests sent to your load balancer. Each log contains information such as the time the request was received, the client's IP address, latencies, request paths, and server responses. By default, its disabled
If the load balancer nodes for your Classic Load Balancer can distribute requests regardless of Availability Zone, this is known as cross-zone load balancing. With cross-zone load balancing enabled, your load balancer nodes distribute incoming requests evenly across the Availability Zones enabled for your load balancer. Otherwise, each load balancer node distributes requests only to instances in its Availability Zone.
Cross-zone load balancing is already enabled by default in Application Load Balancer.
For each associated subnet that a Network Load Balancer is in, the Network Load Balancer can only support a single public/internet facing IP address
ELBs are not responsible for terminating EC2 instances. Auto Scaling can terminate instances that fail health checks
ALB allows containers to use dynamic host port mapping so that multiple tasks from the same service are allowed on the same container host
ELB, like CloudFront, only supports valid TCP requests, so DDoS attacks such as UDP and SYN floods are not able to reach EC2 instances
ELB also offers a single point of management and can serve as a line of defense between the internet and your backend, private EC2 instances

Auto Scaling group
------------------------------

The cooldown period is a configurable setting for your Auto Scaling group that helps to ensure that it doesn't launch or terminate additional instances before the previous scaling activity takes effect. The default value is 300 sec. After the Auto Scaling group dynamically, scales using a simple scaling policy, it waits for the cooldown period to complete before resuming scaling activities.
EC2 Auto Scaling provides you with an option to enable automatic scaling for one or more EC2 instances by attaching them to your existing Auto Scaling group. After the instances are attached, they become a part of the Auto Scaling group.
The instance that you want to attach must meet the following criteria:
The instance is in the running state.
The AMI used to launch the instance must still exist.
The instance is not a member of another Auto Scaling group.
The instance is in the same Availability Zone as the Auto Scaling group.
If the Auto Scaling group has an attached load balancer, the instance and the load balancer must both be in EC2-Classic or the same VPC. If the Auto Scaling group has an attached target group, the instance and the load balancer must both be in the same VPC.
Whenever you create an Auto Scaling group, you must specify a launch configuration, a launch template, or an EC2 instance. You can't modify a launch configuration after you've created it.
If any health check returns an unhealthy status the instance will be terminated
If connection draining is enabled, Auto Scaling waits for in-flight requests to complete or timeout before terminating instances
You cannot modify an ASG launch configuration, you must create a new launch configuration and specify the copied AMI


RDS
------------------------------

By default, customers are allowed to have up to a total of 40 Amazon RDS DB instances (only 10 of these can be Oracle or MS SQL unless you have your own licences)
Events - You can use API calls to the Amazon RDS service to list the RDS events in the last 14 days (DescribeEvents API). You can view events from the last 14 days using the CLI. Using the AWS Console you can only view RDS events for the last 1 day
Encryption at rest is supported for all DB types and uses AWS KMS
You cannot encrypt an existing DB, you need to create a snapshot, copy it, encrypt the copy, then build an encrypted DB from the snapshot
You can only scale RDS up (compute and storage)
Scaling of storage can happen while the RDS instance is running without outage however there may be performance degradation. Scaling compute will cause downtime
Multi-AZ RDS creates a replica in another AZ and synchronously replicates to it (DR only)
Amazon Aurora supports a maximum DB size of 64 TiB
All other RDS DB types support a maximum DB size of 16 TiB
RDS-Magnetic is limited to a maximum size of 4 TiB & maximum of 1,000 IOPS
Restored DBs will always be a new RDS instance with a new DNS endpoint
Can restore up to the last 5 minutes
With optional Multi-AZ deployments, Amazon RDS also manages synchronous data replication across Availability Zones with automatic failover.
By default, customers are allowed to have up to a total of 40 Amazon RDS DB instances.
10 can be Oracle or SQL Server DB instances under the "License Included" model. 
RDS for SQL Server has a limit of 30 databases on a single DB instance.
RDS for MySQL or MariaDB, you can access the slow query logs for your database to determine if there are slow-running SQL queries
Amazon RDS retains backups of a DB Instance for a limited, user-specified period of time called the retention period, which by default is 7 days but can be set to up to 35 days.
To use public connectivity, simply create your DB Instances with the Publicly Accessible option set to yes. With Publicly Accessible active, your DB Instances within a VPC will be fully accessible outside your VPC by default.
Read Replicas
Read replicas are used for read heavy DBs and replication is asynchronous
You can have 5 read replicas of a production DB
Read replicas are available for MySQL, PostgreSQL, MariaDB and Aurora (not SQL Server or Oracle)
The read replicas storage type and instance class can be different from the source but the compute should be at least the performance of the source
Read replicas can be in another region (uses asynchronous replication)
DynamoDB and CloudFront do not have a Read Replica feature
When you create or modify your DB instance to run as a Multi-AZ deployment, Amazon RDS automatically provisions and maintains a synchronous standby replica in a different Availability Zone. Updates to your DB Instance are synchronously replicated across Availability Zones to the standby

Aurora
------------------------------

High performance, low price
Scales in 10GB increments
Scales up to 32vCPUs and 244GB RAM
2 copies of data are kept in each AZ with a minimum of 3 AZ’s (6 copies)
Can handle the loss of up to two copies of data without affecting DB write availability and up to three copies without affecting read availability
Two types of replication: Aurora replica (up to 15), MySQL Read Replica (up to 5)
Automatic failover is available for Aurora replicas only
	If you have an Amazon Aurora Replica in the same or a different Availability Zone, when failing over, Amazon Aurora flips the canonical name record (CNAME) for your DB Instance to point at the healthy replica, which in turn is promoted to become the new primary. Start-to-finish, failover typically completes within 30 seconds.
	If you do not have an Amazon Aurora Replica (i.e. single instance), Aurora will first attempt to create a new DB Instance in the same Availability Zone as the original instance. If unable to do so, Aurora will attempt to create a new DB Instance in a different Availability Zone. From start to finish, failover typically completes in under 15 minutes.
Automated Backups
When automated backups are turned on for your DB Instance, Amazon RDS automatically performs a full daily snapshot of your data (during your preferred backup window) and captures transaction logs (as updates to your DB Instance are made)
Automated backups are enabled by default and data is stored on S3 and is equal to the size of the DB
Amazon RDS retains backups of a DB Instance for a limited, user-specified period of time called the retention period, which by default is 7 days but can be up to 35 days
The granularity of point-in-time recovery is 5 minutes
Automated backups are deleted when you delete the RDS DB instance
DB Snapshot
Restored DBs will always be a new RDS instance with a new DNS endpoint
Can restore up to the last 5 minutes
You cannot restore from a DB snapshot to an existing DB – a new instance is created when you restore
Only default DB parameters and security groups are restored – you must manually associate all other DB parameters and SGs

Dynamo DB
•	Amazon DynamoDB is a fully managed cloud database and supports both document and key-value store models
•	Amazon DynamoDB stores three geographically distributed replicas of each table to enable high availability and data durability
•	Data is synchronously replicated across 3 AZs
•	An item is a collection of attributes
•	The aggregate size of an item cannot exceed 400KB including keys and all attributes
•	Can store pointers to objects in S3, including items over 400KB
•	DynamoDB Streams help you to keep a list of item level changes or provide a list of item level changes that have taken place in the last 24hrs
•	Store more frequently and less frequently accessed data in separate tables
•	Push button scaling without downtime
•	You can scale down only 4 times per calendar day
•	These are the limits unless you request a higher amount:
US East (N. Virginia) Region:
•	Per table – 40,000 read capacity units and 40,000 write capacity units
•	Per account – 80,000 read capacity units and 80,000 write capacity units
All Other Regions:
•	Per table – 10,000 read capacity units and 10,000 write capacity units
•	Per account – 20,000 read capacity units and 20,000 write capacity units
•	Global tables provide automatic multi-master replication to AWS regions world-wide, so you can deliver low-latency data access to your users no matter where they are located
•	256 tables per account per region
•	One read capacity unit represents one strongly consistent read per second, or two eventually consistent reads per second for items up to 4KB
•	One write capacity unit represents one write per second for an item up to 1KB
•	DynamoDB is more cost effective for read heavy workloads
•	Priced based on provisioned throughput (read/write) regardless of whether you use it or not
•	Write throughput per hour for every 10 units
•	Read throughput per hour for every 50 units
•	Amazon DynamoDB is integrated with AWS Lambda so that you can create triggers—pieces of code that automatically respond to events in DynamoDB Streams
•	Amazon DynamoDB stores structured data indexed by primary key, and allows low latency read and write access to items ranging from 1 byte up to 400KB
•	In order to optimize your costs across AWS services, large objects or infrequently accessed data sets should be stored in Amazon S3, while smaller data elements or file pointers (possibly to Amazon S3 objects) are best saved in Amazon DynamoDB.
RedShift
•	The size of a single node is 160GB and clusters can be created up to a petabyte or more
•	Multi-node consists of Leader node & Compute nodes. There can be Up to 128 compute nodes
•	Amazon RedShift Spectrum is a feature of Amazon Redshift that enables you to run queries against exabytes of unstructured data in Amazon S3, with no loading or ETL required
•	Amazon Redshift retains backups for 1 day. You can configure this to be as long as 35 days
Elasticache
•	Elasticache EC2 nodes cannot be accessed from the Internet, nor can they be accessed by EC2 instances in other VPCs
•	Can be on-demand or reserved instances too (but not Spot instances)
•	Access to Elasticache nodes is controlled by VPC security groups and subnet groups (when deployed in a VPC)
•	You cannot move an existing Amazon ElastiCache Cluster from outside VPC into a VPC
•	You can run a maximum of 100 nodes per region. If you need more nodes, please fill in the ElastiCache Limit Increase Request form.
•	There are two types of ElastiCache engine:
•	Memcached 
•	simplest model, can run large nodes with multiple cores/threads, can be scaled in and out, can cache objects such as DBs
•	Not persistent
•	Ideal front-end for data stores (RDS, Dynamo DB etc.)
•	Does not support multi-AZ failover or replication
•	Does not support snapshots
•	You can place nodes in different AZs
•	Redis 
•	complex model, supports encryption, master / slave replication, cross AZ (HA), automatic failover and backup/restore
•	Data is persistent
•	Can be used as a datastore
•	Scales by adding shards, not nodes
•	A Redis shard is a subset of the cluster’s keyspace, that can include a primary node and zero or more read-replicas
•	Multi-AZ is possible using read replicas in another AZ in the same region
•	Charges
•	Pricing is per Node-hour consumed for each Node Type
•	Partial Node-hours consumed are billed as full hours
•	There is no charge for data transfer between Amazon EC2 and Amazon Elasticache within the same Availability Zone
Snowball
•	Uses 256-bit encryption (managed with the AWS KMS) and tamper-resistant enclosures with TPM
•	Snowball (80TB) (50TB model available only in the USA)
•	Snowball Edge (100TB) comes with onboard storage and compute capabilities
•	Snowmobile – exabyte scale with up to 100PB per Snowmobile
•	Snowball can import to S3 or export from S3
•	Import/export is when you send your own disks into AWS – this is being deprecated in favour of Snowball
•	Snowball must be ordered from and returned to the same region
VPC
•	By default you can create up to 5 VPCs per region
•	Public subnets are subnets that have:
•	“Auto-assign public IPv4 address” set to “Yes”
•	The subnet route table has an attached Internet Gateway
•	Options for connecting to a VPC are:
•	Hardware based VPN
•	Direct Connect
•	VPN CloudHub
•	Software VPN
•	Once the VPC is created you cannot change the CIDR block
•	The first 4 and last 1 IP addresses in a subnet are reserved
•	Each subnet must reside entirely within one Availability Zone and cannot span zones
•	Routing	
o	Each subnet has a route table the router uses to forward traffic within the VPC
o	Up to 200 route tables per VPC
o	Up to 50 route entries per route table
o	Each subnet can only be associated with one route table
o	Can assign one route table to multiple subnets
•	You can only attach one Internet gateway to a custom VPC
•	IPv6 addresses are all public and the range is allocated by AWS
•	IGWs must be detached before they can be deleted
•	Every EC2 instance has a primary interface known as eth0 which cannot be detached
•	You can have up to 5 elastic IPs per account
•	Elastic IPs are retained in your account whereas auto-assigned public IPs are released

NAT Gateway
•	NAT gateways are managed for you by AWS, it can scale automatically up to 45Gbps.
•	NAT GW is not associated with any security groups
•	Egress only NAT gateways operate on IPv6 whereas NAT gateways operate on IPv4
•	Port forwarding is not supported

Security Group
•	Security groups act like a firewall at the instance level
•	You can use security group names as the source or destination in other security groups
•	You can use the security group name as a source in its own inbound rules
•	Up to 5 security groups can be added per EC2 instance interface
•	There is no limit on the number of EC2 instances within a security group

Network ACL’s
•	Network ACL’s function at the subnet level
•	The VPC router hosts the network ACL function
•	NACLs only apply to traffic that is ingress or egress to the subnet not to traffic within the subnet
•	All subnets must be associated with a network ACL
•	NACL is the first line of defence, the security group is the second line
•	Also recommended to have software firewalls installed on your instances
•	Security Groups usually control the list of ports that are allowed to be used by your EC2 instances and the NACLs control which network or list of IP addresses can connect to your whole VPC.
•	A network ACL contains a numbered list of rules that we evaluate in order, starting with the lowest numbered rule. The highest number that you can use for a rule is 32766

VPN
•	A Virtual Private Gateway (VGW) is required on the AWS side
•	A Customer Gateway is required on the customer side
•	An Internet routable IP address is required on the customer gateway
•	Two tunnels per connection must be configured for redundancy
•	VPN CloudHub is used for hardware-based VPNs and allows you to configure your branch offices to go into a VPC and then connect that to the corporate DC
•	Cannot access Elastic IPs on your VPC via the VPN – Elastic IPs can only be connected to via the Internet

Enhanced Networking
•	Enhanced networking provides higher bandwidth, higher packet-per-second (PPS) performance, and consistently lower inter-instance latencies
•	If your packets-per-second rate appears to have reached its ceiling, you should consider moving to enhanced networking because you have likely reached the upper thresholds of the VIF driver

VPC Flow Logs
•	Flow Logs capture information about the IP traffic going to and from network interfaces in a VPC
•	Flow log data is stored using Amazon CloudWatch Logs
•	Flow logs can be created at the following levels:
o	VPC
o	Subnet
o	Network interface
•	You can’t tag a flow log
•	You can’t change the configuration of a flow log after it’s been created
VPC Peering
•	Peering connections can be created with VPCs in different regions(available in most regions now)
•	There is no single point of failure or bandwidth bottleneck
•	50 VPC peers per VPC, up to 125 by request
•	VPC peering connection does not support edge to edge routing. This means that if either VPC in a peering relationship has one of the following connections, you cannot extend the peering relationship to that connection:
•	A VPN connection or an AWS Direct Connect connection to a corporate network
•	An internet connection through an internet gateway
•	An internet connection in a private subnet through a NAT device
•	A VPC endpoint to an AWS service; for example, an endpoint to Amazon S3.
•	(IPv6) A ClassicLink connection. You can enable IPv4 communication between a linked EC2-Classic instance and instances in a VPC on the other side of a VPC peering connection. However, IPv6 is not supported in EC2-Classic, so you cannot extend this connection for IPv6 communication.

VPC endpoints
•	An Interface endpoint uses AWS PrivateLink and is an elastic network interface (ENI) with a private IP address that serves as an entry point for traffic destined to a supported service
•	A gateway endpoint is a gateway that is a target for a specified route in your route table, used for traffic destined to a supported AWS service
•	By default, IAM users do not have permission to work with endpoints
•	You can create an IAM user policy that grants users the permissions to create, modify, describe, and delete endpoints
•	Gateway endpoints are available for:
o	DyanmoDB
o	S3
Regional Edge Location
•	An edge location is the location where content is cached (separate to AWS regions/AZs)
•	Regional Edge Caches are located between origin web servers and global edge locations and have a larger cache
•	Regional Edge caches are used for custom origins, but not Amazon S3 origins
•	Dynamic content goes straight to the origin and does not flow through Regional Edge caches
•	An origin is the origin of the files that the CDN will distribute
•	Origins can be either an S3 bucket, an EC2 instance, an Elastic Load Balancer, or Route 53 – can also be external (non-AWS)
•	A custom origin server is a HTTP server which can be an EC2 instance or an on-premise/non-AWS based web server
•	Objects are cached for 24 hours by default
•	CloudFront keeps persistent connections open with origin servers
Route53
•	Amazon Route 53 is a highly available and scalable Domain Name System (DNS) service
•	Route 53 offers the following functions:
•	Domain name registry
•	DNS resolution
•	Health checking of resources
•	Changes to Name Servers may not take effect for up to 48 hours due to the DNS record Time To Live (TTL) values
•	AWS offer a 100% uptime SLA for Route 53
•	There is a default limit of 50 domain names but this can be increased by contacting support

Hosted Zones
•	A hosted zone is a collection of records for a specified domain
•	A hosted zone is analogous to a traditional DNS zone file; it represents a collection of records that can be managed together
•	There are two types of zones:
•	Public host zone – determines how traffic is routed on the Internet
•	Private hosted zone for VPC – determines how traffic is routed within VPC (resources are not accessible outside the VPC)
•	Health checks can be pointed at:
•	Endpoints
•	Status of other health checks
•	Status of a CloudWatch alarm
API Gateway
•	All of the APIs created with Amazon API Gateway expose HTTPS endpoints only 
•	Supported data formats include JSON, XML, query string parameters, and request headers
•	Can enable Cross Origin Resource Sharing (CORS) for multiple domain use with Javascript/AJAX
•	API Gateway provides several features that assist with creating and managing APIs are Metering, Security, Resiliency, Operations Monitoring, Lifecycle Management
•	API Gateway allows you to maintain a cache to store API responses
•	SDK Generation for iOS, Android and JavaScript
•	Request/response data transformation and API mocking
•	Provides Swagger support
•	With Amazon API Gateway, you only pay when your APIs are in use
Direct Connect
•	Each AWS Direct Connect connection can be configured with one or more virtual interfaces (VIFs)
•	Public VIFs allow access to public services such as S3, EC2, and DynamoDB
•	Private VIFs allow access to your VPC
•	From Direct Connect you can connect to all AZs within the region
•	You can only have one 0.0.0.0/0 (all IP addresses) entry per route table
•	You can bind multiple ports for higher bandwidth
•	Available in 1Gbps and 10Gbps
•	Speeds of 50Mbps, 100Mbps, 200Mbps, 300Mbps, 400Mbps, and 500Mbps can be purchased through AWS Direct Connect Partners
•	For HA you must have 2 DX connections – can be active/active or active/standby
•	You cannot extend your on-premise VLANs into the AWS cloud using Direct Connect
•	Can aggregate up to 4 Direct Connect ports into a single connection using Link Aggregation Groups (LAG)
Amazon CloudWatch
•	Amazon CloudWatch is a monitoring service for AWS cloud resources and the applications you run on AWS
•	CloudWatch is for performance monitoring (CloudTrail is for auditing)
•	Monitor resources such as:
•	EC2 instances
•	DynamoDB tables
•	RDS DB instances
•	Custom metrics generated by applications and services
•	Any log files generated by your applications
•	Alarms can be used to monitor any Amazon CloudWatch metric in your account
•	Basic monitoring = 5 mins (free for EC2 Instances, EBS volumes, ELBs and RDS DBs)
•	Detailed monitoring = 1 min (chargeable)
•	There is no standard metric for memory usage on EC2 instances
•	Amazon CloudWatch uses Amazon SNS to send email
•	CloudWatch retains metric data as follows: 
•	Data points with a period of less than 60 seconds are available for 3 hours. These data points are high-resolution custom metrics.
•	Data points with a period of 60 seconds (1 minute) are available for 15 days.
•	Data points with a period of 300 seconds (5 minute) are available for 63 days.
•	Data points with a period of 3600 seconds (1 hour) are available for 455 days (15 months).
•	The CloudWatch Logs agent provides an automated way to send log data to CloudWatch Logs from Amazon EC2 instances. The agent is comprised of the following components:
•	A plug-in to the AWS CLI that pushes log data to CloudWatch Logs.
•	A script (daemon) that initiates the process to push data to CloudWatch Logs.
•	A cron job that ensures that the daemon is always running.
 CloudTrail
•	AWS CloudTrail is a web service that records activity made on your account and delivers log files to an Amazon S3 bucket
•	CloudTrail records account activity and service events from most AWS services and logs the following records:
•	The identity of the API caller
•	The time of the API call
•	The source IP address of the API caller
•	The request parameters
•	The response elements returned by the AWS service
•	CloudTrail log file integrity validation feature allows you to determine whether a CloudTrail log file was unchanged, deleted, or modified since CloudTrail delivered it to the specified Amazon S3 bucket
•	CloudTrail is per AWS account
•	Trails can be configured to log data events and management events
•	CloudTrail log files are encrypted using S3 Server Side Encryption (SSE)
•	You can also enable encryption using SSE KMS for additional security
•	CloudTrail is for auditing (CloudWatch is for performance monitoring)
•	CloudTrail is about logging and saves a history of API calls for your AWS account
•	CloudTrail provides visibility into user activity by recording actions taken on your account
•	CloudWatch is used to collect and track metrics, collect and monitor log files, and set alarms
OpsWorks
•	AWS OpsWorks is a configuration management service that provides managed instances of Chef and Puppet 
•	OpsWorks is an automation platform that transforms infrastructure into code
•	OpsWorks consists of Stacks and Layers
Cloud Formation
•	AWS CloudFormation provides a common language for you to describe and provision all the infrastructure resources in your cloud environment
•	CloudFormation can be used to provision a broad range of AWS resources
•	Think of CloudFormation as deploying infrastructure as code
•	Elastic Beanstalk is more focussed on deploying applications on EC2 (PaaS)
•	CloudFormation can deploy Elastic Beanstalk-hosted applications however the reverse is not possible
•	Provides WaitCondition function
•	Can create roles in IAM
•	VPCs can be created and customized
•	VPC peering in the same AWS account can be performed
Amazon Elastic Transcoder
•	Amazon Elastic Transcoder is a highly scalable, easy to use and cost effective way for developers and businesses to convert (or “transcode”) video and audio files from their source format into versions that will playback on devices like smartphones, tablets and PCs
•	You are charged based on the duration of the content and the resolution or format of the media
•	Picks up files from an input S3 bucket and saves the output to an output S3 bucket
Amazon Kinesis
•	Amazon Kinesis makes it easy to collect, process, and analyze real-time, streaming data so you can get timely insights and react quickly to new information
•	There are four types of Kinesis service and these are Kinesis Video Streams, Kinesis Data Streams, Kinesis Data Firehose & Kinesis Data Analytics
•	Kinesis Video Stream
•	Durably stores, encrypts, and indexes video data streams, and allows access to data through easy-to-use APIs
•	Stores data for 24 hours by default, up to 7 days
•	Stores data in shards – 5 transaction per second for reads, up to a max read rate of 2MB per second and 1000 records per second for writes up to a max of 1MB per second
•	Kinesis Data Streams 
•	High-level architecture
	Producers continually push data to Kinesis Data Streams
	Consumers process the data in real time
	Consumers can store their results using an AWS service such as Amazon DynamoDB, Amazon Redshift, or Amazon S3
	Kinesis Streams applications are consumers that run on EC2 instances
	Shards are uniquely identified groups or data records in a stream
	Records are the data units stored in a Kinesis Stream
	By default, records of a stream are accessible for up to 24 hours from the time they are added to the stream (can be raised to 7 days  by enabling extended data retention)
•	The maximum size of a data blob (the data payload before Base64-encoding) within one record is 1 megabyte (MB)
•	A shard is the base throughput unit of an Amazon Kinesis data stream
•	One shard provides a capacity of 1MB/sec data input and 2MB/sec data output
•	Each shard can support up to 1000 PUT records per second
•	A stream is composed of one or more shards
•	The time period from when a record is added to when it is no longer accessible is called the retention period. 
Kinesis Firehose
•	Kinesis Data Firehose is the easiest way to load streaming data into data stores and analytics tools
•	Kinesis Data Streams can be used as the source(s) to Kinesis Data Firehose
•	Firehose synchronously replicates data across three AZs as it is transported to destinations
•	Each delivery stream stores data records for up to 24 hours
•	The maximum size of a record (before Base64-encoding) is 1000 KB
•	Firehose Destinations include:
•	Amazon S3
•	Amazon Redshift
•	Amazon Elasticsearch Service
•	Splunk
•	Can encrypt data with an existing AWS Key Management Service (KMS) key
•	Server-side-encryption can be used if Kinesis Streams is used as the data source
Kinesis Data Analytics
•	Amazon Kinesis Data Analytics is the easiest way to process and analyze real-time, streaming data
•	Can use standard SQL queries to process Kinesis data streams
•	A Kinesis Data Analytics application consists of three components:
	Input – the streaming source for your application
	Application code – a series of SQL statements that process input and produce output
	Output – one or more in-application streams to hold intermediate results
•	Kinesis Data Analytics supports two types of inputs: streaming data sources and reference data sources:
Amazon Workspaces
•	Amazon WorkSpaces is a managed desktop computing service running on the AWS cloud
•	Workspaces are persistent
•	The user volume (D:) on the WorkSpace is backed up every 12 hours
•	You do not need an AWS account to login to workspaces
AWS IAM
•	IAM is not used for application-level authentication
•	Identity Federation (including AD, Facebook etc.) can be configured allowing secure access to resources in an AWS account without creating an IAM user account
•	It is a best practice to always setup multi-factor authentication on the root account
•	IAM is universal (global) and does not apply to regions
•	IAM is eventually consistent
•	You can allow users and services to assume a role
•	Groups are collections of users and have policies attached to them
•	A group is not an identity and cannot be identified as a principal in an IAM policy and VPC is also not a principal
•	IAM replicates data across multiple data centres around the world
•	Power user access allows all permissions except the management of groups and users in IAM
•	Temporary security credentials consist of the AWS access key ID, secret access key, and security token
•	IAM can assign temporary security credentials to provide users with temporary access to services/resources
•	The sign-in URL includes the account ID or account alias, e.g:
	https://My_AWS_Account_ID.signin.aws.amazon.com/console/
•	Principals:
	An entity that can take an action on an AWS resource
	IAM users, roles, federated users, and applications are all AWS principals
•	Requests:
	Principals send requests via the Console, CLI, SDKs, or APIs
	Requests are:
	Actions (or operations) that the principal wants to perform
	Resources upon which the actions are performed
	Principal information including the environment from which the request was made
•	Authentication:
	A principal sending a request must be authenticated to send a request to AWS
	To authenticate from the console, you must sign in with your user name and password
	To authenticate from the API or CLI, you must provide your access key and secret key
•	Authorization:
	IAM uses values from the request context to check for matching policies and determines whether to allow or deny the request
	IAM policies are stored in IAM as JSON documents and specify the permissions that are allowed or denied
•	IAM policies can be:
•	User (identity) based policies
•	Resource-based policies
•	Best practice for root accounts:
	Don’t use the root user credentials
	Don’t share the root user credentials
	Create an IAM user and assign administrative permissions as required
	Enable MFA
•	IAM users can be created to represent applications and these are known as “service accounts”
•	You can have up to 5000 users per AWS account
•	You should create individual IAM accounts for users (best practice not to share accounts)
•	Groups
	Groups are collections of users and have policies attached to them
	A group is not an identity and cannot be identified as a principal in an IAM policy
	You cannot nest groups (groups within groups)
•	A role can be assigned to a federated user who signs in using an external identity provider
•	IAM users or AWS services can assume a role to obtain temporary security credentials that can be used to make AWS API calls
•	Only one role can be assigned to an EC2 instance at a time
•	Applications retrieve temporary security credentials from the instance metadata
•	Policies are documents that define permissions and can be applied to users, groups and roles
•	A permissions policy must also be attached to the user in the trusted account
•	AWS recommends using Cognito for identity federation with Internet identity providers
AWS STS (Security Token Service)
•	The AWS Security Token Service (STS) is a web service that enables you to request temporary, limited-privilege credentials for IAM users or for users that you authenticate (federated users)
•	By default, AWS STS is available as a global service, and all AWS STS requests go to a single endpoint at https://sts.amazonaws.com
•	The AWS STS API action returns temporary security credentials that consist of:
•	An access key which consists of an access key ID and a secret ID
•	A session token
•	Expiration or duration of validity
•	Users (or an application that the user runs) can use these credentials to access your resources
IAM Best Practices
•	Lock away the AWS root user access keys
•	Use groups to assign permissions to IAM users
•	Enable MFA for privileged users
•	Use roles for applications that run on AWS EC2 instances
•	Delegate by using roles instead of sharing credentials
•	Use policy conditions for extra security
•	Monitor activity in your AWS account
•	AWS Key Management Service (AWS KMS) 
o	KMS is a managed service that makes it easy for you to create and control the encryption keys used to encrypt your data. The master keys that you create in AWS KMS are protected by FIPS 140-2 validated cryptographic modules. AWS KMS is integrated with most other AWS services that encrypt your data with encryption keys that you manage. AWS KMS is also integrated with AWS CloudTrail to provide encryption key usage logs to help meet your auditing, regulatory and compliance needs.
Elastic network interface
•	Elastic network interface (ENI) is a logical networking component in a VPC that represents a virtual network card. You can attach a network interface to an EC2 instance in the following ways:
•	When it's running (hot attach)
•	When it's stopped (warm attach)
•	When the instance is being launched (cold attach).
Amazon Data Lifecycle Manager
•	Amazon DLM is used to automate the creation, retention, and deletion of snapshots taken to back up your Amazon EBS volumes. Automating snapshot management helps you to:
•	Protect valuable data by enforcing a regular backup schedule.
•	Retain backups as required by auditors or internal compliance.
•	Reduce storage costs by deleting outdated backups.
•	Combined with the monitoring features of Amazon CloudWatch Events and AWS CloudTrail, Amazon DLM provides a complete backup solution for EBS volumes at no additional cost
SNS
•	SNS supports a wide variety of needs including event notification, monitoring applications, workflow systems, time-sensitive information updates, mobile applications, and any other application that generates or consumes notifications
•	SNS Subscribers:
•	HTTP/HTTPS
•	Email/Email-JSON
•	SQS
•	Application
•	Lambda
•	SNS supports notifications over multiple transport protocols:
•	HTTP/HTTPS – subscribers specify a URL as part of the subscription registration
•	Email/Email-JSON – messages are sent to registered addresses as email (text-based or JSON-object)
•	SQS – users can specify an SQS standard queue as the endpoint
•	SMS – messages are sent to registered phone numbers as SMS text messages
•	SNS supports CloudTrail auditing for authenticated calls
•	Amazon SNS topic - Topic names are limited to 256 characters. Alphanumeric characters plus hyphens (-) and underscores (_) are allowed. Topic names must be unique within an AWS account. After you delete a topic, you can reuse the topic name. When a topic is created, Amazon SNS will assign a unique ARN (Amazon Resource Name) to the topic, which will include the service name (SNS), region, AWS ID of the user and the topic name
•	For sending mails/notification, you should use SNS instead of SES (Simple Email Service) when you want to monitor your EC2 instances.
SQS
•	Messages are 256KB in size
•	Messages can be kept in the queue from 1 minute to 14 days (default is 4 days)
•	The visibility timeout is the amount of time a message is invisible in the queue after a reader picks up the message
•	If a job is processed within the visibility timeout the message will be deleted
•	The maximum visibility timeout for an Amazon SQS message is 12 hours
•	An Amazon SQS message can contain up to 10 metadata attributes
•	Queue names must be unique within a region
•	Long polling - ReceiveMessageWaitTime is set to a non-zero value (up to 20 seconds)
•	Short polling - ReceiveMessageWaitTime is set to 0
•	SQS is PCI DSS level 1 compliant and HIPAA eligible
•	In-flight messages are messages that have been picked up by a consumer but not yet deleted from the queue
o	Standard queues have a limit of 120,000 in-flight messages per queue
o	FIFO queues have a limit of 20,000 in-flight messages per queue
•	Queue names can be up to 80 characters
•	Messages are retained for 4 days by default up to 14 days
•	FIFO queues support up to 3000 messages per second when batching or 300 per second otherwise
•	The maximum messages size is 256KB
•	CloudWatch considers a queue to be active for up to 6 hours if it contains any messages or if any API action accesses it
•	Amazon Simple Queue Service (SQS) and Amazon Simple Workflow Service (SWF) are the services that you can use for creating a decoupled architecture in AWS
SWF
•	SWF has a completion time of up to 1 year for workflow executions
•	SWF uses a task-oriented API
•	SWF ensures a task is assigned once and never duplicated
•	A domain is a logical container for application resources such as workflows, activities, and executions
•	SWF applications include the following logical components:
o	Domains
o	Workflows
o	Activities
o	Task Lists
o	Workers
o	Workflow Execution

AWS Organizations
•	Root account with organizational units and AWS accounts behind the OU’s
•	Available in two feature sets:
o	Consolidated billing
o	All features
•	Limit of 20 linked accounts for consolidated billing (default)

Active Directory Service for Microsoft Active Directory
•	Fully managed AWS services on AWS infrastructure
•	Best choice if you have more than 5000 users and/or need a trust relationship set up
•	Includes software pathing, replication, automated backups, replacing failed DCs and monitoring
•	Runs on a Windows Server
•	Requires a VPN or Direct Connect connection
•	You can also use Active Directory credentials to authenticate to the AWS management console without having to set up SAML authentication
•	Monitoring provided through CloudTrail, notifications through SNS, daily automated snapshots
•	Two editions:
o	Standard for up to 5000 users and 30,000 directory objects
o	Enterprise for large organizations up to 50,000 objects

Simple AD
•	An inexpensive Active Directory-compatible service with common directory features
•	Standalone, fully managed, directory on the AWS cloud
•	Simple AD is generally the least expensive option
•	Best choice for less than 5000 users and don’t need advanced AD features
•	AWS provides monitoring, daily snapshots, and recovery services
•	Available in two editions:
o	Small – supports up to 500 users (approximately 2000 objects)
o	Large – supports up to 5000 users (approximately 20,000 objects)
•	Not compatible with RDS SQL server
•	Does not support trust relationships with other domains (use AWS MS AD)
AD Connector
•	AD Connector is a directory gateway for redirecting directory requests to your on-premise Active Directory
•	Connects your existing on-premise AD to AWS
•	Best choice when you want to use an existing Active Directory with AWS services
•	AD Connector comes in two sizes:
o	Small – designed for organizations up to 500 users
o	Large – designed for organizations up to 5000 users
•	The VPC must be connected to your on-premise network via VPN or Direct Connect
•	Not compatible with RDS SQL
•	You can use AD Connector for multi-factor authentication using RADIUS-based MFA infrastructure

AWS Support Plans
•	There are 4 types of AWS support plans:
o	Basic
o	Developer
o	Business
o	Enterprise

Miscellaneous
•	Use access levels to review IAM permissions
•	You cannot restore a snapshot of a root volume without downtime
•	Encryption has to be done during volume creation. You cannot create an encrypted snapshot of an unencrypted volume or change existing volume from unencrypted to encrypted
•	You are limited to an aggregate of 100 TiB of PIOPS volumes per region
•	There is no additional charge for AWS CloudFormation
•	AWS Shield Standard, when used with Amazon CloudFront and Amazon Route 53, provides comprehensive protection against all known infrastructure layer (layer 3 and layer 4) attacks. For additional protection against application layer (layer 7) attacks, use AWS WAF to apply custom mitigation rules.
•	Randomizing object names provides no value in upload speed, random prefixes are used for intensive read requests
•	The SSH protocol uses TCP and port 22
•	Windows uses RDP protocol and port 3389
•	The ping command is a type of ICMP traffic 
•	The term pilot light is often used to describe a DR scenario in which a minimal version of an environment is always running in the cloud.
•	Outputs is an optional section of the CloudFormation template that describes the values that are returned whenever you view your stack's properties. 
•	Backups must remain enabled for Read Replicas to work.
•	AWS Trusted Advisor analyzes your AWS environment and provides best practice recommendations in these five categories: Cost Optimization, Performance, Fault Tolerance, Security, and Service Limits.
•	When failing over, Amazon RDS simply flips the canonical name record (CNAME) in Route53 for your DB instance to point at the standby, which in turn is promoted to become the new primary.
•	Instance types comprise varying combinations of CPU, memory, storage, and networking capacity
•	Storage optimized instances are designed for workloads that require high, sequential read and write access to very large data sets on local storage. They are optimized to deliver tens of thousands of low-latency, random I/O operations per second (IOPS) to applications.
•	Typical database block sizes range from 2 KB to 32 KB. Amazon Redshift uses a block size of 1 MB, which is more efficient and further reduces the number of I/O requests needed to perform any database loading or other operations that are part of query execution.
•	RDS synchronously replicates the data to a standby instance in a different Availability Zone (AZ) that is in the same region and not in a different one
•	AWS Key Management Service (KMS) is a multi-tenant, managed service that allows you to use and manage encryption keys. 
•	AWS CloudHSM is a cloud-based hardware security module (HSM) that enables you to easily generate and use your own encryption keys on the AWS Cloud. 
•	Remote Desktop connection to access your EC2 instance, you have to ensure that the Remote Desktop Protocol is allowed in the security group. By default, the server listens on TCP port 3389 and UDP port 3389.
•	Enhanced networking uses single root I/O virtualization (SR-IOV) to provide high-performance networking capabilities on supported instance types. SR-IOV is a method of device virtualization that provides higher I/O performance and lower CPU utilization when compared to traditional virtualized network interfaces
•	There is no additional charge for using enhanced networking.
•	An Elastic IP address is a static IPv4 address designed for dynamic cloud computing. An Elastic IP address is a public IPv4 address, which is reachable from the internet. currently Elastic IP addresses for IPv6 is not supported. An Elastic IP address is for use in a specific region only.
•	Use instance metadata and custom resource tags to track and identify your AWS resource
•	You disable automated backups for a DB instance by setting the backup retention parameter to 0. Disabling automatic backups for a DB instance deletes all existing automated backups for the instance. If you disable and then re-enable automated backups, you are only able to restore starting from the time you re-enabled automated backups.
•	A tag is a label that you or AWS assigns to an AWS resource. Each tag consists of a key and a value. A key can have more than one value. You can use tags to organize your resources, and cost allocation tags to track your AWS costs on a detailed level. AWS provides two types of cost allocation tags, an AWS generated tags and user-defined tags. All tags can take up to 24 hours to appear in the Billing and Cost Management console
•	The route table entries enable EC2 instances in the subnet to use IPv4 to communicate with other instances in the VPC, and to communicate directly over the Internet
•	All data transferred between any type of gateway appliance and AWS storage is encrypted using SSL. By default, all data stored by AWS Storage Gateway in S3 is encrypted server-side with Amazon S3-Managed Encryption Keys (SSE-S3)
•	Throughput Optimized HDD is cheaper than HDD, it is primarily designed and used for frequently accessed, throughput-intensive workloads. Cold HDD perfectly fits for their infrequently accessed data and provides the lowest cost, unlike Throughput Optimized HDD
•	To access EFS file systems from on-premises, you must have an AWS Direct Connect connection between your on-premises datacenter and your Amazon VPC. Amazon EFS does not support access over AWS VPN.
•	When your computing needs change, you can modify your Standard or Convertible Reserved Instances and continue to take advantage of the billing benefit. You can modify the Availability Zone, scope, network platform, or instance size (within the same instance type) of your Reserved Instance. You can also sell your unused instance on the Reserved Instance Marketplace.
•	Amazon EC2 requires Windows boot volumes to use MBR partitioning. As discussed in Partitioning Schemes, this means that boot volumes cannot be bigger than 2 TiB. 
•	Linux boot volumes may be either MBR or GPT, and Linux GPT boot volumes are not subject to the 2-TiB limit.
•	You should avoid booting from a RAID volume. Grub is typically installed on only one device in a RAID array, and if one of the mirrored devices fails, you may be unable to boot the operating system.
•	An Elastic IP address is for use in a specific region only.
•	A spread placement group supports a maximum of seven running instances per Availability Zone.
CIDR
•	To add a CIDR block to your VPC, the following rules apply:
•	The allowed block size is between a /28 netmask and /16 netmask.
•	The CIDR block must not overlap with any existing CIDR block that's associated with the VPC.
•	You cannot increase or decrease the size of an existing CIDR block.
•	To calculate the total number of IP addresses of a given CIDR Block, you simply need to follow the 2 easy steps below. Let's say you have a CIDR block /27: 
•	Subtract 32 with the mask number :  (32 - 27) = 5
•	Raise the number 2 to the power of the answer in Step #1 : 
2^ 5 = (2 * 2 * 2 * 2 * 2) = 32
•	The answer to Step #2 is the total number of IP addresses available in the given CIDR netmask. Don't forget that in AWS, the first 4 IP addresses and the last IP address in each subnet CIDR block are not available for you to use, and cannot be assigned to an instance.
•	The /32 denotes one IP address and the /0 refers to the entire network
•	When you create a VPC, you must specify an IPv4 CIDR block for the VPC. The allowed block size is between a /16 netmask (65,536 IP addresses) and /28 netmask (16 IP addresses). 

CloudWatch Alarm that performs status checks on your EBS volume
•	Volume status checks are automated tests that run every 5 minutes and return a pass or fail status.
o	If all checks pass, the status of the volume is ok.
o	If a check fails, the status of the volume is impaired.
o	If the status is insufficient-data, the checks may still be in progress on the volume.

VPC – EC2 DNS hostname
•	When you launch an EC2 instance into a default VPC, AWS provides it with public and private DNS hostnames that correspond to the public IPv4 and private IPv4 addresses for the instance.
•	However, when you launch an instance into a non-default VPC, AWS provides the instance with a private DNS hostname only. New instances will only be provided with public DNS hostname depending on these two DNS attributes: the DNS resolution and DNS hostnames, that you have specified for your VPC, and if your instance has a public IPv4 address.
Disaster recovery strategy - Cost effective and to keep minimum version of application always available
•	The term pilot light is often used to describe a DR scenario in which a minimal version of an environment is always running in the cloud. With AWS you can maintain a pilot light by configuring and running the most critical core elements of your system in AWS. When the time comes for recovery, you can rapidly provision a full-scale production environment around the critical core.

AWSPrivateLink 
•	Use private ips inside vpc to communicate between aws services. You cannot load balance to EC2-Classic Instances when registering their Instance IDs as targets. However, if you link these EC2-Classic instances to the load balancer's VPC using ClassicLink and use the private IPs of these EC2-Classic instances as targets, then you can load balance to the EC2-Classic instances.

User Data
•	When you launch an instance in Amazon EC2, you have the option of passing user data to the instance that can be used to perform common automated configuration tasks and even run scripts after the instance starts
•	You can pass two types of user data to Amazon EC2: shell scripts and cloud-init directives
•	User data is data that is supplied by the user at instance launch in the form of a script
•	User data is limited to 16KB

System Status
•	System status checks detect (StatusCheckFailed_System) problems with your instance that require AWS involvement to repair
•	Instance status checks (StatusCheckFailed_Instance) detect problems that require your involvement to repair
•	You can create Amazon CloudWatch alarms that monitor Amazon EC2 instances and automatically perform an action if the status check fails
