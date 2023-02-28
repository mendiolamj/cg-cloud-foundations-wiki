<img width="1167" alt="Screen Shot 2023-02-28 at 9 40 34 AM" src="https://user-images.githubusercontent.com/25653204/221886615-0276ef1e-6885-4f0e-9eea-0355fbded825.png">


<img width="1148" alt="Screen Shot 2023-02-28 at 9 40 49 AM" src="https://user-images.githubusercontent.com/25653204/221886678-e6b7a55c-2615-483c-a0f9-bdef191b7e4d.png">


<img width="1173" alt="Screen Shot 2023-02-28 at 9 44 38 AM" src="https://user-images.githubusercontent.com/25653204/221887772-ff87bbe4-97b2-4e28-8407-5fc715249601.png">

<img width="866" alt="Screen Shot 2023-02-28 at 9 44 56 AM" src="https://user-images.githubusercontent.com/25653204/221887854-9319d2c5-d7c7-48a8-aa0b-a92876918a37.png">

main.tf example:


```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-west-2"
}

resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}

```

<img width="956" alt="Screen Shot 2023-02-28 at 9 45 53 AM" src="https://user-images.githubusercontent.com/25653204/221888089-80ef16b9-1d31-41e7-976a-1f7af076ec22.png">


<img width="1004" alt="Screen Shot 2023-02-28 at 9 46 10 AM" src="https://user-images.githubusercontent.com/25653204/221888162-18be991b-9980-4979-8c03-ba42a1af2da8.png">


**Give Terraform files logical names:**

```code
main.tf: contains all providers, resources and data sources
variables.tf: contains all defined variables
output.tf: contains all output resources
```


```code
provider.tf: contains the terraform block and provider block
data.tf: contains all data sources
variables.tf: contains all defined variables
locals.tf: contains all local variables
output.tf: contains all output resources
```






