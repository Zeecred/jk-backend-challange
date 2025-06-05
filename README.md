# 🧠 Desafio Backend (API) - Fullstack Developer

Neste desafio, você deverá desenvolver uma **API RESTful** para gerenciar usuários, que será consumida pelo frontend desenvolvido no desafio complementar.

## ⚙️ Tecnologias Obrigatórias

- [Node.js](https://nodejs.org/)
- [Express.js](https://expressjs.com/)
- [TypeScript](https://www.typescriptlang.org/)
- [MySQL](https://www.mysql.com/)

---

## 🎯 O que deve ser implementado

### Recursos da API

- `POST /users/auth/login` — login com e-mail e senha
- `DELETE /users/auth/logout` — logout do sistema e inativação do token de acesso
- `GET /users` — listagem de usuários
- `POST /users` — criação de usuário
- `PUT /users/:id` — edição de usuário
- `DELETE /users/:id` — remoção de usuário

> A autenticação deve set feita com token JWT.

---

## 📚 Requisitos Técnicos

- Uso de **TypeScript**
- Organização por camadas (controllers, services, routes, etc)
- Conexão com banco de dados MySQL (pode usar ORM como TypeORM ou query builder)
- CORS liberado para consumo pelo frontend

---

## 🌟 Diferenciais

Os seguintes pontos não são obrigatórios, mas contarão como **diferenciais** na avaliação:

- Documentação da API com Swagger ou similar
- Testes unitários (com Jest)
- Middleware de tratamento de erros
- Validação de entrada (ex: class-validator ou similar)
- Scripts para criação e seed do banco

---

## 📁 Estrutura Inicial do Projeto
```
node-api-challenge/
├── src/
│   ├── controllers/
│   ├── entities/
│   ├── routes/
│   ├── services/
│   ├── middlewares/
│   ├── database/
│   │   ├── migrations/
│   │   └── seed/
│   ├── utils/
│   └── index.ts
├── .env.example
├── tsconfig.json
├── package.json
└── README.md
```
- 📄 .env.example
```
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=suasenha
DB_NAME=desafio_fullstack
JWT_SECRET=seusegredojwt
```
---

### 🧩 Modelagem de Banco com Separação PF e PJ
#### 🗂️ Tabelas
```
-- Tabela base de usuários
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  email VARCHAR(100) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  type ENUM('individual', 'business') NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- Pessoa Física
CREATE TABLE individual_person (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  name VARCHAR(100) NOT NULL,
  cpf VARCHAR(14) NOT NULL UNIQUE,
  birth_date DATE NOT NULL,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

-- Pessoa Jurídica
CREATE TABLE business_person (
  id INT AUTO_INCREMENT PRIMARY KEY,
  user_id INT NOT NULL,
  fantasy_name VARCHAR(100) NOT NULL,
  cnpj VARCHAR(18) NOT NULL UNIQUE,
  company_name VARCHAR(100),
  job_title VARCHAR(100),
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

---

## 🛠 Como rodar o projeto

```bash
# Instalar dependências
npm install

# Rodar migrations (se houver)
npm run migration:run

# Iniciar projeto
npm run dev
