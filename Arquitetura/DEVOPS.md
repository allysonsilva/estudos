# DEVOPS

## DEVOPS PRACTICES

### Continuous Integration (CI):

Continuous integration focuses on integrating changes from different sources as soon as possible. Integration covers building, reviewing code quality, and testing. The main idea of CI is finding bugs and conflicts as quickly as possible and solving them early in the software life cycle.

This practice concentrates on integrating the code several times a day, or more commonly, with every commit. With every commit, a hierarchy of tests are run, starting from unit tests to integration tests. Also, software executables are built to check inconsistencies between libraries or dependencies. With validation provided by the test and build results, success or failures indicate whether the codebase works. Tests and builds of the CI could run on on­premise systems or cloud providers. CI systems are expected to be always up and running and be on the lookout for the future commits from developers.

Continuous integration enables software development teams to work collaboratively, without stepping on each other's toes, by automating builds and source code integration to maintain source code integrity. It also integrates with DevOps tools to create an automated code delivery pipeline. Continuous integration helps development teams avoid integration hell where the software works on individual developers’ machines, but it fails when all developers integrate their code. Forrester, one of the top analyst firms, in its latest report on continuous integration tools, identified the top ten tools in the domain: Atlassian Bamboo, AWS CodeBuild, CircleCI, CloudBees Jenkins, Codeship, GitLab CI, IBM UrbanCode Build, JetBrains TeamCity, Microsoft VSTS, and Travis CI.

### Continuous Delivery (CD):

Continuous delivery focuses on delivering and packaging the software under test as soon as possible. Similar to CI, CD aims to create production­ready packages and deploy them to the production environment. With this idea, all changes will be in the service of customers, and developers will be able to see their recent commits live.

CD is an extension of CI to automatically deliver or deploy packages that have been validated by CI. CD focuses on creating and publishing software packages and Docker containers in the cloudnative world automatically with every commit. CD focuses on updating applications on the customer side or public clouds with the latest packages automatically. In this book, both continuous delivery and deployment topics are covered and abbreviated as CD.

The continuous delivery tools bundle applications, infrastructure, middleware, and their supporting installation processes and dependencies into release packages that transition across the lifecycle. The objective of this is to keep the code in a deployable state all the time. The latest Forrester report on continuous delivery and release automation highlighted 15 most significant vendors in the domain: Atlassian, CA Technologies, Chef Software, Clarive, CloudBees, Electric Cloud, Flexagon, Hewlett Packard Enterprise (HPE), IBM, Micro Focus, Microsoft, Puppet, Red Hat, VMware, and XebiaLabs. However, if we look at the continuous delivery tools that are primarily supporting Kubernetes, the tools such as Weave Cloud, Spinnaker (by Netflix), Codefresh , Harness , and GoCD are quite popular.

### Continuous Deployment

Continuous deployment is a process that takes the artifacts produced by the continuous delivery process and deploys into a production setup, ideally every time a developer updates the code! Organizations that follow continuous deployment practices deploy code into production more than one hundred times a day. Blue-green, A/B testing, and canary releases are the three main approaches or practices people follow in continuous deployment.

### Monitoring and logging:

Monitoring metrics and collecting application logs is critical for investigating the causes of problems. Creating notifications over parameters and active control of systems help to create a reliable environment for end users. One of the most crucial points is that monitoring could create a proactive path for finding problems rather than waiting for customers to encounter issues and raise tickets.

### Communication and collaboration:

The communication and collaboration of different stakeholders is crucial to success in DevOps. All tools, procedures, and organizational changes should be implemented to increase communication and cooperation. Knowledge and information sharing with open communication channels between teams enables transparency and leads to successful organizations.

## DEVOPS TOOLCHAIN

The DevOps toolchain enlists practices that connect development and operations teams with the aim of creating a value chain. The main stages of the chain, along with their interconnectivity, are presented as follows:

![The DevOps toolchain](./Images/DevOps/The%20DevOps%20toolchain.png "The DevOps toolchain")

The inputs and outputs of each stage are presented in the following flow chart:

![Detailed steps of the DevOps toolchain](./Images/DevOps/Detailed%20steps%20of%20the%20DevOps%20toolchain.png "Detailed steps of the DevOps toolchain")

When a new project is on the table, the chain originates from planning and then progresses to creating the software. The next steps are verification and packaging. Completion of packaging marks the end of the development phase. Thereafter, operations begin from release, followed by configuration. Any feedback and insights obtained at the end of monitoring feed in to the development phase again, thereby completing the cycle. It is important not to skip any part of the toolchain and create an environment where processes feed each other with complete data. For instance, if monitoring fails to provide accurate information about the production environment, development may not have any idea about the outages in production. The development team will be under the false impression that their application is running and scaling with customer demand. However, if the monitoring feeds planning with accurate information, development teams could plan and fix their problems in scaling. As DevOps tries to remove the barriers between development and operations, meticulous execution of each stage in the DevOps tool chain is crucial. The most natural and expected benefits of DevOps can be summarized as follows:

- **Speed:** The DevOps model and its continuous delivery principles decrease the time to deliver new features to the market.
- **Reliability:** Continuous integration and testing throughout the product's life cycle helps to increase reliability of products. With metrics collected by monitoring systems, applications evolve to be more stable and reliable.
- **Scalability:** Not only software but also infrastructure is managed as code in the DevOps environment. This approach makes it easier to manage, deploy, and scale with customer demand.

DevOps culture, with its best practices and toolchains, provides many benefits to organizations. However, before implementation, understanding the current company's culture and creating a feasible action plan for introducing DevOps is crucial.
