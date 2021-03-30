# Sync com Bucket S3 #

Demonstrar como realizar sincronismo de arquivos em um repo GitHub com objetos em um Bucket S3.

> Referências
- [https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html](https://docs.aws.amazon.com/cli/latest/reference/s3/sync.html)
- [https://docs.github.com/pt/actions/learn-github-actions](https://docs.github.com/pt/actions/learn-github-actions)

## Passo 1

O primeiro passo é criar o Bucket S3.

1. Faça login no AWS Console.

2. Em **Serviços** selecione **S3**.

3. Selecione o botão **Criar bucket**.

4. Na tela de criação de bucket preencha as informações abaixo.

   - Nome do Bucket: **`<S3_NOME_DO_BUCKET>`**
   - Região da AWS: `selecionar conforme preferência`
   - Bloquear todo o acesso público: `desmarcar`
   - Reconheço que as configurações atuais podem fazer com que este bucket e os objetos dentro dele se tornem públicos: `marcar`

     > Mantenha as demais opções padrões. 

## Passo 2

Configure credenciais de acesso da conta AWS que será utilizada para sincronizar o repo GitHub com o bucket S3.

1. Acesse a página [https://console.aws.amazon.com/iam/home?region=us-east-1#/security_credentials$access_key](https://console.aws.amazon.com/iam/home?region=us-east-1#/security_credentials$access_key).

2. Crie uma chave de acesso e capture os valores **ID da chave de acesso (AWS_ACCESS_KEY_ID)** e **Chave de acesso secreta (AWS_SECRET_ACCESS_KEY)**.

   > Estas informações serão necessárias mais adiante.

## Passo 3

O terceiro passo é configurar o repositório GitHub e configurá-lo para sincronizar com o bucket S3.

1. Acesse o GitHub [https://github.com/](https://github.com/).

2. Selcione ou crie um repositório GitHub e acesse o menu **Settings**.

   > Referência [https://docs.github.com/pt/github/getting-started-with-github/create-a-repo](https://docs.github.com/pt/github/getting-started-with-github/create-a-repo).

3. Em **Secrets** clique em **New repository secret** e adicione as variáveis abaixo.

   - **`AWS_ACCESS_KEY_ID`** : `access key capturada no passo 2`
   - **`AWS_SECRET_ACCESS_KEY`** : `secret access key capturada no passo 2`

4. Publique arquivos no repositório GitHub que serão sincronizados com o bucket S3.

   > Caso tenha dúvidas para publicar conteúdo em repositório GitHub, consulte a doc oficial em [https://docs.github.com/pt/github/managing-files-in-a-repository/adding-a-file-to-a-repository-using-the-command-line](https://docs.github.com/pt/github/managing-files-in-a-repository/adding-a-file-to-a-repository-using-the-command-line).

5. Publique no repositório GitHub o arquivo `.github/workflows/main.yml` com o conteúdo abaixo.

   > Esse arquivo configura o Workflow de sincronismo do repositório GitHub com o bucket S3.

   > Substitua as variáveis `<REGIÃO_AWS>` e `NOME_DO_BUCKET` pelos valores capturados nos passos anteriores.

   ```
   name: Sync GitHub to S3

   on:
     push:
       branches:
       - master

   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:

       - name: Checkout repo
         uses: actions/checkout@v1

       - name: Set Credentials
         uses: aws-actions/configure-aws-credentials@v1
         with:
           aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
           aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
           aws-region: **<REGIÃO_AWS>**

       - name: Deploy objects to S3 bucket
         run: |
           aws s3 sync ./ s3://<NOME_DO_BUCKET> \
           --exclude '.git/*' \
           --exclude '.github/*' \
           --exclude 'README.md' \
           --acl public-read \
           --follow-symlinks \
           --delete
   ```

7. No repositório GitHub acesse a opção **Actions** e verifique o status da execução do Workflow.

   > Nesse ponto é esperado que o Workflow execute com sucesso e sincronize os arquivos do repositório GitHub com o Bucket S3.

   ![Workflow Actions](/images/workflow-actions.PNG)   

## Passo 4

Verifique no Bucket S3 se os objetos foram atualizados.

1. Faça login no AWS Console.

2. Em **Serviços** selecione **S3**.

3. Verifique a data e hora dos objetos no bucket.
