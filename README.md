# SmartScale

**Core reutilizável para gestão e geração automática de escalas de turno/plantão**, construído como uma linha de produto de software: a lógica de domínio (elegibilidade, cobertura mínima, distribuição justa, trocas, auditoria) é genérica e configurável — cada organização a adapta às suas próprias regras, sem reescrever o núcleo do sistema.

O projeto nasceu como **MilScale** (estudo de caso aplicado a um batalhão militar) e evoluiu, na disciplina de Desenvolvimento Orientado a Reuso de Software, para o **SmartScale**: a generalização desse núcleo em uma linha de produto, da qual diferentes aplicações podem ser derivadas.

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

Duas aplicações já foram especificadas em cima desse core, cada uma com sua própria configuração de domínio:

### MilScale — aplicação militar

Estudo de caso original: escalas de serviço de um batalhão em Curitiba, com cargos militares (recruta, cabo, sargento, tenente etc.), serviços específicos por posto (guarda, cabo de dia, oficial de dia) e escala no formato Nx1 (N dias livres por 1 de serviço).

### SmartScale Hospitalar — aplicação para ambiente hospitalar

Instanciação do core para hospitais: cobertura mínima por setor crítico (UTI, pronto-socorro, centro cirúrgico), categorias profissionais da saúde (médico, enfermeiro, técnico), conformidade com a CLT e a LGPD, e um motor de otimização (Python + OR-Tools) rodando como worker assíncrono numa arquitetura de microsserviços.

## Modelagem e design (do core)

- Diagrama de casos de uso
- Modelo lógico de banco de dados
- Catálogo de requisitos reutilizáveis (linha de produto de software)
- Mapa de empatia e personas
- Protótipo navegável (Figma/HTML)

## Status atual

O projeto está na fase de **modelagem, prototipação e documentação de arquitetura**. A implementação do core e das aplicações derivadas ainda não foi iniciada.

A construção vai seguir os princípios de **desenvolvimento orientado a reuso de software**, com o core estruturado em **módulos reutilizáveis**, separando explicitamente o que é comum a qualquer organização (Engenharia de Domínio) do que é específico de cada aplicação derivada, como o MilScale ou o SmartScale Hospitalar (Engenharia de Aplicação).

## Papel no projeto

Atuei no levantamento de requisitos, modelagem do banco de dados, definição das regras de negócio e fluxo de aprovação de trocas do core, e na documentação de arquitetura da aplicação hospitalar (arc42/C4), em equipe (Equipe Tijucas), nas disciplinas de Desenvolvimento Orientado a Reuso de Software e Arquitetura e Soluções Cloud (PUCPR).

## Time

Projeto desenvolvido em equipe na PUCPR: Enzo Watanabe de Lima, João Guilherme Salomão, João Guilherme Cordeiro e Rafael Zeni.
