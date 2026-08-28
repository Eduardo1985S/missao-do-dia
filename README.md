# 🎯 Missão do Dia — Aplicativo Mobile em React Native (Expo SDK 54)

> *"Qual missão você vai cumprir hoje?"*

Guia didático completo e passo a passo desenvolvido para aulas de **React Native com Expo**. Este material ensina como criar uma aplicação mobile profissional com JavaScript puro, **CRUD completo**, **persistência local com AsyncStorage**, **compartilhamento nativo** e princípios de **Clean Code**.

---

## 📋 Sumário

1. [Visão Geral do Projeto](#-visão-geral-do-projeto)
2. [Tecnologias Utilizadas](#-tecnologias-utilizadas)
3. [Passo a Passo de Instalação e Execução](#-passo-a-passo-de-instalação-e-execução)
4. [Estrutura de Pastas e Arquitetura](#-estrutura-de-pastas-e-arquitetura)
5. [Explicação Arquitetural e Fluxo de Dados](#-explicação-arquitetural-e-fluxo-de-dados)
6. [Como o CRUD Funciona no Aplicativo](#-como-o-crud-funciona-no-aplicativo)
7. [Persistência Local com AsyncStorage](#-persistência-local-com-asyncstorage)
8. [Compartilhamento de Progresso](#-compartilhamento-de-progresso)
9. [Guia dos Componentes Visuais](#-guia-dos-componentes-visuais)
10. [Código Completo de Cada Arquivo](#-código-completo-de-cada-arquivo)
11. [Mapa Mental do Fluxo de Dados](#-mapa-mental-do-fluxo-de-dados)

---

## 🎯 Visão Geral do Projeto

O **Missão do Dia** é um gerenciador diário de metas e tarefas com gamificação leve (cálculo de porcentagem, barra de progresso e celebração de 100% de conquistas).

Cada missão é representada pelo seguinte modelo de dados:

```javascript
{
  id: "m83k19z-4k8f2a",          // Identificador único gerado automaticamente
  titulo: "Estudar JavaScript",  // Título obrigatório da missão
  descricao: "Revisar arrays",   // Detalhes opcionais
  categoria: "estudos",          // Chave da categoria selecionada
  concluida: false,              // Status booleano (pendente ou concluída)
  createdAt: "2026-08-28T..."    // Data e hora de criação (ISO string)
}
```

---

## 🛠 Tecnologias Utilizadas

* **React Native**: Framework para construção de interfaces mobile nativas.
* **Expo (SDK 54)**: Plataforma e ecossistema de desenvolvimento ágil.
* **JavaScript puro (ES6+)**: Sem TypeScript para manter o foco didático nos conceitos fundamentais de React Native.
* **@react-native-async-storage/async-storage**: Persistência de dados no dispositivo (offline-first).
* **@expo/vector-icons (Ionicons)**: Biblioteca de ícones padronizada.
* **Share API nativa**: Compartilhamento do progresso com WhatsApp, Redes Sociais e e-mails.
* **StyleSheet nativo**: Estilização performática e sem dependências pesadas de UI.

---

## 🚀 Passo a Passo de Instalação e Execução

Siga os comandos abaixo no seu terminal:

### 1. Criar o projeto Expo com template em branco
```bash
npx create-expo-app@latest missao-do-dia --template blank
```

### 2. Entrar na pasta do projeto
```bash
cd missao-do-dia
```

### 3. Instalar os pacotes necessários
```bash
# Dependências do app (armazenamento, compartilhamento e ícones)
npx expo install @react-native-async-storage/async-storage expo-sharing @expo/vector-icons

# (Opcional) Para rodar também no navegador Web
npx expo install react-dom react-native-web
```

### 4. Iniciar o servidor de desenvolvimento
```bash
npx expo start
```

### 📱 Como testar:
1. **No celular**: Baixe o aplicativo **Expo Go** na Google Play Store (Android) ou App Store (iOS) e escaneie o QR Code exibido no terminal.
2. **Na Web**: Pressione a tecla `w` no terminal para abrir o aplicativo no navegador.

---

## 📂 Estrutura de Pastas e Arquitetura

O projeto adota uma arquitetura em camadas simples e modular:

```text
missao-do-dia/
├── App.js                         # Ponto de entrada (SafeAreaView e StatusBar)
├── app.json                       # Configurações do Expo
├── package.json                   # Dependências do projeto
├── README.md                      # Documentação didática para alunos
└── src/
    ├── constants/
    │   └── categories.js          # Categorias, cores temáticas e ícones
    ├── services/
    │   └── storage.js             # Camada de acesso ao AsyncStorage
    ├── utils/
    │   ├── missionUtils.js        # Funções puras: ID, cálculos de progresso e filtros
    │   └── shareUtils.js          # Formatação e disparo do Share nativo
    ├── components/
    │   ├── ProgressBar.js         # Barra de progresso, percentual e banner de conquista
    │   ├── FilterButtons.js       # Filtros em pílula (Todas, Pendentes, Concluídas)
    │   ├── MissionCard.js         # Card individual com botões de ação
    │   ├── MissionForm.js         # Modal com formulário reutilizável (Criar/Editar)
    │   └── EmptyState.js          # Estado visual amigável quando não há missões
    └── screens/
        └── HomeScreen.js          # Tela principal (Estado, CRUD e Eventos)
```

---

## 🧠 Explicação Arquitetural e Fluxo de Dados

A arquitetura separa as responsabilidades de forma clara:

1. **Interface de Usuário (`screens/` e `components/`)**: Responsável unicamente pela renderização visual e captura de toques/eventos do usuário.
2. **Regras de Negócio e Utilitários (`utils/`)**: Funções puras e determinísticas (cálculo de porcentagem, filtragem de arrays e geração de IDs).
3. **Camada de Serviço (`services/`)**: Centraliza as chamadas ao `AsyncStorage`. Os componentes nunca chamam `AsyncStorage.getItem` diretamente; eles utilizam funções semânticas como `getMissions()` e `saveMissions()`.

---

## 🔄 Como o CRUD Funciona no Aplicativo

O CRUD (Create, Read, Update, Delete) está centralizado em `src/screens/HomeScreen.js`:

### 1. CREATE (Criar)
* O usuário clica no botão flutuante **`+` (FAB)**.
* O componente `MissionForm` abre em modo de criação (`editingMission = null`).
* Ao clicar em "Criar missão", a função `handleCreateMission` cria um novo objeto com `generateId()`, data atual e `concluida: false`.
* O novo item é inserido no topo da lista (`[newMission, ...missions]`) e gravado no AsyncStorage.

### 2. READ (Listar / Ler)
* Ao abrir o app, o `useEffect` dispara `loadMissionsFromStorage()`, recuperando os dados salvos.
* A lista é exibida com um `FlatList`.
* O usuário pode alternar entre os filtros **Todas**, **Pendentes** e **Concluídas** sem perder os dados originais.

### 3. UPDATE (Atualizar)
* **Editar Dados**: Clicar no botão de lápis (✏️) abre o mesmo `MissionForm`, porém pré-preenchido com os dados da missão (`editingMission`). Ao salvar, a função `handleUpdateMission` atualiza o item correspondente.
* **Alternar Conclusão**: Clicar no botão de check (✓) executa `handleToggleComplete`, invertendo o valor de `mission.concluida` e disparando um feedback sonoro/visual amigável (`"🔥 Boa! Missão concluída!"`).

### 4. DELETE (Excluir)
* Clicar no botão de lixeira (🗑️) no card aciona o `Alert.alert` nativo do React Native.
* O usuário confirma se deseja realmente apagar a missão.
* A função `handleDeleteMission` filtra a lista excluindo o ID e salva a lista atualizada.

---

## 💾 Persistência Local com AsyncStorage

O `AsyncStorage` é uma API assíncrona, não criptografada, de chave-valor, equivalente ao `localStorage` da web.

Como o AsyncStorage só aceita **strings**, utilizamos:
* `JSON.stringify(array)` para **salvar**:
  ```javascript
  const jsonValue = JSON.stringify(missions);
  await AsyncStorage.setItem('@missao_do_dia:missions', jsonValue);
  ```
* `JSON.parse(string)` para **ler**:
  ```javascript
  const jsonValue = await AsyncStorage.getItem('@missao_do_dia:missions');
  return jsonValue != null ? JSON.parse(jsonValue) : [];
  ```

---

## 📤 Compartilhamento de Missões

O aplicativo oferece duas formas práticas de compartilhamento nativo via `Share.share()`:

### 1. Compartilhar uma Missão Específica (Direto no Card)
Ao clicar no ícone de compartilhamento presente no card da missão desejada, o app envia os detalhes daquela missão individual:

```text
MISSÃO DO DIA

Missão: Estudar JavaScript
Categoria: Estudos
Descrição: Revisar métodos de arrays
Status: Em andamento

Desafio lançado! Qual será a sua missão hoje?
```

### 2. Compartilhar Progresso Geral (Cabeçalho)
No topo da tela, o botão **"Progresso"** permite compartilhar o resumo consolidado de todas as missões.

A API nativa `Share.share({ message })` abre automaticamente a gaveta de compartilhamento do Android ou iOS (WhatsApp, Telegram, redes sociais, SMS, etc.).

---

## 🧩 Guia dos Componentes Visuais

| Componente | Responsabilidade |
| :--- | :--- |
| **`HomeScreen`** | Mantém os estados globais (`missions`, `selectedFilter`, `isFormVisible`, `loading`), orquestra o CRUD e renderiza a lista. |
| **`ProgressBar`** | Exibe a proporção de tarefas concluídas, a barra percentual e o banner especial quando 100% das missões são concluídas. |
| **`FilterButtons`** | Abas em pílula com contadores dinâmicos para alternar entre Todos, Pendentes e Concluídos. |
| **`MissionCard`** | Card com cores dinâmicas da categoria, status, texto riscado quando concluído e botões de ação. |
| **`MissionForm`** | Modal de formulário com seletor interativo de categorias (chips) e validação de título obrigatório. |
| **`EmptyState`** | Mensagem ilustrada e botão de atalho quando não há missões na lista ou no filtro atual. |

---

## 🗺 Mapa Mental do Fluxo de Dados

```text
       ┌───────────────────────────────┐
       │   CRIAR / EDITAR MISSÃO       │
       │   (MissionForm / Modal)       │
       └──────────────┬────────────────┘
                      │
                      ▼
       ┌───────────────────────────────┐
       │      ESTADO LOCAL             │
       │      (useState em HomeScreen) │
       └──────────────┬────────────────┘
                      │
                      ▼
       ┌───────────────────────────────┐
       │   PERSISTÊNCIA                │
       │   (AsyncStorage - storage.js) │
       └──────────────┬────────────────┘
                      │
                      ▼
       ┌───────────────────────────────┐
       │   READ (Leitura & Filtros)    │
       │   FlatList + MissionCard      │
       └──────────────┬────────────────┘
                      │
            ┌─────────┴─────────┐
            ▼                   ▼
   ┌─────────────────┐ ┌─────────────────┐
   │ UPDATE / DELETE │ │ COMPARTILHAR    │
   │ (Status/Edição) │ │ (Share API)     │
   └─────────────────┘ └─────────────────┘
```

---

## 📚 Princípios de Clean Code Aplicados no Projeto

1. **Nomes de Variáveis Semânticos**: Uso de `completedMissions`, `isAllCompleted`, `selectedFilter` em vez de abreviações crípticas (`c`, `x`, `flt`).
2. **Funções com Responsabilidade Única (SRP)**: Cálculos matemáticos ficam em `missionUtils.js`, armazenamento em `storage.js` e interface em componentes dedicados.
3. **Componentes Reutilizáveis**: O `MissionForm` atende perfeitamente aos casos de **criação** e **edição** sem duplicar código.
4. **Tratamento de Exceções**: Todas as chamadas assíncronas contêm blocos `try/catch/finally` para garantir que o usuário receba feedback caso algo falhe.
