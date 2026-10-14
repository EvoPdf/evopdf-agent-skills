# AWS: samples

Every code sample of the documentation topics behind this skill, complete and in the order of the topic.

## Using EvoPdf Next for .NET in AWS Elastic Beanstalk on Linux

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-aws-elastic-beanstalk-linux.htm

### Install the Required Packages

**.ebextensions/evopdf.config**

```yaml
packages:
  yum:
    nss: []
    at-spi2-atk: []
    cairo: []
    pango: []
    dejavu-sans-fonts: []
    dejavu-serif-fonts: []
    dejavu-sans-mono-fonts: []
files:
  "/etc/fonts/local.conf":
    mode: "000644"
    owner: root
    group: root
    content: |
      <fontconfig>
        <alias>
          <family>sans-serif</family>
          <prefer><family>DejaVu Sans</family></prefer>
        </alias>
      </fontconfig>
container_commands:
  01_evopdf_loadhtml:
    command: "chmod +x evopdf_runtimes/linux-x64/native/evopdf_loadhtml"
  02_evopdf_pdfprocessor:
    command: "chmod +x evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor"
```

### Raise the Proxy Limits

**.platform/nginx/conf.d/evopdf.conf**

```nginx
proxy_read_timeout 300;
client_max_body_size 50M;
```

### Include the Files in the Publish Output

```xml
<ItemGroup>
  <None Include=".ebextensions\**" CopyToPublishDirectory="PreserveNewest" />
  <None Include=".platform\**" CopyToPublishDirectory="PreserveNewest" />
</ItemGroup>
```

### Publish and Package the Application

```bash
dotnet publish -c Release -r linux-x64 --self-contained false -o publish
```

## Using EvoPdf Next for .NET in AWS Elastic Beanstalk on Windows

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-aws-elastic-beanstalk-windows.htm

### Publish and Package the Application

```bash
dotnet publish -c Release -r win-x64 --self-contained false -o publish
```

**aws-windows-deployment-manifest.json**

```json
{
  "manifestVersion": 1,
  "deployments": {
    "aspNetCoreWeb": [
      {
        "name": "evopdf-app",
        "parameters": {
          "appBundle": "site.zip",
          "iisPath": "/",
          "iisWebSite": "Default Web Site"
        }
      }
    ]
  }
}
```

## Using EvoPdf Next for .NET in AWS Lambda

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-aws-lambda.htm

### Create the Lambda Function Project

**Project file**

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <GenerateRuntimeConfigurationFiles>true</GenerateRuntimeConfigurationFiles>
    <AWSProjectType>Lambda</AWSProjectType>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Amazon.Lambda.Core" Version="2.5.0" />
    <PackageReference Include="Amazon.Lambda.APIGatewayEvents" Version="2.7.1" />
    <PackageReference Include="Amazon.Lambda.Serialization.SystemTextJson" Version="2.4.4" />
    <PackageReference Include="EvoPdf.Next.HtmlToPdf.Linux" Version="14.86.0" />
  </ItemGroup>
</Project>
```

**Function.cs**

```csharp
using Amazon.Lambda.APIGatewayEvents;
using Amazon.Lambda.Core;
using EvoPdf.Next;

[assembly: LambdaSerializer(typeof(Amazon.Lambda.Serialization.SystemTextJson.DefaultLambdaJsonSerializer))]

namespace PdfFunction;

public class Function
{
    public APIGatewayHttpApiV2ProxyResponse FunctionHandler(APIGatewayHttpApiV2ProxyRequest request, ILambdaContext context)
    {
        string url = null;
        request.QueryStringParameters?.TryGetValue("url", out url);

        HtmlToPdfConverter converter = new HtmlToPdfConverter();

        byte[] pdf = string.IsNullOrEmpty(url)
            ? converter.ConvertHtml("<b>Hello World</b> from EvoPdf Next!", null)
            : converter.ConvertUrl(url);

        return new APIGatewayHttpApiV2ProxyResponse
        {
            StatusCode = 200,
            Body = Convert.ToBase64String(pdf),
            IsBase64Encoded = true,
            Headers = new Dictionary<string, string> { ["Content-Type"] = "application/pdf" }
        };
    }
}
```

### Build the Container Image

```bash
dotnet publish -c Release -r linux-x64 --self-contained false -o publish
```

```dockerfile
FROM public.ecr.aws/lambda/dotnet:10

# Install the packages required by the HTML to PDF Converter and the DejaVu font families
RUN dnf install -y nss at-spi2-atk cairo pango dejavu-sans-fonts dejavu-serif-fonts dejavu-sans-mono-fonts && dnf clean all

# Set DejaVu Sans as the sans-serif font
RUN printf '<fontconfig><alias><family>sans-serif</family><prefer><family>DejaVu Sans</family></prefer></alias></fontconfig>' > /etc/fonts/local.conf

# Copy the published function
COPY publish/ ${LAMBDA_TASK_ROOT}

# Ensure execute permissions for the HTML to PDF Converter runtime
RUN chmod +x ${LAMBDA_TASK_ROOT}/evopdf_runtimes/linux-x64/native/evopdf_loadhtml

