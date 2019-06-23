# ECS

**Assumptions:**

* Your source code is hosted in a Guthub repository
* Your service is already set up to run with Docker and a Dockerfile already exists in your repository

To set up a service running on ECS, copy the `buildspec.yml` file in this directory into the root directoy of the Github repository of your project. The `buildspec.yml` file assumes that you have a Dockerfile at `/app/Dockerfile.prod`. If your Dockerfile is somewhere else in the repository, will may need to edit the path in the `buildspec.yml` to reflect this.

Next, you will need to execute the `cluster.template.yml` Cfn template in this directory. See the commented out lines at the top of this file for an example command of how to do this. This template will set up the infrastructure required to run your ECS service, including the VPC, load balancer, and ECS cluster.

Lastly, you will need to execute the `service.github.template.yml` Cfn template. As with the previous template, the command to run is in the comments at the top of this file. This template will set up the actual ECS service and task definition as well as the routing of requests from the load balancer to the ECS service. Additionally, this template configures a CodePipeline to deploy your GitHub project to ECS. However, I will likely separate the CodePipeline CFN from this template in future to facilitate multi-stage deploys.
