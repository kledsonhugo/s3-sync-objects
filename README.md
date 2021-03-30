# Sync de arquivos GitHub com objetos em um Bucket S3 #

Demonstrar como realizar sincronismo de arquivos em um repo GitHub com objetos em um bucket S3.

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

   - url: `http://fiap-cloud-vds-aws-s3-<SEU_NOME>.s3-website-us-east-1.amazonaws.com`

     > Guarde esta informação pois precisará a frente.
