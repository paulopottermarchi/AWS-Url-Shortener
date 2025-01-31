# Create URL Shortener - AWS Lambda

## Descrição
Este projeto implementa um encurtador de URL utilizando AWS Lambda e Amazon S3 para armazenar e redirecionar URLs encurtadas. Ele gera um código curto para a URL original, armazena os dados e permite a recuperação da URL original.

## Tecnologias Utilizadas
- **Java 17**
- **AWS Lambda**
- **Amazon S3**
- **Maven**
- **Jackson** (para manipulação de JSON)
- **Lombok** (para reduzir boilerplate code)

## Estrutura do Projeto
```
com.rocketseat.createUrlShortner
├── Main.java  # Classe principal que lida com a requisição Lambda para criação de URLs
├── UrlData.java  # Classe para armazenar dados da URL
com.rocketseat.redirectUrlShortener
├── Main.java  # Classe principal que lida com a requisição Lambda para redirecionamento de URLs
├── UrlData.java  # Classe utilizada para desserialização de dados
├── pom.xml  # Arquivo de dependências do Maven
```

## Configuração e Execução

### 1. Configurar AWS Credentials
Certifique-se de que seu ambiente tem as credenciais da AWS configuradas corretamente. Elas devem ter permissões para acessar o Amazon S3 e executar funções Lambda.

### 2. Compilar o Projeto
Execute o seguinte comando para compilar o projeto e gerar o arquivo `.jar`:
```sh
mvn clean package
```

### 3. Implantar no AWS Lambda

1. Acesse o [AWS Lambda](https://aws.amazon.com/lambda/).
2. Crie uma nova função Lambda para criar URLs encurtadas.
3. Selecione "Upload de um arquivo .zip ou .jar" e carregue o `.jar` gerado na etapa anterior.
4. Configure a função para usar Java 17 como runtime.
5. Defina a classe de entrada como `com.rocketseat.createUrlShortner.Main::handleRequest`.
6. Crie outra função Lambda para redirecionamento e defina a classe de entrada como `com.rocketseat.redirectUrlShortener.Main::handleRequest`.
7. Adicione permissões para que ambas as funções possam acessar o S3.

### 4. Testar a Função

#### Criar uma URL encurtada

Envie uma requisição HTTP com JSON no seguinte formato:

```json
{
  "body": "{ \"originalUrl\": \"https://exemplo.com\", \"expirationTime\": \"3600\" }"
}
```

A resposta conterá um código curto para acessar a URL:

```json
{
  "code": "abcd1234"
}
```

#### Redirecionar uma URL encurtada

Envie uma requisição GET para a função Lambda responsável pelo redirecionamento, passando o código curto na URL:

```
GET /abcd1234
```

Se a URL ainda for válida, a resposta será:

```json
{
  "statusCode": 302,
  "headers": {
    "Location": "https://exemplo.com"
  }
}
```

Se a URL expirou:

```json
{
  "statusCode": 410,
  "body": "This URL has expired"
}
```

## Licença
Este projeto é open-source e está sob a licença MIT.

