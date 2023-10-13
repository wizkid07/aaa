### SaaS is a business model

Technology will be used to realize some of those goals, but SaaS is about putting in place a mindset and model that targets a specific set of business objectives.

Key business objectives that are associated with adopting a SaaS delivery model.

- Agility
	- the idea that they must be ready to continually adapt to the market, customer, and competitive dynamics
- Operational efficiency
	- a culture and tooling that is focused on creating an operational footprint that promotes frequent and rapid release of new features.
	- unified experience that allows you to manage, operate, and deploy all customer environments collectively
- Frictionless onboarding
	- As part of being more agile and embracing growth, you must also place a premium on decreasing any friction in the tenant customer onboarding process. This applies universally to business to business (B2B) and business-to-customer (B2C) customers
- Innovation
	- Moving to SaaS isn’t just about addressing the needs of current customers; it’s about putting in place the foundational elements that allow you to innovate. You want to react and respond to customer needs in your SaaS model
- Market response
	- SaaS moves away from the traditional notion of quarterly releases and twoyear plans. It relies on its agility to give the organization the ability to react and respond to market dynamics in near real-time.
- Growth
	- SaaS is a growth-centric business strategy. Aligning all the moving parts of the organization around agility and efficiency gives SaaS organizations the ability to target a growth model

Being multi-tenant without achieving agility, operational efficiency, or frictionless onboarding, for example, would undermine the success of your SaaS business.

***DEFINITION***

SaaS is a business and software delivery model that gives organizations the ability to offer their solutions in a low-friction, service-centric model that maximizes value for customers and providers. It relies on agility and operational efficiency as pillars of a business strategy that promotes growth, reach, and innovation.

A big part of moving to SaaS means moving away from the one-off customizations that might be part of a traditional software model. Any effort to offer specialization for customers generally takes us away from the core values we are trying to achieve with SaaS.


### You’re a service, not a product

Features and functionality are certainly important to every product, SaaS places more emphasis on the experience customers will have with your service.

In a service-centric model, you think more about how customers are onboarded to your service, how quickly they achieve value, and how rapidly you can introduce features that address customer needs. Details associated with how your service is built, operated, and managed are out of your customer’s view.

We’re in a restaurant, we certainly care about the food, but we also care about the service. How quickly does your server come to your table, how often do they refill your water, how fast does the food come— these are all measures of the service experience. This is the same mindset and value system that should shape how we think about building a SaaS service.

Your backlog of work will now put these experience attributes on equal or higher footing than features and functions.
### The initial motivation

The classic model for packaging and delivering software solutions

![[Screenshot 2023-10-09 at 8.59.02 AM.png]]

each customer’s installation is treated as a standalone environment that is dedicated to that customer. You can imagine how this dynamic can impact the operational footprint of the software provider. The more you allow customers to have one-off environments, the more challenging it becomes to manage, update, and support the varying configurations of each customer.

the collective overhead and impact of this model can begin to fundamentally undermine the success of a software business. The first pain point might be operational efficiency. The incremental staffing and costs associated with bringing on customers begins to erode the margins of the business.

operational issues are just part of the challenge. The real issue is that this model, as it scales, directly begins to impact the business’s ability to release new features and keep pace with the market. When each customer has their own environment, providers must balance a host of update, migration, and customer requirements as they attempt to introduce new capabilities into their system.

The overhead of releasing new features becomes so significant that teams become more focused on the mechanics of testing, and less on the new functionality that drives the innovation of their offering.

### Moving to a unified experience

A conceptual view of an environment where all of the customers are managed, onboarded, billed, and operated through a shared model


![[Screenshot 2023-10-09 at 9.10.08 AM.png]]

The basic idea is that you have a single SaaS environment, and each one of your customers is viewed as a tenant of that environment, consuming the resources they need. A tenant could be a company with many users, or it could correlate directly to an individual user.

