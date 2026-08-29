# Case Study — Plataforma de Chamados IPEM/RN

> Case study sanitizado. O código-fonte e os dados reais permanecem privados por envolverem contexto institucional.

## Contexto

A plataforma foi criada para substituir atendimentos informais de suporte por um fluxo rastreável, com abertura, triagem, atribuição, atendimento, resolução, avaliação, relatórios e auditoria.

O problema não era apenas registrar chamados: o sistema precisava representar perfis diferentes, preservar histórico, controlar visibilidade, tratar prazos operacionais e evoluir sem comprometer o uso diário.

## Stack

- Java 17
- Spring Boot 3.3
- Spring Security
- JWT
- PostgreSQL
- Flyway
- HTML/CSS/JavaScript
- Docker
- GitHub Actions

## Decisões de arquitetura

O sistema evoluiu para um **monólito modular**, evitando a complexidade operacional de microsserviços antes de existir necessidade real de distribuição independente.

As responsabilidades são separadas por domínio e camada, com regras de negócio protegidas de detalhes de infraestrutura. As mudanças arquiteturais são acompanhadas por documentação e testes para reduzir regressões.

## Autenticação e autorização

A aplicação trabalha com perfis como administrador, gerente, técnico e usuário interno. A autorização não depende apenas do que a interface exibe: as regras são verificadas no backend.

Entre as medidas aplicadas ao longo da evolução estão:

- controle de acesso por perfil e permissão;
- isolamento dos chamados do usuário interno;
- invalidação de sessões/tokens após mudanças de credencial;
- redução da exposição de dados pessoais nas respostas operacionais;
- proteção de fluxos administrativos;
- rastreabilidade de operações relevantes.

## Persistência e consistência

O PostgreSQL é a fonte de verdade do sistema e o Flyway controla a evolução do schema.

Para cenários em que duas pessoas podem alterar o mesmo registro, foram introduzidos mecanismos de controle otimista de concorrência, evitando sobrescritas silenciosas.

Também foram tratadas regras de tempo e fuso horário para que métricas de SLA e histórico não dependam do relógio local do navegador ou do servidor.

## Qualidade

A evolução do projeto é validada por uma combinação de:

- testes unitários de regras de negócio;
- testes de autorização;
- testes de regressão dos perfis;
- verificações de migrations;
- smoke tests do frontend;
- CI antes de integrações relevantes;
- validações de Docker/deploy.

Bugs encontrados durante homologação são tratados como requisitos de engenharia: primeiro é reproduzida a falha, depois a regra é corrigida e uma validação é adicionada para reduzir a chance de regressão.

## Exemplos de problemas tratados

### Abertura de chamado pelo usuário autenticado

O solicitante não deve precisar informar novamente dados que já pertencem à sua sessão. A solução passou a derivar identidade e dados relacionados no backend, reduzindo inconsistências e impedindo que payloads manipulados indiquem outro solicitante.

### Concorrência

Chamados e cadastros podem ser editados por mais de um perfil. O uso de versionamento otimista permite detectar uma atualização concorrente e tratar o conflito explicitamente.

### Segurança de dados

Dados pessoais e credenciais não devem circular em objetos de resposta sem necessidade operacional. O projeto evoluiu para DTOs e contratos mais restritos, além de manter segredos fora do versionamento.

## Trade-offs

### Monólito modular vs. microsserviços

Foi mantido um único deploy porque o domínio ainda se beneficia mais de consistência transacional, simplicidade operacional e velocidade de evolução do que de distribuição independente.

### JWT vs. sessão tradicional

JWT foi mantido pela integração com o frontend e outros módulos, mas com mecanismos adicionais de invalidação para evitar tratar tokens como irrevogáveis.

### Frontend simples vs. framework SPA

A interface atual prioriza entrega, estabilidade e manutenção com HTML/CSS/JavaScript. A adoção de um framework só deve ocorrer se a complexidade de estado e componentes justificar a mudança.

## Resultado profissional demonstrado

Este projeto representa experiência prática em:

- levantamento e evolução de requisitos reais;
- backend Java/Spring;
- modelagem relacional;
- segurança e autorização;
- tratamento de concorrência;
- migrations;
- testes e CI;
- Docker e implantação;
- manutenção de software em uso.

## Confidencialidade

Nenhuma credencial, dado pessoal, endereço interno, documento institucional ou informação operacional sensível é publicada neste case study.
