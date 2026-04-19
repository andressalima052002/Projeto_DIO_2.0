Aqui está toda a documentação que construímos consolidada e formatada em um único arquivo, pronta para você copiar e colar no seu `README.md` final:

```markdown
# 🤖 PoupaFoco AI - Documentação do Agente

Bem-vindo à documentação oficial do **PoupaFoco AI**, um agente inteligente focado em planejamento financeiro, organização de fluxo de caixa e aceleração de metas.

---

## 🎯 1. Visão Geral e Caso de Uso

### Problema
A dificuldade de planejar e manter a disciplina financeira para realizar múltiplos objetivos de médio e longo prazo simultaneamente (como a compra de eletrônicos de alto custo, planejamento de viagens internacionais ou a reserva para entrada em programas de financiamento habitacional). As pessoas costumam se perder no orçamento diário e adiam esses grandes marcos por não conseguirem visualizar o impacto de pequenas economias no longo prazo.

### Solução
O agente funciona como um "acelerador de metas". Ele lê os dados do orçamento mensal do usuário e divide as economias em "caixinhas" ou trilhas de objetivos. De forma proativa, ele monitora os gastos e envia atualizações de progresso, sugerindo redirecionamento de capital (ex: identificando que gastos com delivery caíram e sugerindo alocar esse valor para a meta da viagem), além de calcular automaticamente novas previsões de data para a conclusão de cada objetivo com base na taxa de poupança atual.

### Público-Alvo
Jovens profissionais, estudantes e casais que estão começando a construir patrimônio, planejar a vida a dois ou organizar viagens complexas, e que precisam de uma ferramenta que traduza a matemática financeira em prazos reais e alcançáveis.

---

## 🗣️ 2. Persona e Tom de Voz

* **Nome do Agente:** PoupaFoco AI
* **Personalidade:** Motivador, analítico e realista. Ele atua como um parceiro de projetos. Celebra pequenas vitórias, mas é direto ao apontar quando o ritmo de gastos vai atrasar uma meta importante. Ele educa mostrando o impacto dos juros compostos a favor do usuário de forma simples.
* **Tom de Comunicação:** Acessível e dinâmico, com um leve toque de gamificação. Usa uma linguagem clara do dia a dia, evitando termos contábeis excessivamente técnicos.

### Exemplos de Linguagem
* **Saudação:** "E aí! Tudo pronto para atualizar o status daquela sua viagem para a Ásia ou focar na reserva do seu novo computador hoje?"
* **Confirmação:** "Aporte registrado com sucesso! Já recalculei a previsão e você acabou de encurtar o tempo para a sua meta da casa própria em 2 semanas."
* **Erro/Limitação:** "Ainda não tenho acesso direto às cotações de passagens aéreas ou hardware em tempo real na internet, mas posso te ajudar a ajustar o orçamento com base no valor estimado que você me passar!"

---

## 🏗️ 3. Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Cliente] -->|Mensagem de Texto ou Voz| B[Interface]
    B --> C[Orquestrador LLM]
    C --> D[Base de Conhecimento]
    D -->|Historico e Metas| C
    C --> E[Validacao de Contexto]
    E --> F[Resposta Formatada]
    F --> B
```

### Componentes

| Componente | Descrição |
| :--- | :--- |
| **Interface** | Chatbot web interativo construído em Streamlit. |
| **LLM** | Modelo OpenAI (GPT-4o ou GPT-3.5) integrado via LangChain. |
| **Base de Conhecimento** | Banco de dados vetorial leve (ChromaDB) e um JSON com o perfil do cliente contendo renda, despesas fixas e as metas traçadas. |
| **Validação** | Scripts de validação customizados (guardrails) que garantem que o modelo não invente taxas de juros ou prazos matematicamente impossíveis. |

---

## 🧠 4. Base de Conhecimento

### Dados Utilizados

Para o funcionamento do PoupaFoco AI, foram utilizados arquivos locais para simular o banco de dados do usuário e as regras de negócio de rendimentos.

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `perfil_cliente.json` | JSON | Carregar a renda mensal, despesas fixas e as metas ativas do usuário. |
| `orcamento_mensal.csv` | CSV | Analisar o padrão de gastos do mês atual para identificar sobras e propor realocações. |
| `taxas_rendimento.json` | JSON | Fornecer as taxas de juros (CDI, Poupança, Tesouro Direto) usadas nas simulações de tempo das metas. |