The diagram also includes a range of shared services that surround your SaaS environment. These services are global to all of the tenants of your SaaS environment. This means that onboarding and identity, for example, are shared by all tenants of this environment. The same is true for management, operations, deployment, billing, and metrics.

This idea of a unified set of services that are applied universally to all of your tenants is foundational to SaaS. By sharing these concepts, you’re able to address a number of the challenges that were associated with the classic model described above.

somewhat subtle element of this diagram is that all of the tenants in this environment are running the same version of your application.

In the unified model, new features can be deployed to all tenants by a single, shared process. 

manage and monitor your tenants through a common operational experience, allowing new tenants to be added without adding incremental operational overhead. This is a core part of the SaaS value proposition that gives teams the ability to reduce operational expenses, and improve overall organisational agility

Having this shared experience is what allows you to drive the growth, agility, and operational efficiency that is connected to the overall objectives of a SaaS business.

### Control plane vs. application plane

The following diagram divides your SaaS environment into two distinct planes. On the right side is the control plane. This side of the diagram includes all the functionality and services that are used to onboard, authenticate, manage, operate, and analyze a multi-tenant environment

This control plane is foundational to any multi-tenant SaaS model. Every SaaS solution—regardless of application deployment and isolation scheme—must include those services that give you the ability to manage and operate your tenants through a single, unified experience.

![[Screenshot 2023-10-09 at 9.42.43 AM.png]]

If you look inside any one of the core services, for example, you won’t find tenant isolation and the other constructs that are part of your multi-tenant application functionality. These services are global to all tenants.

The left side of the diagram references the application plane of a SaaS environment. This is where the multi-tenant functionality of your application resides.

You’ll also note that we’ve broken out provisioning. This is done to highlight the fact that any provisioning of resources for tenants during onboarding would be part of this application domain

### Core services

- Onboarding
	- Every SaaS solution must provide a frictionless mechanism for introducing new tenants into your SaaS environment
	- Generally, this service orchestrates other services to create users, tenant, isolation policies, provision, and per-tenant resources.
- Tenant
	- The tenant service provides a way to centralize the policies, attributes, and state of tenants. The key is that tenants are not individual users. In fact, a tenant is likely associated with many users.
- Identity
	- SaaS systems need a clear way to connect users to tenants that will bring tenant context to the authentication and authorization experience of their solutions. This influences both the onboarding experience and the overall management of user profiles.
- Billing
	- As part of adopting SaaS, organizations often embrace new billing models. They may also explore integration with third-party billing providers.
- Metrics
	- SaaS teams rely heavily on their ability to capture and analyze rich metric data that brings more visibility to how tenants use their system, how they consume resources, and how their tenants engage their systems.
- Admin user management
	- SaaS systems must support both tenant users and admin users. The admin users represent the administrators of a SaaS provider. T

### Re-defining multi-tenancy

The terms multi-tenancy and SaaS are often tightly connected. In some instances, organizations describe SaaS and multi-tenancy as the same thing. While this might seem natural, equating SaaS and multitenancy tends to lead teams to take a purely technical view of SaaS when, in reality, SaaS is more of a business model than an architecture strategy.

![[Screenshot 2023-10-09 at 1.44.41 PM.png]]

The product service shares all of its resources (compute and storage) with all tenants. This aligns with the classic definition of multi-tenancy. 

However, if you look at the order service, you’ll see that it has shared compute, but separate storage for each tenant.  

The catalog service adds another variation where the compute is separate for every tenant (a separate microservice deployment for each tenant), but it shares the storage for all tenants. 

Variations of this nature are common in SaaS environments. Noisy neighbor, tiering models, isolation needs—these are amongst a list of reasons that you might selectively share or silo parts of your SaaS solution.

This is why it becomes necessary to move away from using the term multi-tenant to characterize SaaS environments. Instead, we can talk about how multi-tenancy is implemented within your application, but avoid using it to characterize a solution as being SaaS. If the term multi-tenant is to be used, it makes more sense to use it to describe the entire SaaS environment as being multi-tenant, knowing that some parts of the architecture may be shared and some may not. Overall, you are still operating and managing this environment in a multi-tenant model.

