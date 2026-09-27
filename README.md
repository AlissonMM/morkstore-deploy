# morkstore-deploy

Infraestrutura Docker do Mork Store: `docker-compose.yml` sobe MySQL, Kafka
(KRaft, broker único), a API (Spring Boot), o analytics-service (Quarkus) e
o frontend (Angular SSR).

## Estrutura esperada

Este repositório espera ser clonado **ao lado** dos outros três, todos como
irmãos na mesma pasta:

```
algum-diretorio/
├── morkstore-deploy/          (este repo)
├── morkstore-api/
├── morkstore-analytics-service/
└── morkstore-frontend/
```

```bash
git clone https://github.com/AlissonMM/morkstore-deploy.git
git clone https://github.com/AlissonMM/morkstore-api.git
git clone https://github.com/AlissonMM/morkstore-analytics-service.git
git clone https://github.com/AlissonMM/morkstore-frontend.git
```

## Subir a stack

```bash
cd morkstore-deploy
cp .env.example .env   # edite as senhas e URLs antes de ir para produção
docker compose up -d --build
```

Portas padrão: frontend `4200`, API `8080`, analytics `8091` (configuráveis
no `.env`). MySQL e Kafka não são expostos ao host.

## Antes de publicar numa VM

- [ ] Definir `MYSQL_ROOT_PASSWORD`, `ADMIN_PASSWORD` e `JWT_SECRET` no
      `.env` (nunca commitar esse arquivo). O `JWT_SECRET` precisa ter pelo
      menos 32 caracteres: `openssl rand -base64 48`.
- [ ] Ajustar `FRONTEND_ORIGIN`, `PUBLIC_API_URL`, `PUBLIC_ANALYTICS_URL` no
      `.env` para o domínio/IP público real.
- [ ] Abrir no firewall só as portas do frontend, da API e do analytics.
- [ ] Considerar HTTPS (ex.: proxy reverso com Caddy/nginx + Let's Encrypt)
      antes de expor login/senha publicamente.

## Derrubar

```bash
docker compose down       # mantém os dados nos volumes
docker compose down -v    # apaga também os dados (MySQL e Kafka)
```
