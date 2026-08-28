# Claude AGENTS Guide

Este documento define as regras e diretrizes que você deve seguir como assistente de IA ao interagir com os arquivos do projeto.

## 🎯 Objetivo

O objetivo é auxiliar o usuário (desenvolvedor) na manutenção e evolução do projeto React Native "Missão do Dia" de forma eficiente, segura e alinhada às boas práticas do ecossistema.

## 📋 Diretrizes Gerais

### 1. **Compreensão do Projeto**
- **Contexto:** É um aplicativo React Native desenvolvido com **Expo**, focado em gestão pessoal de tarefas diárias (missões).
- **Persistência:** Utiliza **AsyncStorage** para salvar dados localmente.
- **Estado:** Gerencia estado com `useState` e `useEffect`.
- **Navegação:** Usa **React Navigation** (Stack e Bottom Tabs).
- **Estilo:** Styling via **Styled Components**.

### 2. **Segurança e Privacidade**
- **NUNCA** compartilhar ou expor chaves de API, tokens, senhas ou qualquer dado sensível.
- Ao lidar com dados do usuário (missões, progresso), priorizar sempre a privacidade.
- Evitar armazenar dados sensíveis desnecessariamente.

### 3. **Boas Práticas de Desenvolvimento**
- **Código Limpo:** Seguir princípios SOLID, DRY e KISS.
- **Performance:** Otimizar renderizações, evitar cálculos pesados em loops, usar `useCallback` quando necessário.
- **Acessibilidade:** Garantir contraste adequado, tamanho de fonte legível e suporte a leitores de tela.
- **Internacionalização:** Preparar o código para suportar múltiplos idiomas.

## 📝 Diretrizes Específicas

### 1. **Edição de Código**
- **Edições diretas:** Fazer apenas edições simples e correções imediatas.
- **Refatoração:** Para mudanças maiores ou refatoração, criar um plano detalhado e solicitar confirmação antes de aplicar.
- **Segurança:** Sempre validar dados de entrada e prevenir XSS.

### 2. **Testes e Validação**
- **Testes manuais:** Sempre que possível, sugerir testes manuais после de alterações.
- **Testes automatizados:** Caso existam testes no projeto, executá-los após alterações relevantes.
- **Cross-platform:** Testar em ambos os ambientes (Android e iOS) quando possível.

### 3. **Documentação**
- Manter arquivos de documentação (README, CHANGELOG) atualizados.
- Comentar código complexo ou não trivial.
- Documentar novas funcionalidades implementadas.

## 🚨 O Que EVITAR

- ❌ Fazer alterações drásticas sem perguntar.
- ❌ Remover código sem entender completamente seu propósito.
- ❌ Ignorar avisos de segurança ou performance.
- ❌ Usar dependências desnecessárias ou não seguras.
- ❌ Compartilhar informações sensíveis.
- ❌ Quebrar o fluxo de desenvolvimento (ex: interromper um processo sem motivo).

## 🔧 Ferramentas e Tecnologias

### Ferramentas de Linha de Comando
- **npm**: Gerenciamento de dependências.
- **Expo CLI**: Execução do projeto local.
- **git**: Controle de versão (manter mensagens de commit claras).

### Tecnologias
- **React Native / Expo**: Framework principal.
- **Styled Components**: Estilização.
- **AsyncStorage**: Armazenamento local.
- **React Navigation**: Navegação entre telas.
- **Firebase**: Autenticação e banco de dados (se aplicável).

## 📊 Workflow Recomendado

1. **Analisar** a solicitação do usuário.
2. **Compreender** o impacto da mudança no projeto.
3. **Planejar** a implementação (especialmente para mudanças grandes).
4. **Implementar** seguindo as boas práticas.
5. **Testar** (manual ou automaticamente).
6. **Documentar** se necessário.
7. **Confirmar** com o usuário.

Lembre-se: seu objetivo é ser um assistente valioso que acelera o desenvolvimento, mantém a qualidade do código e protege o projeto. Siga estas diretrizes para garantir interações produtivas e seguras.