### The extreme case

![[Screenshot 2023-10-09 at 1.48.11 PM.png]]

Here each tenant has a separate deployment. Even though these tenants are running in a siloed infrastructure, they are still managed and operated collectively. They share one unified onboarding, identity, metrics, billing, and operational experience.

### Removing the single-tenant term

Is the preceding diagram a single-tenant environment? While each tenant technically has its own stack, these tenants are still being operated and managed in a multi-tenant model. This is why the term singletenant is generally avoided. Instead, all environments are characterized as multi-tenant, as they are just implementing some variation of tenancy where some or all of the resources are shared or dedicated.


### Introducing silo and pool

Two terms that we use to characterize the use of resources in a SaaS environment are silo and pool. These terms allow us to label the nature of SaaS environments, using multi-tenant as an over-arching description that can be applied to any number of underlying models.

At the most basic level, the term silo is meant to describe scenarios where a resource is dedicated to a given tenant. Conversely, the pool model is used to describe scenarios where a resource is shared by tenants.

![[Screenshot 2023-10-09 at 1.52.50 PM.png]]

Noisy neighbor, isolation, tiering, and a host of other reasons might influence how and when you choose to apply the silo or pooled model.

### Full stack silo and pool

![[Screenshot 2023-10-09 at 1.56.57 PM.png]]

This mix of silo and pooled models in the same SaaS environment isn’t all that atypical. Imagine, for example, that you have a set of basic tier tenants that pay a moderate price for using your system. These tenants are placed in the pooled environment. Meanwhile, you may also have premium tier tenants that are willing to pay more for the privilege of running in a silo. These customers are deployed with separate stacks

Even in this model, where you may have allowed tenants to run in their own full stack silo, it would be essential that these siloes don’t allow any one-off variation or customisation for these tenants. In all respects, each of these stacks should be running the same configuration of the stack, with the same version of the software. When a new version is released, it’s deployed to the pooled tenant environment, and each of the siloed environments.

### SaaS vs. Managed Service Provider (MSP)


![[Screenshot 2023-10-09 at 3.42.14 PM.png]]

This diagram represents one approach to the MSP model. On the left you’ll see customers that run in the MSP model. Generally, the approach here would be to use whatever automation is available to provision each customer environment, and install the software for that customer.

Generally, as a SaaS business, the success of your offering relies on your ability to be deeply involved in all the moving parts of the experience. This usually means having your finger on the pulse of the onboarding experience, understanding how operational events impact tenants, tracking key metrics and analytics, and being close to your customer. 

In an MSP model where this is handed over to someone else, you may end up being a level removed from the key details that are core to operating a SaaS business.

### SaaS migration

SaaS migration should start with a clear view of the target customers, the service experience, the operational goals, and so on. Having a clearer focus on what your SaaS business needs to look like will have a profound impact on the shape, priorities, and path you take to migrate your solution to SaaS.

This is not a purely technical exercise

![[Screenshot 2023-10-09 at 3.52.02 PM.png]]

Now, as we look at migration, you’ll see that these same shared services play a key role in any migration story. The following diagram provides a conceptual view of the migration landscape.

![[Screenshot 2023-10-09 at 3.55.41 PM.png]]

This diagram represents the target experience for any migration path. It includes all the same shared services that were described previously. 

In the middle is a placeholder for your application. The key idea is that you can land any number of application models in the middle of this environment. Your first step in migration might have each tenant running in its own silo. Or, you may have some hybrid architecture where elements are siloed and other bits of functionality are addressed through a collection of modernized microservices.

Any SaaS migration needs to support these foundational shared services to give your business the ability to operate in a SaaS model. All variations of the application architecture need SaaS identity, for example. You’ll need tenant-aware operations to manage and monitor your SaaS solution.

