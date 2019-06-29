# ECS

## Overview

The Cfn templates in this directory can be used to set up a service on ECS with continuous delivery via Code Pipelines.

**Assumptions:**

* Your source code is hosted in a Github repository
* Your service is already set up to run with Docker and a Dockerfile already exists in your repository
* You have a domain with the hosted zone set up in Route53

## Usage

### Step 1

Copy the `buildspec.yml` file in this directory into the root directoy of the Github repository of your project. The `buildspec.yml` file assumes that you have a Dockerfile at `app/Dockerfile.prod`. If your Dockerfile is somewhere else in the repository, you will need to edit the path in the `buildspec.yml` to reflect this.

### Step 2

Next, you will need to execute the `cluster.template.yml` Cfn template in this directory. See the commented out lines at the top of this file for an example command of how to do this. This template will set up the infrastructure required to run your ECS service, including the VPC, load balancer, ECS cluster and ECR repository.

### Step 3

Once the ECS cluster is set up, you will need to execute the `service.template.yml` Cfn template. As with the previous template, the command to run is in the comments at the top of this file. This template will set up the actual ECS service and task definition as well as the routing of requests from the load balancer to the ECS service. 

### Step 4

You will now have a working service running on ECS. The next step is to set up the Code Pipeline. For this, first add a directory named `cfn` to the root of your Github repository and copy the `service.template.yml` Cfn template into that directory. You should now alter all the parameters in the `service.template.yml` template in your repo so that the default values are set to the values you want for your application.

### Step 5

Finally, execute the `pipeline.github.template.yml` Cfn template in this directory (after setting the environment variables as per the comments at the top of the file). This will create a pipeline hooked up to your Github repository. From now on, any changes pushed to your Github repository will trigger a deploy on this pipeline, which will deploy the latest version of your application to your ECS cluster.

### Step 6 (optional)

Copy the `pipeline.github.template.yml` and `cluster.template.yml` Cfn templates to the `cfn` directory in your repository and set the default values as appropriately. You can use the templates to update your pipeline and cluster infrastructure as needed in future.

## Future work

* Automate the above steps with a CLI script
* Allow automated geneartion of new projects using the above templates
