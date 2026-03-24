# CRM Imobiliário V4

Versão 4 já adaptada para uso profissional com PostgreSQL, variáveis de ambiente e estrutura pronta para produção local ou publicação em nuvem.

## Principais melhorias da V4
- PostgreSQL como banco principal via `DATABASE_URL`
- arquivo `.env.example` para configuração segura
- `SECRET_KEY` fora do código fixo
- compatível com SQLite apenas como fallback de emergência
- caminhos internos ajustados para abrir corretamente templates e arquivos estáticos
- script `init_db.py` para criar as tabelas manualmente, se desejar
- dashboard, autenticação JWT, financeiro, locação, funil comercial, tarefas e WhatsApp

## 1. Criar o arquivo .env
Copie o `.env.example` para `.env` e ajuste os valores.

Windows:
```bat
copy .env.example .env
```

Mac/Linux:
```bash
cp .env.example .env
```

Exemplo de conteúdo:
```env
DATABASE_URL=postgresql+psycopg2://crm_user:SuaSenhaForteAqui123@localhost:5432/crm_imobiliario
SECRET_KEY=troque-esta-chave-por-uma-chave-segura
ACCESS_TOKEN_EXPIRE_MINUTES=480
```

## 2. Instalar dependências
```bash
python -m venv .venv
```

Windows:
```bat
.venv\Scriptsctivate
```

Mac/Linux:
```bash
source .venv/bin/activate
```

```bash
pip install -r requirements.txt
```

## 3. Criar o banco
No PostgreSQL, crie o banco `crm_imobiliario` e o usuário `crm_user`.

Exemplo SQL:
```sql
CREATE USER crm_user WITH PASSWORD 'SuaSenhaForteAqui123';
CREATE DATABASE crm_imobiliario;
GRANT ALL PRIVILEGES ON DATABASE crm_imobiliario TO crm_user;
```

## 4. Criar as tabelas
Você pode apenas iniciar o sistema, pois ele já chama `Base.metadata.create_all(...)` automaticamente.

Ou, se preferir, rodar antes:
```bash
python init_db.py
```

## 5. Iniciar o CRM
Desenvolvimento:
```bash
uvicorn app.main:app --reload
```

Produção local / servidor:
```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

## Acesso inicial
Na primeira execução, o sistema cria automaticamente:
- email: `admin@crm.com`
- senha: `admin123`

## Rotas principais
- `/docs`
- `/dashboard`
- `/reports/dashboard-summary`
- `/notifications/overview`
- `/whatsapp/client/{client_id}`

## Recomendação profissional
Antes de usar com clientes reais:
- troque a senha do admin
- troque a `SECRET_KEY`
- faça backup do PostgreSQL
- publique o sistema atrás de HTTPS
