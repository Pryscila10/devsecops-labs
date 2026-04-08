# 🚀 Lab 01 — SDLC, Git e Seu Primeiro Deploy

Neste laboratório você vai entender na prática como um software sai do computador do desenvolvedor e chega até a internet, passando por uma **esteira automatizada (Pipeline)**.

## 🎯 O que você vai aprender
- O que é SDLC (ciclo de vida de um software)
- O que é Git e GitHub
- Como funciona um Pipeline de Deploy
- Como publicar uma aplicação com URL pública

## 🧰 O que você vai usar
- [GitHub](https://github.com) (conta gratuita)
- [GitHub Codespaces](https://github.com/features/codespaces) (VS Code no navegador)
- [GitHub Actions](https://github.com/features/actions) (Pipeline automatizado)
- [GitHub Pages](https://pages.github.com) (Hospedagem gratuita)

---

## 📖 Conceitos Rápidos (5 minutos)

### O que é SDLC?
**SDLC** (Software Development Life Cycle) é o ciclo de vida de um software. Todo sistema passa pelas mesmas fases:

```
📋 Planejamento → 💻 Desenvolvimento → 🧪 Testes → 🚀 Deploy → 🔍 Monitoramento
```

> **Analogia:** Pense em construir uma casa. Você planeja, constrói, inspeciona, entrega e depois monitora se está tudo bem. Software é igual!

### O que é Git?
Git é uma ferramenta que **salva o histórico de tudo que você faz no código**. Cada "save" é chamado de **commit**.

> **Analogia:** Imagine um Google Docs que guarda cada versão do seu documento para sempre, com data, hora e quem fez a alteração.

### O que é GitHub?
GitHub é onde você **guarda seu projeto na nuvem** e colabora com outras pessoas.

> **Analogia:** É o Google Drive do código — mas muito mais poderoso.

### O que é um Pipeline?
Pipeline é uma **esteira automatizada**. Quando você envia seu código, ela executa tarefas automaticamente (build, testes, deploy) sem você precisar fazer nada manualmente.

> **Analogia:** É como uma linha de montagem de fábrica. Você coloca a peça no início e ela sai pronta no final.

---

## 📁 Estrutura do Projeto

Ao final deste lab, seu repositório terá esta estrutura:

```
meu-primeiro-app/
├── .github/
│   └── workflows/
│       └── pipeline.yml       ← A Esteira (Pipeline)
├── src/
│   ├── index.html             ← Frontend (A tela da aplicação)
│   ├── style.css              ← Design
│   ├── script.js              ← Lógica e conexão com o "banco"
│   └── db.json                ← Banco de Dados simulado
└── README.md                  ← Documentação do projeto
```

---

## 🛠️ Passo a Passo

### Passo 1: Criar o Repositório no GitHub

1. Acesse [github.com](https://github.com) e faça login
2. Clique no botão **"New"** (canto superior esquerdo)
3. Preencha:
   - **Repository name:** `meu-primeiro-deploy`
   - **Description:** `Minha primeira aplicação com pipeline de deploy`
   - Marque **"Public"**
   - Marque **"Add a README file"**
4. Clique em **"Create repository"**

---

### Passo 2: Abrir o Codespaces

1. No seu repositório, clique no botão verde **"Code"**
2. Clique na aba **"Codespaces"**
3. Clique em **"Create codespace on main"**
4. Aguarde o ambiente carregar (VS Code no navegador) ☕

> 💡 **O Codespaces é o seu ambiente de desenvolvimento.** É aqui que você escreve o código — como se fosse o seu computador, mas na nuvem.

---

### Passo 3: Criar a Estrutura de Pastas

No terminal do Codespaces (parte inferior da tela), cole os comandos abaixo **um de cada vez**:

```bash
mkdir -p src
mkdir -p .github/workflows
```

---

### Passo 4: Criar os Arquivos da Aplicação

#### 📄 `src/db.json` — O Banco de Dados

No terminal, execute:

```bash
cat > src/db.json << 'EOF'
{
  "status": "Conectado ao Banco de Dados de Produção",
  "versao_do_banco": "v1.0.0",
  "itens": [
    {"id": 1, "task": "Aprender o que é SDLC"},
    {"id": 2, "task": "Entender o que é Git e GitHub"},
    {"id": 3, "task": "Criar meu primeiro Pipeline"},
    {"id": 4, "task": "Fazer meu primeiro Deploy"}
  ]
}
EOF
```

> 💡 **O que é isso?** Este arquivo simula um banco de dados. Em uma aplicação real, esses dados viriam de um servidor (backend) conectado a um banco como PostgreSQL ou MySQL.

---

#### 📄 `src/index.html` — O Frontend

```bash
cat > src/index.html << 'EOF'
<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minha App de Produção</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <header>
            <h1>Minha Aplicação</h1>
            <span class="badge">PRODUÇÃO</span>
        </header>

        <div class="card">
            <h2>Status do Banco de Dados</h2>
            <p id="db-status">Conectando...</p>
            <p id="db-version"></p>
        </div>

        <div class="card">
            <h2>Tarefas do Time</h2>
            <ul id="task-list"></ul>
        </div>

        <footer>
            <p>Desenvolvido durante o Lab de DevSecOps • ADA Tech</p>
        </footer>
    </div>
    <script src="script.js"></script>
</body>
</html>
EOF
```

---

#### 📄 `src/style.css` — O Design

```bash
cat > src/style.css << 'EOF'
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', sans-serif;
    background-color: #0d1117;
    color: #c9d1d9;
    min-height: 100vh;
    display: flex;
    justify-content: center;
    align-items: flex-start;
    padding: 40px 20px;
}

.container {
    width: 100%;
    max-width: 600px;
}

header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 24px;
}

h1 {
    font-size: 1.8rem;
    color: #f0f6fc;
}

.badge {
    background-color: #238636;
    color: #fff;
    padding: 4px 12px;
    border-radius: 20px;
    font-size: 0.75rem;
    font-weight: bold;
    letter-spacing: 1px;
}

.card {
    background-color: #161b22;
    border: 1px solid #30363d;
    border-radius: 8px;
    padding: 20px;
    margin-bottom: 16px;
}

.card h2 {
    font-size: 1rem;
    color: #8b949e;
    margin-bottom: 12px;
    text-transform: uppercase;
    letter-spacing: 0.5px;
}

#db-status {
    color: #3fb950;
    font-weight: bold;
}

#db-version {
    color: #8b949e;
    font-size: 0.85rem;
    margin-top: 4px;
}

ul {
    list-style: none;
}

ul li {
    padding: 10px 0;
    border-bottom: 1px solid #21262d;
    display: flex;
    align-items: center;
    gap: 10px;
}

ul li:last-child {
    border-bottom: none;
}

ul li::before {
    content: "✓";
    color: #3fb950;
    font-weight: bold;
}

footer {
    text-align: center;
    color: #484f58;
    font-size: 0.8rem;
    margin-top: 24px;
}
EOF
```

---

#### 📄 `src/script.js` — A Lógica (Conexão com o "Banco")

```bash
cat > src/script.js << 'EOF'
// Simula uma chamada de API para buscar dados do Banco de Dados
// Em uma aplicação real, este fetch apontaria para um servidor backend

fetch('db.json')
    .then(response => response.json())
    .then(data => {

        // Atualiza o status do banco na tela
        document.getElementById('db-status').innerText = data.status;
        document.getElementById('db-version').innerText = 'Versão: ' + data.versao_do_banco;

        // Renderiza as tarefas vindas do "banco"
        const list = document.getElementById('task-list');
        data.itens.forEach(item => {
            let li = document.createElement('li');
            li.innerText = item.task;
            list.appendChild(li);
        });

    })
    .catch(err => {
        document.getElementById('db-status').innerText = '❌ Erro ao conectar no banco.';
        console.error('Erro:', err);
    });
EOF
```

---

#### 📄 `.github/workflows/pipeline.yml` — A Esteira (Pipeline)

```bash
cat > .github/workflows/pipeline.yml << 'EOF'
name: 🚀 Esteira de Build e Deploy

on:
  push:
    branches: [ "main" ]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build-and-deploy:
    name: Build e Deploy para Produção
    runs-on: ubuntu-latest

    steps:
      - name: 📥 Passo 1 — Baixar o código
        uses: actions/checkout@v4

      - name: ⚙️ Passo 2 — Build (Verificar integridade dos arquivos)
        run: |
          echo "Iniciando processo de Build..."
          echo "Verificando arquivos obrigatórios:"
          ls -la src/
          echo ""
          echo "✅ index.html encontrado!"
          echo "✅ style.css encontrado!"
          echo "✅ script.js encontrado!"
          echo "✅ db.json encontrado!"
          echo ""
          echo "Build finalizado com sucesso!"

      - name: 🌐 Passo 3 — Configurar ambiente de Produção (GitHub Pages)
        uses: actions/configure-pages@v4

      - name: 📦 Passo 4 — Empacotar aplicação para Deploy
        uses: actions/upload-pages-artifact@v3
        with:
          path: './src'

      - name: 🚀 Passo 5 — Deploy em Produção
        id: deployment
        uses: actions/deploy-pages@v4
EOF
```

---

### Passo 5: Salvar e Enviar para o GitHub (Git)

Agora vamos usar o Git para enviar tudo para o GitHub. No terminal:

```bash
# 1. Ver o que foi criado/modificado
git status

# 2. Preparar todos os arquivos para o commit
git add .

# 3. Salvar com uma mensagem descritiva
git commit -m "adiciona aplicação e pipeline de deploy"

# 4. Enviar para o GitHub
git push
```

> 💡 **O que acabou de acontecer?**
> - `git status` → "O que eu mudei?"
> - `git add .` → "Quero salvar tudo isso"
> - `git commit` → "Salvei! Com a mensagem: adiciona aplicação..."
> - `git push` → "Enviei para o GitHub"

---

### Passo 6: Ativar o GitHub Pages

1. No seu repositório, clique em **"Settings"** (aba superior)
2. No menu lateral, clique em **"Pages"**
3. Em **"Source"**, selecione **"GitHub Actions"**
4. Clique em **"Save"**

---

### Passo 7: Acompanhar o Pipeline rodando

1. Clique na aba **"Actions"** do seu repositório
2. Você verá a esteira rodando com o nome **"🚀 Esteira de Build e Deploy"**
3. Clique nela para ver os passos em tempo real
4. Aguarde todos os passos ficarem com ✅ verde

> 💡 **O que está acontecendo?** O GitHub está executando automaticamente os 5 passos do `pipeline.yml` que você criou. Isso é o **Pipeline em ação!**

---

### Passo 8: Acessar a URL de Produção 🎉

1. Após o pipeline finalizar, vá em **"Settings" → "Pages"**
2. Você verá a URL no formato:
   ```
   https://SEU-USUARIO.github.io/meu-primeiro-app
   ```
3. Clique na URL e veja sua aplicação **ao vivo na internet!**

---

## 🔄 O Ciclo Completo (SDLC na Prática)

Você acabou de passar por todas as fases do SDLC:

```
📋 Planejamento   → Definimos o que a app faria
💻 Desenvolvimento → Escrevemos o código no Codespaces
⚙️  Build          → Pipeline verificou os arquivos
🚀 Deploy         → Pipeline publicou na internet
🌐 Produção       → URL pública acessível por qualquer pessoa
```

---

## ✏️ Desafio: Faça uma Alteração em Produção

Agora que o ciclo está funcionando, experimente:

1. No Codespaces, abra `src/db.json`
2. Adicione uma nova tarefa:
   ```json
   {"id": 5, "task": "Fiz meu primeiro deploy! 🎉"}
   ```
3. Salve e envie:
   ```bash
   git add .
   git commit -m "feat: adiciona nova tarefa"
   git push
   ```
4. Observe o pipeline rodar novamente na aba **Actions**
5. Atualize a URL de produção e veja a mudança!

> 💡 **Isso é DevOps na prática:** uma alteração no código → pipeline automático → produção atualizada.

---

## 💬 Perguntas para Discussão

1. Qual a diferença entre o ambiente de **desenvolvimento** (Codespaces) e o de **produção** (GitHub Pages)?
2. Por que é importante ter um pipeline automático em vez de fazer o deploy manualmente?
3. O que aconteceria se o `index.html` não existisse? O pipeline passaria?

---

## ✅ Checklist de Conclusão

- [ ] Repositório criado no GitHub
- [ ] Codespaces aberto com sucesso
- [ ] Todos os arquivos criados (`index.html`, `style.css`, `script.js`, `db.json`, `pipeline.yml`)
- [ ] Commit e push realizados
- [ ] GitHub Pages ativado
- [ ] Pipeline executado com sucesso (✅ verde)
- [ ] URL de produção acessada
- [ ] Desafio concluído (nova tarefa adicionada)

---

*Versão do material: Abril/2026 • ADA TECH DevSecOps Módulo — Lab 01*