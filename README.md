<!-- 














  ** DO NOT EDIT THIS FILE
  ** 
  ** This file was automatically generated by the `build-harness`. 
  ** 1) Make all changes to `README.yaml` 
  ** 2) Run `make init` (you only need to do this once)
  ** 3) Run`make readme` to rebuild this file. 
  **
  ** (We maintain HUNDREDS of open source projects. This is how we maintain our sanity.)
  **















  -->
[![README Header][readme_header_img]][readme_header_link]

[![Cloud Posse][logo]](https://cpco.io/homepage)

# terraform-aws-elasticsearch [![Codefresh Build Status](https://g.codefresh.io/api/badges/pipeline/cloudposse/terraform-modules%2Fterraform-aws-elasticsearch?type=cf-1)](https://g.codefresh.io/public/accounts/cloudposse/pipelines/5d22bfe5a7e22ea3b67ea820) [![Latest Release](https://img.shields.io/github/release/cloudposse/terraform-aws-elasticsearch.svg)](https://github.com/cloudposse/terraform-aws-elasticsearch/releases/latest) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


Terraform module to provision an [`Elasticsearch`](https://aws.amazon.com/elasticsearch-service/) cluster with built-in integrations with [Kibana](https://aws.amazon.com/elasticsearch-service/kibana/) and [Logstash](https://aws.amazon.com/elasticsearch-service/logstash/).


---

This project is part of our comprehensive ["SweetOps"](https://cpco.io/sweetops) approach towards DevOps. 
[<img align="right" title="Share via Email" src="https://docs.cloudposse.com/images/ionicons/ios-email-outline-2.0.1-16x16-999999.svg"/>][share_email]
[<img align="right" title="Share on Google+" src="https://docs.cloudposse.com/images/ionicons/social-googleplus-outline-2.0.1-16x16-999999.svg" />][share_googleplus]
[<img align="right" title="Share on Facebook" src="https://docs.cloudposse.com/images/ionicons/social-facebook-outline-2.0.1-16x16-999999.svg" />][share_facebook]
[<img align="right" title="Share on Reddit" src="https://docs.cloudposse.com/images/ionicons/social-reddit-outline-2.0.1-16x16-999999.svg" />][share_reddit]
[<img align="right" title="Share on LinkedIn" src="https://docs.cloudposse.com/images/ionicons/social-linkedin-outline-2.0.1-16x16-999999.svg" />][share_linkedin]
[<img align="right" title="Share on Twitter" src="https://docs.cloudposse.com/images/ionicons/social-twitter-outline-2.0.1-16x16-999999.svg" />][share_twitter]


[![Terraform Open Source Modules](https://docs.cloudposse.com/images/terraform-open-source-modules.svg)][terraform_modules]



It's 100% Open Source and licensed under the [APACHE2](LICENSE).







We literally have [*hundreds of terraform modules*][terraform_modules] that are Open Source and well-maintained. Check them out! 






## Introduction

This module will create:
- Elasticsearch cluster with the specified node count in the provided subnets in a VPC
- Elasticsearch domain policy that accepts a list of IAM role ARNs from which to permit management traffic to the cluster
- Security Group to control access to the Elasticsearch domain (inputs to the Security Group are other Security Groups or CIDRs blocks to be allowed to connect to the cluster)
- DNS hostname record for Elasticsearch cluster (if DNS Zone ID is provided)
- DNS hostname record for Kibana (if DNS Zone ID is provided)

__NOTE:__ To enable [zone awareness](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-managedomains.html#es-managedomains-zoneawareness) to deploy Elasticsearch nodes into two different Availability Zones, you need to set `zone_awareness_enabled` to `true` and provide two different subnets in `subnet_ids`.
If you enable zone awareness for your domain, Amazon ES places an endpoint into two subnets.
The subnets must be in different Availability Zones in the same region.
If you don't enable zone awareness, Amazon ES places an endpoint into only one subnet.

## Usage


**IMPORTANT:** The `master` branch is used in `source` just as an example. In your code, do not pin to `master` because there may be breaking changes between releases.
Instead pin to the release tag (e.g. `?ref=tags/x.y.z`) of one of our [latest releases](https://github.com/cloudposse/terraform-aws-elasticsearch/releases).


Basic [example](examples/basic)

```hcl
module "elasticsearch" {
  source                  = "git::https://github.com/cloudposse/terraform-aws-elasticsearch.git?ref=master"
  namespace               = "eg"
  stage                   = "dev"
  name                    = "es"
  dns_zone_id             = "Z14EN2YD427LRQ"
  security_groups         = ["sg-XXXXXXXXX", "sg-YYYYYYYY"]
  vpc_id                  = "vpc-XXXXXXXXX"
  subnet_ids              = ["subnet-XXXXXXXXX", "subnet-YYYYYYYY"]
  zone_awareness_enabled  = "true"
  elasticsearch_version   = "6.5"
  instance_type           = "t2.small.elasticsearch"
  instance_count          = 4
  iam_role_arns           = ["arn:aws:iam::XXXXXXXXX:role/ops", "arn:aws:iam::XXXXXXXXX:role/dev"]
  iam_actions             = ["es:ESHttpGet", "es:ESHttpPut", "es:ESHttpPost"]
  encrypt_at_rest_enabled = true
  kibana_subdomain_name   = "kibana-es"

  advanced_options {
    "rest.action.multi.allow_explicit_index" = "true"
  }
}
```






## Makefile Targets
```
Available targets:

  help                                Help screen
  help/all                            Display help for all targets
  help/short                          This help short screen
  lint                                Lint terraform code

```
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| advanced_options | Key-value string pairs to specify advanced configuration options | map(string) | `<map>` | no |
| allowed_cidr_blocks | List of CIDR blocks to be allowed to connect to the cluster | list(string) | `<list>` | no |
| attributes | Additional attributes (e.g. `1`) | list(string) | `<list>` | no |
| automated_snapshot_start_hour | Hour at which automated snapshots are taken, in UTC | number | `0` | no |
| availability_zone_count | Number of Availability Zones for the domain to use. | number | `2` | no |
| create_iam_service_linked_role | Whether to create `AWSServiceRoleForAmazonElasticsearchService` service-linked role. Set it to `false` if you already have an ElasticSearch cluster created in the AWS account and AWSServiceRoleForAmazonElasticsearchService already exists. See https://github.com/terraform-providers/terraform-provider-aws/issues/5218 for more info | bool | `true` | no |
| dedicated_master_count | Number of dedicated master nodes in the cluster | number | `0` | no |
| dedicated_master_enabled | Indicates whether dedicated master nodes are enabled for the cluster | bool | `false` | no |
| dedicated_master_type | Instance type of the dedicated master nodes in the cluster | string | `t2.small.elasticsearch` | no |
| delimiter | Delimiter to be used between `namespace`, `environment`, `stage`, `name` and `attributes` | string | `-` | no |
| dns_zone_id | Route53 DNS Zone ID to add hostname records for Elasticsearch domain and Kibana | string | `` | no |
| ebs_iops | The baseline input/output (I/O) performance of EBS volumes attached to data nodes. Applicable only for the Provisioned IOPS EBS volume type | number | `0` | no |
| ebs_volume_size | EBS volumes for data storage in GB | number | `0` | no |
| ebs_volume_type | Storage type of EBS volumes | string | `gp2` | no |
| elasticsearch_version | Version of Elasticsearch to deploy | string | `6.5` | no |
| enabled | Set to false to prevent the module from creating any resources | bool | `true` | no |
| encrypt_at_rest_enabled | Whether to enable encryption at rest | bool | `true` | no |
| encrypt_at_rest_kms_key_id | The KMS key ID to encrypt the Elasticsearch domain with. If not specified, then it defaults to using the AWS/Elasticsearch service KMS key | string | `` | no |
| environment | Environment, e.g. 'prod', 'staging', 'dev', 'pre-prod', 'UAT' | string | `` | no |
| iam_actions | List of actions to allow for the IAM roles, _e.g._ `es:ESHttpGet`, `es:ESHttpPut`, `es:ESHttpPost` | list(string) | `<list>` | no |
| iam_authorizing_role_arns | List of IAM role ARNs to permit to assume the Elasticsearch user role | list(string) | `<list>` | no |
| iam_role_arns | List of IAM role ARNs to permit access to the Elasticsearch domain | list(string) | `<list>` | no |
| iam_role_max_session_duration | The maximum session duration (in seconds) for the user role. Can have a value from 1 hour to 12 hours | number | `3600` | no |
| instance_count | Number of data nodes in the cluster | number | `4` | no |
| instance_type | Elasticsearch instance type for data nodes in the cluster | string | `t2.small.elasticsearch` | no |
| kibana_subdomain_name | The name of the subdomain for Kibana in the DNS zone (_e.g._ `kibana`, `ui`, `ui-es`, `search-ui`, `kibana.elasticsearch`) | string | `kibana` | no |
| log_publishing_application_cloudwatch_log_group_arn | ARN of the CloudWatch log group to which log for ES_APPLICATION_LOGS needs to be published | string | `` | no |
| log_publishing_application_enabled | Specifies whether log publishing option for ES_APPLICATION_LOGS is enabled or not | bool | `false` | no |
| log_publishing_index_cloudwatch_log_group_arn | ARN of the CloudWatch log group to which log for INDEX_SLOW_LOGS needs to be published | string | `` | no |
| log_publishing_index_enabled | Specifies whether log publishing option for INDEX_SLOW_LOGS is enabled or not | bool | `false` | no |
| log_publishing_search_cloudwatch_log_group_arn | ARN of the CloudWatch log group to which log for SEARCH_SLOW_LOGS needs to be published | string | `` | no |
| log_publishing_search_enabled | Specifies whether log publishing option for SEARCH_SLOW_LOGS is enabled or not | bool | `false` | no |
| name | Solution name, e.g. 'app' or 'jenkins' | string | `` | no |
| namespace | Namespace, which could be your organization name or abbreviation, e.g. 'eg' or 'cp' | string | `` | no |
| node_to_node_encryption_enabled | Whether to enable node-to-node encryption | bool | `false` | no |
| security_groups | List of security group IDs to be allowed to connect to the cluster | list(string) | `<list>` | no |
| stage | Stage, e.g. 'prod', 'staging', 'dev', OR 'source', 'build', 'test', 'deploy', 'release' | string | `` | no |
| subnet_ids | Subnet IDs | list(string) | - | yes |
| tags | Additional tags (e.g. `map('BusinessUnit','XYZ')` | map(string) | `<map>` | no |
| vpc_id | VPC ID | string | - | yes |
| zone_awareness_enabled | Enable zone awareness for Elasticsearch cluster | bool | `true` | no |

## Outputs

| Name | Description |
|------|-------------|
| domain_arn | ARN of the Elasticsearch domain |
| domain_endpoint | Domain-specific endpoint used to submit index, search, and data upload requests |
| domain_hostname | Elasticsearch domain hostname to submit index, search, and data upload requests |
| domain_id | Unique identifier for the Elasticsearch domain |
| elasticsearch_user_iam_role_arn | The ARN of the IAM role to allow access to Elasticsearch cluster |
| elasticsearch_user_iam_role_name | The name of the IAM role to allow access to Elasticsearch cluster |
| kibana_endpoint | Domain-specific endpoint for Kibana without https scheme |
| kibana_hostname | Kibana hostname |
| security_group_id | Security Group ID to control access to the Elasticsearch domain |





## References

For additional context, refer to some of these links. 

- [What is Amazon Elasticsearch Service](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/what-is-amazon-elasticsearch-service.html) - Complete description of Amazon Elasticsearch Service
- [Amazon Elasticsearch Service Access Control](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-ac.html) - Describes several ways of controlling access to Elasticsearch domains
- [VPC Support for Amazon Elasticsearch Service Domains](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-vpc.html) - Describes Elasticsearch Service VPC Support and VPC architectures with and without zone awareness
- [Creating and Configuring Amazon Elasticsearch Service Domains](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-createupdatedomains.html) - Provides a complete description on how to create and configure Amazon Elasticsearch Service (Amazon ES) domains
- [Kibana and Logstash](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-kibana.html) - Describes some considerations for using Kibana and Logstash with Amazon Elasticsearch Service
- [Amazon Cognito Authentication for Kibana](https://docs.aws.amazon.com/elasticsearch-service/latest/developerguide/es-cognito-auth.html) - Amazon Elasticsearch Service uses Amazon Cognito to offer user name and password protection for Kibana
- [Control Access to Amazon Elasticsearch Service Domain](https://aws.amazon.com/blogs/security/how-to-control-access-to-your-amazon-elasticsearch-service-domain/) - Describes how to Control Access to Amazon Elasticsearch Service Domain
- [elasticsearch_domain](https://www.terraform.io/docs/providers/aws/r/elasticsearch_domain.html) - Terraform reference documentation for the `elasticsearch_domain` resource
- [elasticsearch_domain_policy](https://www.terraform.io/docs/providers/aws/r/elasticsearch_domain_policy.html) - Terraform reference documentation for the `elasticsearch_domain_policy` resource


## Help

**Got a question?** We got answers. 

File a GitHub [issue](https://github.com/cloudposse/terraform-aws-elasticsearch/issues), send us an [email][email] or join our [Slack Community][slack].

[![README Commercial Support][readme_commercial_support_img]][readme_commercial_support_link]

## DevOps Accelerator for Startups


We are a [**DevOps Accelerator**][commercial_support]. We'll help you build your cloud infrastructure from the ground up so you can own it. Then we'll show you how to operate it and stick around for as long as you need us. 

[![Learn More](https://img.shields.io/badge/learn%20more-success.svg?style=for-the-badge)][commercial_support]

Work directly with our team of DevOps experts via email, slack, and video conferencing.

We deliver 10x the value for a fraction of the cost of a full-time engineer. Our track record is not even funny. If you want things done right and you need it done FAST, then we're your best bet.

- **Reference Architecture.** You'll get everything you need from the ground up built using 100% infrastructure as code.
- **Release Engineering.** You'll have end-to-end CI/CD with unlimited staging environments.
- **Site Reliability Engineering.** You'll have total visibility into your apps and microservices.
- **Security Baseline.** You'll have built-in governance with accountability and audit logs for all changes.
- **GitOps.** You'll be able to operate your infrastructure via Pull Requests.
- **Training.** You'll receive hands-on training so your team can operate what we build.
- **Questions.** You'll have a direct line of communication between our teams via a Shared Slack channel.
- **Troubleshooting.** You'll get help to triage when things aren't working.
- **Code Reviews.** You'll receive constructive feedback on Pull Requests.
- **Bug Fixes.** We'll rapidly work with you to fix any bugs in our projects.

## Slack Community

Join our [Open Source Community][slack] on Slack. It's **FREE** for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build totally *sweet* infrastructure.

## Newsletter

Sign up for [our newsletter][newsletter] that covers everything on our technology radar.  Receive updates on what we're up to on GitHub as well as awesome new projects we discover. 

## Office Hours

[Join us every Wednesday via Zoom][office_hours] for our weekly "Lunch & Learn" sessions. It's **FREE** for everyone! 

[![zoom](https://img.cloudposse.com/fit-in/200x200/https://cloudposse.com/wp-content/uploads/2019/08/Powered-by-Zoom.png")][office_hours]

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-elasticsearch/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://cpco.io/help-out) with our other projects, we would love to hear from you! Shoot us an [email][email].

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2020 [Cloud Posse, LLC](https://cpco.io/copyright)



## License 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know by [leaving a testimonial][testimonial]!

[![Cloud Posse][logo]][website]

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We ❤️  [Open Source Software][we_love_open_source].

We offer [paid support][commercial_support] on all of our projects.  

Check out [our other projects][github], [follow us on twitter][twitter], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.



### Contributors

|  [![Erik Osterman][osterman_avatar]][osterman_homepage]<br/>[Erik Osterman][osterman_homepage] | [![Andriy Knysh][aknysh_avatar]][aknysh_homepage]<br/>[Andriy Knysh][aknysh_homepage] | [![Igor Rodionov][goruha_avatar]][goruha_homepage]<br/>[Igor Rodionov][goruha_homepage] | [![Sarkis Varozian][sarkis_avatar]][sarkis_homepage]<br/>[Sarkis Varozian][sarkis_homepage] |
|---|---|---|---|

  [osterman_homepage]: https://github.com/osterman
  [osterman_avatar]: https://img.cloudposse.com/150x150/https://github.com/osterman.png
  [aknysh_homepage]: https://github.com/aknysh
  [aknysh_avatar]: https://img.cloudposse.com/150x150/https://github.com/aknysh.png
  [goruha_homepage]: https://github.com/goruha
  [goruha_avatar]: https://img.cloudposse.com/150x150/https://github.com/goruha.png
  [sarkis_homepage]: https://github.com/sarkis
  [sarkis_avatar]: https://img.cloudposse.com/150x150/https://github.com/sarkis.png

[![README Footer][readme_footer_img]][readme_footer_link]
[![Beacon][beacon]][website]

  [logo]: https://cloudposse.com/logo-300x69.svg
  [docs]: https://cpco.io/docs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=docs
  [website]: https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=website
  [github]: https://cpco.io/github?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=github
  [jobs]: https://cpco.io/jobs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=jobs
  [hire]: https://cpco.io/hire?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=hire
  [slack]: https://cpco.io/slack?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=slack
  [linkedin]: https://cpco.io/linkedin?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=linkedin
  [twitter]: https://cpco.io/twitter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=twitter
  [testimonial]: https://cpco.io/leave-testimonial?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=testimonial
  [office_hours]: https://cloudposse.com/office-hours?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=office_hours
  [newsletter]: https://cpco.io/newsletter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=newsletter
  [email]: https://cpco.io/email?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=email
  [commercial_support]: https://cpco.io/commercial-support?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=commercial_support
  [we_love_open_source]: https://cpco.io/we-love-open-source?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=we_love_open_source
  [terraform_modules]: https://cpco.io/terraform-modules?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=terraform_modules
  [readme_header_img]: https://cloudposse.com/readme/header/img
  [readme_header_link]: https://cloudposse.com/readme/header/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=readme_header_link
  [readme_footer_img]: https://cloudposse.com/readme/footer/img
  [readme_footer_link]: https://cloudposse.com/readme/footer/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=readme_footer_link
  [readme_commercial_support_img]: https://cloudposse.com/readme/commercial-support/img
  [readme_commercial_support_link]: https://cloudposse.com/readme/commercial-support/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-elasticsearch&utm_content=readme_commercial_support_link
  [share_twitter]: https://twitter.com/intent/tweet/?text=terraform-aws-elasticsearch&url=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_linkedin]: https://www.linkedin.com/shareArticle?mini=true&title=terraform-aws-elasticsearch&url=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_reddit]: https://reddit.com/submit/?url=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_facebook]: https://facebook.com/sharer/sharer.php?u=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_googleplus]: https://plus.google.com/share?url=https://github.com/cloudposse/terraform-aws-elasticsearch
  [share_email]: mailto:?subject=terraform-aws-elasticsearch&body=https://github.com/cloudposse/terraform-aws-elasticsearch
  [beacon]: https://ga-beacon.cloudposse.com/UA-76589703-4/cloudposse/terraform-aws-elasticsearch?pixel&cs=github&cm=readme&an=terraform-aws-elasticsearch
