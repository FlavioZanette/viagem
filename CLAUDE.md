# Guia para sessões Claude neste repositório

App de viagem de um casal (set–out/2026). Arquivo único `index.html` (com cópia idêntica em `viagem.html`), publicado via GitHub Pages em https://flaviozanette.github.io/viagem/.

## Regras invioláveis

1. **NUNCA commitar dados pessoais ou sensíveis** — este repo é PÚBLICO e o histórico do git é permanente. Proibido em qualquer arquivo: números de passaporte, localizadores/PNR de reservas (avião, trem), números de bilhete, números de programas de fidelidade, nomes completos, CPF, telefone, e-mail, endereço, códigos de porta/cofre de hospedagem, dados de cartão. Dados desse tipo entram somente nos campos editáveis do próprio app (ficam no localStorage do navegador do usuário, nunca no código).
2. `index.html` e `viagem.html` devem permanecer **idênticos** — toda edição vale para os dois.
3. Fundo claro sempre (o app não tem dark mode, por decisão do dono).
4. Antes de qualquer commit, reler o diff procurando dados do item 1.

## Arquitetura (resumo)

- App single-file em HTML+CSS+JS vanilla, 4 abas (📍Hoje · 🗓️Roteiro · 🚄Reservas · ✅Listas), navegação por dock inferior. A aba Hoje é calculada pelo relógio do aparelho (fuso Europe/Madrid, dia vira às 04:00) e tem um seletor de simulação de data.
- Roteiros dia a dia vivem no objeto `DAY_ROUTES` (chave `cidade-DD`): array `pts` de paradas com horário `t`, coordenadas `ll`, modo (`start`/`walk`/`transport`), texto `how`, narração `desc`, distância `dist` e minutos a pé `wmin`. Pins e rota completa viram links do Google Maps automaticamente.
- Dados do usuário só em `localStorage` (chaves com prefixo `viagem_`). As listas editáveis são semeadas por `BUY_DEFAULT`/listas de bagagem; para alterar um item que já pode estar salvo nos celulares, seguir o padrão existente em `buyLoad()`: rewrite idempotente por regex (e/ou flag one-shot `viagem_mig_*`) — nunca mudar apenas o seed.
- Deploy: push na `main` → GitHub Pages reconstrói em ~1–4 min. Verificar com `curl` + `grep` no HTML publicado antes de declarar concluído.
