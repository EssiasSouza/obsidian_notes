

## [Internal Developer Platforms](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/10-platformengineering/11-platformengineering/001-section1-idp-platform#internal-developer-platforms)

Internal Developer Platforms (IDPs) aim to simplify developers' work. They do this by bringing together various tools and products in one place. This helps reduce the mental effort needed to switch between different systems and makes it easier for new team members to get started.

IDPs let developers handle tasks on their own, such as:

- Setting up infrastructure
- Deploying applications
- Starting new services

This frees up platform teams to focus on improving the platform itself.

Gartner predicts that by 2026, 80% of large software companies will have platform engineering teams. These teams will provide reusable tools and services for building applications.
## [Key Goals of Platform Engineering Teams](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/10-platformengineering/11-platformengineering/001-section1-idp-platform#key-goals-of-platform-engineering-teams)

1. Help developers work independently
2. Reduce mental load for developers
3. Create reusable best practices or "golden paths"
4. Automate common tasks like setting up clusters or CI/CD pipelines

## [Benefits of Platforms and IDPs](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/10-platformengineering/11-platformengineering/001-section1-idp-platform#benefits-of-platforms-and-idps)

Organizations often see these advantages:

- **Speed**: Get new apps to customers faster
- **Control**: Ensure safe and secure cloud operations
- **Cost Savings**: Use cloud resources more efficiently
- **Continuous Improvement**: Share learnings across the organization
## [Challenges in Platform Engineering](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/10-platformengineering/11-platformengineering/001-section1-idp-platform#challenges-in-platform-engineering)

Building and maintaining IDPs can be tricky. Some common issues include:

1. **Getting People to Use It**: Convincing developers to adopt the platform can be hard
2. **Finding the Right Level of Detail**: Deciding how much complexity to show users
3. **Overcoming Resistance**: Even good platforms may face resistance from different groups in the organization
4. **Fixing Problems**: Making sure developers can troubleshoot issues effectively

To solve these challenges, platform teams need to:

- Understand what users really need
- Keep improving the platform
- Work closely with development teams

By focusing on these areas, organizations can create powerful IDPs that boost productivity and innovation.
## [Access Your Cloud IDE](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/10-platformengineering/11-platformengineering/001-section1-idp-platform#access-your-cloud-ide)

Let's prepare our developer environment for the subsequent steps in the workshop and explore what tools are available to us. In this workshop, we will extensively use the CLI terminal, so let's open it.



## [Explore Your Code Editor IDE Configured Earlier

](https://catalog.us-east-1.prod.workshops.aws/event/dashboard/en-US/workshop/10-platformengineering/11-platformengineering/001-section1-idp-platform#explore-your-code-editor-ide-configured-earlier)

Refer to the workshop start section on IDE configuration and terminal access.

![Code Editor Terminal](https://static.us-east-1.prod.workshops.aws/96f7e562-bb21-41c9-bd01-e94e07fb83e1/static/images/module1-section1/CodeEditorTerminal.png?Key-Pair-Id=K36Q2WVO3JP7QD&Policy=eyJTdGF0ZW1lbnQiOlt7IlJlc291cmNlIjoiaHR0cHM6Ly9zdGF0aWMudXMtZWFzdC0xLnByb2Qud29ya3Nob3BzLmF3cy85NmY3ZTU2Mi1iYjIxLTQxYzktYmQwMS1lOTRlMDdmYjgzZTEvKiIsIkNvbmRpdGlvbiI6eyJEYXRlTGVzc1RoYW4iOnsiQVdTOkVwb2NoVGltZSI6MTc4NTg0ODc1MX19fV19&Signature=fiJMajrphymC4zx-kskA29fkvXjXUWxuDw1WjYoPw79kqTccjPB4QaG5tKXuQ3wNperxfaPC-QjtMH8JTIlXICT0QDymxMsb6bLR4%7EzLOr-2l-54nMxpbc1MagPNZAGmpf8QxgTfFSb2FxDiQ-J-YPVSrhmR05cVaYL945scfsEOqXSyGBvQqtzceEKkQmq6NMuiTZpkmI8OC2bonXd-NaL67TsccbB8J3k%7Eh9M8QrQAC0jsYyJakcxcGWH5Gzq45JXxetBgmFQTxllN71JX%7Ese3QYj7ZIuMLcGNVOwFlbR2NGDeSNVBms3Y1W6Jw2k2K5fkecxSNnMR8Lm4E74W2w__)

The upcoming sections will involve using the IDE terminal to execute CLI commands.

The platform has been configured with many tools. You can check that everything has been correctly deployed with the command:

```bash
1
argocd-sync
```