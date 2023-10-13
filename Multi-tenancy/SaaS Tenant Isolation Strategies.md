### The Isolation Mindset

- Isolation is not optional

- Authentication and authorization are not equal to isolation

- Isolation enforcement should not be left to service developers 
	- while developers are never expected to introduce code that might violate isolation, it’s unrealistic to expect that they will never un-intentionally cross a tenant boundary. To mitigate this, scoping of access to resources should be controlled through some shared mechanism that is responsible for applying isolation rules (outside the view of developers)

- If there’s not out-of-the box isolation solution, you may have to build it yourself
	- there may be scenarios where your isolation model is not directly addressed by a corresponding tool or technology. The absence of a clear solution should not represent an opportunity to lower your isolation requirements—even if that means building something of your own.

- Isolation is not a resource-level construct
	- Even in scenarios where resources are shared—in fact, especially in environments where resources are shared—there are ways to achieve isolation. In this shared resource model, isolation can be a logical construct that is enforced by run-time applied policies. The key point here is that isolation should not be equated to having siloed resources.

- Domains may impose specific isolation requirements
	- Some high compliance industries, for example, will require that every tenant have its own database. In these cases, the shared, policy-based approaches to isolation may not be adequate.


### Core Isolation Concepts

Part of the challenge of isolation is that there are multiple definitions of tenant isolation. For some, isolation is almost a business construct where they think about entire customers requiring their own environments. For others, isolation is more of an architectural construct that overlays the services and constructs of your multi-tenant environment.


### Silo Isolation

![[Screenshot 2023-10-09 at 7.41.44 PM.png]]


where each tenant is running a fully siloed stack of resources

this full-stack model does not represent a SaaS environment. However, if you’ve surrounded these separate stacks with shared identity, onboarding, metering, metrics, deployment, analytics, and operations, then we’d still say this is still a valid variant of SaaS that trades economies of scale and operational efficiency for compliance, business, or domain considerations

this model of isolation is a much simpler to enforce.

it can be appealing to those that have very strict isolation requirements.

Pros
	Supporting challenging compliance models
	No noisy neighbor concerns
	Tenant cost tracking
	Limited blast radius

Cons
	Scaling issues
	Cost
	Agility
	Onboarding automation
	Decentralized management and monitoring


### Pool Isolation


![[Screenshot 2023-10-10 at 4.01.04 PM.png]]

In this model, you’ll notice that our tenants are consuming infrastructure that is shared by all tenants. This enables the resources to scale in direct proportion to the actual load being imposed by the tenants.

The key here is that—even though this is a more challenging environment to isolation— you cannot use this as a rationale to relax the isolation requirements of your environment. If anything, these shared model increases the chance for cross-tenant access and, as such, it represents an area that requires you to be especially diligent about ensuring that resources are isolated.

Pros
	Agility
	Cost efficiency
	Simplified management and operations
	Innovation

Cons
	Noisy neighbor
	Tenant cost tracking 
	Blast radius 
	Compliance pushback

### The Bridge Model

As you look at real application problems and you decompose our systems into smaller services, you will often discover that your solution will require a mix of the silo and pool models. This mixed model is what we would refer to as a bridge model of isolation.

![[Screenshot 2023-10-10 at 5.16.35 PM.png]]

### Tier-Based Isolation

![[Screenshot 2023-10-10 at 5.18.11 PM.png]]


In this case, it’s less about how you’re isolating tenants and more about how you might package and offer different flavors of isolation to different tenants with different profiles. Still, this is another consideration that could determine which models of isolation you’ll need to support to address the full spectrum of customers you want to engage.

### Identity and Isolation

![[Screenshot 2023-10-10 at 5.19.30 PM.png]]

This diagram represents a generalization of how identity gets connected to the broader isolation story. Here you’ll notice that, as a user is authenticated, the system will return tenant context back to your application that includes the user’s binding to a tenant as well as the policies that will be used to enforce isolation for that tenant. 

This context then flows through all of our interactions and is used by the downstream elements of the SaaS environment to scope access to resource (in this case a database).

### Implementing Silo Isolation

### Implementing Pool Isolation

There is often a fundamental mismatch between the tools and mechanisms that provide isolation and the nature of tenants consuming a shared resource. This is further complicated by the fact that each resource we need to isolate in the pool model may require a different approach to enforcing isolation. While these challenges are real, they should not represent an opportunity to somehow relax your isolation requirements. This just means you’ll have to work a bit harder to find the right combination of tools and construct to isolate some resources in a pooled model.

### Run-time, Policy-Based Isolation with IAM


