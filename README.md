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

5. Clique sobre o nome do Bucket.

6. No menu **Propriedades** navegue até **Hospedagem de Site estático**, clique em **Editar** e preencha as informações abaixo.

   - Hospedagem de site estático: `Ativar`
   - Documento de índice: `index.html`
   - Documento de erro opcional: `error.html`

7. No menu **Propriedades** verifique a opção **Endpoint de site de bucket** e guarde a url conforme exemplo abaixo.

   - url: `http://<S3_NOME_DO_BUCKET>.s3-website-<REGIÃO_AWS>.amazonaws.com`

     > Guarde esta informação pois precisará a frente.

## Passo 2

Obtenha credenciais de acesso à conta AWS para permitir que o repositório GitHub gerencie os objetos do bucket S3.

1. Acesse a página [https://console.aws.amazon.com/iam/home?region=us-east-1#/security_credentials$access_key](https://console.aws.amazon.com/iam/home?region=us-east-1#/security_credentials$access_key).

2. Crie uma chave de acesso e capture os valores das variáveis **ID da chave de acesso (AWS_ACCESS_KEY_ID)** e **Chave de acesso secreta (AWS_SECRET_ACCESS_KEY)**.

   > Guarde estas informações pois também serão necessárias.
   > Para sua proteção, você nunca deve compartilhar suas chaves secretas com ninguém. Como prática recomendada, recomendamos alternância frequente de chaves.

## Passo 3

O terceiro passo é configurar um repositório GitHub e configurá-lo para realizar o sincronismo de arquivos com o bucket S3.

1. Acesse o GitHub [https://github.com/](https://github.com/).

2. Crie um repositório conforme doc [https://docs.github.com/pt/github/getting-started-with-github/create-a-repo](https://docs.github.com/pt/github/getting-started-with-github/create-a-repo).

3. Dentro do repositório acesse a opção **Settings**.

4. No menu **Secrets** clique em **New repository secret** e adicione as variáveis abaixo.

   - **`AWS_ACCESS_KEY_ID`** : `access key capturada no passo 2`
   - **`AWS_SECRET_ACCESS_KEY`** : `secret access key capturada no passo 2`

5. 
[Workflow Dir](/images/workflow-dir.png | width=100)

6. Publique conteúdos no repositório GitHub.

   > Caso tenha dúvidas para publicar conteúdo em um repositório GitHub consulte a doc oficial em [https://docs.github.com/pt/github/managing-files-in-a-repository/adding-a-file-to-a-repository-using-the-command-line](https://docs.github.com/pt/github/managing-files-in-a-repository/adding-a-file-to-a-repository-using-the-command-line).
