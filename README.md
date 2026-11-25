# 🧾 ScanIA

Aplicativo mobile desenvolvido com React Native e Expo para digitalização e leitura inteligente de notas fiscais. Utiliza a câmera ou a galeria do dispositivo para capturar imagens, armazená-las com segurança e extrair informações por meio de inteligência artificial.

Projeto acadêmico desenvolvido no curso de Análise e Desenvolvimento de Sistemas (ADS) do IESB.

## 🎯 Objetivo

Desenvolver uma aplicação mobile para facilitar o registro e o acompanhamento de despesas a partir da leitura de notas fiscais e recibos.

O aplicativo permite fotografar documentos, processar as imagens por meio de uma Supabase Edge Function e organizar automaticamente as informações extraídas, como estabelecimento, data, valor total, categoria e itens da compra.

## 🚀 Funcionalidades

- Cadastro de usuários com e-mail e senha;
- Login e gerenciamento de sessão;
- Captura de notas fiscais utilizando a câmera;
- Seleção de imagens diretamente da galeria;
- Pré-visualização da imagem antes do envio;
- Redimensionamento e compressão das imagens;
- Armazenamento privado das notas fiscais no Supabase Storage;
- Processamento das imagens por Supabase Edge Functions;
- Extração de dados utilizando inteligência artificial;
- Identificação do estabelecimento;
- Extração da data e do valor total da compra;
- Identificação da categoria da despesa;
- Identificação da forma de pagamento;
- Extração dos produtos, quantidades e preços;
- Histórico de notas fiscais digitalizadas;
- Visualização dos detalhes de cada nota;
- Edição manual das informações extraídas;
- Exclusão de notas fiscais;
- Indicadores de processamento das notas;
- Resumo mensal das despesas;
- Gráfico de distribuição de gastos por categoria.

## 🛠️ Tecnologias

| Tecnologia | Utilização |
|---|---|
| React Native | Desenvolvimento da aplicação mobile |
| Expo SDK 58 | Ambiente de desenvolvimento |
| Expo Router | Navegação e organização das telas |
| TypeScript | Tipagem e organização do código |
| Supabase | Backend e integração com serviços |
| Supabase Auth | Cadastro, login e gerenciamento de sessão |
| PostgreSQL | Banco de dados relacional |
| Supabase Storage | Armazenamento privado das imagens |
| Supabase Edge Functions | Processamento das notas fiscais |
| Anthropic API | Extração de informações por inteligência artificial |
| TanStack Query | Gerenciamento e cache de dados |
| React Hook Form | Gerenciamento de formulários |
| Zod | Validação de dados |
| expo-image-picker | Captura e seleção de imagens |
| expo-image-manipulator | Redimensionamento e compressão de imagens |
| React Native SVG | Elementos gráficos e visualizações |
| Jest | Testes automatizados |
| Prettier | Padronização do código |

## 📁 Estrutura do projeto

```text
ScanIA/
├── app/
│   ├── (auth)/                 # Autenticação
│   │   ├── login.tsx
│   │   └── register.tsx
│   ├── (app)/
│   │   ├── (tabs)/             # Telas principais
│   │   │   ├── index.tsx       # Início e histórico
│   │   │   ├── scan.tsx        # Digitalização de notas
│   │   │   ├── summary.tsx     # Resumo mensal
│   │   │   └── profile.tsx     # Perfil
│   │   └── receipt/            # Detalhes das notas
│   └── _layout.tsx
├── src/
│   ├── components/             # Componentes reutilizáveis
│   ├── constants/              # Categorias e constantes
│   ├── context/                # Contexto de autenticação
│   ├── hooks/                  # Hooks personalizados
│   ├── lib/                    # Configurações do Supabase
│   ├── schemas/                # Validação com Zod
│   ├── services/               # Serviços e integração com backend
│   ├── theme/                  # Identidade visual
│   ├── types/                  # Tipos TypeScript
│   └── utils/                  # Funções auxiliares
├── supabase/
│   ├── functions/
│   │   └── scan-receipt/
│   │       ├── index.ts        # Função de processamento
│   │       └── parse.ts        # Normalização dos dados
│   └── schema.sql              # Estrutura e políticas do banco
├── __tests__/                  # Testes automatizados
├── .env.example                # Exemplo de variáveis
├── .gitignore
├── app.json
├── babel.config.js
├── eas.json
├── package.json
├── package-lock.json
├── tsconfig.json
└── README.md
```

## ⚙️ Configuração e execução

### Pré-requisitos

- Node.js LTS 22 ou superior;
- npm;
- Expo Go instalado em um dispositivo mobile;
- Conta no Supabase;
- Chave da Anthropic API, caso utilize a extração real por IA.

### 1. Instalação

Clone o repositório:

```bash
git clone https://github.com/joaoemanuel-p/ScanIA.git
cd ScanIA
```

Instale as dependências:

```bash
npm install
```

Confira a compatibilidade das dependências com a versão do Expo:

```bash
npx expo install --fix
```

### 2. Configuração do Supabase

