# What is Terraform?

Terraform is an open-source “**Infrastructure as code**” software tool originally developed by HashiCorp, that provides a consistent CLI workflow to manage hundreds of cloud services and also enables to automate and manage your infrastructure and platform and services that run on the platform as well.

It uses a **declarative** language, **Declarative = define WHAT end result you want.**

A declarative coding tool, Terraform enables developers to use a high-level configuration language called HCL (HashiCorp Configuration Language) to describe the desired “end-state” cloud or on-premises infrastructure for running an application. It then generates a plan for reaching that end-state and executes the plan to provision the infrastructure.

# What is Infrastructure as Code (IaC)?

Infrastructure as Code is the process of provisioning and configuring an environment through code instead of manually setting up the required devices and systems. Once code parameters are defined, developers run scripts, and the IaC platform builds the cloud infrastructure automatically.

Such automatic IT setups enable teams to quickly create the desired cloud setting to test and run their software. Infrastructure as Code allows developers to generate any infrastructure component they need, including networks, load balancers, databases, virtual machines, and connection types.

# Why Infrastructure as Code (IaC)?

To better understand the advantages of Terraform, it helps to first understand the benefits of [Infrastructure as Code (IaC)](https://www.ibm.com/cloud/learn/infrastructure-as-code "infrastructure-as-code"). IaC allows developers to codify infrastructure in a way that makes provisioning automated, faster, and repeatable.

**Infrastructure as code can help with the following:**

- **Improve speed:** Automation is faster than manually navigating an interface when you need to deploy and/or connect resources.
- **Improve reliability:** If your infrastructure is large, it becomes easy to misconfigure a resource or provision services in the wrong order. With IaC, the resources are always provisioned and configured exactly as declared.
- **Prevent configuration drift:** Configuration drift occurs when the configuration that provisioned your environment no longer matches the actual environment. (See ‘Immutable infrastructure’ below.)
- **Support experimentation, testing, and optimization:** Because Infrastructure as Code makes provisioning new infrastructure so much faster and easier, you can make and test experimental changes without investing lots of time and resources; and if you like the results, you can quickly scale up the new infrastructure for production.

# Why Terraform?

There are a few key reasons developers choose to use Terraform over other Infrastructure as Code tools:

- **Open source:** Terraform is backed by large communities of contributors who build plugins to the platform. Regardless of which cloud provider you use, it’s easy to find plugins, extensions, and professional support. This also means Terraform evolves quickly, with new benefits and improvements added consistently.
- **Platform agnostic:** Meaning you can use it with any cloud services provider. Most other IaC tools are designed to work with single cloud provider.
- **Immutable infrastructure:** Most Infrastructure as Code tools create mutable infrastructure, meaning the infrastructure can change to accommodate changes such as a middleware upgrade or new storage server. The danger with mutable infrastructure is configuration drift—as the changes pile up, the actual provisioning of different servers or other infrastructure elements ‘drifts’ further from the original configuration, making bugs or performance issues difficult to diagnose and correct. Terraform provisions immutable (an immutable infrastructure is one that can’t be changed) infrastructure, which means that with each change to the environment, the current configuration is replaced with a new one that accounts for the change, and the infrastructure is reprovisioned. Even better, previous configurations can be retained as versions to enable rollbacks if necessary or desired.