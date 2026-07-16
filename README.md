# MOC · SHE — protótipo navegável

Mock de alta fidelidade da solução **MOC-SHE** (Gestão de Alteração / *Management of Change*), construído a partir da
reunião de requisitos de **15/07/2026** — Vânia Barbosa (SHE), Tamires Cavani (Drive Data), Flávio Brianezi (TI).

Arquivo único `index.html`, sem build e sem dependência externa (os gráficos são SVG escritos à mão).
Dados fictícios, nada é persistido — recarregar a página volta ao estado inicial.

## Rodar

```bash
node serve.js     # http://localhost:4180
```

## Contexto

A unidade usa hoje o **VelocityEHS** (sistema global de EHS, `unileverhsnet.kminnovations.net`) para abrir MOC.
Dois motivos levam ao sistema próprio:

1. A última atualização do global **removeu as perguntas de qualidade**, que a unidade precisa por causa das auditorias.
2. Com a venda da divisão de alimentos, a unidade **deixa de ser Unilever** e perde o acesso ao sistema global.

Piloto em **Pouso Alegre**, depois **Garanhuns**. Decisão Power Apps × web em aberto — orçamento nas duas frentes,
com inclinação declarada por web.

## O que o protótipo cobre

**Módulo 1 — Gestão de MOC**
- Fluxo de criação em 6 etapas (tipo → informações → avaliação de impacto → ações → aprovadores → revisão)
- Avaliação de impacto com 17 questões em 6 dimensões; cada "Sim" **gera um item de ação** com responsável e prazo
- **Bloco de Qualidade em destaque** (6 questões) — a lacuna que motivou o projeto
- Classificação automática simples × complexo → régua de 3 ou 5 aprovadores
- Aprovação sequencial por níveis, rejeição com motivo obrigatório
- Bloqueio de encerramento com ação em aberto; ação crítica bloqueia o início
- Notificações por e-mail com **lembrete automático** (a queixa: "recebi e não recebo mais")
- Trilha de auditoria append-only
- Detalhe do MOC espelha a navegação do sistema atual (Início / Avaliação de impacto / Aprovações / Itens de ação /
  Anexos / Notificações de E-mail / Trilha da Auditoria)

**Módulo 2 — Painel analítico** (integrado, sem ferramenta externa)
- 7 seções: sumário, série histórica desde 2020, ciclo de vida, geográfica, responsabilidades, ações, impactos
- Filtros globais de período, unidade, responsável, status e tipo de impacto

**Configuração e administração**
- Questionário de impacto, régua de aprovadores e SLAs são **cadastro, não código**
- Usuários e perfis (Admin Master, Aprovador, Solicitante, Visualizador)

## Perfis (seletor no topo)

| Perfil | Pessoa | O que muda |
|---|---|---|
| Admin Master | Vânia Barbosa | Vê todos os MOCs e configura o sistema |
| Solicitante | Leonardo Siqueira | Cria MOCs, vê onde participa |
| Aprovador | Ana P Gomes (Qualidade) | Tem fila de aprovação |
| Visualizador | Tamires Cavani | Somente leitura — botões de escrita somem |

## Convenções do código

- Tudo em `index.html`; o JS vive num único `<script>` ao final.
- Botões de escrita levam a classe `perm-w` (ocultos no perfil Visualizador via `<style id="permStyle">`).
- Manual em `HELP[]` e tour em `TOUR[]` — **atualizar os dois a cada feature nova**.
- `HOJE` é fixo em 15/07/2026 (data da reunião) para os prazos e atrasos fazerem sentido.
- Base histórica do BI gerada com PRNG determinístico (`mulberry32`, seed 20260715) — os números não mudam entre reloads.
- No painel, as categorias do sumário são **mutuamente exclusivas**: concluído + rejeitado + ativo-em-atraso +
  ativo-no-prazo = total. "Em atraso" conta só o que está ativo e fora do prazo; o MOC que fechou atrasado aparece
  na taxa de conclusão no prazo.

## Ainda em aberto (dependem da área de negócio)

- Lista definitiva das perguntas da avaliação de impacto
- Processo documentado passo a passo
- Matriz de complexidade oficial (quantos e quais aprovadores)
- SLAs oficiais de etapas e aprovações
- Validação dos requisitos de BI
- Decisão Power Apps × web

As 17 questões, a régua 3/5 e os SLAs do protótipo são **propostas** baseadas no que foi descrito na reunião.

## Correções em relação à documentação recebida

A documentação gerada a partir do vídeo (pacote "Manus AI") traz dois erros que foram corrigidos aqui:

- **MOC = Management of Change / Gestão de Alteração**, não "Mudança Organizacional Controlada".
- As unidades são **Pouso Alegre** e **Garanhuns** (a transcrição registrou "Pouso" e "Guaranhuns").
- O sistema atual é **VelocityEHS**, não SAP (a pergunta sobre SAP ficou sem resposta na reunião).
