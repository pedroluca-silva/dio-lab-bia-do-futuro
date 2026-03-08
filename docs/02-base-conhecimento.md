# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar recomendações |
| `produtos_financeiros.json` | JSON | Sugerir produtos adequados ao perfil |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Adição do produto Fundo imobiliário

---
## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os dados são carregados manualmente ou através do código abaixo:
``` Python 
import pandas as pd
import json
#CSV
transacoes = pd.read_csv('./data/transacoes.csv')

historico = pd.read_csv('./data/historico_atendimento.csv')
#JSON
with open('./data/perfil_investidor.json', encoding='utf-8') as f:
    perfil = json.load(f)

with open('./data/produtos_financeiros.json', encoding='utf-8') as f:
    produtos = json.load(f)
```
### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Para que o agente consiga gerar respostas contextualizadas, é necessário fornecer informações relevantes sobre o cliente, seus objetivos financeiros e o conjunto de produtos disponíveis.

Em ambientes simples ou durante a fase de prototipagem, esses dados podem ser inseridos diretamente no prompt ou carregados manualmente no sistema. Já em aplicações mais completas, o ideal é que essas informações sejam obtidas dinamicamente a partir de bancos de dados, APIs ou arquivos estruturados.

Abaixo está um exemplo fictício de base de conhecimento utilizada pelo agente.

---
```
DADOS DO CLIENTE E TIPOS DE PERFIS FINANCEIROS (data/perfil_investidor.json):

{
  "nome": "Ricardo Almeida",
  "idade": 41,
  "profissao": "Gerente de Logística",
  "renda_mensal": 9800.00,
  "objetivo_principal": "Diversificar investimentos e aumentar patrimônio",
  "patrimonio_total": 125000.00,
  "reserva_emergencia_atual": 20000.00,
  "aceita_risco": true,
  "metas": [
    {
      "meta": "Aumentar carteira de investimentos",
      "valor_necessario": 200000.00,
      "prazo": "2028-06"
    },
    {
      "meta": "Planejamento de aposentadoria",
      "valor_necessario": 800000.00,
      "prazo": "2045-01"
    }
  ]
}

{
  "Perfis de Investidores": [
    {
      "perfil": "Conservador",
      "caracteristicas": [
        "Busca estabilidade financeira",
        "Prioriza preservação do capital",
        "Evita grandes oscilações"
      ],
      "comportamento": [
        "Prefere investimentos previsíveis",
        "Evita ativos com volatilidade elevada"
      ],
      "horizonte_investimento": [
        "Curto prazo",
        "Até 2 anos"
      ]
    },
    {
      "perfil": "Moderado",
      "caracteristicas": [
        "Combina segurança com potencial de retorno",
        "Aceita pequenas oscilações de mercado",
        "Busca diversificação"
      ],
      "comportamento": [
        "Mantém investimentos mesmo com pequenas quedas",
        "Busca equilíbrio entre risco e retorno"
      ],
      "horizonte_investimento": [
        "Médio prazo",
        "Entre 2 e 5 anos"
      ]
    },
    {
      "perfil": "Arrojado",
      "caracteristicas": [
        "Alta tolerância a risco",
        "Busca retornos elevados",
        "Confortável com volatilidade"
      ],
      "comportamento": [
        "Mantém investimentos durante ciclos de mercado",
        "Aceita oscilações significativas"
      ],
      "horizonte_investimento": [
        "Longo prazo",
        "Mais de 5 anos"
      ]
    }
  ]
}

TRANSACOES DO CLIENTE (data/transacoes.csv):

data,descricao,categoria,valor,tipo
2025-10-01,Salário,receita,9800.00,entrada
2025-10-02,Financiamento imobiliário,moradia,2400.00,saida
2025-10-04,Supermercado,alimentacao,620.00,saida
2025-10-06,Streaming,lazer,49.90,saida
2025-10-08,Plano de saúde,saude,450.00,saida
2025-10-11,Restaurante,alimentacao,210.00,saida
2025-10-14,Aplicativo de transporte,transporte,90.00,saida
2025-10-18,Conta de energia,moradia,220.00,saida
2025-10-22,Academia,saude,130.00,saida
2025-10-27,Combustível,transporte,360.00,saida

HISTORICO DE ATENDIMENTO DO CLIENTE (data/historico_atendimento.csv):

data,canal,tema,resumo,resolvido
2025-09-12,chat,CDB,Cliente solicitou informações sobre rentabilidade de CDB,sim
2025-09-18,telefone,Cartão de crédito,Cliente reportou transação desconhecida,sim
2025-09-29,chat,Tesouro IPCA,Cliente pediu explicação sobre proteção contra inflação,sim
2025-10-07,email,Atualização cadastral,Cliente atualizou endereço residencial,sim
2025-10-20,chat,Planejamento financeiro,Cliente pediu orientação sobre diversificação,sim

PRODUTOS DISPONIVEIS PARA ENSINO (data/produtos_financeiros.json):

[
  {
    "nome": "Tesouro IPCA+",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "IPCA + taxa prefixada",
    "aporte_minimo": 30.00,
    "indicado_para": "Investidores que buscam proteção contra inflação"
  },
  {
    "nome": "CDB com Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "103% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Reserva de emergência e liquidez imediata"
  },
  {
    "nome": "Debêntures",
    "categoria": "renda_fixa",
    "risco": "medio",
    "rentabilidade": "CDI + taxa adicional",
    "aporte_minimo": 1000.00,
    "indicado_para": "Investidores que buscam renda maior que renda fixa tradicional"
  },
  {
    "nome": "ETF de Mercado",
    "categoria": "renda_variavel",
    "risco": "medio",
    "rentabilidade": "Acompanha índice de mercado",
    "aporte_minimo": 100.00,
    "indicado_para": "Investidores que desejam diversificação"
  },
  {
    "nome": "Ações de Tecnologia",
    "categoria": "renda_variavel",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Investidores com foco em crescimento de longo prazo"
  }
]
```


---
## Exemplo de Contexto Montado

Embora a base de conhecimento contenha diversas informações detalhadas, na prática muitas aplicações utilizam apenas um subconjunto desses dados no prompt final enviado ao modelo.

Esse processo de resumir e organizar o contexto ajuda a reduzir o uso de tokens e melhora a eficiência das respostas geradas pelo modelo.

Abaixo está um exemplo de como essas informações poderiam ser condensadas antes de serem enviadas ao modelo de linguagem.


```
Dados do Cliente:
- Nome: Ricardo Almeida
- Aceita riscos: True
- Objetivo: Diversificar investimentos e aumentar patrimônio
- Reserva atual: R$ 20.000

Últimas transações:
- Moradia: R$ 2.620
- Alimentação: R$ 830
- Transporte: R$ 450
- Saúde: R$ 580
- Lazer: R$ 49,90
- Total de saídas: R$ 4.529,90

Perfis de Investidores:
- Conservador: Baixa tolerância ao risco
- Moderado: Tolerância média ao risco
- Arrojado: Alta tolerância ao risco

Produtos disponíveis para recomendação:
- Tesouro IPCA+ (risco baixo)
- CDB com Liquidez Diária (risco baixo)
- Debêntures (risco médio)
- ETF de Mercado (risco médio)
- Ações de Tecnologia (risco alto)
```
