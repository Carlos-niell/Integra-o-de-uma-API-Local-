# 🚀 Integração de uma API Local — Node.js & Angular (SENAI)

![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)
![Angular](https://img.shields.io/badge/Angular_20-DD0031?style=for-the-badge&logo=angular&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap_5-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)
![Angular Material](https://img.shields.io/badge/Angular_Material-3F51B5?style=for-the-badge&logo=angular&logoColor=white)
![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)
![Jest](https://img.shields.io/badge/Jest-C21325?style=for-the-badge&logo=jest&logoColor=white)

Aplicação Full-Stack desenvolvida para a atividade prática do **SENAI** sobre **consumo e integração de uma API REST local**, conectando um **Back-End em Node.js (Express)** a um **Front-End em Angular 20** estilizado com **Bootstrap 5** e **Angular Material (`MatSnackBar`)**.

---

## 🛠️ Tecnologias e Ferramentas Utilizadas

| Camada / Função | Ferramenta | Descrição |
| :--- | :--- | :--- |
| **Back-End** | ![Node.js](https://img.shields.io/badge/-Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white) ![Express](https://img.shields.io/badge/-Express-000000?style=flat-square&logo=express&logoColor=white) | Servidor HTTP REST na porta `3000` com persistência em arquivo JSON (`db.json`) e suporte a CORS. |
| **Front-End** | ![Angular](https://img.shields.io/badge/-Angular_CLI-DD0031?style=flat-square&logo=angular&logoColor=white) ![TypeScript](https://img.shields.io/badge/-TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white) | SPA (Single Page Application) na porta `4200` utilizando `HttpClient`, `Observable` (RxJS), Services e Rotas. |
| **Estilização & UI** | ![Bootstrap](https://img.shields.io/badge/-Bootstrap_5-7952B3?style=flat-square&logo=bootstrap&logoColor=white) ![Material](https://img.shields.io/badge/-Material-3F51B5?style=flat-square&logo=angular&logoColor=white) | Layout responsivo (Navbar, Cards, Tabelas e Formulários) com Bootstrap 5 e alertas de feedback (`MatSnackBar`). |
| **Testes de Rotas** | ![Postman](https://img.shields.io/badge/-Postman-FF6C37?style=flat-square&logo=postman&logoColor=white) ![Jest](https://img.shields.io/badge/-Jest-C21325?style=flat-square&logo=jest&logoColor=white) | Simulação e validação dos métodos HTTP (`GET`, `POST`, `PUT`, `DELETE`) via Postman e testes automatizados com Jest/Supertest. |

---

## 📂 Estrutura do Projeto

```text
interacao-api/
├── postman_collection.json          # Coleção do Postman pronta para importação (GET, POST, PUT, DELETE)
├── README.md                        # Documentação principal do projeto
└── src/
    ├── backend/                     # Servidor Back-End (Node.js + Express)
    │   ├── app.js                   # Definição das rotas REST (/clientes) e regras de negócio
    │   ├── server.js                # Inicialização do servidor na porta 3000
    │   ├── db.json                  # Base de dados local em formato JSON
    │   └── package.json             # Dependências e scripts do servidor
    └── frontend/                    # Aplicação Front-End (Angular 20)
        ├── angular.json             # Configuração do Angular CLI e integração do Bootstrap 5
        ├── package.json             # Dependências do Front-End
        └── src/app/
            ├── models/
            │   └── cliente.ts       # Modelo de dados (id, nome, endereco)
            ├── services/
            │   └── cliente.service.ts # Comunicação HTTP com a API e feedback (MatSnackBar)
            ├── components/
            │   ├── menu/              # Barra de navegação (Navbar Bootstrap)
            │   ├── listar-cliente/    # Listagem em tabela e exclusão de clientes
            │   ├── cadastrar-cliente/ # Formulário de cadastro de novos clientes
            │   └── atualizar-cliente/ # Formulário de edição de cliente existente
            ├── app-routing-module.ts  # Configuração de rotas da aplicação
            └── app-module.ts          # Módulo principal (HttpClientModule, FormsModule, Material)
```

---

## 🔄 Como Funciona a Aplicação (Passo a Passo da Arquitetura)

1. **Inicialização da Interface (`ListarClienteComponent`):**
   * Ao acessar `http://localhost:4200`, o sistema redireciona automaticamente para `/clientes`.
   * O componente chama `ClienteService.getClientes()`, que dispara um `GET http://localhost:3000/clientes` usando o `HttpClient` do Angular.
   * Os dados retornados populam a tabela estilizada com Bootstrap.

2. **Cadastro de Cliente (`CadastrarClienteComponent`):**
   * Na rota `/clientes/cadastrar`, o usuário preenche **Nome** e **Endereço** utilizando `[(ngModel)]` (`FormsModule`).
   * Ao clicar em **Salvar**, o `ClienteService.cadastrarCliente()` envia um `POST http://localhost:3000/clientes` com o payload JSON.
   * O servidor valida os campos, gera o próximo `id`, grava em `db.json` e retorna status `201 Created`.
   * O Front-End exibe um alerta verde de confirmação (`"Cliente cadastrado com sucesso"`) e redireciona para a listagem.

3. **Edição de Cliente (`AtualizarClienteComponent`):**
   * Ao clicar em **Editar** na tabela, o usuário é levado para `/clientes/atualizar/:id`.
   * O componente consulta `GET http://localhost:3000/clientes?id=:id` para preencher o formulário com os dados atuais.
   * Ao salvar, é disparado um `PUT http://localhost:3000/clientes/:id`, exibindo a mensagem `"Cliente atualizado com sucesso"`.

4. **Exclusão de Cliente (`ListarClienteComponent`):**
   * Ao clicar em **Excluir**, após confirmação do usuário, o método `ClienteService.excluirCliente(id)` envia um `DELETE http://localhost:3000/clientes/:id`.
   * O servidor remove o registro, retorna `204 No Content`, o Front-End exibe `"Cliente excluído com sucesso"` e atualiza a tabela.

5. **Tratamento de Respostas Positivas e Negativas (`MatSnackBar`):**
   * **Sucesso (`snack-success` — Verde):** exibido via operador RxJS `tap()` após operações bem-sucedidas.
   * **Erro (`snack-error` — Vermelho):** interceptado via `catchError()` no `ClienteService`, diferenciando:
     * **Status `0`:** `"Falha de conexão com o servidor"` (ex.: Back-End desligado)
     * **Status `400`:** `"Campos 'nome' e 'endereco' são obrigatórios"`
     * **Status `404`:** `"Recurso não encontrado (404)"`
     * **Status `500`:** `"Erro interno do servidor (500)"`

---

## 📡 Documentação das Rotas da API (Back-End)

URL Base: `http://localhost:3000`

| Método | Rota | Parâmetros / Body | Retorno Esperado | Descrição |
| :---: | :--- | :--- | :---: | :--- |
| `GET` | `/clientes` | — | `200 OK` (`Cliente[]`) | Lista todos os clientes cadastrados. |
| `GET` | `/clientes?id={id}` | Query param `id` | `200 OK` (`Cliente[]`) | Filtra e retorna o cliente pelo ID informado. |
| `POST` | `/clientes` | Body JSON: `{ "nome": "...", "endereco": "..." }` | `201 Created` (`Cliente`) | Cadastra um novo cliente (retorna `400` se faltar dados). |
| `PUT` | `/clientes/:id` | Path param `id` + Body JSON `{ "nome", "endereco" }` | `200 OK` (`Cliente`) | Atualiza um cliente existente (retorna `404` se não existir). |
| `DELETE` | `/clientes/:id` | Path param `id` | `204 No Content` | Exclui o cliente pelo ID (retorna `404` se não existir). |

---

## 💻 Passo a Passo para Executar Localmente

### Pré-requisitos
* **Node.js** (versão 18.x LTS ou 20.x LTS recomendada)
* **Git** instalado
* **Postman** (opcional, para testar as rotas isoladamente)

### 1️⃣ Clonar o Repositório

```powershell
git clone https://github.com/Carlos-niell/Integra-o-de-uma-API-Local-.git
cd Integra-o-de-uma-API-Local-
```

### 2️⃣ Iniciar o Back-End (Node.js)

Abra um terminal e execute:

```powershell
cd src/backend
npm install
npm start
```

> O servidor Back-End ficará ativo em: **`http://localhost:3000`**
> *(Opcional)* Para rodar os testes automatizados da API: `npm test`

### 3️⃣ Iniciar o Front-End (Angular)

Abra um **segundo terminal** (mantenha o Back-End rodando no primeiro) e execute:

```powershell
cd src/frontend
npm install
npm start
```

> A aplicação Angular ficará disponível no navegador em: **`http://localhost:4200`**

---

## 🧪 Testando a API no Postman

O projeto inclui o arquivo **`postman_collection.json`** na raiz do repositório com todas as requisições prontas:

1. Abra o **Postman**.
2. Clique no botão **Import** (canto superior esquerdo).
3. Selecione o arquivo `postman_collection.json` localizado na raiz do projeto.
4. Com o Back-End rodando (`npm start` em `src/backend`), execute as 5 requisições da coleção:
   * `1. Listar todos os clientes (GET)`
   * `2. Buscar cliente por ID (GET ?id=1)`
   * `3. Cadastrar novo cliente (POST)`
   * `4. Atualizar cliente (PUT)`
   * `5. Excluir cliente (DELETE)`
