# Base de Conhecimento

>[!TIP]
> **Prompt usado nesta etapa:**
> Preciso organizar a base de conhecimento do meu agente financeiro educativo.
> Tenho estes arquivos de dados: [liste os arquivos].
> Me ajude a:
> (1) entender o que cada arquivo contém,
> (2) decidir como usar cada um,
> (3) criar um exemplo de contexto formatado para incluir no prompt.

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização pelo Agente (Edu) |
|---------|---------|------------------------------|
| `historico_atendimento.csv` | CSV | Fornece o contexto histórico de todas as interações anteriores com o cliente. O agente utiliza esses dados para dar **continuidade ao atendimento de forma mais eficiente e fluida**, evitando que o usuário precise repetir informações, dúvidas ou problemas que já foram abordados no passado. |
| `perfil_investidor.json` | JSON | Contém as informações detalhadas sobre as características e tolerância a risco do usuário. É utilizado pelo agente para **personalizar as explicações sobre dúvidas** financeiras e focar nas **necessidades específicas de aprendizado do cliente**, adaptando a didática e o nível de complexidade das respostas. |
| `produtos_financeiros.json` | JSON | Atua como a base de conhecimento de ofertas da instituição. Serve para que o agente **conheça a fundo os produtos disponíveis** (taxas, prazos, riscos, etc.) e garanta que eles **possam ser ensinados ao cliente** de forma clara, aliando teoria de educação financeira com opções práticas de investimento. |
| `transacoes.csv` | CSV | Fornece o extrato detalhado de movimentações financeiras. O agente (Edu) acessa este arquivo para **analisar o padrão de gastos do cliente** no dia a dia. O objetivo é **usar essas informações de forma estritamente didática**, trazendo exemplos da própria realidade do cliente para ensinar conceitos de planejamento financeiro. |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

O produto financeiro "Fundo de Mercado" foi substituído por "Fundo de Investimento Imobiliário (FII)" nos dados mockados. Essa alteração foi realizada para alinhar a base de conhecimento à experiência prévia com produtos financeiros, permitindo uma validação mais assertiva das respostas geradas, com o objetivo de garantir que o agente tivesse informações precisas e atualizadas sobre os investimentos disponíveis, facilitando a geração de conteúdo relevante e confiável para os usuários.

---

## Estratégia de Integração

### Como os dados são carregados?

> Descreva como seu agente acessa a base de conhecimento.

Os dados podem ser inseridos manualmente de forma direta no prompt (via copiar e colar) ou carregados programaticamente no início da execução. Para o carregamento automatizado dos arquivos CSV e JSON, utilizamos as bibliotecas `pandas` e `json` do Python, conforme o código abaixo:

```python
import pandas as pd
import json

perfil = json.load(open('./data/perfil_investidor.json'))
transacoes = pd.read_csv('./data/transacoes.csv')
historico = pd.read_csv('./data/historico_atendimento.csv')
produtos = json.load(open('./data/produtos_financeiros.json'))
```

### Como os dados são usados no prompt?

> Os dados vão no system prompt? São consultados dinamicamente?

Para simplificar, os dados são injetados diretamente no prompt, garantindo que o agente tenha o melhor contexto possível. Vale ressaltar que, em soluções mais robustas, o ideal é que essas informações sejam carregadas dinamicamente para proporcionar maior flexibilidade.

```text
DADOS E PERFIL DO CLIENTE (data/perfil_investidor.json)
{
  "nome": "Francisco Alves",
  "idade": 52,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}

TRANSAÇÕES DO CLIENTE (data/transacoes.csv)
data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,5000.00,entrada
2025-10-02,Aluguel,moradia,1200.00,saida
2025-10-03,Supermercado,alimentacao,450.00,saida
2025-10-05,Netflix,lazer,55.90,saida
2025-10-07,Farmácia,saude,89.00,saida
2025-10-10,Restaurante,alimentacao,120.00,saida
2025-10-12,Uber,transporte,45.00,saida
2025-10-15,Conta de Luz,moradia,180.00,saida
2025-10-20,Academia,saude,99.00,saida
2025-10-25,Combustível,transporte,250.00,saida

HISTÓRICO DE ATENDIMENTO (data/historico_atendimento.csv)
data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim

PRODUTOS FINANCEIROS (data/produtos_financeiros.json)
[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Imobiliário (FII)",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "6% a 12% ao ano, proveniente principalmente do Dividend Yield (DY)",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil moderado que busca diversificação e renda recorrente mensal"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]
```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

O exemplo de contexto montado abaixo baseia-se nos dados originais da base de conhecimento. Os dados foram sintetizados em uma estrutura mais objetiva para otimizar o consumo de tokens. Contudo, vale destacar que garantir que o agente receba todas as informações essenciais para um atendimento de qualidade é sempre a prioridade, mesmo que isso custe mais tokens.

```text
DADOS DO CLIENTE:
- Nome: Francisco Alves
- Perfil: Moderado
- Objetivo: Construir reserva de emergência
- Reserva atual: R$ 10.000 (meta: R$ 15.000)

RESUMO DE GASTOS:
- Moradia: R$ 1.380
- Alimentação: R$ 570
- Transporte: R$ 295
- Saúde: R$ 188
- Lazer: R$ 55,90
- Total de saídas: R$ 2.488,90

PRODUTOS DISPONÍVEIS PARA EXPLICAR:
- Tesouro Selic (risco baixo)
- CDB Liquidez Diária (risco baixo)
- LCI/LCA (risco baixo)
- Fundo Imobiliário - FII (risco médio)
- Fundo de Ações (risco alto)
```
