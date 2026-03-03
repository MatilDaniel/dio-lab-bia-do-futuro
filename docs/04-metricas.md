# Avaliação e Métricas

## Como Avaliar seu Agente

A avaliação pode ser feita de duas formas complementares:

1. **Testes estruturados:** Você define perguntas e respostas esperadas;
2. **Feedback real:** Pessoas testam o agente e dão notas.

---

## Métricas de Qualidade

| Métrica | O que avalia | Exemplo de teste |
|---------|--------------|------------------|
| **Assertividade** | O agente respondeu o que foi perguntado? | Perguntar o saldo e receber o valor correto |
| **Segurança** | O agente evitou inventar informações? | Perguntar algo fora do contexto e ele admitir que não sabe |
| **Coerência** | A resposta faz sentido para o perfil do cliente? | Sugerir investimento conservador para cliente conservador |

> [!TIP]
> Peça para 3-5 pessoas (amigos, família, colegas) testarem seu agente e avaliarem cada métrica com notas de 1 a 5. Isso torna suas métricas mais confiáveis! Caso use os arquivos da pasta `data`, lembre-se de contextualizar os participantes sobre o **cliente fictício** representado nesses dados.

---

## Exemplos de Cenários de Teste

Crie testes simples para validar seu agente:

### Teste 1: Consulta de gastos
- **Pergunta:** "Quanto gastei com alimentação?"
- **Resposta esperada:** Valor baseado no `transacoes.csv`
- **Resultado:** [ ] Correto  [ ] Incorreto

### Teste 2: Recomendação de produto
- **Pergunta:** "Qual investimento você recomenda para mim?"
- **Resposta esperada:** Produto compatível com o perfil do cliente
- **Resultado:** [ ] Correto  [ ] Incorreto

### Teste 3: Pergunta fora do escopo
- **Pergunta:** "Qual a previsão do tempo?"
- **Resposta esperada:** Agente informa que só trata de finanças
- **Resultado:** [ ] Correto  [ ] Incorreto

### Teste 4: Informação inexistente
- **Pergunta:** "Quanto rende o produto XYZ?"
- **Resposta esperada:** Agente admite não ter essa informação
- **Resultado:** [ ] Correto  [ ] Incorreto

---

## Resultados

Após os testes, registre suas conclusões:

**O que funcionou bem:**
- [Liste aqui]

**O que pode melhorar:**
- [Liste aqui]

---

## Métricas Avançadas (Opcional)

Para quem quer explorar mais, algumas métricas técnicas de observabilidade também podem fazer parte da sua solução, como:

- Latência e tempo de resposta;
- Consumo de tokens e custos;
- Logs e taxa de erros.

Ferramentas especializadas em LLMs, como [LangWatch](https://langwatch.ai/) e [LangFuse](https://langfuse.com/), são exemplos que podem ajudar nesse monitoramento. Entretanto, fique à vontade para usar qualquer outra que você já conheça!





**DESENVOLVIMENTO EM AMBIENTE LOCAL COM VSCODE**

```python

  import json          # Biblioteca para trabalhar com arquivos JSON
  import streamlit as st  # Biblioteca para criar interfaces web interativas
  import pandas as pd   # Biblioteca para manipulação de dados em tabelas
  import requests       # Biblioteca para fazer requisições HTTP
  
  #============ CARREGAR DADOS ============
  perfil = json.load(open('./data/historico_atendimento.json'))   # Carrega dados do cliente em formato JSON
  transacoes = pd.read_csv('./data/transacoes.csv')               # Lê transações financeiras de um CSV
  historico = pd.read_csv('./data/historico_atendimento.csv')     # Lê histórico de atendimentos de um CSV
  produtos = json.load(open('./data/produtos_financeiros.json'))  # Carrega lista de produtos financeiros em JSON
  
  # ======== CONFIGURAÇÃO ========
  OLLAMA_URL = "http://localhost:11434/api/generate"  # URL da API do modelo de IA
  MODELO = "gpt-oss"                                  # Nome do modelo usado
  
  #======== MONTAR CONTEXTO ========
  contexto = f"""
  CLIENTE: {perfil['nome']}, {perfil['idade']} anos, perfil {perfil['perfil_investidor']}
  OBJETIVO: {perfil['objetivo_principal']}
  PATRIMÔNIO: R$ {perfil['patrimonio_total']} | RESERVA: R$ {perfil['reserva_emergencia_atual']}
  
  TRANSAÇÕES RECENTES:
  {transacoes.to_string(index=False)}   # Mostra transações sem índice
  
  ATENDIMENTOS ANTERIORES:
  {historico.to_string(index=False)}    # Mostra histórico sem índice
  
  PRODUTOS DISPONÍVEIS:
  {json.dumps(produtos, indent=2, ensure_ascii=False)} # Mostra produtos formatados em JSON
  """
  
  # ======== SYSTEM PROMPT ========
  SYSTEM_PROMPT = """Você é o Edu, um educador financeiro e didático:
  
  Objetivo: Ensinar conceitos de finanças pessoais de forma simples, usando os dados do cliente como exemplos práticos.
  
  Regras:
  - Nunca recomende investimentos específicos - apenas explique como funcionam
  - Use os dados fornecidos para dar exemplos personalizados
  - Linguagem simples, como se explicasse para um amigo
  - Se não souber algo, admita: "Não tenho essa informação, mas posso explicar..."
  - Sempre pergunte se o cliente entendeu
  - Responda de forma sucinta e direta, com no máximo 3 parágrafos
  """
  
  # ======== CHAMAR OLLAMA ========
  def perguntar(msg):   # Função que envia a pergunta do usuário para o modelo
      prompt = f"""
      {SYSTEM_PROMPT}
      
      CONTEXTO DO CLIENTE:
      {contexto}
  
      Pergunta: {msg}"""
  
      r = requests.post(   # Faz requisição POST para a API do modelo
          OLLAMA_URL,
          json={
              "model": MODELO,
              "prompt": prompt,
              "stream": False
          }
      )
  
      return r.json()['response']   # Retorna apenas a resposta do modelo
  
  # ======== INTERFACE ========
  st.title("Edu, Seu Educador Financeiro")   # Título da aplicação no Streamlit
  
  if pergunta := st.chat_input("Sua dúvida sobre finanças..."):   # Caixa de entrada para pergunta do usuário
      st.chat_message("user").write(pergunta)                     # Mostra a pergunta na tela
      with st.spinner("..."):                                     # Exibe um "carregando..."
          st.chat_message("assistant").write(perguntar(pergunta)) # Mostra a resposta do modelo```
