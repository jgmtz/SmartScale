# SmartScale

**Core reutilizável para gestão e geração automática de escalas de turno/plantão**, construído como uma linha de produto de software: a lógica de domínio (elegibilidade, cobertura mínima, distribuição justa, trocas, auditoria) é genérica e configurável — cada organização a adapta às suas próprias regras, sem reescrever o núcleo do sistema.

O projeto nasceu como **MilScale** (estudo de caso aplicado a um batalhão militar) e evoluiu, na disciplina de Desenvolvimento Orientado a Reuso de Software, para o **SmartScale**: a generalização desse núcleo em uma linha de produto, da qual diferentes aplicações podem ser derivadas.

> **Onde está o código:** o core (`smartscale-core`) e a primeira aplicação derivada dele, o **MilScale**, estão implementados em [Jgbr11/Sistema-SmartScale](https://github.com/Jgbr11/Sistema-SmartScale). O sistema inteiro foi montado em torno do core: o MilScale só implementa os pontos de variação dele. Este repositório é a vitrine da linha de produto (o problema, o core e as aplicações).

| Repositório | O que é |
|---|---|
| [jgmtz/SmartScale](https://github.com/jgmtz/SmartScale) (este) | Visão geral da linha de produto SmartScale |
| [Jgbr11/Sistema-SmartScale](https://github.com/Jgbr11/Sistema-SmartScale) | Código: módulo `smartscale-core` + aplicação MilScale |

## O problema (genérico)

Qualquer organização que opere por turnos ou plantões — forças armadas, hospitais, segurança, indústria, operações 24h — enfrenta o mesmo desafio central: montar escalas que respeitem regras de elegibilidade por cargo/qualificação, garantam cobertura mínima, distribuam a carga de forma justa e permitam gerenciar trocas com um fluxo de aprovação auditável. Feito manualmente em planilha, isso não escala, gera erros e abre margem para falhas de cobertura.

## O core

O núcleo do SmartScale resolve esse problema de forma **independente de domínio**:

- **Modelo de elegibilidade configurável** — cada organização define seus próprios cargos, qualificações e regras de quem pode ocupar qual função na escala
- **Motor de geração/otimização de escalas**, respeitando regras de carga horária, descanso obrigatório, cobertura mínima e distribuição justa
- **Fluxo de solicitação, triagem e aprovação de trocas** entre membros da escala
- **Perfis de acesso** por papel (colaborador escalado, supervisor, gestor, administrador)
- **Notificações automáticas** e **auditoria completa** de toda alteração na escala
- **Edição manual** da escala gerada, com validação de conflitos

O que muda de uma aplicação para outra é só a **camada de configuração**: os cargos, as regras específicas de cada categoria profissional, os setores/unidades e a legislação aplicável. O core (elegibilidade, otimização, trocas, auditoria) é o mesmo.

## Aplicações construídas sobre o core

Duas aplicações foram especificadas em cima desse core, cada uma com sua própria configuração de domínio. A primeira, o MilScale, já está implementada.

### MilScale — aplicação militar (implementada)

Estudo de caso original: escalas de serviço de um batalhão em Curitiba, com cargos militares (recruta, cabo, sargento, tenente etc.), serviços específicos por posto (guarda, cabo de dia, oficial de dia) e escala no formato Nx1 (N dias livres por 1 de serviço).

Como o core é usado no código ([Jgbr11/Sistema-SmartScale](https://github.com/Jgbr11/Sistema-SmartScale)):

| Core (`smartscale-core`, JAR próprio, sem Spring nem JPA) | Especialização MilScale |
|---|---|
| `PessoaEscalada` (interface genérica) | `Militar` |
| `TipoTurno` (interface genérica) | `TipoServico` |
| `CriterioDeOrdenacao` (ponto de variação) | `CriterioOrdenacaoMilitar`: maior número de dias sem serviço primeiro |
| `MotorDeRodizio` (fila e rodízio) | Usado como está, sem nenhuma alteração |

O `MotorDeRodizio` não sabe o que é um militar: ele só enxerga as interfaces do core. Uma aplicação nova da linha, como a hospitalar, reaproveita o motor inteiro e implementa só essas interfaces e o seu próprio critério de ordenação.

**Stack:** Java 21, Spring Boot 3.3, Flyway, MySQL 8 (Docker) e H2; React 19, Vite e TypeScript; testes com JUnit 5 e Vitest.

**Arquitetura:** monólito hexagonal (Ports & Adapters). O `smartscale-core` é um módulo Maven separado, empacotado como JAR próprio, e o backend do MilScale o consome como qualquer outra dependência. No código, o que é genérico está marcado como `NÚCLEO REUTILIZÁVEL (LPS)` e o que é específico como `Especialização MilScale`.

**Variabilidade:** o critério da fila da escala é escolhido por configuração (`maior-folga`, `menor-carga` ou `mais-moderno`), sem mudar código.

**Principais regras de negócio do MilScale:**
- **Geração da escala:** ninguém tira duas vagas no mesmo dia; intervalo mínimo de 3 dias de folga entre serviços (3x1); quem está afastado não é escalado; a fila prioriza quem está há mais tempo sem servir. Se faltar gente, a escala relaxa só o intervalo mínimo em vez de deixar vaga aberta.
- **Elegibilidade:** cada tipo de serviço aceita combinações de posto, subunidade e curso.
- **Trocas de serviço:** substituição ou troca mútua, com o fluxo aceite do colega → triagem do Cabo → autorização do Sargenteante.
- **Perfis:** Sargenteante, Cabo da Sargenteação, Sd EP da Sargenteação e Militar Escalado, com controle de acesso no servidor.
- **Escala em rascunho ou publicada**, dias travados manualmente e realocação automática quando surge um afastamento depois da escala pronta.
- **Privacidade:** dados pessoais só aparecem para a sargenteação ou para a própria pessoa.

Instruções para rodar (backend, frontend e Docker Compose), testes e documentação da disciplina (padrões de projeto, variabilidade e rastreabilidade de requisitos) estão no [repositório do código](https://github.com/Jgbr11/Sistema-SmartScale).

### SmartScale Hospitalar — aplicação para ambiente hospitalar (especificada)

Instanciação do core para hospitais: cobertura mínima por setor crítico (UTI, pronto-socorro, centro cirúrgico), categorias profissionais da saúde (médico, enfermeiro, técnico), conformidade com a CLT e a LGPD, e um motor de otimização (Python + OR-Tools) rodando como worker assíncrono numa arquitetura de microsserviços.

## Modelagem e design (do core)

- Diagrama de casos de uso
- Modelo lógico de banco de dados
- Catálogo de requisitos reutilizáveis (linha de produto de software)
- Mapa de empatia e personas
- Protótipo navegável (Figma/HTML)

## Status atual

- **Core (`smartscale-core`): implementado**, como módulo Maven separado e versionado (1.0.0).
- **MilScale: implementado** sobre o core, com backend, frontend, testes e Docker Compose.
- **SmartScale Hospitalar: especificado** (requisitos e arquitetura arc42/C4), ainda sem implementação.

A construção segue os princípios de **desenvolvimento orientado a reuso de software**: o core concentra o que é comum a qualquer organização (Engenharia de Domínio), e cada aplicação derivada, como o MilScale ou o SmartScale Hospitalar, traz só o que é específico dela (Engenharia de Aplicação).

## Papel no projeto

Atuei no levantamento de requisitos, modelagem do banco de dados, definição das regras de negócio e fluxo de aprovação de trocas do core, e na documentação de arquitetura da aplicação hospitalar (arc42/C4), em equipe (Equipe Tijucas), nas disciplinas de Desenvolvimento Orientado a Reuso de Software e Arquitetura e Soluções Cloud (PUCPR).

## Time

Projeto desenvolvido em equipe na PUCPR: Enzo Watanabe de Lima, João Guilherme Salomão, João Guilherme Cordeiro e Rafael Zeni.
