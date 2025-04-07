<div align="center" style="font-family: Arial, sans-serif; font-size: 20px; line-height: 1.5;">

# 🌟 **Curso AWS Developer - EDN** 🌟  
# 🌟 Developer Associate 🌟

<a href="https://escoladanuvem.org"><a href="https://aws.amazon.com/pt/?nc2=h_lg">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Amazon_Web_Services_Logo.svg/2560px-Amazon_Web_Services_Logo.svg.png" width="150" alt="AWS Logo">
</a>

<img src="https://cdn.worldvectorlogo.com/logos/amazon-s3-simple-storage-service.svg" width="80" alt="S3 Logo"/>-
<img src="https://cdn.worldvectorlogo.com/logos/aws-lambda-64-1.svg" width="80" alt="Lambda Logo"/>-
<img src="https://raw.githubusercontent.com/pulumi/pulumi-aws-apigateway/main/assets/logo.png" width="80" alt="API Gateway Logo"/>-
<img src="https://github.com/HalleyVeras/Escola_da_Nuvem/blob/main/Documentos/download%20(4)_processed.png?raw=true" width="180" alt="EDN Logo">

</div>

---
## 🎮 Laboratório 4 — Jogo de Adivinhação com AWS Lambda, API Gateway e S3

### 📘 Resumo do laboratório
Neste laboratório, você aprenderá a criar e gerenciar uma aplicação *serverless* na AWS, integrando múltiplos serviços para desenvolver um jogo de adivinhação.

---

### 🎯 Objetivos
Ao concluir este laboratório, você aprenderá a:

- Desenvolver uma função AWS Lambda em Python para implementar a lógica do jogo.
- Criar uma API RESTful com Amazon API Gateway para expor a função Lambda.
- Publicar um site estático no Amazon S3 para interação com o usuário.
- Conectar o frontend à API Gateway, permitindo chamadas à função Lambda.

---

### 🧑‍💻 Início
Acesse o console da AWS com a conta fornecida pela Escola da Nuvem. Caso apareça a mensagem de erro com "To logout, click here", clique no link azul para sair e entre novamente.

> **Atenção:** Sempre confira no canto superior direito se está logado na conta correta.

---

### 1️⃣ Criação da função Lambda

1.1 Acesse o serviço **AWS Lambda** e clique em **Criar função**.

1.2 Selecione **Criar do zero**.

1.3 Nomeie a função como: `LambdaGame-seunome-sobrenome`

1.4 Escolha o runtime: **Python 3.9**. Clique em **Criar função** no final da página.

1.5 Baixe o arquivo `.zip` chamado `lambda_function.zip`.

---

### 2️⃣ Upload do código para o Lambda

2.1 Na função criada, vá até a seção **Origem do código** > clique em **Fazer upload de** > selecione **Arquivos .zip**.

2.2 Faça upload do arquivo `lambda_function.zip` baixado anteriormente.

2.3 Após o upload, clique em **Salvar** > depois em **Deploy**.

---

### 3️⃣ Criando a API REST com Amazon API Gateway

3.1 Acesse o serviço **Amazon API Gateway** > clique em **APIs** > selecione **API HTTP** > clique em **Compilar**.

3.2 Nomeie a API como `Api-seunome-sobrenome` > clique em **Adicionar integrações**.

3.3 Selecione **Lambda** > escolha a função `LambdaGame` criada anteriormente.

> Verifique a **região da AWS** para garantir que a função apareça corretamente.

3.4 Configure a rota:
- Método: `GET`
- Caminho do recurso: `/jogo`

Clique em **Avançar**.

3.5 Altere o estágio de `$default` para `prod` e mantenha **implantação automática** ativada. Clique em **Avançar > Criar**.

3.6 No menu lateral, clique em sua API criada > na seção **Estágios**, copie a URL de **Invocar URL** e salve.

3.7 Vá até **CORS** no menu lateral > clique em **Configurar**:
- `Access-Control-Allow-Origin`: *
- `Access-Control-Allow-Headers`: content-type
- `Access-Control-Allow-Methods`: GET

Clique em **Salvar**.

---

### 4️⃣ Site estático no Amazon S3

4.1 Baixe e abra o arquivo `index.html` com um editor de texto.

4.2 Substitua no código:
- `Invocar URL` → pela URL da API
- `Caminho do recurso` → `jogo`

4.3 Adicione seu nome após "Escola da Nuvem💙" no HTML.

4.4 Acesse o serviço **S3** > clique em **Buckets de uso geral** > **Criar bucket**.

4.5 Nome do bucket: `s3-website-seunome-sobrenome` (único globalmente).

4.6 Clique em **Criar bucket**.

4.7 Clique no bucket criado > **Carregar** > **Adicionar arquivos** > selecione `index.html` > **Carregar**.

4.8 Vá em **Propriedades** > role até **Hospedagem de site estático** > clique em **Editar**.

4.9 Ative a hospedagem:
- Marque **Ativar**
- Documento de índice: `index.html`

Clique em **Salvar alterações**.

4.10 Copie o link de **Endpoint de site de bucket** e abra em uma nova aba.

> Provavelmente você verá um erro 403 — isso será resolvido a seguir.

---

### 5️⃣ Configuração de permissões públicas do bucket

5.1 Vá em **Permissões** > clique em **Editar** em "Bloquear acesso público".

5.2 Desmarque **Bloquear todo o acesso público** > clique em **Salvar alterações** > **Confirmar**.

5.3 Vá até **Política do bucket** > clique em **Editar**.

5.4 Copie o **ARN do bucket** (ex: `arn:aws:s3:::s3-website-seunome-sobrenome`).

5.5 Clique em **Gerador de políticas** > preencha:
- **Select Type of Policy**: S3 Bucket Policy
- **Effect**: Allow
- **Principal**: *
- **Actions**: GetObject
- **ARN**: cole o ARN do bucket e adicione `/*`

5.6 Clique em **Add Statement** > **Generate Policy**.

5.7 Copie o JSON gerado > volte ao campo da política do bucket > cole o JSON.

> Verifique se não há espaços em branco no início ou final do JSON.

5.8 Salve as alterações. Agora seu site estará disponível publicamente!

---

### ✅ Funcionamento do jogo
- O usuário acessa o site via S3
- Digita um número de 1 a 10 e clica em "Enviar"
- O site envia uma requisição via API Gateway para a função Lambda
- A Lambda responde informando se o número foi adivinhado corretamente



