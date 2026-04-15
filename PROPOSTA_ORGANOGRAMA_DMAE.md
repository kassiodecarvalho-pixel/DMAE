# Proposta inicial — Organograma digital do DMAE

## 1) Objetivo
Construir um **organograma institucional navegável** do DMAE, com foco em:
- localizar rapidamente **pessoas, setores, equipamentos e responsáveis**;
- abrir um **perfil completo por servidor(a)**;
- manter governança de dados com **edição restrita por perfis de acesso**;
- permitir atualização contínua sem depender de equipe de TI para cada ajuste pequeno.


## 1.1) Base legal validada (Decreto nº 23.656/2026)
Leitura confirmada do decreto consolidado no repositório, com os seguintes pontos para amarrar no sistema:
- O decreto consolida a estrutura organizacional do DMAE e revoga o decreto anterior (23.344/2025).
- A estrutura inclui Conselho Consultivo, Diretoria-Geral e Delegação de Controle.
- A Diretoria-Geral se desdobra em Presidência, Diretoria Executiva e demais diretorias com unidades/códigos (ex.: GAB-PRES, GAB-DE, C-CRAEPC, C-PATRI etc.).
- Há um **Anexo I** com detalhamento analítico de unidades, denominações, níveis e vagas; este anexo deve ser a fonte principal para carga inicial de dados.
- O próprio decreto permite disciplinamento por Instrução Normativa, sem alterar denominações oficiais do Anexo I.

Implicação prática para o projeto:
- O organograma deve respeitar a hierarquia e nomenclaturas do decreto e registrar vigência das estruturas (inclusive revogações/substituições).
- Recomenda-se manter campo de "ato normativo de origem" por unidade/cargo para auditoria e rastreabilidade jurídica.

## 2) O que cada “caixa” do organograma deve mostrar
Campos recomendados para o cartão principal:
- Foto
- Nome completo
- Cargo
- Função
- Setor / unidade
- Ramal / contato institucional

Ao clicar no cartão, abrir página de perfil com:
- dados funcionais principais;
- superior hierárquico e equipe vinculada;
- contratos em que atua como fiscal;
- suplente(s) do contrato;
- equipamentos e ativos sob responsabilidade;
- histórico de movimentações (opcional);
- anexos (portarias, atos de designação etc.).

## 3) Arquitetura recomendada (simples e escalável)

### Front-end (tela do usuário)
- **React + TypeScript**
- Biblioteca de organograma: **React Flow** (ou alternativa D3 para customização extrema)
- Benefícios: visual moderno, filtros, zoom, busca por nome/cargo/setor, cards com foto.

### Back-end (regras de negócio + API)
- **Python (FastAPI)**
- Benefícios: rápida implementação, ótimo para integrações e documentação automática da API.

### Banco de dados
- **PostgreSQL**
- Benefícios: robusto, relacional (ideal para hierarquia, contratos, suplências e vínculos entre tabelas).

### Autenticação e perfis de acesso
- Login corporativo (se houver): **AD/LDAP ou SSO**
- Perfis sugeridos:
  - **Leitor**: consulta geral
  - **Editor Setorial**: edita somente sua área
  - **Administrador**: gerencia estrutura completa e permissões

### Hospedagem
- Preferencialmente em infraestrutura da Prefeitura/órgão (intranet ou nuvem governamental)
- Opção de publicação web interna para acesso por todo o DMAE com controle de segurança.

## 4) BI x Aplicação Web (HTML)

### Quando usar BI (Power BI, por exemplo)
- Excelente para **painéis analíticos** (indicadores, contagem de cargos, vacâncias, distribuição por áreas).
- Bom para dashboards executivos.

### Quando usar Aplicação Web (recomendado para seu caso)
- Melhor para **navegação operacional** do organograma.
- Permite **perfil detalhado por pessoa**, edição controlada, trilha de auditoria e integrações.

### Melhor abordagem prática
- **Aplicação Web como sistema principal** + **BI conectado ao banco** para indicadores.
- Assim vocês têm operação diária + visão gerencial sem duplicar trabalho.

## 5) Modelagem de dados (resumo)
Tabelas iniciais:
- `unidades` (diretorias, departamentos, divisões)
- `pessoas`
- `vinculos_hierarquicos` (quem responde para quem)
- `cargos_funcoes`
- `contratos`
- `fiscais_contrato`
- `suplentes_contrato`
- `equipamentos`
- `responsaveis_equipamento`
- `usuarios_sistema` e `perfis_acesso`
- `auditoria_alteracoes`

## 6) Etapas do projeto
1. **Levantamento funcional** (com base no decreto e nas áreas).
2. **Protótipo visual** (layout semelhante às referências enviadas).
3. **MVP** com organograma + busca + perfil de pessoa.
4. **Módulo de contratos/suplentes**.
5. **Módulo de equipamentos/responsáveis**.
6. **Integração com BI** para indicadores.
7. **Treinamento e governança** (quem atualiza o quê, periodicidade e validação).

## 7) Stack recomendada para iniciar agora
- Front-end: React + TypeScript + React Flow + Tailwind (UI)
- Back-end: FastAPI + SQLAlchemy
- Banco: PostgreSQL
- Autenticação: SSO/LDAP (se disponível)
- Deploy: Docker + Nginx

## 8) Próximo passo imediato (baixo risco)
Criar um **protótipo navegável** com dados fictícios do DMAE contendo:
- 1 presidência/superintendência
- 3 diretorias
- 2 níveis de subdivisão por diretoria
- 30 pessoas de exemplo
- abertura de perfil por clique
- filtro por nome/cargo/unidade

Esse protótipo já valida o visual e a usabilidade antes do investimento completo.

## 9) Protótipo v0 entregue neste repositório
Arquivo: `prototype_dmae_v0.html`

Como abrir localmente:
1. Baixe/clone o repositório.
2. Abra o arquivo `prototype_dmae_v0.html` no navegador.
3. Use o campo de busca para localizar pessoas/unidades.
4. Clique em uma caixa para abrir o perfil (contratos, suplentes e equipamentos).

Observação: os dados são fictícios para validação de layout e navegação.
