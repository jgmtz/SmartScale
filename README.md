# SmartScale

Sistema web para automatizar a criação e gestão de escalas de serviço/plantão em organizações com rotinas de turnos fixos e regras de revezamento — desenvolvido a partir de um caso real de um batalhão militar (**MilScale**) e 
generalizado para qualquer organização com necessidades semelhantes.

## O problema

Organizações com escalas de plantão/serviço (forças armadas, hospitais, segurança, operações 24h) frequentemente controlam isso manualmente em planilhas, o que gera:
- Falta de rastreabilidade sobre trocas de turno
- Dificuldade em aplicar regras de elegibilidade por cargo/qualificação
- Nenhuma notificação automática de quem está escalado
- Processo de aprovação de troca sem fluxo definido

## A solução

O SmartScale digitaliza esse processo com:
- **Perfis de usuário com permissões diferenciadas** (do colaborador escalado até o gestor da escala)
- **Regras de elegibilidade configuráveis** — cada função/cargo só pode ser preenchida por quem tem a qualificação exigida
- **Fluxo de solicitação e aprovação de trocas de turno**, com etapas de triagem antes da aprovação final
- **Notificação automática** ao colaborador no dia anterior ao seu turno (D-1)
- **Modelo de dados reutilizável**, pensado para se adaptar a diferentes tipos de organização (não só uma regra fixa de escala)

> Este projeto nasceu como **MilScale**, um estudo de caso aplicado a um batalhão militar em Curitiba, e evoluiu para o **SmartScale**: uma versão generalizada do mesmo conceito, capaz de atender qualquer organização com necessidade de escalas de turno.

## Principais funcionalidades

- Login e autenticação por perfil
- Visualização de escala pessoal e completa (conforme permissão)
- CRUD de regras de escala e de perfis de usuário (para administradores)
- Solicitação, triagem e aprovação de trocas de turno
- Bloqueio de dias/travas de escala
- Notificações automáticas D-1

## Modelagem e design

- Diagrama de casos de uso
- Modelo lógico de banco de dados
- Catálogo de requisitos reutilizáveis (pensado para reuso de software / linhas de produto)
- Mapa de empatia e personas
- Protótipo navegável (Figma/HTML)

## Status atual

O projeto está na fase de modelagem e prototipação: requisitos levantados, modelo de dados definido, regras de negócio documentadas e telas prototipadas no Figma. A implementação do sistema ainda não foi iniciada.

A construção vai seguir os princípios de desenvolvimento orientado a reuso de software (linha de produto de software), com o sistema estruturado em módulos reutilizáveis — separando o que é comum a qualquer organização (autenticação, gestão de perfis, fluxo de aprovação de trocas) do que é específico de cada tipo de organização (as regras de escala em si).

## Papel no projeto

Atuei no levantamento de requisitos, modelagem do banco de dados e definição das regras de negócio e fluxo de aprovação de trocas, em equipe (Equipe Tijucas), como parte da disciplina Desenvolvimento Orientado a Reuso de Software (PUCPR).

## Time

Projeto desenvolvido em equipe na PUCPR.
