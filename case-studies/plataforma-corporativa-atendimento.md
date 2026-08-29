# Case Study — Plataforma corporativa de atendimento interno

> Case study sanitizado. Nomes da organização, usuários, dados, credenciais, endereços, infraestrutura interna e código proprietário foram deliberadamente omitidos.

## Contexto

Uma operação interna de suporte precisava substituir solicitações informais por um fluxo rastreável de atendimento. O sistema deveria permitir abertura e acompanhamento de solicitações, distribuição para técnicos, controle de prioridade e status, histórico, notificações, avaliação e relatórios, sem ampliar o acesso a informações além do necessário para cada perfil.

## Responsabilidade técnica

Atuação no desenvolvimento e evolução do sistema, incluindo backend, regras de negócio, segurança, persistência, interface inicial, testes, CI e decisões de implantação.

## Stack

- Java 17 e Spring Boot 3;
- Spring Security e JWT;
- PostgreSQL;
- Flyway;
- JPA/Hibernate;
- HTML, CSS e JavaScript no frontend inicial;
- Docker;
- GitHub Actions.

## Arquitetura

A solução evoluiu como **monólito modular**, mantendo implantação simples sem abrir mão de limites claros entre domínios. A escolha evitou a complexidade operacional prematura de microsserviços e preservou uma rota de evolução futura.

Responsabilidades foram separadas entre identidade/acesso, usuários, catálogo, atendimento, notificações, relatórios, auditoria e infraestrutura compartilhada.

## Decisões de segurança

### RBAC e fonte de verdade no servidor

A autorização é aplicada no backend. Informações deriváveis da sessão autenticada não são confiadas ao payload do navegador, reduzindo risco de manipulação de identidade e acesso indevido a registros de terceiros.

### Tokens e credenciais

A evolução de segurança incluiu invalidação de sessões após mudanças de credencial, proteção de fluxos de redefinição de senha, redução de dados sensíveis em respostas operacionais e remoção de segredos do repositório.

### Auditoria e rastreabilidade

Movimentações relevantes são registradas para permitir investigação de alterações, acompanhamento operacional e preservação de histórico.

## Integridade e concorrência

Entidades críticas passaram a utilizar controle otimista de versão para evitar sobrescritas silenciosas quando dois usuários alteram o mesmo registro simultaneamente. Migrations versionadas com Flyway mantêm a evolução do banco reproduzível e auditável.

## Tempo e regras operacionais

Datas são persistidas de forma consistente e apresentadas de acordo com o fuso operacional da aplicação. Métricas históricas e SLAs foram tratados como regras de domínio, não apenas cálculos visuais no frontend.

## Qualidade

A evolução do projeto incorporou:

- testes unitários de regras de negócio;
- testes de autorização e isolamento entre perfis;
- regressão de interface para diferentes papéis;
- testes de concorrência e erros;
- validações de migrations;
- smoke tests funcionais;
- CI antes de integração e produção;
- documentação de arquitetura e decisões técnicas.

## Operação

O empacotamento com Docker e a configuração por ambiente permitem separar código de credenciais e parâmetros de implantação. A esteira de CI atua como gate para reduzir regressões antes de novas versões.

## Trade-offs

### Monólito modular vs. microsserviços

O monólito modular foi escolhido porque o domínio e a escala operacional não justificavam o custo de múltiplos serviços, observabilidade distribuída, contratos remotos e infraestrutura adicional.

### JWT vs. sessão tradicional

JWT simplificou o consumo da API, mas exigiu estratégia explícita de expiração e invalidação quando credenciais mudam. Essa limitação foi tratada como parte da arquitetura de segurança.

### Frontend simples vs. framework dedicado

A interface inicial permaneceu em HTML/CSS/JavaScript para reduzir complexidade e acelerar validação do fluxo. A separação de responsabilidades permite substituir essa camada posteriormente sem reescrever as regras centrais.

## Resultado técnico

O projeto deixou de ser apenas um CRUD de chamados e passou a demonstrar um conjunto mais próximo de um sistema corporativo real: regras por perfil, segurança em camadas, histórico, concorrência, migrations, testes, CI, operação e decisões arquiteturais documentadas.

## O que este case demonstra

- modelagem de regras de negócio reais;
- Java/Spring Boot além de projetos de curso;
- segurança aplicada a autorização e dados;
- PostgreSQL e evolução de schema;
- engenharia orientada a testes e CI;
- capacidade de justificar trade-offs arquiteturais;
- integração entre desenvolvimento e infraestrutura.

---

Este documento descreve apenas práticas e decisões técnicas que podem ser divulgadas publicamente. Nenhum dado institucional ou material restrito é necessário para compreender as competências demonstradas.