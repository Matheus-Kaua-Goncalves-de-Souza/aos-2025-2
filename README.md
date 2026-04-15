# 🧠 API de Currículo - (ex04-express-curriculo)

![Node.js](https://img.shields.io/badge/Node.js-18.x-green?logo=node.js)
![Express](https://img.shields.io/badge/Express-5.x-lightgrey?logo=express)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-16.x-blue?logo=postgresql)
![Sequelize](https://img.shields.io/badge/Sequelize-ORM-blue?logo=sequelize)
![License](https://img.shields.io/badge/license-MIT-yellow.svg)
![Build](https://img.shields.io/badge/build-passing-brightgreen)

---

## 📋 Descrição

A **API de Currículo (CV)** é uma aplicação RESTful construída com **Node.js, Express e Sequelize (PostgreSQL)**, desenvolvida para gerenciar informações de currículos de forma modular, segura e escalável.  
Seu objetivo é oferecer uma estrutura limpa e extensível para manipular dados pessoais, experiências, formações, habilidades, resumos e links externos de profissionais.

---

## 🚀 Features Principais

- ✅ **Arquitetura MVC Robusta** — Separação clara de responsabilidades entre Modelos, Rotas, Controladores e Validações.
- 🔒 **Validação de Entrada Segura** — Implementação com `express-validator` para garantir integridade dos dados.
- 🌐 **Roteamento Aninhado (Nested Routing)** — Estrutura intuitiva com sub-rotas dependentes de `Person`.
- 🧩 **Relacionamento Relacional Completo** — `Person` como entidade principal com cinco entidades filhas (`Experience`, `Education`, `Skill`, `Resume`, `ExternalLink`).
- 💾 **Integridade de Dados Garantida** — `onDelete: 'CASCADE'` para exclusão em cascata.
- ⚡ **Eager Loading Otimizado** — Recupera todos os dados relacionados em uma única query.
- 🧱 **Deploy Serverless Ready** — Totalmente preparado para hospedagem na **Vercel**.

---

## 🧰 Tech Stack

| Categoria          | Tecnologias           |
| ------------------ | --------------------- |
| **Servidor**       | Node.js + Express     |
| **Banco de Dados** | PostgreSQL            |
| **ORM**            | Sequelize             |
| **Validação**      | express-validator     |
| **Transpilação**   | Babel (ES Modules)    |
| **Utilitários**    | asyncHandler, nodemon |
| **Deploy**         | Vercel                |

---

## 🏗️ Arquitetura do Projeto

A arquitetura segue o padrão **MVC (Model–View–Controller)** e o **princípio da responsabilidade única (SRP)**, com camadas bem definidas:

```
api/
│
├── models/              # Modelos Sequelize (estrutura e associações)
│   ├── person.js
│   ├── experience.js
│   ├── education.js
│   ├── skill.js
│   ├── resume.js
│   ├── externalLink.js
│   └── index.js         # Inicializa Sequelize e executa .associate()
│
├── controllers/         # Lógica de negócio (CRUD)
│   ├── personController.js
│   ├── experienceController.js
│   ├── educationController.js
│   ├── skillController.js
│   ├── resumeController.js
│   └── externalLinkController.js
│
├── routes/              # Definição de endpoints e roteamento aninhado
│   ├── peopleRoutes.js
│   ├── experienceRoutes.js
│   ├── educationRoutes.js
│   ├── skillRoutes.js
│   ├── resumeRoutes.js
│   └── externalLinkRoutes.js
│
├── validators/          # Regras de validação com express-validator
│   ├── personValidator.js
│   ├── experienceValidator.js
│   ├── educationValidator.js
│   ├── skillValidator.js
│   ├── resumeValidator.js
│   └── externalLinkValidator.js
│
├── middleware/          # Middlewares (tratamento de erros, etc.)
│
├── utils/               # Funções auxiliares (asyncHandler, etc.)
│
└── index.js             # Ponto de entrada principal da API
```

### 🔄 Fluxo de uma Requisição

```
Cliente → Rota → Validador → Controlador → Modelo → Banco de Dados → Resposta JSON
```

Exemplo:

```
POST /people/:personId/skills
    ↳ skillValidator.validateCreateSkill
        ↳ skillController.createSkill
            ↳ Skill.create({ personId, name, level })
                ↳ Retorna 201 Created com JSON
```

---

## ⚙️ Instalação e Execução Local

### 1. Clonar o Repositório

```bash
git clone https://github.com/SEU-USUARIO/api-curriculo.git
cd api-curriculo
```

### 2. Instalar Dependências

```bash
npm install
```

### 3. Configurar Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto (ou copie do `.env.sample`):

```
PORT=3000
POSTGRES_URL=postgres://usuario:senha@localhost:5432/curriculo_db
ERASE_DATABASE=false
```

### 4. Rodar o Servidor

```bash
npm start
```

A API estará disponível em:  
👉 `http://localhost:3000`

---

## 📚 Documentação da API (Endpoints)

### 🧍 Person (Entidade Pai)

| Método | Endpoint            | Descrição                                               |
| ------ | ------------------- | ------------------------------------------------------- |
| GET    | `/people`           | Lista todas as pessoas                                  |
| GET    | `/people/:personId` | Retorna uma pessoa e todas as suas relações             |
| POST   | `/people`           | Cria uma nova pessoa                                    |
| PUT    | `/people/:personId` | Atualiza informações da pessoa                          |
| DELETE | `/people/:personId` | Exclui a pessoa e todos os dados relacionados (CASCADE) |

---

### 🧠 Resume

| Método | Endpoint                              | Descrição                        |
| ------ | ------------------------------------- | -------------------------------- |
| GET    | `/people/:personId/resumes`           | Lista todos os resumos da pessoa |
| POST   | `/people/:personId/resumes`           | Cria um novo resumo              |
| PUT    | `/people/:personId/resumes/:resumeId` | Atualiza um resumo               |
| DELETE | `/people/:personId/resumes/:resumeId` | Exclui um resumo                 |

---

### 🎓 Education

| Método | Endpoint                                    | Descrição                  |
| ------ | ------------------------------------------- | -------------------------- |
| GET    | `/people/:personId/educations`              | Lista formações acadêmicas |
| POST   | `/people/:personId/educations`              | Cria uma nova formação     |
| PUT    | `/people/:personId/educations/:educationId` | Atualiza uma formação      |
| DELETE | `/people/:personId/educations/:educationId` | Exclui uma formação        |

---

### 💼 Experience

| Método | Endpoint                                      | Descrição                        |
| ------ | --------------------------------------------- | -------------------------------- |
| GET    | `/people/:personId/experiences`               | Lista experiências profissionais |
| POST   | `/people/:personId/experiences`               | Cria uma nova experiência        |
| PUT    | `/people/:personId/experiences/:experienceId` | Atualiza uma experiência         |
| DELETE | `/people/:personId/experiences/:experienceId` | Exclui uma experiência           |

---

### 🛠️ Skill

| Método | Endpoint                            | Descrição                    |
| ------ | ----------------------------------- | ---------------------------- |
| GET    | `/people/:personId/skills`          | Lista habilidades da pessoa  |
| POST   | `/people/:personId/skills`          | Adiciona uma nova habilidade |
| PUT    | `/people/:personId/skills/:skillId` | Atualiza uma habilidade      |
| DELETE | `/people/:personId/skills/:skillId` | Remove uma habilidade        |

---

### 🔗 External Links

| Método | Endpoint                          | Descrição                                           |
| ------ | --------------------------------- | --------------------------------------------------- |
| GET    | `/people/:personId/links`         | Lista links externos da pessoa                      |
| POST   | `/people/:personId/links`         | Cria um novo link (ex: LinkedIn, GitHub, Portfólio) |
| PUT    | `/people/:personId/links/:linkId` | Atualiza um link                                    |
| DELETE | `/people/:personId/links/:linkId` | Exclui um link                                      |

---

## ☁️ Deploy

O projeto está **100% compatível com deploy serverless na [Vercel](https://curriculo-express-9103iiy8a-matheus-projects-c7b72954.vercel.app)**.

### ✅ Variáveis de ambiente esperadas:

```
POSTGRES_URL=postgres://usuario:senha@host:5432/nome_banco
ERASE_DATABASE=false
```

### 📦 Estrutura de Deploy

- `vercel.json` define o comportamento da API e os builds.
- O banco de dados PostgreSQL pode ser hospedado em **Neon**, **Supabase** ou **Railway**.

---

## 🧑‍💻 Autor

**Matheus Kauã**  
Desenvolvedor Full Stack  
📧 mkmateus.dev@gmail.com  
🌐 [GitHub](https://github.com/mk-matheus) | [LinkedIn](www.linkedin.com/in/mk-matheus)

---

## 📄 Licença

Este projeto está licenciado sob a licença **MIT** – veja o arquivo [LICENSE](LICENSE) para mais detalhes.
