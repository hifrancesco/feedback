### Run locally

- Login
```bash
aws-azure-login --mode=gui --no-prompt
```

- Assume a role
```bash
awsume franky_epa
```

```bash
AWS_PAGER="" aws grafana describe-workspace-authentication --workspace-id g-a1738b1243 | jq '.authentication.saml.configuration.idpMetadata.xml' -r | pbcopy
```

```bash
python -m venv .venv
pip install PyYAML
pip install requests
pip install boto3
pip freeze > requirements.txt
pip install -r requirements.txt
```


```bash
cd .venv/lib/python3.10/site-packages 
zip -r ../../../../package.zip .
cd ../../../../
zip package.zip main.py repositories.yml
unzip -l package.zip
mv package.zip terraform
```

[AWS Python Package Depedencies](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html#python-package-dependencies)

---

## To Do List - Guideline

- [x] POC - Who, What, When, Where, Why, How
  - [x] Write POC script to get data from the GitHub API
  - [x] Reiterate the script to get relevant data from the GitHub API
  - [x] Keep in mind these questions when creating a POC
    - [x] Who is going to consume this data?
      - [x] Can you gather active teams using GitHub Actions?
      - [x] Can you provide useful analytics?
      - [x] Can you expand on this tool with new features?
    - [x] What should the script do with the data?
      - [x] Can you gather all the relevant API endpoints?
      - [x] Can you display workflow metrics?
      - [x] Can your codebase be expanded?
    - [x] Where is the data going to be stored?
      - [x] Can you make it simplistic enough?
      - [x] Can you reduce the costs of storage while maintaining high value?
      - [x] Can you compare old data with new data?
    - [x] Which tools shall I use to collect the data?
      - [x] Can the resources talk to one another?
      - [x] Can these tools be easily maintained?
      - [x] Can you provide value using the available tools?
    - [x] When is the data going to be used?
      - [x] Can you provide hourly, daily, weekly, monthly insights?
      - [x] Can you filter by active, inactive workflows?
      - [x] Can you analyse workflows by teams?
    - [x] Why is the data important?
      - [x] Can you explain the insights to other teams?
      - [x] Can you track usage of this data?
      - [x] Can you add relevant features from users' feedback?
    - [x] How is the data going to be analysed?
      - [x] Can your data be consumed by different platforms?
      - [x] Can your data contain only useful info?
      - [x] Can your data be analysed using different charts?
- [x] MVP - 
  - [x] Deploy manually - automate resources locally using terraform 
    - [x] Deploy Lambda successfully 
      - [x] Zip the package.zip file
      - [x] Add relevant IAM policies to execute the lambda
      - [x] Reiterate the script to make the lambda run and not on the CLI
    - [x] When the lambda is executed, the logs are pushed to CloudWatch
      - [x] Trigger lambda automatically using EventBridge
  - [x] You can use the AWS CLI to to get IAM policies
    - There are two types of IAM policy attachments:
    **Managed Policy** attachments are standalone entities that can be attached to multiple IAM identities
    In this type, you can attach an IAM policy that is managed and created independently from IAM roles or users.
    They attachments are NOT specific to a single identity and CAN be shared or reused across multiple identities.
    **Inline policies** attachments are created and managed within the IAM identity itself. 
    In this type, you can directly attach a policy to a specific IAM user, group, or role.
    These attachments are specific to a single identity and CANNOT be shared or reused across multiple identities.
    [Attach Policies to the IAM role on iam.tf](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy_attachment)
    - [x] IAM hierarchy 
    ```mermaid
    graph TB
    A[Documents] --> B[Policies]
    B --> C[Attachments]
    ```
    - Documents: IAM policies are defined as JSON documents that specify permissions and access control rules.
    - Policies: Policies are the actual IAM policy documents that define permissions. They can be either managed policies or inline policies.
    - Attachment: Attachments refer to the process of associating or linking IAM policies to IAM identities, such as users, groups, or roles. Attachments connect the policies to the relevant identities, allowing them to inherit the defined permissions.

  - [ ] Deploy automatically - automate resources using a GitHub Actions pipeline
  	- [ ] terraform-apply-dev
  	- [ ] terraform-apply-prod
  	- [ ] workflow.yml
    	- [ ] pull_request
    	- [ ] workflow_dispatch

  - [ ] Put it into a module and test it independently (Advanced)


## Goals
- [ ] Definition of Done: Lambda deploys successfully
- [ ] Definition of Done: Cloudwatch displays logs successfully
- [ ] Definition of Done: Grafana analyses data successfully


- [ ] Lambda code can grab data from the GitHub API
- [ ] Lambda outputs logs to CloudWatch
- [ ] Grafana uses CloudWatch as a data source


---

### Project Submission

- [ ]  Declaration Form
  - [ ] Updated KSB mapping with time logged over the 12 weeks
  - [ ] Signed by manager and me
- [ ] Architecture Diagram
- [ ] Team Structure Diagram
- [ ] Specific Technical Outputs
  - [ ] Documentation of work e.g. code snippets, configuration etc.
  - [ ] This can be screenshots, videos, raw files
  - [ ] Cover all techniques used - think about all the tools
you’d like to ensure the assessor recognises
  - [ ] Info can be redacted as necessary to protect any sensitive information
- [ ] A description of how you’re meeting all of the KSBs
	– [ ] By default you can (and should) use the KSB mapping attached to the Declaration Form
	– [ ] But you’re also welcome to add some 300 words to describe the project generally if you think it would be helpful
- [ ] A presentation of evidence ready for the Practical Assessment
    ```txt
    We have an example evidence pack, including an example presentation to give
    you an idea of what you need to put together. Feel free to include more detail
    than our example pack if you have time; the more evidence you can include,
    the less has to ride on the Practical Assessment itself. However, you must still
    have access to all of the systems that you have used for evidence during the
    assessment - including pipelines, repositories, dashboards etc. - so that you can
    demonstrate them to the assessor.
    ```
- [ ] Refer to the `Project Submission and Practical Assessment` for all details.

---

### Code Quality
- [x] Code optimisation of the script 
  - [x] Test it on all workflows using pagination for one repository
  - [x] Test it on all workflows using pagination for all repositories (internal, private)
  - [x] Add unit tests
    - [ ] Write an unit test for the get_authentication_token method
    - [ ] Write an unit test for the get_weekly_log_group_name method
    - [ ] Write an unit test for the get_duration method
    - [ ] Write an unit test the publish_metrics_to_cloudwatch method
    - [ ] Write an unit test for the get_week_time method
    - [ ] Write an unit test for the process_repositories method
  - [x] Add integration tests
    - [ ] Write an integration test for test_get_authentication_token method
    - [ ] Write an integration test for test_get_weekly_log_group_name method
    - [ ] Write an integration test for test_get_duration method
    - [ ] Write an integration test for test_publish_metrics_to_cloudwatch method
    - [ ] Write an integration test for test_get_week_time method
    - [ ] Write an integration test for test_process_repositories method

- [ ] Pass Criteria
- Writes code, both general purpose and infrastructure-as-code (including cloud infrastructure) that is correctly versioned and easy to merge, while adhering to the principles of distributed Source Control.
  - [ ] Refactor the code. Do not change the logic. Take a screenshot of before and after. You must justify why it looks cleaner. 
  - [ ] DRY and KISS principles.
  - [ ] List comprehensions.
  - [ ] Include at least 2 before and after screenshots.
  - [ ] Tools used: Python, Terraform (providers), Git locally and on GitHub, PyPI - a repository and distribution service for open-source Python packages, requirements.txt (alternative is pyproject.toml combined with poetry as a package manager)
> As long as you can justify why it looks cleaner, easier to maintain and scale.
- Demonstrates an iterative approach to evolving code consistent with cloud security best practice, evidenced by a lack of vulnerabilities and that all dependent components are present at run time.
  - [ ] Integrate dependabot for keeping up libraries up to date (dependency management tool)
  - [ ] Include SonarCube for code sniffing or CodeQL.
- Writes code around unit tests, including the appropriate use of test doubles and mocking strategies.
  - [ ] Make sure all unit tests pass.
  - [ ] Integration tests are optional and there is no need to test every single method.
- Explains troubleshooting methods used to identify and resolve issues and gives an example of identifying and remediating an issue that compromised code quality.
  - [ ] Debugging tool from VSCode, logging module, print statement, exception handling with and try-except blocks.

K2 -
K5 - 
K7 -
K14 - 
S9 - 
S11 - 
S14 - 
S17 - 
S18 - 
S20 - 
S22 -

---

### Meeting User Needs

- [ ] Pass Criteria
- Writes user stories that are understandable to a wide range of stakeholders, stand up to scrutiny and lend themselves to a solution based on common architectural patterns - i.e. reducing the number of moving/redundant parts; passes all acceptance tests.
  - [ ] Share at least 3 clean Jira cards following the user story guideline,
  - [ ] One must be optional and that is the should-have criteria for distinction.
- The piece of code meets the ‘must have’ identified functional/non-functional user needs encapsulated in the acceptance criteria for the task.
  - [ ] MUST have 4-5 acceptence criterias e.g. in order for this user story to be satisfied, X and Y features need to work (definition of done).
  - [ ] Include functional and non-functional user needs.
- Creates a quality product in terms of Mean Time To Recovery (MTTR) - i.e. reduced time to fix bugs.
  - [ ] Explain the feedback mechanism for the workflow. What's the journey of that bug fix? What's the lifecycle of this bug fix? What's the procedure?

- [ ] Distinction Criteria
- Produces a piece of code that meets the ‘should have’ identified functional/non-functional user needs encapsulated in the acceptance criteria for the task.
  - [ ] Should have criteria for functional/non-functional user needs
  Compare old data with new data for (historic trail)?
  Send out email (SES)
  - [ ] Include a new dashboard (json) for single repositories

K4 - 
K10 - 
K21 - 
S3 - 

### CI-CD Pipeline

- [ ] Pass Criteria
- Builds a fully functioning, automated CI-CD pipeline with all tests passing.
  - [ ] All tests pass
- Evidences a code commit progressing seamlessly from a build artefact to the end user.
- Explains the pipeline capability, including the benefits of frequent merging of code, in terms of Continuous Integration/Delivery/Deployment.
  - [ ] What does the pipeline do? Explain everything codebase, unit tests, integration tests, credentials, terraform?

K1 - 
K15 -
S15 - 

### Refreshing and Patching

- [ ] Pass Criteria
- Deploys immutable infrastructure that enables the regular recycling of servers and refreshing of associated software based on manual processes.

- [ ] Distinction Criteria
- Fully automates the refreshing and patching process.

- Built infrastructure using terraform
- Lambda (serverless)
- Grafana is a managed service (able to tear it down and bring it back up with SAML authenticated)
- Explain why you used these tools
- not relying on the current state of the infrastructure, and fully automated using the ci-cd pipeline


K8 - 
S5 - 

### Operability

- [ ] Pass Criteria
- Installs and manages monitoring and alerting tools that provide coverage of the infrastructure and applications, including RAM and CPU utilisation, application error rates and availability (health check).
- Configures appropriate alerting thresholds and visualisations. Interprets these in terms of failure scenarios and remedial/follow up actions taken to deliver continuous improvement.

- [ ] Distinction Criteria
- Introduces custom metrics that provide additional improvement areas.
- Explains how these improvement areas may be interpreted, implemented and delivered.

- Measure Lambda failures (Logs)
- Try to track RAM utilisation for a lambda
- Talk to your tutor (AWS x-tray?)

- Check the lambda is NOT ERRORING and that it works (application) (RAM, CPU utilisation)
- Grafana (health check) (availability) HTTPS request 
- Alert if Lambda fails or if Grafana goes down 
- How is the system working? (Distinction) - how long is the lambda working?

K11 - 
S6 - 
S19 - 
B3 - 

### Data Persistence

- [ ] Pass Criteria
- Employs and operates an appropriate data persistence technology, such as database,configuration/infrastructure state management to meet non-functional and functional needs.
- Explains troubleshooting steps taken to locate issues
across the end-to-end service.

- terraform.tfstate file in the S3
- cloudwatch logs of my lambda
- show an example of a troubleshooting (IAM permissions, event bridge, secrets) include use the cli?

K12 - The persistence/data layer, including which database/storage technologies are appropriate to
each platform type and application when considering non-functional and functional needs; e.g.
monolith, microservice, read heavy, write heavy, recovery plans.

S7 - Navigate and troubleshoot stateful distributed systems, in order to locate issues across the end-
to-end service.

### Automation

- [ ] Pass Criteria
- Introduces process efficiencies by automating the setting up/deploying of the project (infrastructure and applications) from scratch, both locally, including all tests, and to a hosted environment.
- [ ] Distinction Criteria
- Identifies an additional opportunity and introduces automation that reduces overall effort.

- Clear documentation, README.md
- Distinction (automated SAML)
- Json Dashboards 

Things that I didn't have to do, but I did them to save time.

K13 - 

K17 -

S12 -

#### Terraform Phase (without GitHub Actions)

- [x] Create an aws iam role with AdministratorAccess e.g. `terraform-service-account`
- [x] Create an S3 bucket manually to store the terraform.tf state file e.g. `github-actions-metrics`
- [x] You can export the credentials in the CLI or put it in the `.env` file and add this to your `.gitignore` file

```
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=
```

- [x] Create a backend.tf file

```
terraform {
  backend "s3" {
    bucket = "github-actions-metrics"
    key    = "terraform.tfstate"
    region = "eu-west-1"
  }
}
```

- [x] Create a providers.tf file

```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

# Configure the AWS Provider
provider "aws" {
  region = "eu-west-1"
}
```

- [x] Create an iam.tf file
- [ ] Create a lambda.tf file
- [ ] Create a cloudwatch.tf file
- [ ] Create a grafana.tf file
- [ ] Create an s3.tf file
- [ ] Create a variables.tf file
- [ ] Create an outputs.tf file
- [ ] Create a main.tf file




- [x] Deploy lambda function with the package.zip file
  - [x] If you are handling exceptions, make sure you raise the exception e.g. `raise e`
  - [x] The print function may only work locally using the CLI
  - [ ] Refactor the code and use logging where possible
    ```py
    try:
      pass
    except Exception as e:
      print(f"Error occurred while checking existing log groups: {str(e)}")
      raise e
    ```
  - [x] Add relevant permissions in the iam.tf file
  - [x] Change the runtime settings on lambda and change the handler to `main.handler` or the equivalent of the `main.py` script
  - [x] The issue was that the main.py script had this running. 
    ```py
    if __name__ == "__main__:
        github_metrics_publisher = GitHubMetricsPublisher()
        github_metrics_publisher.handler(None, None)
    ```
  - [x] This handler function should be outside the class and not be part of the class as a method.
    ```py
      def handler(self, event, context):
        with open("repositories.yml") as file:
            repositories = yaml.safe_load(file)
        self.process_repositories(repositories)
        
    ```
  - [x] It is now refactored.

    ```py
    def handler(event, context):
        with open("repositories.yml") as file:
            repositories = yaml.safe_load(file)
        process_repos = GitHubMetricsPublisher()
        process_repos.process_repositories(repositories)
    ```
  - [x] Make sure your lambda got permissions to upload to CloudWatch
    ```json
    actions = [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams",
        "logs:DeleteLogGroup",
        "logs:PutLogEvents",
    ]
    ```

- [ ] Deploy CloudWatch log group
- [ ] In your code base, logGroupName references to that log group you just deployed using terraform
- [ ] The logstream is created by the lambda function
- [x] logstream name needs to be auto-generated, so that they do not overwrite with another one e.g. one timestamp per day for all github actions minutes
- [x] Include a uuid when creating a logstream name https://docs.python.org/3/library/uuid.html
- [ ] Keep in mind how you are planning on pulling the data
- [ ] Idea: pull github actions minutes from dev and main branch (each needs to have a different logstream name)
- [ ] Set your lambda to run on schedule using event bridge
- [ ] Use terraform to set up event bridge to invoke lambda


### Data Security

- [ ] Pass Criteria
- Builds in security so that all data in transit is encrypted and secure.
- Explains the types of threats and the rationale behind the decision to either encrypt data at rest or not.

- Is my app using TLS, HTTPS for data communication?
- Is Grafana talking to the Enterprise App via TLS?
- Is the lambda making GitHub API calls via HTTPS?
- How is my Grafana talking to CloudWatch?
- Is the terraform.tfstatele encrypted at rest?
- Whenever you are storing data at rest (cloudwatch)? No PII. No names. Just repository names and IDs, workflow IDs, duration
- Are there any data protection concerns?


K16 - 

S10 - 
---

### General Feedback

- [ ] serverless.yml in the root folder
  - iam roles that would be created by serverless would have the least privileges
  - scheduler events (sns based trigger, sqs based trigger)
  - create cloudformation resources
  - do not need to create the dependency package for lambda
  - if anything changes in the code, it just zips it and deploys it

  - [ ] Get workspace SAML authentication working
    - [ ] Configuration settings in Azure, check doc
  - [ ] Get XML file from the Azure application
    - [ ] Store it in SSM
  - [ ] Generate API KEY from grafana dashboard (rotation) longer than 30 days
    - [ ] Create dashboards manually and reverse engineer it

### New Features
- separate workflows by team
- integrate with CloudCity


### Grafana - CloudWatch

- [x] need to find a way to variabilise the weekly log groups because they change weekly in the json file. 
- maybe an AWS CLI query (github actions) to get the latest weekly log group - filter option
- maybe implement a regex expression


```bash
terraform plan -var="weekly_log_group_dates=/aws/lambda/workflow_runs_metrics-2023-06-13_2023-06-19"
terraform apply -var="weekly_log_group_dates=/aws/lambda/workflow_runs_metrics-2023-06-13_2023-06-19"
```

The expected output of the AWS CLI should be -> /aws/lambda/workflow_runs_metrics-2023-06-13_2023-06-19 but the latest one every week




```bash
aws logs describe-log-groups --query "logGroups[?starts_with(logGroupName, '/aws/lambda/workflow_runs_metrics')]"

aws logs describe-log-groups --query 'logGroups[*].logGroupName' --output table

aws logs describe-log-groups | jq -r '.logGroups | sort_by(.creationTime) | reverse | .[0].logGroupName'
```



```bash
latest_weekly_log_group=$(aws logs describe-log-groups | jq -r '.logGroups | sort_by(.creationTime) | reverse | .[0].logGroupName')
echo "latest_log_group_name=$latest_weekly_log_group" >> $GITHUB_OUTPUT
```
- [ ] can you find a better way of running this command?


The latest one should be variabilised 

Need to run the AWS CLI command 

### Learnings

- UID removed, so the dashboards are populated automatically
- Automated queries for user




### Priority: 
- include have a list of 100 repos
- finish ci-cd pipeline
- add dependabot
- mvp unit tests
- lambda failure check