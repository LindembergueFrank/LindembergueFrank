# Case Study — NetWatch RN

> Case study de produto em desenvolvimento. O repositório principal permanece privado enquanto o MVP é validado.

## Problema

Pequenas equipes de TI frequentemente descobrem indisponibilidades somente após a reclamação de um usuário. O NetWatch RN foi concebido para transformar esse processo reativo em monitoramento simples e verificável.

O primeiro MVP responde a quatro perguntas:

1. O dispositivo está acessível?
2. Qual foi a latência observada?
3. Quando ocorreu a última verificação?
4. Como a disponibilidade evoluiu ao longo do tempo?

## Stack

- Python 3.12
- Django 5.2
- PostgreSQL 16
- Gunicorn
- Docker Compose
- pytest
- GitHub Actions

## Arquitetura

A primeira versão usa um **monólito modular Django**. Essa escolha reduz custo operacional e permite validar o produto antes de introduzir componentes distribuídos.

```text
Navegador
   │
Django / Gunicorn
   ├── Dashboard
   ├── Administração
   └── Monitoramento
         ├── Sondas de disponibilidade/latência
         └── PostgreSQL
```

## Decisões de engenharia

### Persistir leituras, não apenas o estado atual

A disponibilidade de um dispositivo não é útil apenas como um indicador online/offline. O histórico permite calcular disponibilidade, identificar degradação e investigar eventos anteriores.

### Separar "offline" de "nunca medido"

Um equipamento sem leitura ainda não pode ser classificado como indisponível. O domínio diferencia ausência de dados de falha de conectividade.

### Configuração por ambiente

Hosts, credenciais e parâmetros operacionais são fornecidos por ambiente. Segredos não são versionados e o container não precisa conhecer valores sensíveis em build time.

### Health check sem vazamento de informação

O serviço possui uma verificação de saúde que confirma dependências essenciais sem retornar stack traces, credenciais ou detalhes internos desnecessários.

## Segurança

Mesmo sendo um produto de monitoramento, o sistema deve tratar destinos de rede como dados sensíveis. Entre os princípios adotados estão:

- não versionar endereços de redes de clientes;
- validar destinos antes da sondagem;
- manter segredos fora do Git;
- limitar informações devolvidas por health checks;
- executar o container com usuário não privilegiado;
- planejar autenticação e autorização antes de uso externo.

## Qualidade

A fundação do MVP inclui testes automatizados para domínio, serviços e dashboard, além de CI com PostgreSQL e validação do Docker Compose.

A evolução segue unidades de trabalho pequenas e revisáveis. Uma alteração só deve gerar commit quando entrega valor técnico real ou corrige uma falha comprovada.

## Roadmap técnico

As próximas etapas naturais incluem:

- agendamento robusto das sondagens;
- autenticação e autorização;
- retenção e agregação de histórico;
- métricas e observabilidade;
- alertas;
- tratamento de redes e destinos permitidos;
- backup e recuperação;
- homologação em ambiente containerizado.

## O que o projeto demonstra

O NetWatch RN conecta desenvolvimento e infraestrutura, demonstrando conhecimento de:

- redes TCP/IP;
- troubleshooting;
- Python/Django;
- PostgreSQL;
- Docker;
- testes automatizados;
- CI;
- modelagem de disponibilidade e latência;
- preocupação com segurança operacional.

## Trade-off principal

O objetivo não é reproduzir ferramentas maduras de observabilidade. O projeto prioriza um escopo enxuto, adequado a pequenas operações e útil como plataforma para evoluir conceitos de monitoramento, confiabilidade e engenharia de sistemas.