# The only writable folder of a Lambda function
ENV HOME=/tmp

CMD ["PdfFunction::PdfFunction.Function::FunctionHandler"]
```

```bash
docker build --platform linux/amd64 --provenance=false --sbom=false -t pdf-function .
```

```bash
dotnet publish -c Release -r linux-arm64 --self-contained false -o publish
docker build --platform linux/arm64 --provenance=false --sbom=false -t pdf-function .
```

### Push the Image and Create the Function

```bash
aws ecr create-repository --repository-name pdf-function --region us-east-1
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
docker tag pdf-function:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/pdf-function:latest
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/pdf-function:latest
```

## Using EvoPdf Next for .NET in Amazon ECS on AWS Fargate

Topic: https://www.evopdf.com/help/evopdf-next-dotnet/html/publish-to-amazon-ecs.htm

### Build the Container Image

```bash
dotnet publish -c Release -r linux-x64 --self-contained false -o publish
```

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:10.0

# Install the packages required by the HTML to PDF Converter
RUN apt-get update && \
    apt-get install -y \
        libnss3 \
        libatk-bridge2.0-0 \
        libcairo2 \
        libpango-1.0-0 && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy the published application
COPY publish/ .

# Ensure execute permissions for the HTML to PDF Converter runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_loadhtml

# Ensure execute permissions for the PDF Processor runtime
RUN chmod +x /app/evopdf_runtimes/linux-x64/native/evopdf_pdfprocessor

# Listen on port 8080
ENV ASPNETCORE_URLS=http://+:8080
EXPOSE 8080

# Start the application
ENTRYPOINT ["dotnet", "MyApp.dll"]
```

```bash
docker build --platform linux/amd64 -t pdf-app .
```

### Push the Image to Amazon ECR

```bash
aws ecr create-repository --repository-name pdf-app --region us-east-1
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
docker tag pdf-app 123456789012.dkr.ecr.us-east-1.amazonaws.com/pdf-app:1
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/pdf-app:1
```

### Create the Cluster and the Task Definition

```bash
aws iam create-role --role-name ecsTaskExecutionRole --assume-role-policy-document '{"Version":"2012-10-17","Statement":[{"Effect":"Allow","Principal":{"Service":"ecs-tasks.amazonaws.com"},"Action":"sts:AssumeRole"}]}'
aws iam attach-role-policy --role-name ecsTaskExecutionRole --policy-arn arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy
aws ecs create-cluster --cluster-name pdf-cluster
aws logs create-log-group --log-group-name /ecs/pdf-app
```

**task.json**

```json
{
  "family": "pdf-app",
  "networkMode": "awsvpc",
  "requiresCompatibilities": ["FARGATE"],
  "cpu": "2048",
  "memory": "4096",
  "executionRoleArn": "arn:aws:iam::123456789012:role/ecsTaskExecutionRole",
  "runtimePlatform": { "cpuArchitecture": "X86_64", "operatingSystemFamily": "LINUX" },
  "containerDefinitions": [{
    "name": "pdf-app",
    "image": "123456789012.dkr.ecr.us-east-1.amazonaws.com/pdf-app:1",
    "essential": true,
    "portMappings": [{ "containerPort": 8080 }],
    "logConfiguration": { "logDriver": "awslogs", "options": {
      "awslogs-group": "/ecs/pdf-app", "awslogs-region": "us-east-1", "awslogs-stream-prefix": "pdf" } }
  }]
}
```

```bash
aws ecs register-task-definition --cli-input-json file://task.json
```

### Run the Application

```bash
VPC=$(aws ec2 describe-vpcs --filters Name=isDefault,Values=true --query "Vpcs[0].VpcId" --output text)
SUBNET=$(aws ec2 describe-subnets --filters Name=vpc-id,Values=$VPC Name=default-for-az,Values=true --query "Subnets[0].SubnetId" --output text)
SG=$(aws ec2 create-security-group --group-name pdf-app-sg --description "pdf-app" --vpc-id $VPC --query GroupId --output text)
aws ec2 authorize-security-group-ingress --group-id $SG --protocol tcp --port 8080 --cidr 0.0.0.0/0

TASK=$(aws ecs run-task --cluster pdf-cluster --launch-type FARGATE --task-definition pdf-app \
  --network-configuration "awsvpcConfiguration={subnets=[$SUBNET],securityGroups=[$SG],assignPublicIp=ENABLED}" \
  --query "tasks[0].taskArn" --output text)
aws ecs wait tasks-running --cluster pdf-cluster --tasks $TASK

ENI=$(aws ecs describe-tasks --cluster pdf-cluster --tasks $TASK --query "tasks[0].attachments[0].details[?name=='networkInterfaceId'].value" --output text)
aws ec2 describe-network-interfaces --network-interface-ids $ENI --query "NetworkInterfaces[0].Association.PublicIp" --output text
```

### Troubleshooting

```bash
aws ecs describe-tasks --cluster pdf-cluster --tasks $TASK --query "tasks[0].stoppedReason"
```