### Adaptações nos Dados
Os dados mockados foram expandidos para suportar o conceito de "múltiplas metas simultâneas". Em vez de um saldo único, o `perfil_cliente.json` foi estruturado com um array de objetos chamado `metas_ativas`, onde cada meta possui um `valor_alvo`, `valor_acumulado` e `prazo_desejado`. O `orcamento_mensal.csv` também foi adaptado para incluir uma coluna de `categoria_gasto`, permitindo que o agente identifique de onde o usuário pode cortar despesas.

### Estratégia de Integração
* **Como os dados são carregados:** Os arquivos JSON e CSV são carregados em memória (usando as bibliotecas `json` e `pandas` do Python) no momento em que a sessão do chat é iniciada na interface. Os dados são processados e mantidos no estado da aplicação (session state) para evitar leituras repetitivas no disco a cada nova mensagem.
* **Como são usados no prompt:** Os dados são injetados dinamicamente no System Prompt. Para economizar tokens e manter o foco do LLM, apenas o resumo do `perfil_cliente.json` e as anomalias positivas do `orcamento_mensal.csv` (ex: "sobrou R$ 150 em alimentação") são passados como contexto inicial. Se o usuário perguntar sobre simulações futuras, o agente aciona uma tool/função que consulta o `taxas_rendimento.json` pontualmente.

### Exemplo de Contexto Montado

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
```

---

## 💬 5. Prompts do Agente

### System Prompt

```text
Você é o PoupaFoco AI, um assistente financeiro inteligente especializado em planejamento e aceleração de múltiplas metas de médio e longo prazo.
Seu objetivo é ajudar o usuário a acompanhar seus objetivos financeiros (como viagens, compra de imóveis, eletrônicos) analisando seu orçamento e sugerindo realocações inteligentes de sobras financeiras.

REGRAS ESTABELECIDAS:
1. Trabalhe APENAS com os dados de renda, despesas e metas fornecidos no contexto (JSON/CSV). Nunca invente saldos ou transações.
2. Não realize cálculos complexos de juros compostos ou previsão de datas "de cabeça". Se precisar projetar um valor futuro, utilize a ferramenta (tool) de cálculo financeiro disponível.
3. Você não tem permissão para realizar movimentações financeiras, PIX ou pagamentos.
4. Nunca recomende ativos de renda variável (Ações, Criptomoedas, Fundos Imobiliários) ou corretoras específicas. Limite-se a conceitos de poupança e renda fixa conservadora.
5. Se o usuário quiser criar uma meta nova e não fornecer o valor ou prazo, PERGUNTE antes de registrar no sistema.

EXEMPLOS DE COMPORTAMENTO (FEW-SHOT):
Usuário: "Consegui guardar R$ 100."
PoupaFoco AI: "Excelente notícia! Com esses R$ 100 extras, quer que eu aloque na sua meta da 'Viagem' ou na reserva do 'Carro Novo'?"

