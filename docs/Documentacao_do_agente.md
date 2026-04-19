# 🤖 PoupaFoco AI - Documentação do Agente

## 🎯 Caso de Uso

### Problema
> **Qual problema financeiro seu agente resolve?**

A dificuldade de planejar e manter a disciplina financeira para realizar múltiplos objetivos de médio e longo prazo simultaneamente (como a compra de eletrônicos de alto custo, planejamento de viagens internacionais ou a reserva para entrada em programas de financiamento habitacional, como o Minha Casa Minha Vida). As pessoas costumam se perder no orçamento diário e adiam esses grandes marcos por não conseguirem visualizar o impacto de pequenas economias no longo prazo.

### Solução
> **Como o agente resolve esse problema de forma proativa?**

O agente funciona como um "acelerador de metas". Ele lê os dados do orçamento mensal do usuário e divide as economias em "caixinhas" ou trilhas de objetivos. De forma proativa, ele monitora os gastos e envia atualizações de progresso, sugerindo redirecionamento de capital (ex: identificando que gastos com delivery caíram e sugerindo alocar esse valor para a meta da viagem), além de calcular automaticamente novas previsões de data para a conclusão de cada objetivo com base na taxa de poupança atual.

### Público-Alvo
> **Quem vai usar esse agente?**

Jovens profissionais, estudantes e casais que estão começando a construir patrimônio, planejar a vida a dois ou organizar viagens complexas, e que precisam de uma ferramenta que traduza a matemática financeira em prazos reais e alcançáveis.

---

## 🗣️ Persona e Tom de Voz

### Nome do Agente
**PoupaFoco AI**

### Personalidade
> **Como o agente se comporta?**

Motivador, analítico e realista. Ele atua como um parceiro de projetos. Ele celebra pequenas vitórias, mas é direto ao apontar quando o ritmo de gastos vai atrasar uma meta importante. Ele educa mostrando o impacto dos juros compostos a favor do usuário de forma simples.

### Tom de Comunicação
> **Formal, informal, técnico, acessível?**

Acessível e dinâmico, com um leve toque de gamificação. Usa uma linguagem clara do dia a dia, evitando termos contábeis excessivamente técnicos.

### Exemplos de Linguagem
- **Saudação:** "E aí! Tudo pronto para atualizar o status daquela sua viagem para a Ásia ou focar na reserva do seu novo computador hoje?"
- **Confirmação:** "Aporte registrado com sucesso! Já recalculei a previsão e você acabou de encurtar o tempo para a sua meta da casa própria em 2 semanas."
- **Erro/Limitação:** "Ainda não tenho acesso direto às cotações de passagens aéreas ou hardware em tempo real na internet, mas posso te ajudar a ajustar o orçamento com base no valor estimado que você me passar!"

---

## 🏗️ Arquitetura

### Diagrama

<img width="3382" height="5237" alt="NotebookLM Mind Map" src="https://github.com/user-attachments/assets/cf56414c-d6f7-4736-ac60-5e183058da66" />

## 🛡️ Segurança e Anti-Alucinação

### Estratégias Adotadas

- [x] O agente não cria cálculos de juros complexos da própria "cabeça"; ele utiliza ferramentas (*tools*) de matemática estritas em Python acionadas pelo LLM para garantir 100% de precisão nos cálculos de tempo/valor.
- [x] Respostas sobre prazos e valores acumulados sempre incluem o disclaimer de que são simulações baseadas nos dados fornecidos pelo usuário.
- [x] Quando o agente não possui os dados do custo de vida ou orçamento do usuário para uma meta nova, ele tem a diretriz estrita de perguntar os valores antes de assumir qualquer premissa.
- [x] O prompt do sistema proíbe explicitamente a recomendação de ativos de risco ou corretoras específicas.

### Limitações Declaradas
> **O que o agente NÃO faz?**

- Não recomenda fundos de investimento específicos, ações ou criptomoedas para acelerar as metas; seu foco é em organização de fluxo de caixa e renda fixa conservadora.
- Não realiza pagamentos, transações PIX ou resgates automáticos na conta do usuário (atua apenas na camada de planejamento e simulação).
- Não analisa contratos imobiliários, vistos para viagens ou aprovação de crédito, restringindo-se à matemática do planejamento financeiro.
