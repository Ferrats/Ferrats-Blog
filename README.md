# Leo Ferrato — blog pessoal

Blog com **EmDash 1.0.1 + Astro**, baseado no template oficial `blog-cloudflare`. Interface pública em português, temas claro/escuro, textos, páginas, categorias, tags, busca e RSS.

A página inicial traz “Ideias em construção”, uma apresentação curta e os textos publicados. O conteúdo inicial inclui **Um caderno aberto** (identificado como demonstração) e **Sobre**. Tudo pode ser editado pelo CMS. Comentários estão desativados neste primeiro teste.

## Testar no computador

Instale Node.js 24 LTS (mínimo 22.16) e execute:

```bash
git clone https://github.com/Ferrats/Ferrats-Blog.git
cd Ferrats-Blog
npm ci
npm run dev
```

- Blog: http://localhost:4321
- Painel: http://localhost:4321/_emdash/admin/

Conclua o assistente do painel e cadastre sua conta e passkey. Mantenha o conteúdo de exemplo para testar. D1 e R2 são simulados localmente em `.wrangler/`; rodar localmente não exige login na Cloudflare. O conteúdo local não é transferido automaticamente para produção.

## Publicar na sua Cloudflare

Este projeto usa **Workers**, com D1 (conteúdo) e R2 (mídia). Não configure como um projeto Pages estático.

### Pelo terminal

```bash
npx wrangler login
npm run deploy
```

Wrangler provisiona os recursos nomeados em `wrangler.jsonc` no primeiro deploy. Se solicitado, habilite R2 na conta. O adaptador Astro também configura bindings de sessões KV e processamento de imagens. Confira no painel da Cloudflare os produtos ativados e seus limites.

Abra a URL `workers.dev` exibida ao final, acrescente `/_emdash/admin/` e conclua o cadastro do administrador. O assistente deve ser concluído logo após a publicação. A URL final depende da sua conta e só existirá após o deploy.

### Conectar o GitHub pelo painel

Em **Workers & Pages**, crie um Worker importando `Ferrats/Ferrats-Blog`:

| Campo | Valor |
| --- | --- |
| Branch de produção | `main` |
| Diretório raiz | `/` |
| Comando de build | `npm run build` |
| Comando de deploy | `npx wrangler deploy` |
| Versão do Node | `24` |

Use npm com `package-lock.json` (instalação reproduzível: `npm ci`). O nome do Worker deve ser `ferrats-blog`, conforme `wrangler.jsonc`. Autorize a integração a acessar este repositório e a criar os recursos quando solicitado.

## Seu primeiro teste do CMS

1. Abra **Posts → Um caderno aberto** e altere uma frase.
2. Salve e publique as alterações; recarregue o blog.
3. Crie um novo post como rascunho, adicione uma capa e selecione uma categoria.
4. Confira a prévia e publique quando quiser.
5. Teste `/posts`, `/search?q=EmDash`, `/category/experimentos` e `/rss.xml`.

O CMS e seus componentes podem conter textos em inglês; a interface pública deste tema foi adaptada para pt-BR. Datas usam o fuso de São Paulo.

## Onde alterar

- `seed/seed.json`: estrutura e conteúdo inicial, aplicado a um banco novo.
- `src/pages/`: páginas públicas renderizadas no servidor.
- `src/styles/theme.css`: ajustes visuais do tema.
- `astro.config.mjs`: integração EmDash, D1, R2 e fontes.
- `wrangler.jsonc`: nomes dos recursos Cloudflare.

Depois do primeiro setup, edite os textos **no painel**. Alterar o seed e fazer outro deploy não sobrescreve o banco existente. Os posts ficam no D1, e os uploads no R2; o GitHub guarda o código do tema.

Plugins isolados não estão habilitados neste teste. Se ativar plugins que armazenem segredos, configure `EMDASH_ENCRYPTION_KEY` como segredo do Worker antes de salvar esses valores e guarde uma cópia segura. Não coloque credenciais no repositório.

## Validação

```bash
npm run typecheck
npm run build
```

## Referências

- [Documentação EmDash](https://docs.emdashcms.com/)
- [Deploy em Cloudflare](https://docs.emdashcms.com/deployment/cloudflare/)
- [Template original](https://github.com/emdash-cms/templates/tree/main/blog-cloudflare)
