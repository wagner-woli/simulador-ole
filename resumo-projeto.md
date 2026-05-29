# Simulador Woli Mídia — Resumo do Projeto

## O que é
Ferramenta comercial interna para vendedores da **Woli Mídia** gerarem propostas de campanha para redes varejistas do segmento **Eletro Móveis / Indústria & Serviços**.

---

## Tecnologia
- **1 arquivo só**: `index.html` (~1700 linhas)
- Sem framework, sem build — HTML + CSS + JavaScript puro
- **jsPDF** (CDN) para geração de PDF
- **Google Fonts** (Montserrat + Open Sans)
- Hospedado no **GitHub Pages**: https://wagner-woli.github.io/simulador-ole/
- Código: https://github.com/wagner-woli/simulador-ole

---

## O que o simulador faz
1. Vendedor seleciona **quantas lojas** de cada rede participam da campanha
2. Escolhe o **pacote de frequência** (Presença / Conversão / Liderança / Ataque)
3. Define a **duração do criativo** (30s / 1min / 1min30 / 2min)
4. Define o **número de semanas** (blocos)
5. Adiciona opcionalmente **produção de criativo** (R$1.500 a R$3.500)
6. Gera uma **proposta em PDF** com logo, tabela de preços e dados do executivo

---

## Modelo de preço

| Pacote | Frequência | Preço/loja/bloco |
|---|---|---|
| Presença | 5x/h | R$ 40 |
| Conversão | 10x/h | R$ 80 |
| Liderança | 15x/h | R$ 120 |
| Ataque | 20x/h | R$ 160 |

- Desconto por duração: 30s = 0%, 1min = 10%, 1min30 = 20%, 2min = 30%
- 1 bloco = semana Seg–Sáb · 51h

---

## Redes cadastradas

| Rede | Lojas | Estados | Status |
|---|---|---|---|
| Zema | 496 | MG SP ES GO BA MS | Ativa |
| Dujuca | 29 | MG | Ativa |
| MM Lojas | 230 | MS PR SC | Ativa |
| Colombo | 359 | RS SC PR | Ativa |
| Lojas Ramos | 30 | MG BA | Ativa |
| Simonetti | 72 | ES BA | Em implantação |
| Fujioka | 55 | GO DF MT TO | Em implantação |
| Solar Magazine | 57 | CE | Em implantação |
| Móveis Linhares | 22 | ES | Em implantação |
| TV Multiloja | 78 | PR | Em implantação |
| Valdar | 60 | PR | Em implantação |

---

## Como fazer edições
Abrir o projeto no Claude Code e dizer em português o que precisa mudar. Exemplos:
- "libera a Simonetti"
- "corrige o total da Fujioka para 60"
- "adiciona a rede X com Y lojas nos estados A e B"

O Claude faz a edição, commita e publica automaticamente. O site atualiza em 1–2 minutos.

---

## Próximas evoluções possíveis
- Liberar redes "Em implantação" conforme onboarding
- Simulador por segmento (ex: só Sul, só MG)
- Versão para outros produtos Woli além de TV em loja
- Separar dados em arquivo externo para edição mais fácil
- Histórico de propostas geradas