Crie um projeto no [Supabase](https://supabase.com) e execute o arquivo:

```text
supabase/schema.sql
```

Esse arquivo configura:

- Tabela `receipts`, responsável pelo armazenamento das notas fiscais;
- Tabela `receipt_items`, responsável pelos itens extraídos;
- Relacionamentos entre notas e produtos;
- Índices para consultas;
- Políticas de Row Level Security (RLS);
- Bucket privado `receipts` no Supabase Storage;
- Políticas de acesso às imagens.

Configure a autenticação por e-mail no painel do Supabase.

### 3. Configuração das variáveis de ambiente

Crie um arquivo `.env` na raiz do projeto utilizando `.env.example` como referência:

```env
EXPO_PUBLIC_SUPABASE_URL=https://SEU-PROJETO.supabase.co
EXPO_PUBLIC_SUPABASE_KEY=SUA-CHAVE-PUBLICA
```

Utilize a URL do projeto e a chave pública (publishable/anon) fornecidas pelo Supabase.

Não coloque a chave `service_role` ou a chave da Anthropic em variáveis `EXPO_PUBLIC_`.

### 4. Configuração do backend

Instale e configure o acesso à CLI do Supabase:

```bash
npx supabase login
npx supabase link --project-ref SEU_PROJECT_REF
```

Para utilizar o processamento de demonstração, sem chamar uma API de inteligência artificial, configure:

```bash
npx supabase secrets set MOCK_AI=true
```

Publique a Edge Function:

```bash
npx supabase functions deploy scan-receipt
```

Caso ocorra um erro relacionado ao Docker durante a publicação, utilize:

```bash
npx supabase functions deploy scan-receipt --use-api
```

### 5. Configuração da inteligência artificial

O projeto permite utilizar dois modos de processamento.

**Modo de demonstração**

Com `MOCK_AI=true`, a função utiliza dados fictícios de uma nota de exemplo, permitindo testar o fluxo de processamento sem chamar a API da Anthropic.

**Modo com inteligência artificial**

Para utilizar a extração real, configure os secrets no Supabase:

```bash
npx supabase secrets set MOCK_AI=false ANTHROPIC_API_KEY=SUA_CHAVE
```

Opcionalmente, configure o modelo:

```bash
npx supabase secrets set ANTHROPIC_MODEL=claude-haiku-4-5-20251001
```

A chave da Anthropic permanece exclusivamente nos secrets da Edge Function, sem ser exposta no aplicativo mobile.

### 6. Inicialização

Depois de instalar as dependências e configurar o ambiente:

```bash
npm run typecheck
npm test
npx expo start -c
```

Abra o aplicativo em um dispositivo Android ou iOS utilizando o Expo Go.

## 🔒 Segurança e privacidade

O ScanIA utiliza recursos do Supabase para proteger as informações pessoais e financeiras dos usuários.

Entre os principais mecanismos estão:

- Autenticação pelo Supabase Auth;
- Row Level Security (RLS) nas tabelas do banco;
- Acesso às próprias notas fiscais por usuário autenticado;
- Bucket privado para armazenamento das imagens;
- Políticas de acesso às imagens por pasta de usuário;
- Uso de URLs assinadas para visualizar imagens privadas;
- Validação e normalização dos dados extraídos;
- Limitação do tamanho das imagens processadas;
- Separação entre credenciais públicas e secrets do backend;
- Armazenamento da chave Anthropic exclusivamente nos secrets da Edge Function.

As informações extraídas podem ser revisadas e corrigidas pelo usuário antes de serem utilizadas.

## 🤖 Processamento das notas fiscais

O processamento acontece em etapas:

1. O usuário fotografa uma nota ou seleciona uma imagem da galeria.
2. A imagem é redimensionada e comprimida para envio.
3. O arquivo é armazenado no bucket privado do Supabase Storage.
4. Um registro da nota é criado no banco de dados.
5. A Edge Function `scan-receipt` recebe o identificador da nota.
6. A função recupera a imagem e realiza a leitura.
7. Os dados extraídos são normalizados e validados.
8. As informações são armazenadas nas tabelas `receipts` e `receipt_items`.
9. O usuário pode consultar e editar os dados na aplicação.

O processamento utiliza os estados `pending`, `done` e `error` para representar o andamento da leitura.

## 📊 Resumo financeiro

A aplicação oferece uma tela de resumo mensal que apresenta:

- Total gasto no período;
- Quantidade de notas fiscais lidas;
- Média de valor por nota;
- Distribuição dos gastos por categoria;
- Gráfico visual de categorias.

As categorias incluem Alimentação, Transporte, Moradia, Saúde, Lazer, Educação, Compras, Contas e Outros.

## 🧪 Testes

O projeto possui testes automatizados utilizando Jest.

Para executar:

```bash
npm test
```

Para verificar a tipagem:

```bash
npm run typecheck
```

Para formatar os arquivos:

```bash
npm run format
```

Os testes estão organizados para validar funcionalidades como:

- Formatação e manipulação de datas;
- Cálculos e formatação de valores monetários;
- Normalização e interpretação dos dados extraídos;
- Cálculo e organização dos resumos financeiros.

A execução dos testes no ambiente local é necessária para confirmar os resultados.

## 📌 Status do projeto

Aplicação estruturada para digitalização de notas fiscais, armazenamento privado de imagens, extração de informações por inteligência artificial e organização de despesas por categorias e períodos.

O processamento real por IA depende da configuração da chave da Anthropic nos secrets do Supabase. O modo de demonstração utiliza dados fictícios para validar o fluxo.

## 👨‍💻 Autor

**João Emanuel Pinheiro Leite**

Análise e Desenvolvimento de Sistemas — IESB.

## 🔗 Repositório

[https://github.com/joaoemanuel-p/ScanIA](https://github.com/joaoemanuel-p/ScanIA)
