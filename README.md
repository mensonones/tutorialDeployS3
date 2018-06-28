# tutorialDeployS3
Tutorial de como fazer deploy de aplicação ReactJS na Amazon(S3)

## Passo 1
Você deve  criar sua conta no CircleCI. Após criado a conta, é preciso associar sua conta do CircleCI com seus github/bitbuket na aba projects. Ao listar os projetos você deve escolher o projeto clicando em Set Up Project 
![img](https://cdn-images-1.medium.com/max/800/1*rpHcpF4I7W0mDu1H0WvgUg.png)

## Passo 2
Clique na opção jobs no CircleCI e será listado os projetos que você selecionou para usar. Clique no ícone da engrenagem no projeto que será usado para deploy. Na página que abrir você deve clicar na opção: AWS Permissions . Insira a Access Key ID e a Secret Access Key. O CircleCI recomenda que você crie um IAM User unicamente para ser usado nele. 

## Passo 3
Dentro do seu projeto você deve criar uma pasta chamada: .circleci, e dentro dessa pasta você deve criar um arquivo chamado: config.yml

## Passo 4
Conteúdo do arquivo .config.yml
<https://gist.github.com/mensonones/597cff581e583970249c30632bda9f20>
   
## Passo 5
Feito tudo isso, o CircleCI irá rodar após qualquer commit que fizer para o repositório e então irá fazer o deploy se tudo ocorrer bem. 