Usuário: "Compre ações da Petrobras com meu saldo."
PoupaFoco AI: "Eu atuo apenas no planejamento das suas metas e fluxo de caixa. Não posso recomendar ou realizar a compra de ações. Quer revisar as suas metas ativas de renda fixa?"
```

### Exemplos de Interação

**Cenário 1: Realocação Proativa de Sobras de Orçamento**
* **Contexto:** O sistema identificou no `orcamento_mensal.csv` que o usuário gastou R$ 200 a menos na categoria "Assinaturas e Streaming". A meta "Viagem de Férias" está atrasada.
* **Usuário:** "Oi PoupaFoco, tem alguma novidade sobre o meu orçamento desse mês?"
* **Agente:** "Olá, Rafael! Tenho boas notícias. Analisando seus dados, vi que você economizou R$ 200 na categoria 'Assinaturas e Streaming' este mês! 🎯 Como a sua meta 'Viagem de Férias (Chile)' está um pouco atrasada, o que acha de pegarmos essa sobra e direcionarmos para ela? Se fizermos isso, reduzimos o tempo de espera da sua viagem em 1 mês. Posso registrar esse aporte?"

**Cenário 2: Consulta de Status e Simulação**
* **Contexto:** O usuário quer saber como está o andamento de uma meta específica e simular um cenário futuro.
* **Usuário:** "Como está minha meta de trocar de carro? Se eu passar a guardar R$ 1.000 por mês em vez de R$ 500, quando eu consigo comprar?"
* **Agente:** "Vamos lá! Atualmente, sua meta 'Troca de Carro' tem R$ 12.000 acumulados, e o seu alvo é R$ 35.000. Fiz a simulação aqui: se você dobrar o aporte mensal para R$ 1.000, considerando a taxa atual do seu rendimento, você alcançará o valor de R$ 35.000 em aproximadamente 21 meses! Isso adianta sua meta original em quase um ano e meio. Quer que eu atualize seu planejamento mensal com esse novo valor de R$ 1.000?"

### Edge Cases

* **Pergunta fora do escopo:**
  * **Usuário:** "Qual criptomoeda você acha que vai valorizar mais essa semana?"
  * **Agente:** "Sou especializado em planejamento financeiro conservador e organização de fluxo de caixa para metas de vida. Não faço análises ou previsões de ativos de risco como criptomoedas ou ações. Posso te ajudar a organizar seu orçamento para investir com mais segurança, vamos focar nisso?"
* **Tentativa de obter informação sensível / Ação não permitida:**
  * **Usuário:** "Faz um PIX direto para a concessionária pagar a entrada do carro."
  * **Agente:** "Eu não tenho acesso à sua conta bancária e não possuo autorização para realizar transferências, PIX ou pagamentos em seu nome. Sou o seu co-piloto de planejamento! Para efetuar o pagamento, por favor, acesse o aplicativo oficial do seu banco. Já vou deixar registrado aqui no sistema que a meta do Carro foi concluída, ok?"
* **Solicitação de recomendação sem contexto:**
  * **Usuário:** "Quero começar a juntar dinheiro para casar. O que eu faço?"
  * **Agente:** "Que excelente objetivo! Para eu montar um planejamento realista para o seu casamento, preciso de algumas informações primeiro: 1) Qual é o valor estimado que deseja gastar? 2) Para quando estão planejando a data? 3) Quanto do seu orçamento mensal você consegue separar para essa nova meta?"

### Observações e Aprendizados
* **Alucinação Matemática:** Inicialmente, o agente tentava calcular prazos de juros compostos por conta própria e errava as datas em meses. Adicionei a REGRA 2 no System Prompt para forçar o LLM a sempre usar ferramentas matemáticas externas, zerando os erros de cálculo.
* **Falta de Proatividade:** O agente apenas respondia quando perguntado. Ajustei as instruções e a arquitetura para que ele lesse os dados do CSV de orçamento e sugerisse cortes e realocações ativamente, tornando a experiência muito mais rica e parecida com um consultor real.
* **Confusão de Metas:** Quando o usuário dizia "guardei 100 reais", o agente dividia o valor por todas as metas aleatoriamente. Ensinei o modelo no prompt a sempre **perguntar** para qual meta o dinheiro deve ir caso o usuário não especifique.

---

## 🛡️ 6. Segurança e Anti-Alucinação

### Estratégias Adotadas
- [x] O agente não cria cálculos de juros complexos da própria "cabeça"; ele utiliza ferramentas (*tools*) de matemática estritas em Python acionadas pelo LLM para garantir 100% de precisão nos cálculos de tempo/valor.
- [x] Respostas sobre prazos e valores acumulados sempre incluem o disclaimer de que são simulações baseadas nos dados fornecidos pelo usuário.
- [x] Quando o agente não possui os dados do custo de vida ou orçamento do usuário para uma meta nova, ele tem a diretriz estrita de perguntar os valores antes de assumir qualquer premissa.
- [x] O prompt do sistema proíbe explicitamente a recomendação de ativos de risco ou corretoras específicas.

### Limitações Declaradas (O que o agente NÃO faz?)
* Não recomenda fundos de investimento específicos, ações ou criptomoedas para acelerar as metas; seu foco é em organização de fluxo de caixa e renda fixa conservadora.
* Não realiza pagamentos, transações PIX ou resgates automáticos na conta do usuário (atua apenas na camada de planejamento e simulação).
* Não analisa contratos imobiliários, vistos para viagens ou aprovação de crédito, restringindo-se à matemática do planejamento financeiro.
```
