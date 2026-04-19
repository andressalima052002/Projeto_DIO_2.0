# Base de Conhecimento

## Dados Utilizados

Para o funcionamento do PoupaFoco AI, foram utilizados arquivos locais para simular o banco de dados do usuário e as regras de negócio de rendimentos.

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `perfil_cliente.json` | JSON | Carregar a renda mensal, despesas fixas e as metas ativas do usuário. |
| `orcamento_mensal.csv` | CSV | Analisar o padrão de gastos do mês atual para identificar sobras e propor realocações. |
| `taxas_rendimento.json` | JSON | Fornecer as taxas de juros (CDI, Poupança, Tesouro Direto) usadas nas simulações de tempo das metas. |

---

## Adaptações nos Dados

> **Você modificou ou expandiu os dados mockados? Descreva aqui.**

Sim. Os dados mockados foram expandidos para suportar o conceito de "múltiplas metas simultâneas". Em vez de um saldo único, o `perfil_cliente.json` foi estruturado com um array de objetos chamado `metas_ativas`, onde cada meta possui um `valor_alvo`, `valor_acumulado` e `prazo_desejado`. O `orcamento_mensal.csv` também foi adaptado para incluir uma coluna de `categoria_gasto`, permitindo que o agente identifique de onde o usuário pode cortar despesas.

---

## Estratégia de Integração

### Como os dados são carregados?
> **Descreva como seu agente acessa a base de conhecimento.**

Os arquivos JSON e CSV são carregados em memória (usando as bibliotecas `json` e `pandas` do Python) no momento em que a sessão do chat é iniciada na interface. Os dados são processados e mantidos no estado da aplicação (session state) para evitar leituras repetitivas no disco a cada nova mensagem.

### Como os dados são usados no prompt?
> **Os dados vão no system prompt? São consultados dinamicamente?**

Os dados são injetados dinamicamente no System Prompt. Para economizar tokens e manter o foco do LLM, apenas o resumo do `perfil_cliente.json` e as anomalias positivas do `orcamento_mensal.csv` (ex: "sobrou R$ 150 em alimentação") são passados como contexto inicial. Se o usuário perguntar sobre simulações futuras, o agente aciona uma tool/função que consulta o `taxas_rendimento.json` pontualmente.

---

## Exemplo de Contexto Montado

> **Mostre um exemplo de como os dados são formatados para o agente.**

```text
Dados do Cliente:
- Nome: Rafael Silva
- Renda Mensal Disponível: R$ 5.200
- Fundo de Emergência: Em construção (R$ 3.000)

Metas Ativas:
1. Troca de Carro (Seminovo)
   - Alvo: R$ 35.000
   - Acumulado: R$ 12.000
   - Status: No prazo

2. Viagem de Férias (Chile)
   - Alvo: R$ 8.000
   - Acumulado: R$ 2.500
   - Status: Atrasado

3. Reserva para Pós-Graduação
   - Alvo: R$ 15.000
   - Acumulado: R$ 1.000
   - Status: Iniciado recentemente

Últimas movimentações e insights:
- O cliente economizou R$ 200 na categoria "Assinaturas e Streaming" este mês.
- Sugestão habilitada: Perguntar se deseja realocar os R$ 200 para a meta "Viagem de Férias (Chile)".
