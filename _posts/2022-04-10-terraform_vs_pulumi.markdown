---
layout: default
title: Terraform vs. Pulumi
categories: infrastructure
tags: infrastructure infrastructureascode iac terraform pulumi
excerpt_separator: <!--more-->
---

If you're here, there's a 99% chance you already know what both of these technologies are, so I'm not going to waste your time describing in detail what they are, or why Infrastructure-as-Code is worth your time. Let's get right into it.

<!--more-->

> Note: I am aware that Terraform has a CDK, but this post will only be comparining traditional Terraform to Pulumi.

I've seen a lot of comparision videos / articles on these two, and overwhelmingly they tend to compare surface-level differences (e.g. syntax). My intent here is to go beyond the surface and give a much more useful comparison if you're trying to decide between the two for your project.

# Terraform Overview
- Terraform code is written using HCL (Hashicorp Configuration Language). It looks fairly similar to JSON, and has a very easy to understand syntax.
- Terraform has been around for a long time, it was first released in 2014. Just by nature of it being around for a while, it has great community support and tons of reusable modules and providers. 
    - At the time of this writing, there are "1987 providers, 8867 modules & counting" on the [public registry](https://registry.terraform.io/).
- Terraform uses a [state file](https://www.terraform.io/language/state) to keep track of your resources. This means that, by default, you don't need a third party service in order to use Terraform.
    - Best practice is to [store your state file remotely](https://www.terraform.io/language/state/remote).
    - Terraform Cloud can store this for you as well, and there is a pretty decent [free option](https://cloud.hashicorp.com/products/terraform/pricing).

# Pulumi Overview
- Pulumi code is written using general purpose programming languages (Golang, Python, Typescript, C#, etc.)
- Pulumi is newer, it was released in 2018. Even still, they have done a great job building the community.
    - At the time of this writing, there are 83 "packages" on [Pulumi's registry](https://www.pulumi.com/registry/). In this context, packages are analogous to Terraform providers.
    - Pulumi does not have it's own managed public registry for "modules", however you can look at each language's package manager to get a sense for what's out there. For example, a search of "pulumi" on [pypi](https://pypi.org/search/?q=pulumi) resulted in 144 results.
    - There is also a [Slack community](https://slack.pulumi.com/) with 7000+ members. The Pulumi team appears to be pretty responsive on there.
- Pulumi keeps track of state using the Pulumi Service. By default, this is a cloud service hosted by Pulumi. However, you can host [state using the backend you choose](https://www.pulumi.com/docs/intro/concepts/state/).
- Pulumi has an [Automation API](https://www.pulumi.com/docs/guides/automation-api/).

# How NOT to Choose
Both tools are fantastic, and there are going to be many reasons to choose one or the other. Don't let the following reasons be the deciding factor.

## Syntax
I've seen a lot of comparisons that really focus on the HCL vs programming language syntax. While that is a difference, I don't think it's a reason to choose one or the other. Any decent engineer should be able to quickly pick up the syntax easily in either case.

## But Pulumi is not declarative!
Wrong! Both Pulumi and Terraform are *declarative* infrastructure as code tools. Just because Pulumi code is written in a programming langauge, does not automatically make it imperative. The code surrounding it could be, but the code defining the infrastructure resources is still declarative. You *declare* the state you want your infrastructure to be in, and both tools handle the rest.

## Pulumi lets me do loops and conditionals!
Yes it does, and [so does Terraform](https://www.terraform.io/language/expressions)! Both tools are going to support most (if not all) use cases you'll need here.

# Categories
## Simplicity: Winner - Terraform
I'm giving Terraform the edge here: you write some HCL, run a CLI command, and the resources show up and the state file pops up in your local directory. It's very straighforward and there's no 3rd party service involved. You can add complexity only when you need it (storing state file remotely, using Terraform Cloud, etc.).

Pulumi, by default, requires an account to get started. When you create a new project, it will open a web browser and ask you to login. By default, you use the Pulumi Service to store state, unless you [explicitly opt-out](https://www.pulumi.com/docs/intro/concepts/state/) and use your own state backend. You have to do a bit of research before realizing this is an option. Pulumi takes the approach "we don't want you to shoot yourself in the foot unless you tell us you want to". Depending on what you're looking for, that could be a good or a bad thing.

## Most Powerful: Winner - Pulumi
Pulumi wins the category for 2 primary reasons
- The [Automation API](https://www.pulumi.com/docs/guides/automation-api/)
- Advanced abstraction capabilities

The Automation API is, in my opinion, Pulumi's *killer* feature. This unlocks the ability to put infrastructure code behind custom abstractions. For example, you can create a self-service infrastructure REST API that you provide to other engineers on your team. Or maybe you have a multi-tenant cloud infrastructure, and your customers need isolated resources. Instead of writing custom, imperative code to connect to each provider's API, you can use Pulumi instead to describe the resources that your customers need. Embedding infrastructure code into custom applications is exteremely powerful, and it allows some really interesting use cases!

Programming languages are great at abstractions, and so you get to take advantage of that with Pulumi. If you need something more in-depth than Terraform's modules, Pulumi can help with this. Depending on the language, you can take advantage of interfaces, inheritance, methods, etc.

## Flexibility: Winner - Pulumi
Once again, Pulumi wins this category:
- You can convert Terraform to Pulumi via the [tf2pulumi](https://github.com/pulumi/tf2pulumi) tool.
- You can [reference the Terraform state file](https://www.pulumi.com/docs/guides/adopting/from_terraform/#referencing-terraform-state) directly from Pulumi code.

Both of these points make it really easy to go from Terraform to Pulumi, or use both simultaneously. For example, you could have your shared infrastructure (things like VPC, security groups, etc.) created in Terraform, and have applications reference those outputs and define infrastructure in Pulumi.

Ironically, because Pulumi makes it so easy to work with Terraform, it also makes it less scary to choose Terraform to start with. In other words, it's (relatively) easy to go from Terraform to Pulumi, but really difficult to go from Pulumi to Terraform.

## Reusable Components: Winner - Terraform*
Like mentioned above, the Terraform registry has 8867 modules that you can use. Most likely, any module you'd need for a standard use-case probably exists already. Simply reference the module in your code, and off you go!

> \* It's worth noting that Pulumi has the ability to convert Terraform HCL to Pulumi code via the [tf2pulumi](https://github.com/pulumi/tf2pulumi) tool. So technically, you can use any Terraform module in Pulumi, you just need to go through the steps of converting it.

## Cloud API Coverage: Winner - Pulumi
For the big 3 cloud providers (AWs, Azure, GCP), Pulumi has what they call "native" providers. They essentially wrap the cloud's native API, and can therefore offer same-day, 100% coverage of what the cloud itself offers. This is a pretty big deal if you need the latest and greatest.

Terraform providers need to be kept up to date by the maintainers. For most use cases, this probably is not a big deal, but it's worth calling out.

# What should I choose if...?
## ... I am starting from scratch?
Depends! But starting with Terraform probably boxes you in the least, because you can switch to Pulumi fairly easily later if you decide you need to. See section on [flexibility](#flexibility-winner---pulumi). 

## ... I am already using one of them?
It's probably best to continue using what you're using, unless you have a compelling reason to switch. Both are fantastic.

## ... need to build multi-tenant infrastructure programatically?
Pulumi gives you more power here using the Automation API.

## ... I'm looking to build out self-service infrastructure for my organization?
Pulumi gives you more power here using the Automation API.

# Conclusion
I am genuinely impressed with both of these tools. The teams working on them clearly have put a lot of thought and effort into the developer experience. You won't go wrong with either! It all comes down to your unique use cases and requirements.
