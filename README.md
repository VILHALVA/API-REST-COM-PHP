# API REST COM PHP
🤤ESSE PROJETO É UMA API PHP BÁSICA QUE LIDA COM AUTENTICAÇÃO E CRUD DE USUÁRIOS EM UM BANCO DE DADOS POSTGRESQL.

<img src="./IMAGENS/FOTO_1.png" align="center" width="500"> <br>
<img src="./IMAGENS/FOTO_2.png" align="center" width="500"> <br>

## DESCRIÇÃO:
Esse projeto é uma API PHP básica que lida com a criação, autenticação, consulta, atualização e exclusão de usuários em um banco de dados PostgreSQL. Ele utiliza o framework MVC (Model-View-Controller) com algumas funcionalidades extras como a validação de dados e geração de tokens JWT para autenticação.

## ESTRUTURA E FUNCIONALIDADES:
1. **Rotas HTTP (App\Http\Route)**:
   O arquivo de rotas define os endpoints que a API expõe. Cada rota está associada a um método de um controlador, que é responsável por realizar a lógica necessária:
   - `GET /`: Carrega a página inicial através de `HomeController@index`.
   - `POST /users/create`: Cria um novo usuário, chamando `UserController@store`.
   - `POST /users/login`: Realiza o login de um usuário, através de `UserController@login`.
   - `GET /users/fetch`: Busca dados do usuário logado, por meio de `UserController@fetch`.
   - `PUT /users/update`: Atualiza dados do usuário logado, chamando `UserController@update`.
   - `DELETE /users/{id}/delete`: Remove um usuário pelo ID, utilizando `UserController@remove`.

2. **Conexão com o banco de dados (App\Models\Database)**:
   O arquivo `Database` define a conexão com um banco de dados PostgreSQL. O método `getConnection` cria uma nova instância do PDO (PHP Data Objects) para realizar consultas no banco.

3. **Modelo de Usuário (App\Models\User)**:
   Este arquivo define as operações CRUD para o modelo `User`:
   - **save**: Insere um novo usuário no banco de dados.
   - **authentication**: Autentica um usuário verificando seu email e senha (hashed).
   - **find**: Retorna os dados de um usuário a partir de seu ID.
   - **update**: Atualiza o nome do usuário com base no ID.
   - **delete**: Exclui um usuário com base no ID.

4. **Serviço de Usuário (App\Services\UserService)**:
   Este arquivo contém a lógica de negócios para as operações de usuário:
   - **create**: Valida os dados, cria o hash da senha e tenta salvar o usuário.
   - **auth**: Valida os dados de login e gera um token JWT para autenticação.
   - **fetch**: Busca os dados do usuário autenticado (via JWT).
   - **update**: Atualiza o nome do usuário autenticado.
   - **delete**: Exclui o usuário autenticado.

## EXECUTANDO O PROJETO:
1. **Importar o `DATABASE.sql`**  
   - Antes de iniciar o site, é necessário importar o arquivo localizado em `./CODIGO/DATABASE.sql` para o seu banco de dados.
   - Certifique-se de que o banco de dados PostgreSQL esteja rodando localmente na porta 5432 e que exista um banco de dados chamado `api`. As credenciais usadas no projeto são `postgres` como usuário e `root` como senha.

2. **Dependências e Servidor**:
   - Certifique-se de que as dependências do projeto estão instaladas corretamente. No diretório do projeto, execute o seguinte comando para instalar ou atualizar as dependências usando o Composer:
     ```bash
     composer update
     ```
   - O projeto precisa que o **PDO** esteja instalado e configurado para PostgreSQL no seu ambiente PHP.
   - Para iniciar o servidor PHP no diretório do projeto, utilize o comando:
     ```bash
     php -S localhost:8000
     ```
   - Utilize ferramentas como o *Postman* ou *cURL* para testar os endpoints da API.

3. **Fluxo de Autenticação**:
   - Para criar um usuário, envie uma requisição `POST` para `/users/create` com os dados `name`, `email`, e `password`.
   - Para realizar login, envie uma requisição `POST` para `/users/login` com os dados `email` e `password`. Se a autenticação for bem-sucedida, você receberá um token JWT.
   - Para acessar rotas protegidas (como `/users/fetch`), envie o token JWT no cabeçalho da requisição como `Authorization: Bearer <token>`.

## USANDO API:
Aqui estão exemplos de como usar a API em diferentes cenários, utilizando *cURL* ou ferramentas como *Postman*:

### 1. **CRIAR UM NOVO USUÁRIO** (`POST /users/create`)
**Requisição:**

```bash
curl -X POST http://localhost:8000/users/create \
-H "Content-Type: application/json" \
-d '{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "mySecretPassword"
}'
```

**Resposta (sucesso):**

```json
{
    "message": "User created successfully!"
}
```

**Resposta (falha):**

```json
{
    "error": "Sorry, user already exists."
}
```

### 2. **LOGIN DO USUÁRIO** (`POST /users/login`)
**Requisição:**

```bash
curl -X POST http://localhost:8000/users/login \
-H "Content-Type: application/json" \
-d '{
    "email": "john@example.com",
    "password": "mySecretPassword"
}'
```

**Resposta (sucesso):**

```json
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Resposta (falha):**

```json
{
    "error": "Sorry, we could not authenticate you."
}
```

### 3. **BUSCAR DADOS DO USUÁRIO AUTENTICADO** (`GET /users/fetch`)
**Requisição:**

```bash
curl -X GET http://localhost:8000/users/fetch \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Resposta (sucesso):**

```json
{
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
}
```

**Resposta (não autorizado):**

```json
{
    "unauthorized": "Please, login to access this resource."
}
```

### 4. **ATUALIZAR O NOME DO USUÁRIO AUTENTICADO** (`PUT /users/update`)
**Requisição:**

```bash
curl -X PUT http://localhost:8000/users/update \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
-H "Content-Type: application/json" \
-d '{
    "name": "Johnathan Doe"
}'
```

**Resposta (sucesso):**

```json
{
    "message": "User updated successfully!"
}
```

**Resposta (não autorizado):**

```json
{
    "unauthorized": "Please, login to access this resource."
}
```

### 5. **EXCLUIR UM USUÁRIO PELO ID** (`DELETE /users/{id}/delete`)
**Requisição:**

```bash
curl -X DELETE http://localhost:8000/users/1/delete \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
```

**Resposta (sucesso):**

```json
{
    "message": "User deleted successfully!"
}
```

**Resposta (não autorizado):**

```json
{
    "unauthorized": "Please, login to access this resource."
}
```

## NÃO SABE?
- Entendemos que para manipular arquivos em muitas linguagens e tecnologias relacionadas, é necessário possuir conhecimento nessas áreas. Para auxiliar nesse aprendizado, oferecemos alguns subsidios:
* [CURSO SUGERIDO](https://github.com/VILHALVA/CURSO-DE-API-REST)
* [CURSO DE PHP](https://github.com/VILHALVA/CURSO-DE-PHP)
* [CURSO DE POSTGRESQL](https://github.com/VILHALVA/CURSO-DE-POSTGRESQL)
* [CONFIRA MAIS CURSOS](https://github.com/VILHALVA?tab=repositories&q=+topic:CURSO)

## CREDITOS:
- [PROJETO CRIADO PELO "EricNeves"](https://github.com/EricNeves/yt-api-com-php)
- [VEJA O VIDEO DESSE PROJETO](https://youtu.be/5fg5NG2ucsA?si=gh9oscrIuXiSPDa8)
- [PROJETO FEITO PELO VILHALVA](https://github.com/VILHALVA)
