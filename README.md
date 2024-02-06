# Playbooks
The contents of this library are provided for informational purposes only. It represents the current product offerings and practices from Amazon Web Services (AWS) as of the date of issue of this document, which are subject to change without notice. Customers are responsible for making their own independent assessment of the information in this document and any use of AWS products or services, each of which is provided “as is” without warranty of any kind, whether express or implied. This document does not create any warranties, representations, contractual commitments, conditions, or assurances from AWS, its affiliates, suppliers, or licensors. The responsibilities and liabilities of AWS to its customers are controlled by AWS agreements, and this document is not part of, nor does it modify, any agreement between AWS and its customers.

© 2024 Amazon Web Services, Inc. or its affiliates. All Rights Reserved. This work is licensed under a Creative Commons Attribution 4.0 International License.

This AWS Content is provided subject to the terms of the AWS Customer Agreement available at http://aws.amazon.com/agreement or other written agreement between the Customer and either Amazon Web Services, Inc. or Amazon Web Services EMEA SARL or both.

## What this Repository Is
This collections of files is provided as an example framework for customers to create, develop, and integrate security playbooks in preparation for potential attack scenarios when using AWS services. 

These playbooks contains translations performed using machine translation with [AWS Translate](https://aws.amazon.com/translate/). These playbooks have not been reviewed by a human translator and may contain contextual or grammatical errors. Please ensure you review the documents before using to ensure correct context and language for your individual use case. If you would like to contribute to correcting errors and reviewing the errors, we welcome that assistance. 

## Was ist dieses Repository
Diese Dateisammlung wird als Beispiel für Kunden bereitgestellt, um Sicherheits-Playbooks zu erstellen, zu entwickeln und zu integrieren, um sich auf mögliche Angriffsszenarien bei der Nutzung von AWS-Services vorzubereiten. [Spielbücher auf Deutsch befinden Sie hier](./docs/de)

Diese Playbooks enthalten Übersetzungen, die mithilfe maschineller Übersetzung mit [AWS Translate] (https://aws.amazon.com/translate/) durchgeführt wurden. Diese Playbooks wurden nicht von einem menschlichen Übersetzer überprüft und können Fehler enthalten. Bitte überprüfen Sie die Dokumente vor der Verwendung, um sicherzustellen, dass der Kontext und die Sprache für Ihren individuellen Anwendungsfall korrekt sind. Wenn Sie zur Korrektur von Fehlern und zur Überprüfung der Fehler beitragen möchten, freuen wir uns über diese Unterstützung. 

## Qué es este repositorio
Esta colección de archivos se proporciona como un marco de ejemplo para que los clientes creen, desarrollen e integren guías de seguridad a fin de prepararse para posibles situaciones de ataque al utilizar los servicios de AWS. [El plan de respuesta en español se encuentran aquí](./docs/es)

Estos manuales contienen traducciones realizadas mediante traducción automática con [AWS Translate] (https://aws.amazon.com/translate/). Estos manuales de estrategias no han sido revisados por un traductor humano y pueden contener errores. Asegúrese de revisar los documentos antes de usarlos para asegurarse de que el contexto y el idioma son correctos para su caso de uso individual. Si desea contribuir a corregir y revisar los errores, agradecemos su ayuda. 

## Qu'est-ce que ce dépôt
Ces collections de fichiers sont fournies à titre d'exemple de cadre permettant aux clients de créer, de développer et d'intégrer des playbooks de sécurité en vue de scénarios d'attaque potentiels lors de l'utilisation des services AWS. [Les playbooks en français se trouvent ici](./docs/fr)

Ces playbooks contiennent des traductions effectuées à l'aide de la traduction automatique avec [AWS Translate] (https://aws.amazon.com/translate/). Ces manuels n'ont pas été révisés par un traducteur humain et peuvent contenir des erreurs. Assurez-vous de lire les documents avant de les utiliser afin de vous assurer que le contexte et la langue sont corrects pour votre cas d'utilisation individuel. Si vous souhaitez contribuer à la correction des erreurs et à la révision des erreurs, cette assistance est la bienvenue. 

## このリポジトリとは何か
このファイルのコレクションは、AWS のサービスを使用する際の潜在的な攻撃シナリオに備えて、セキュリティプレイブックを作成、開発、統合するためのフレームワークの例として提供されています。[日本のプレイブックはこちら](./docs/ja)

これらのプレイブックには、[AWS Translate] (https://aws.amazon.com/translate/) による機械翻訳を使用して行われた翻訳が含まれています。これらのプレイブックは人間の翻訳者によるレビューを受けていないため、誤りがある可能性があります。使用する前に必ずドキュメントを確認して、個々のユースケースに適したコンテキストと言語を確認してください。エラーの修正やエラーの確認にご協力いただける場合は、その支援を歓迎します。 

## 这个仓库是什么
这些文件集是作为示例框架提供的，供客户创建、开发和集成安全行动手册，为使用 AWS 服务时的潜在攻击场景做好准备。[中文剧本位于此处](./docs/zh)

这些手册包含使用 [AWS 翻译] (https://aws.amazon.com/translate/) 进行机器翻译的翻译。这些剧本未经人工翻译人员审查，可能包含错误。在使用之前，请务必仔细阅读文档，以确保您的个人用例的上下文和语言正确。如果您想为更正错误和审查错误做出贡献，我们欢迎您的帮助。 

## Contributing to the Incident Response Playbooks

### Impostor Syndrome Disclaimer
Before we get into the details: We want your help. No, really.

There may be a little voice inside your head that is telling you that you're not ready to contribute to playbooks; that your skills aren't nearly good enough to contribute. What could you possibly offer a project like this one?
We assure you -- the little voice in your head is wrong. If you can write code at all or have experience with incident response, then we need your contributions! Writing perfect playbooks isn't the measure of a good responder (that would disqualify all of us!); it's trying to create something, making mistakes, and learning from those mistakes. That's how we all improve.

We've provided a clear [playbook creation guide](./docs/Playbook_Creation_Process.md). This outlines the process that you'll need to follow to get a playbook developed and approved for use with Ziplines. By making expectations and process explicit, we hope it will make it easier for you to contribute.
And you don't just have to write code. You can help out by writing documentation, tests, or even by giving feedback about this work. (And yes, that includes giving feedback about everything in this README)

## Playbook Index
* [Playbook Project Disclaimer](./Disclaimer.md)

### Preparation
* [Rationalization of Security Incident Handling](./docs/rationalization_of_security_incident_handling.md)
* [Playbook Development Guide](./docs/Playbook_Development_Guide.md)

#### Administrative
* [Playbook Creation Process](./docs/Playbook_Creation_Process.md)
* [Resource Glossary](./docs/Resource_Glossary.md)

#### Communications 

### Response Scenarios
#### 100-200 Level Scenarios
* [Compromised IAM Credential(s)](./docs/Compromised_IAM_Credentials.md)
* [Denial of Service / Distributed Denial of Service](./docs/Denial_of_Service.md)
* [Inappropriate Public Resources: S3)](./docs/S3_Public_Access.md)
* [Inappropriate Public Resources: RDS)](./docs/RDS_Public_Access.md)
* [Unauthorized Network Changes](./docs/Unauthorized_Network_Changes.md)
* [Simple Email Service Compromise](./docs/Responding_to_SES_Events.md)
* [Identifying Exposure of CodeCommit](./docs/Code_Exposure.md)

#### 300-400 Level Scenarios
* [Bitcoin and Cryptojacking](./docs/Cryptojacking.md)
* [Responding to Ransom in AWS](./docs/Responding_to_Ransom_in_AWS.md)
    * [Amazon Elastic Computing (EC2) Linux/Unix](./docs/Ransom_Response_EC2_Linux.md)
    * [Amazon Elastic Computing (EC2) Microsoft Windows](./docs/Ransom_Response_EC2_Windows.md)
    * [Amazon Relational Database Service (RDS)](./docs/Ransom_Response_RDS.md)
    * [Amazon Simple Storage Service (S3)](./docs/Ransom_Response_S3.md)

### AWS Analysis Tools
* [Analyzing VPC Flow Logs with AWS Athena](./docs/Analyzing_VPC_Flow_Logs.md)
* [Assisted Log Enabler for AWS](https://github.com/awslabs/assisted-log-enabler-for-aws): Assisted Log Enabler for AWS is for customers who do not have logging turned on for various services, and lack knowledge of best practices and/or how to turn them on.
* [AWS Security Analytics Bootstrap](https://github.com/awslabs/aws-security-analytics-bootstrap): AWS Security Analytics Bootstrap enables customers to perform security investigations on AWS service logs by providing an Amazon Athena analysis environment that's quick to deploy, ready to use, and easy to maintain.
* [EC2 Forensics](./docs/EC2_Forensics.md)

## References
* [AWS Cloud Adoption Framework](https://aws.amazon.com/professional-services/CAF/)
* [AWS Incident Response Blogs](https://aws.amazon.com/blogs/security/tag/incident-response/)
* [AWS Security Incident Response Guide Wiki](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)
* [AWS Security Incident Response Guide Downloadable](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/aws-security-incident-response-guide.pdf)
* [AWS Security Reference Architecture (AWS SRA)](https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/welcome.html)
* [AWS Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
* [AWS Well Architected Resources](https://aws.amazon.com/architecture/well-architected/)


## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License Summary

The documentation is made available under the Creative Commons Attribution-ShareAlike 4.0 International License. See the LICENSE file.

The sample code within this documentation is made available under the MIT-0 license. See the LICENSE-SAMPLECODE file.
