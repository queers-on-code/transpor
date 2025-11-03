# Coletivo Transpor

Template de referência: https://vercel.com/templates/blog/strapi

# Setup Local
## Passo a passo para testar localmente (sem clonar o repositório original)

Pré-requisitos
- Node.js 18+ instalado
- npm ou yarn
- Dois terminais (um para backend Strapi, outro para frontend Next.js)

1) Criar e executar o backend (Strapi)
- Crie o projeto Strapi (usa SQLite por padrão):
    - npm: npx create-strapi-app@latest transpor-cms --quickstart
    - yarn: yarn create strapi-app transpor-cms --quickstart
- Entre na pasta e confirme que o Strapi rodou em http://localhost:1337:
    - cd transpor-cms
    - npm run develop
- Acesse o painel de administração em http://localhost:1337/admin e crie o usuário administrador.
- No Admin UI, crie os Content Types necessários (ex.: Post) com campos comuns:
    - title (Text), slug (UID), content (Rich Text), excerpt (Text), cover (Media), publishedAt (Date), author (Relation User ou outro Content Type).
- Em Settings → Roles & Permissions → Public, habilite permissões de leitura públicas para o(s) endpoint(s) dos posts (find, findOne) para API REST ou GraphQL.

2) Criar e executar o frontend (Next.js)
- Crie o app Next.js:
    - npx create-next-app@latest transpor-web
    - ou yarn create next-app transpor-web
- Entre na pasta do frontend e instale utilitários:
    - cd transpor-web
    - npm install swr
- Crie variáveis de ambiente (arquivo .env.local na raiz do transpor-web):
    - NEXT_PUBLIC_API_URL=http://localhost:1337
- Exemplo mínimo para buscar posts (pages/index.js ou app/router equivalente):
    - use fetch ou swr para GET `${process.env.NEXT_PUBLIC_API_URL}/api/posts?populate=cover,author`
    - trate a resposta JSON (Strapi v4 retorna em data/attributes).
- Inicie o frontend:
    - npm run dev
- Acesse http://localhost:3000 para ver a listagem de posts.

3) Dicas para testar rapidamente
- Crie alguns posts via Strapi Admin para ter conteúdo visível no frontend.
- Se precisar testar ambos num só comando, use duas abas/terminais:
    - Terminal A: cd transpor-cms && npm run develop
    - Terminal B: cd transpor-web && npm run dev
- Se o Strapi usar outra porta, atualize NEXT_PUBLIC_API_URL no .env.local do frontend.

4) Ajustes comuns (para ficar parecido com o template)
- Endpoints: use ?populate=* ou populate=cover,author para trazer mídias e relações.
- Slug: consulte posts por slug em /api/posts?filters[slug][$eq]=meu-slug
- Autenticação/Preview: para esboçar preview de conteúdo privado, crie token API no Strapi (Settings → API Tokens) e envie Authorization: Bearer <token> nas requests server-side.

Resultado
- Backend Strapi em http://localhost:1337 (Admin em /admin)
- Frontend Next.js em http://localhost:3000
- Sem clonar o repositório original: você reproduziu localmente a estrutura do template usando create-strapi-app + create-next-app e configurou integração via API.