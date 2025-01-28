# Projeto: Automação com AWS SES, Lambda e Step Functions

Este projeto implementa uma automação usando AWS Simple Email Service (SES), AWS Lambda e AWS Step Functions para processar e gerenciar envio de e-mails e outras integrações automatizadas.

## Funcionalidades

- **Envio de e-mails**: Configuração do AWS SES para envio de mensagens automatizadas.
- **Execução de funções Lambda**: Funções Lambda para manipular dados e integrar com outros serviços.
- **Orquestração com Step Functions**: Automação de processos sequenciais e paralelos usando AWS Step Functions.

---

## Estrutura do Projeto

1. **AWS SES**
   - Configuração para envio de e-mails.
   - Verificação de identidade do remetente (e-mail ou domínio).

2. **Funções Lambda**
   - `email`: Função Lambda responsável pelo envio de e-mails.
   - `api_handler`: Função Lambda usada para processar requisições de API.

3. **Step Functions**
   - Máquina de estado que orquestra as funções Lambda.

4. **Rest API**
   - Responsável pela comunicação com o site estático S3 através do método POST.

---

## Pré-requisitos

- Conta na AWS.
- AWS CLI configurado com credenciais válidas.
- Python 3.9+ (para desenvolvimento local das funções Lambda).
- Bucket S3 para repositório dos códigos listados em `SourceCode`
- Bucket S3 para hospedagem dos arquivos 


---

## Problemas Comuns

- **Erro 502 (Bad Gateway)**: Verifique se a função `api_handler` está retornando a estrutura de resposta correta.

---

## Scripts Utilizados no AWS CLI

-- criando um repositório S3 para armazenar o SourceCode --
aws s3api create-bucket --bucket {nome_do_repositorio} --region us-east-1 

-- movendo arquivos do SourceCode para o repositório S3 --
aws s3 cp {nome_do_arquivo} s3://{nome_do_repositorio}/

-- criando stacks do CloudFormation --
aws cloudformation create-stack \
  --stack-name {nome_da_stack} \
  --template-body file://{nome_do_arquivo}.yml \
  --capabilities CAPABILITY_IAM 


Template contains invalid characters (Service: AmazonCloudFormation; Status Code: 400; Error Code: ValidationError; Request ID: 71cade86-24d7-4acf-bfbc-8afd0cf6c053; Proxy: null)
CLIENT_ERROR: git fetch failed with exit status 128 for source 9af03865_66ed_4c4e_be14_1b1b8755a6c2
Resource handler returned message: "User: arn:aws:sts::381492167002:assumed-role/CodePipelineStarterTemplate-Depl-CloudFormationRole-ouWW3jng3ONH/AWSCloudFormation is not authorized to perform: ses:CreateEmailIdentity on resource: arn:aws:ses:us-east-2:381492167002:identity/natalia.araujo1434@gmail.com because no identity-based policy allows the ses:CreateEmailIdentity action (Service: SesV2, Status Code: 403)"
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "S3Permissions",
      "Effect": "Allow",
      "Action": [
        "s3:CreateBucket",
        "s3:ListBucket",
        "s3:GetObject",
        "s3:PutObject",
        "s3:DeleteObject"
      ],
      "Resource": [
        "arn:aws:s3:::sourcecoderepos001",
        "arn:aws:s3:::sourcecoderepos001/*"
      ]
    },
    {
      "Sid": "CloudFormationPermissions",
      "Effect": "Allow",
      "Action": [
        "cloudformation:CreateStack",
        "cloudformation:UpdateStack",
        "cloudformation:DeleteStack",
        "cloudformation:DescribeStacks",
        "cloudformation:DescribeStackResources"
      ],
      "Resource": "*"
    },
    {
      "Sid": "LambdaPermissions",
      "Effect": "Allow",
      "Action": [
        "lambda:CreateFunction",
        "lambda:UpdateFunctionCode",
        "lambda:UpdateFunctionConfiguration",
        "lambda:GetFunction",
        "lambda:DeleteFunction",
        "lambda:InvokeFunction"
      ],
      "Resource": "arn:aws:lambda:*:*:function:*"
    },
    {
      "Sid": "StepFunctionsPermissions",
      "Effect": "Allow",
      "Action": [
        "states:CreateStateMachine",
        "states:UpdateStateMachine",
        "states:DeleteStateMachine",
        "states:DescribeStateMachine",
        "states:StartExecution",
        "states:ListExecutions"
      ],
      "Resource": "arn:aws:states:*:*:stateMachine:*"
    },
    {
      "Sid": "IAMPermissions",
      "Effect": "Allow",
      "Action": [
        "iam:CreateRole",
        "iam:AttachRolePolicy",
        "iam:DetachRolePolicy",
        "iam:DeleteRole",
        "iam:PassRole"
      ],
      "Resource": "arn:aws:iam::*:role/*"
    },
    {
      "Sid": "CloudWatchPermissions",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
        "logs:DescribeLogStreams",
        "logs:GetLogEvents"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CodePipelinePermissions",
      "Effect": "Allow",
      "Action": [
        "codepipeline:CreatePipeline",
        "codepipeline:UpdatePipeline",
        "codepipeline:DeletePipeline",
        "codepipeline:StartPipelineExecution",
        "codepipeline:GetPipelineExecution",
        "codepipeline:GetPipelineState"
      ],
      "Resource": "arn:aws:codepipeline:*:*:*"
    }
  ]
}
