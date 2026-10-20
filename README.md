# Proposta Baile de Máscaras: deploy no Coolify v4

## Como a página foi feita

- É um site estático de **um arquivo** (`public/index.html`), escrito em HTML, CSS e JavaScript puros, sem framework e sem build.
- **Slides:** cada slide é uma `<section class="slide">` de altura de tela. No desktop há rolagem com encaixe (scroll-snap). No celular os slides empilham normalmente.
- **Animações:** são feitas em CSS (`@keyframes`). As entradas são disparadas por `IntersectionObserver`.
- **Ilustrações:** o salão interativo, o telão e os ícones são SVG desenhado no próprio arquivo, sem imagens externas.
- **Preços:** são calculados em JS a partir de `RATE=30`, `HOURS` e `INFRA` (procure por `var RATE`).
- **Mídias:** ficam em `public/media/`. São fotos do Pexels e vídeos do Mixkit, em MP4 (H.264) com cópia em WebM.
- **Fontes:** vêm do Google Fonts, por link.
- **Servidor:** no servidor roda só um **nginx** servindo os arquivos (veja o `Dockerfile` e o `nginx.conf`).

## Subir no Coolify (v4.x) com Dockerfile (recomendado)

1. **Repositório:** crie um repositório no GitHub ou GitLab e suba **o conteúdo desta pasta**: `Dockerfile`, `nginx.conf`, `.dockerignore` e `public/`.
2. **Recurso:** no Coolify, vá em **Projects → (seu projeto) → + New → Resource**.
   - Repositório público: escolha **Public Repository** e cole a URL.
   - Repositório privado: escolha **Private Repository (with GitHub App)** ou **(with Deploy Key)**.
3. **Build Pack:** escolha **Dockerfile**. Em Base Directory, use `/` e em Dockerfile Location, use `/Dockerfile`.
4. **Porta:** em **Ports Exposes**, coloque `80`.
5. **Domínio:** em **Domains**, coloque `https://proposta.seudominio.com.br`.
   - No DNS, crie um registro **A** desse subdomínio apontando para o IP da VPS.
   - O Coolify (Traefik) emite o certificado HTTPS sozinho.
   - Sem domínio, use o domínio sslip.io que o Coolify sugere.
6. **Deploy:** clique em **Deploy**. Leva menos de 1 minuto.
7. **Atualizar:** edite os arquivos, faça commit e push, e clique em **Redeploy**. Com o webhook ou a GitHub App ativos, o deploy acontece no push.

## Alternativa sem Dockerfile (Build Pack "Static")

- Na etapa 3, escolha o build pack **Static**.
- Marque **Is it a static site?** e defina **Publish Directory** como `/public`.
- O Coolify serve a pasta com nginx próprio. Os vídeos e as fotos funcionam do mesmo jeito.

## Testar localmente

```bash
docker build -t proposta . && docker run --rm -p 8080:80 proposta
# abra http://localhost:8080
```

## Observações

- O `nginx.conf` faz cache de 30 dias em `/media/`. Se trocar uma foto, troque também o nome do arquivo.
- A tag `noindex` impede que buscadores indexem a proposta.
- O WhatsApp do botão final está no `index.html`. Procure por `5599981148876`.
