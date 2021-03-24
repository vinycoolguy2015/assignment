**We'll deploy https://github.com/vinycoolguy2015/nodejs-mysql-crud which is forked from https://github.com/chapagain/nodejs-mysql-crud**

  1-Create EC2 IAM role with AWSCodeDeployFullAccess,AWSCodeBuildAdminAccess,AWSCodePipelineFullAccess,AmazonS3FullAccess,AmazonEC2FullAccess,AmazonRDSFullAccess and IAMFullAccess. We’ll assign this role to our Workstation which we’ll use to execute our Terraform code

  2-Launch an EC2 instance with Amazon2 AMI and assign the role we created in Step1.

  3-Execute following commands on our base instance:  
  
      
      sudo yum update -y  
      sudo yum install -y git  
      

  4-Setup default region using aws configure command and set following parameters:  
    
    
    Default region name : us-east-1
   
 
  5- Setup Terraform on base instance using following commands: 
  
  
    wget https://releases.hashicorp.com/terraform/0.14.8/terraform_0.14.8_linux_amd64.zip
    unzip terraform_0.14.8_linux_amd64.zip 
    sudo mv terraform /usr/local/bin
     
  
  6- Create a SSH key on base instance using ssh-keygen command with default parameters.
  
  7- Create a Github personal access token with admin:repo_hook and repo permission.

 8- Execute following commands to create our infrastructure:
  
        cd terraform_files
        terraform init
        terraform plan
        terraform apply --auto-approve
        
  9-Once it’s done, it will create our load balanced nodejs application along with Blue/Green deployment setup using AWS CodePipeline.
  
  10-You can update the branch and corresponding build pipeline will be triggered.
  
  
  **Notes**
  
   In terraform.tfvars file we’ve specified environment as Dev,UAT and Production as environment. So make sure you have a dev/uat/production branch in your repo. You can create as many environments as you want as long as the corresponding branch is there in the repo(Check line 81 of https://raw.githubusercontent.com/vinycoolguy2015/awslambda/master/terraform_codepipeline/terraform_files/codepipeline/main.tf).

   As per https://github.com/chapagain/nodejs-mysql-crud, we need to create a table named users. This process is manual. We need to create a temporary bastion server to ssh into webserver instance.Once we are logged in to webserver, we can connect to RDS and create this table. You can modify the code and use SSM to store our RDS credentials and then use instance userdata to create this table.

   We are using COPY_AUTO_SCALING_GROUP while creating our deployment group. Refer to https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/codedeploy_deployment_group for the issue it causes.
  
  
  
  
  
  
