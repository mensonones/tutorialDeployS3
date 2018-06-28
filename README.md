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

---
# Javascript Node CircleCI 2.0 configuration file
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:9.11
    working_directory: /tmp/nome-pasta-que-quiser
    steps:
      - checkout
      - restore_cache:
          keys:
          - v1-dependencies-{{ checksum "package.json" }}
          - v1-dependencies-
      - run: npm install
      - save_cache:
          paths:
            - node_modules
          key: v1-dependencies-{{ checksum "package.json" }}
      - run: npm run production
      - save_cache:
          paths:
            - dist
            - public
          key: build-{{ .Revision }}
  deploy:
    docker:
      - image: circleci/node:9.11
    working_directory: /tmp/nome-pasta-que-quiser
    steps:
      - restore_cache:
          keys:
          - build-{{ .Revision }}
      - run: |
             sudo apt-get -y -qq install awscli
             aws s3 sync public 's3://nome-bucket-s3/' --delete --exclude "branch/*" --acl public-read
             aws s3 sync dist 's3://nome-bucket-s3/dist' --acl public-read
  branch-deploy:
    docker:
      - image: circleci/node:9.11
    working_directory: /tmp/nome-pasta-que-quiser
    steps:
      - restore_cache:
          keys:
          - build-{{ .Revision }}
      - run: |
             sudo apt-get -y -qq install awscli
             aws s3 sync public "s3://nome-bucket-s3/branch/$CIRCLE_BRANCH/" --acl public-read
             aws s3 sync dist "s3://nome-bucket-s3/branch/$CIRCLE_BRANCH/dist" --acl public-read
workflows:
  version: 2
  build-deploy:
    jobs:
      - build
      - branch-deploy:
          requires:
            - build
          filters:
            branches:
              ignore: master
      - deploy:
          requires:
            - build
          filters:
            branches:
              only: master
---
   
## Passo 5
Feito tudo isso, o CircleCI irá rodar após qualquer commit que fizer para o repositório e então irá fazer o deploy se tudo ocorrer bem. 
