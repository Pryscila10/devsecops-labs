# 🛠️ Prática: Semgrep + Gitleaks no GitHub Codespaces

Este laboratório foca na aplicação do conceito de **Shift-Left**, detectando vulnerabilidades e segredos expostos diretamente no ambiente de desenvolvimento, antes mesmo de realizar um commit.

## 🎯 Objetivos

* Executar análise estática (SAST) em um ambiente cloud.

* Identificar segredos expostos no código e no histórico do Git.

* Validar a correção de vulnerabilidades em tempo real.

## 🧰 Ferramentas Utilizadas

* [GitHub Codespaces](https://github.com/features/codespaces) (IDE na Nuvem)

* [Semgrep](https://semgrep.dev/) (SAST Tool)

* [Gitleaks](https://github.com/gitleaks/gitleaks) (Secrets Scanning)

---

## 📁 Arquivos do Repositório

Antes de começar, crie os arquivos abaixo no seu repositório GitHub. Cada bloco de código deve ser colado no arquivo correspondente.

---

### 📄 `app.js` — Código vulnerável (propositalmente)

> Cole este arquivo na **raiz** do repositório. Ele contém erros propositais para a prática.

```javascript
// Sistema de Login da Aula de DevSecOps
const express = require('express');
const app = express();

// --- ERRO 1: Senha exposta (Secret Exposure) ---
// Imagine que esta é a chave para acessar o cofre da empresa!
const API_KEY_COFRE = "SK-abc1234567890-PROD-SEGREDOS";

app.get('/login', (req, res) => {
    let user = req.query.user;

    // --- ERRO 2: Mensagem de erro com muita informação ---
    // Se o banco falhar, o sistema mostra detalhes técnicos pro atacante
    try {
        res.send("Bem-vindo, " + user);
    } catch (error) {
        res.status(500).send("Erro interno no servidor Node.js v14.1: " + error.stack);
    }
});

app.listen(3000, () => console.log('App rodando na porta 3000'));
```

---

### 📄 `.devcontainer/devcontainer.json` — Configuração do Codespaces

> Crie a pasta `.devcontainer` na raiz do repositório e cole este arquivo dentro dela.\
> Ele instala automaticamente o **Semgrep** e o **Gitleaks** quando o Codespace é criado.

```json
{
    "name": "Aula DevSecOps",
    "image": "mcr.microsoft.com/vscode/devcontainers/python:3.9",
    "features": {
        "ghcr.io/devcontainers/features/common-utils:1": {}
    },
    "postCreateCommand": "pip install semgrep && curl -sSfL https://raw.githubusercontent.com/gitleaks/gitleaks/main/scripts/install.sh | sh -s -- -b /usr/local/bin",
    "customizations": {
        "vscode": {
            "extensions": [
                "SonarSource.sonarlint-vscode",
                "zhuangtongfa.Material-theme"
            ]
        }
    }
}
```

> ⚠️ **Se o Gitleaks não instalar automaticamente**, rode este comando no terminal do Codespace:
>
> ```bash
> GITLEAKS_VERSION=$(curl -s https://api.github.com/repos/gitleaks/gitleaks/releases/latest | grep '"tag_name"' | cut -d'"' -f4 | tr -d 'v') && curl -sSfL "https://github.com/gitleaks/gitleaks/releases/download/v${GITLEAKS_VERSION}/gitleaks_${GITLEAKS_VERSION}_linux_x64.tar.gz" -o gitleaks.tar.gz && tar -xzf gitleaks.tar.gz gitleaks && sudo mv gitleaks /usr/local/bin/ && rm gitleaks.tar.gz
> ```

---

## 🚀 Passo a Passo

### Parte 1: Preparação do Ambiente

1. No topo deste repositório, clique no botão verde **Code**.

2. Selecione a aba **Codespaces** e clique em **Create codespace on main**.

3. Aguarde o carregamento do ambiente (VS Code no navegador).

### Parte 2: Análise Manual

1. Abra o arquivo `app.js`.

2. Tente localizar visualmente qualquer dado sensível (senhas, chaves de API, tokens).

### Parte 3: Executando o "Robô de Segurança" (SAST)

No terminal do seu Codespace, execute o comando abaixo para que o Semgrep analise o código:

```bash
semgrep scan --config auto
```

_Note como a ferramenta aponta a linha exata e explica o risco da vulnerabilidade encontrada._

### Parte 4: Caça aos Segredos no Histórico

O Gitleaks analisa não apenas os arquivos, mas todo o histórico de commits. Execute:

```bash
gitleaks detect -v
```

_Se a ferramenta encontrar "leaks", ela mostrará o "Fingerprint". Guarde essa informação._

### Parte 5: Correção (Shift-Left)

1. Edite o arquivo `app.js`.

2. Remova a chave exposta e substitua por uma chamada de variável de ambiente:

```javascript
const API_KEY_COFRE = process.env.API_KEY;
```

1. Salve o arquivo e rode novamente o `semgrep scan --config auto`.

### Parte 6 (Fins Didáticos): Ignorando Falsos Positivos

Para ignorar segredos antigos em ambiente de aula, use o arquivo `.gitleaksignore`.

1. Crie o arquivo na raiz do projeto.

2. Adicione os **fingerprints** (hashes longos) informados pelo Gitleaks no passo anterior:

```bash
cat > .gitleaksignore << 'EOF'
SEU_FINGERPRINT_AQUI_1
SEU_FINGERPRINT_AQUI_2
EOF
```

1. Rode `gitleaks detect -v` novamente e veja o status passar para "limpo".

---

## 💬 Perguntas para Discussão

1. Por que apenas apagar a chave do arquivo não resolve o problema para o Git?

2. Qual a diferença entre encontrar um erro agora (no Codespaces) vs. encontrar no Servidor de Produção?

3. Se essa fosse uma chave real, qual seria o próximo passo além de apagar o código?

---

## ✅ Checklist de Conclusão

* \[ \] Codespace criado com sucesso.

* \[ \] Scan de Semgrep realizado e erros identificados.

* \[ \] Scan de Gitleaks realizado (com detecção de segredos).

* \[ \] (Opcional) `.gitleaksignore` configurado.

---

_Versão do material: Abril/2026 • ADA TECH DevSecOps Módulo_