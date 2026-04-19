# 🤖 PoupaFoco AI - O seu Co-piloto de Metas Financeiras

Bem-vindo ao repositório do **PoupaFoco AI**, um agente inteligente focado em planejamento financeiro, organização de fluxo de caixa e aceleração de metas de médio e longo prazo.

---

## 📑 Índice
1. [Visão Geral e Caso de Uso](#1-visão-geral-e-caso-de-uso)
2. [Persona e Tom de Voz](#2-persona-e-tom-de-voz)
3. [Arquitetura](#3-arquitetura)
4. [Base de Conhecimento e Dados](#4-base-de-conhecimento-e-dados)
5. [Prompts do Agente](#5-prompts-do-agente)
6. [Segurança e Anti-Alucinação](#6-segurança-e-anti-alucinação)
7. [Avaliação e Métricas](#7-avaliação-e-métricas)

---

## 🎯 1. Visão Geral e Caso de Uso

### O Problema
A dificuldade de planejar e manter a disciplina financeira para realizar múltiplos objetivos simultaneamente (como a compra de eletrônicos, planejamento de viagens ou a reserva para um financiamento habitacional). As pessoas costumam se perder no orçamento diário e adiam esses grandes marcos por não conseguirem visualizar o impacto de pequenas economias no longo prazo.

### A Solução
O PoupaFoco AI funciona como um **"acelerador de metas"**. Ele lê os dados do orçamento mensal do usuário e divide as economias em "caixinhas". De forma proativa, ele monitora os gastos, envia atualizações de progresso e sugere o redirecionamento de capital (ex: identificar que gastos com delivery caíram e sugerir alocar esse valor para a meta da viagem).

### Público-Alvo
Jovens profissionais, estudantes e casais que estão construindo patrimônio e precisam de uma ferramenta que traduza a matemática financeira em prazos reais e alcançáveis.

---

## 🗣️ 2. Persona e Tom de Voz

* **Personalidade:** Motivador, analítico e realista. Atua como um parceiro de projetos. Celebra pequenas vitórias, mas é direto ao apontar atrasos. Educa mostrando o impacto dos juros compostos de forma simples.
* **Tom de Comunicação:** Acessível e dinâmico, com um leve toque de gamificação. Linguagem clara do dia a dia, sem "economês" complexo.
* **Exemplos de Interação:**
  * *"E aí! Tudo pronto para atualizar o status daquela sua viagem para o Chile hoje?"*
  * *"Aporte registrado! Recalculei a previsão e você acabou de encurtar o tempo da sua meta em 2 semanas."*

---

## 🏗️ 3. Arquitetura

```mermaid
flowchart TD
    A[Cliente] -->|Mensagem de Texto ou Voz| B[Interface]
    B --> C[Orquestrador LLM]
    C --> D[Base de Conhecimento]
    D -->|Historico e Metas| C
    C --> E[Validacao de Contexto]
    E --> F[Resposta Formatada]
    F --> B