The general goal is to get your application running in a SaaS model. Then, you can turn your attention to further modernization and refinement of your application. This approach also allows you to move the other parts of your business (marketing, sales, support, and so on) at a faster pace. More importantly, this allows you to begin to engage and collect customer feedback that can be used to shape the ongoing modernization of your environment.

It’s important to note that the shared services that you put in place may not include every feature or mechanism you’ll ultimately need. The main goal is to create the shared mechanisms that are needed at the outset of your migration. This allows you to focus on the elements of the system that are essential to the evolution of your application architecture and your operational evolution.

### SaaS identity

This binding of tenants to users is often referred to as the SaaS identity of your application. As each user authenticates, your identity provider will typically yield a token that includes both the user identity and tenant identity. Connecting tenants to users represents a foundational aspect of your SaaS architecture that has many downstream implications. 

The token from this identity process flows into the microservices of your application and is used to create tenant aware logs, record metrics, meter billing, enforce tenant isolation, and so on. It’s essential that you avoid scenarios that rely on separate, standalone mechanisms that map users to tenants. 

This can undermine the security of your system, and often creates bottlenecks in your architecture.

### Tenant isolation

tenant isolation is separate from general security mechanisms. Your system will support authentication and authorization; however, the fact that a tenant user is authenticated does not mean that your system has achieved isolation. Isolation is applied separately from the basic authentication and authorization that may be part of your application.

Tenant isolation focuses exclusively on using tenant context to limit access to resources. It evaluates the context of the current tenant, and uses that context to determine which resources are accessible for that tenant. It applies this isolation for all users within that tenant. 

This gets more challenging as we look at how tenant isolation is realized across all the different SaaS architecture patterns. In some cases, isolation may be achieved by having entire stacks of resources dedicated to a tenant where network (or more coarse-grained) policies prevent cross-tenant access. In other scenarios, you may have pooled resources (items in an Amazon DynamoDB table) that require more fine-grained policies to control access to the resources.

### Data partitioning

Data partitioning is used to describe different strategies used to represent data in a multi-tenant environment. This term is used broadly to cover a range of different approaches and models that can be used to associate different data constructs with individual tenants. 

Note that there is often a temptation to view data partitioning and tenant isolation as interchangeable. These two concepts are not meant to be equivalent. When we talk about data partitioning, we are talking about how tenant data is stored for individual tenants. Partitioning data does not ensure that the data is isolated. Isolation must still be applied separately to ensure that one tenant can’t access the resources of another tenant.

In a siloed model, you have a distinct storage construct for each tenant with no co-mingled data. 

For pooled partitioning, the data is co-mingled and partitioned based on a tenant identifier that determines which data is associated with each tenant.
### Metering, metrics, and billing

- Metering
	- This concept, while it has many definitions, fits best in the SaaS billing domain. The idea is that you meter tenant activity or consumption of resources to collect the data needed to generate a bill.
- Metrics
	- Metrics represent all the data that you capture to analyze trends across your business, operations, and technology domains. This data is used to across many contexts and roles within the SaaS team.

Now, if we connect these two concepts to examples, you can think of instrumenting your application with specific metering events used to surface the data needed to generate a bill. This could be number of requests, number of active users, or it could map to some aggregate of consumption (requests, CPU, memory) that correlates to some unit that makes sense to your customers. 

In your SaaS environment, you’ll publish these billing events from your application and they will be ingested and applied by the billing construct employed by your SaaS system. This could be a third-party billing system or something custom.

this metric data is published and aggregated into some analytics tooling that allows these different users to build the views of system activity that analyze the aspects of the system that best align with their persona. 

A product owner may want to understand how different tenants are consuming features. 

An architect might need views that help them understand how tenants are consuming infrastructure resources, and so on
### B2B and B2C SaaS

While the markets and customers certainly have different dynamics, the overall principles of SaaS do not somehow change each of these markets

the fundamental underlying values for onboarding are mostly the same. Even if your B2B solution relies on an internal onboarding process, we would still expect that process to be as frictionless and automated as possible. Being B2B doesn’t somehow mean that we’re changing our expectations around time to value for our customers.