![[Screenshot 2023-10-10 at 5.38.20 PM.png]]


In this diagram, you’ll see that we have a microservice that needs to access some downstream resources (databases, S3 buckets, etc.). This microservice was deployed in a pooled model, which means that it will be processing requests from multiple tenants. The job of this microservice is to ensure that, as it processes these requests, it will apply constraints that will prevent tenants from crossing a boundary to another tenant’s resources. In the diagram, you’ll see that our microservice reaches out to the isolation manager to acquire a scoping context that is used to control interactions with and resources that are accessed by the code running in this microservice.


![[Screenshot 2023-10-10 at 5.49.14 PM.png]]

- In the first step of this process, the tenant onboards to your system. 
- During this process, they setup the user for our tenant as well as the IAM policies for that tenant (steps 2 and 3).
- Once the tenant has onboarded, we then hit the microservice of our application (step 4)
- Our job, then, is to look at each request that is sent to this service and narrow the scope of that request based to a single tenant. We do that by asking the isolation manager for a set of credentials that are specific to the current tenant (step 5)
- This isolation manager will look-up the IAM policies for the tenant (step 6) and generate a tenant scoped set of credentials that are returned back to the calling microservice.
- Finally, this microservice uses these credentials to access a database (step 7). With these new tenant-scoped credential, the code of the microservice will be prevented from accessing the resources of another tenant.

### Scaling and Managing Pool Isolation Policies

- If your system has a large number of tenants with a large population of policies, you may find that you will exceed the limits of the IAM service. 
- You may also find it difficult to manage these polices as the number of tenants and the compexity of these policies grow.

One approach to this challenge is to shift to a model where your IAM policies are generated in at run-time. 

The idea here is to have your system implement a mechanism that will examine the current context of a call and generate the required IAM policy onthe-fly.

![[Screenshot 2023-10-10 at 5.55.03 PM.png]]

instead of going directly the IAM to retrieve the policies need to scope access, we have a series of steps that are used to generate a policy.

- isolation manager first makes a request to the token vending machine to get a tenant scoped token (step 1). 
- It’s the job of the vending machine to go to the templates that you have pre-defined for your tenant isolation model (step 2). 
- Think of these as template files that have all of the moving parts of a traditional IAM policy. However, key elements of the file are not filled in (those that represent our tenant context). You might, for example, fill in a table name or the leading key condition of an Amazon DynamoDB table with a tenant identifier. 
- Once you have the template that’s needed, you now call out to the token generator to request a token (step 3). In this step, we also provide the current tenant context. 
- The token generator then fills the tenant details into the template, leaving us with a fully hydrated IAM policy (steps 4 and 5)
- Finally, the token generator uses this policy to generate a token that is scoped according to the provided policy. This token is returned back to the isolation manager (steps 6 and 7). Now, this token can be used to access resources with the tenant context applied.

### Pooled Storage Isolation Strategies

each service may require its own unique approach to implement isolation in the pooled model

As a fully managed storage service, DynamoDB offers you a rich collection of IAM mechanisms to control access to resources. This includes the ability to define a leading key condition in your IAM policy that can restrict access to the items in a DynamoDB table

![[Screenshot 2023-10-10 at 6.30.05 PM.png]]

With Aurora PostgreSQL, you cannot use IAM to scope access to data at the row level. Instead, you’ll need to use the row level security (RLS) feature of PostgreSQL to isolate your tenant data. The diagram in Figure 18 provides a simple example of how you’d setup RLS for a product table in your system.

![[Screenshot 2023-10-13 at 12.42.17 PM.png]]

The first step in configuring RLS is to alter your table to enable row level security for that table. Then, you’ll create an isolation policy for that that requires the tenant_id column to match the value of the current user (which is supplied contextually). Now, with these changes in place, all interactions with this table will be restricted to the rows that are valid for the current tenant.


### Hiding the Details of Pooled Isolation

![[Screenshot 2023-10-13 at 12.44.22 PM.png]]

Here you’ll see that we have two microservices (product and order) that need to acquire credential to comply with the pooled isolation model of our system. What we’ve done here is moved all of the code and details of this process to shared libraries (these are not separate microservices). When our microservice needs scoped credentials, it will call into the isolation manager, passing in a JWT token that that was supplied to the microservices. This isolation manager will then get the tenantId from the token manager, which owns all the logic associated with cracking open the JWT and extracting tenant information. It will then get the policy for this tenant from the policy manager and use that policy to get a set of tenant-scoped credentials. These credentials would then be returned to the calling service.