# Site Universo Brincar, Hotel Infantil

Site institucional do **Hotel Infantil Universo Brincar** (Rua das Perobas, 107, Jabaquara,
São Paulo/SP), pronto para deploy no EasyPanel pelo método **Dockerfile**.

Site estático, sem banco de dados, sem backend e sem build step. O container sobe com
nginx servindo a pasta `site/`.

Endereço no ar: **https://hotelinfantilbrincar.online**

---

## Estrutura

```
universo-brincar/
├── Dockerfile            # imagem nginx, porta 80
├── nginx.conf            # cache, gzip, cabeçalhos de segurança, healthcheck
├── docker-compose.yml    # apenas para testar na sua máquina
├── trocar-dominio.sh     # troca o domínio em todos os arquivos de uma vez
└── site/                 # tudo que vai para o ar
    ├── index.html        # página principal
    ├── privacidade.html  # política de privacidade (LGPD, com seção sobre dados de crianças)
    ├── 404.html
    ├── styles.css
    ├── script.js         # menu, animações, formulário
    ├── config.js         # >>> contatos do site, único arquivo a editar <<<
    ├── favicon.svg
    ├── apple-touch-icon.png
    ├── og-image.png      # imagem de compartilhamento no WhatsApp e redes
    ├── manifest.webmanifest
    ├── robots.txt
    └── sitemap.xml
```

---

## Contatos do site

Tudo fica em `site/config.js`:

```js
window.UB_CONFIG = {
  whatsapp: "5511963954926",              // (11) 96395-4926
  telefone: "(11) 5017-7598",
  telefoneLink: "+551150177598",
  email: "contato@universobrincar.com.br",
  saudacaoWhatsapp: "..."
};
```

Esses dados vieram do site oficial da empresa. Vale confirmar com a cliente,
principalmente o telefone fixo: no cadastro da Receita Federal consta
(11) 5585-0109, enquanto o site informa (11) 5017-7598.

Se o campo `whatsapp` ficar vazio, todos os botões passam a usar o telefone e o
formulário passa a abrir o aplicativo de e-mail. Nada mais precisa ser mexido.

---

## Fotos

O site foi montado com ilustrações próprias, sem fotos. Para colocar fotos reais do
espaço, jogue os arquivos em `site/` e troque os blocos de ilustração da seção
"Nosso espaço". Fotos com pessoas, principalmente crianças, só com autorização de
imagem assinada pelos responsáveis.

---

## Testar na sua máquina

```bash
docker compose up --build     # abre em http://localhost:8080
```

Sem Docker, apenas para ver o visual:

```bash
cd site && python3 -m http.server 8898
```

---

## Deploy no EasyPanel

Já está configurado no painel `allwinmachine.tech`, projeto **sites**, serviço
**universo-brincar**:

- **Source**: Github, repositório `mrqzgabriel/universo-brincar-site`, branch `main`
- **Build**: método **Dockerfile**, arquivo `Dockerfile`
- **Domains**: `hotelinfantilbrincar.online` e `www.hotelinfantilbrincar.online`, porta 80, HTTPS ligado

Para publicar uma alteração: faça o commit, dê push na branch `main` e clique em
**Implantar** no EasyPanel.

O DNS do domínio está na Hostinger, com registro A apontando para o servidor do
EasyPanel.

---

## Detalhes técnicos

- **Imagem**: `nginx:1.27-alpine`, porta 80, healthcheck em `/healthz`.
- **Cache**: HTML sempre revalidado, CSS e JS por 7 dias, imagens por 30 dias. Os
  arquivos são chamados como `styles.css?v=1`; ao editar, suba esse número nas três
  páginas para o navegador pegar a versão nova na hora.
- **Segurança**: `X-Content-Type-Options`, `X-Frame-Options`, `Referrer-Policy`,
  `Permissions-Policy` e uma `Content-Security-Policy` restritiva. Qualquer script
  externo novo (Analytics, Pixel) precisa ser liberado na CSP dentro do `nginx.conf`.
- **Fontes**: Baloo 2 e Nunito, do Google Fonts.
- **Acessibilidade**: navegação por teclado, foco visível, contraste conferido e
  respeito a `prefers-reduced-motion`. Sem JavaScript, o conteúdo aparece igual.

---

## Conteúdo

Os textos estão direto no `site/index.html` e descrevem o que a empresa divulga no
site oficial: berçário, integral de 6 meses a 3 anos, contraturno escolar, horários
flexíveis com atendimento em sábados e feriados mediante agendamento, festas infantis
e festa do pijama, além de profissionais capacitados, área verde e programação
diversificada.

Nada de preço, número de vagas ou promessa de formação da equipe foi inventado. Se a
cliente quiser incluir esses dados, é só acrescentar.

## Fotos usadas

As imagens são de uso livre, obtidas pelo Openverse:

- `site/img/hero.jpg`, "Bath toys", licença PDM (domínio público ou equivalente, sem exigência de crédito).
- `site/img/blocos.jpg`, blocos de montar coloridos, licença CC0, via Openverse.

Para trocar por fotos do próprio negócio, basta substituir os arquivos dentro de
`site/img/` mantendo os mesmos nomes. O formato usado no topo é 4 por 3.
