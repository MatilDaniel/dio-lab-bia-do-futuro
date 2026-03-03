# 💰 EDU — Educador Financeiro com IA Generativa

> Agente financeiro inteligente que antecipa necessidades, personaliza sugestões e orienta o usuário de forma consultiva e segura.

---

## 🎯 Sobre o Projeto

**Edu** é um agente de educação financeira desenvolvido com IA Generativa como solução para o laboratório da [DIO](https://www.dio.me/). O objetivo é oferecer uma experiência de consultoria financeira acessível, personalizada e confiável — indo além dos chatbots reativos tradicionais.

**Problema resolvido:** Muitas pessoas não mantêm controle financeiro por falta de orientação prática e personalizada. A Bia atua como uma consultora proativa, guiando o usuário em decisões de gastos, metas e investimentos.

---

## 🏗️ Arquitetura da Solução

```
📁 dio-lab-bia-do-futuro/
│
├── 📁 src/          # Aplicação principal (Python)
│   └── app.py       # Chatbot interativo com LLM
│
├── 📁 data/         # Base de conhecimento (dados mockados)
│   ├── transacoes.csv
│   ├── historico_atendimento.csv
│   ├── perfil_investidor.json
│   └── produtos_financeiros.json
│
├── 📁 docs/         # Documentação técnica
│   ├── 01-documentacao-agente.md
│   ├── 02-base-conhecimento.md
│   ├── 03-prompts.md
│   ├── 04-metricas.md
│   └── 05-pitch.md
│
├── 📁 assets/       # Diagramas e imagens
└── 📁 examples/     # Referências de implementação
```

---

## 🤖 Funcionalidades do Agente

- **Análise de perfil** — lê o perfil do investidor e histórico de transações para personalizar respostas
- **Orientação proativa** — sugere ações antes de o usuário perguntar
- **Consulta a produtos financeiros** — recomenda produtos alinhados ao perfil do cliente
- **Respostas seguras** — sistema anti-alucinação com base em dados confiáveis
- **Histórico contextual** — utiliza atendimentos anteriores para manter continuidade

---

## 🛠️ Tecnologias Utilizadas

| Camada | Tecnologia |
|---|---|
| LLM | Claude / OpenAI API |
| Interface | Python (Streamlit / CLI) |
| Dados | CSV + JSON (dados mockados) |
| Orquestração | LangChain | PYTHON

---

## 🚀 Como Executar

```bash
# Clone o repositório
git clone https://github.com/MatilDaniel/dio-lab-bia-do-futuro.git
cd dio-lab-bia-do-futuro

# Instale as dependências
pip install -r requirements.txt

# Execute o agente
python src/app.py
```

---

## 📊 Métricas de Avaliação

- ✅ Precisão das respostas financeiras
- ✅ Taxa de respostas sem alucinação
- ✅ Coerência com o perfil do cliente
- ✅ Satisfação simulada do usuário

Detalhes em [`docs/04-metricas.md`](./docs/04-metricas.md).

---

## 👤 Autor

**Daniel Matil** — Desenvolvido como solução para o laboratório *Bia do Futuro* da DIO.

[![GitHub](https://img.shields.io/badge/GitHub-MatilDaniel-181717?style=flat&logo=github)](https://github.com/MatilDaniel)
