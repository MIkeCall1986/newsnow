![](/public/og-image.png)

English | [简体中文](README.zh-CN.md) | [日本語](README.ja-JP.md)

> [!NOTE]
> This is a demo version currently supporting Chinese only. A full-featured version with better customization and English content support will be released later.

**_Elegant reading of real-time and hottest news_**

## Features

- Clean and elegant UI design for optimal reading experience
- Real-time updates on trending news
- GitHub OAuth login with data synchronization
- 30-minute default cache duration (logged-in users can force refresh)
- Adaptive scraping interval (minimum 2 minutes) based on source update frequency to optimize resource usage and prevent IP bans
- support MCP server

```json
{
  "mcpServers": {
    "newsnow": {
      "command": "npx",
      "args": [
        "-y",
        "newsnow-mcp-server"
      ],
      "env": {
        "BASE_URL": "https://newsnow.busiyi.world"
      }
    }
  }
}
```
You can change the `BASE_URL` to your own domain.

## Deployment

### Basic Deployment

For deployments without login and caching:

1. Fork this repository
2. Import to platforms like Cloudflare Page or Vercel

### Cloudflare Page Configuration

- Build command: `pnpm run build`
- Output directory: `dist/output/public`

### GitHub OAuth Setup

1. [Create a GitHub App](https://github.com/settings/applications/new)
2. No special permissions required
3. Set callback URL to: `https://your-domain.com/api/oauth/github` (replace `your-domain` with your actual domain)
4. Obtain Client ID and Client Secret

### Environment Variables

Refer to `example.env.server`. For local development, rename it to `.env.server` and configure:

```env
# Github Client ID
G_CLIENT_ID=
# Github Client Secret
G_CLIENT_SECRET=
# JWT Secret, usually the same as Client Secret
JWT_SECRET=
# Initialize database, must be set to true on first run, can be turned off afterward
INIT_TABLE=true
# Whether to enable cache
ENABLE_CACHE=true
```

### Database Support

Supported database connectors: https://db0.unjs.io/connectors
**Cloudflare D1 Database** is recommended.

1. Create D1 database in Cloudflare Worker dashboard
2. Configure database_id and database_name in wrangler.toml
3. If wrangler.toml doesn't exist, rename example.wrangler.toml and modify configurations
4. Changes will take effect on next deployment

### Docker Deployment

In project root directory:

```sh
docker compose up
```

You can also set Environment Variables in `docker-compose.yml`.

## Development

> [!Note]
> Requires Node.js >= 20

```sh
corepack enable
pnpm i
pnpm dev
```

### Adding Data Sources

Refer to `shared/sources` and `server/sources` directories. The project provides complete type definitions and a clean architecture.

For detailed instructions on how to add new sources, see [CONTRIBUTING.md](CONTRIBUTING.md).

## Roadmap

- Add **multi-language support** (English, Chinese, more to come).
- Improve **personalization options** (category-based news, saved preferences).
- Expand **data sources** to cover global news in multiple languages.

**_release when ready_**
![](https://testmnbbs.oss-cn-zhangjiakou.aliyuncs.com/pic/20250328172146_rec_.gif?x-oss-process=base_webp)

## Contributing

Contributions are welcome! Feel free to submit pull requests or create issues for feature requests and bug reports.

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines on how to contribute, especially for adding new data sources.

## License

[MIT](./LICENSE) © ourongxing

16.03.26 Sync Forc

Ось результати аналізу та стратегія трансформації для проекту **newsnow**, підготовлені у форматі для копіювання в Notion.

---

# 📑 Звіт AI-консультанта: Проект "newsnow"

**newsnow** — це елегантний агрегатор новин у реальному часі, орієнтований на зручне читання гарячих тем із адаптивним керуванням ресурсами.

---

## 🧬 Частина 1: "ДНК" Проекту

Логіку коду проекту можна розбити на такі **атомарні функції**:

*   **Адаптивний парсинг (Scraping/Parsing):** Збір даних із різних джерел (наприклад, Weibo, qqVideo) з інтелектуальним інтервалом (мінімум 2 хвилини), що базується на частоті оновлення джерела для запобігання банів по IP.
*   **Авторизація (OAuth):** Система входу через GitHub OAuth для синхронізації даних користувачів.
*   **Керування кешуванням:** Логіка 30-хвилинного кешування за замовчуванням із можливістю примусового оновлення для авторизованих користувачів.
*   **Інтеграція з базами даних:** Підтримка конекторів db0, зокрема Cloudflare D1, для збереження стану та даних.
*   **Інтерфейс MCP (Model Context Protocol):** Підтримка серверів MCP, що дозволяє інтегрувати зовнішні інструменти та ШІ-моделі безпосередньо в екосистему проекту.
*   **UI/UX Рендеринг:** Клін-дизайн на базі TypeScript та Vite для оптимального читання.

### 💎 Головна технічна цінність
Головна цінність **newsnow** полягає у **чистій архітектурі для додавання нових джерел** та вбудованій підтримці **MCP**. Це робить проект не просто сайтом новин, а "інформаційним хабом", який легко масштабується та готовий до взаємодії з ШІ-агентами "з коробки".

---

## 🚀 Частина 2: "Трансформація" (Інтеграція з Gemini LLM)

Додавання мультимодальної моделі **Gemini** (через **GitHub Models**) кардинально змінює роль проекту: від "читалки" до "аналітичного центру".

### Як зміниться функціонал?
1.  **Миттєвий переклад та локалізація:** Оскільки демо-версія наразі підтримує лише китайську, Gemini може в реальному часі перекладати гарячі новини на англійську чи українську, реалізуючи пункт із дорожньої карти проекту.
2.  **Смислове групування (Clustering):** ШІ може групувати новини з різних джерел за контекстом, а не просто за часом, виділяючи головні тренди дня.
3.  **Автоматичне резюмування (TL;DR):** Замість читання довгих стрічок, користувач отримує стислий виклад головних подій за останні 30 хвилин.
4.  **Персоналізація через інтереси:** Використовуючи roadmap-ідею категорій, Gemini може фільтрувати новини, залишаючи лише ті, що відповідають професійним інтересам користувача.

### Сценарій сервісу "AI News Digest" (newsnow + Gemini + ваші ID_{$})

Сценарій створення автономного новинного сервісу на вашому сайті:
1.  **Збір (newsnow Core):** Система автоматично збирає сирі дані через адаптивні інтервали.
2.  **Фільтрація (ID_{$1}):** Ваш базовий Python-скрипт **ID_{$1}** витягує свіжі заголовки з бази даних Cloudflare D1.
3.  **Аналіз (Gemini):** Дані передаються в Gemini. Модель аналізує тональність новин і пише короткий аналітичний звіт (наприклад: *"Сьогодні в IT-секторі переважають позитивні новини щодо ШІ"*).
4.  **Публікація (ID_{$2}):** Другий скрипт **ID_{$2}** форматує цю аналітику та оновлює ваш сайт або надсилає дайджест підписникам.
5.  **Деплой (GitHub Spark):** Весь цей ланцюжок розгортається як "інтелектуальний застосунок" через **GitHub Spark**, забезпечуючи автоматизацію без потреби в управлінні важкою інфраструктурою.

---

## 📋 План дій для Notion
| Крок | Дія | Результат |
| :--- | :--- | :--- |
| **1** | Форк репозиторію та розгортання в Cloudflare Pages | Робоча база агрегатора |
| **2** | Налаштування Cloudflare D1 для збереження новин | Сховище для обробки ШІ |
| **3** | Підключення Gemini через **MCP Server** | Інтелектуальний аналіз контенту |
| **4** | Зв'язування з вашими скриптами `ID_{$}` | Автоматизований сервіс на сайті |

---

### 💡 Резюме

**Суть:** **Елегантний адаптивний агрегатор новин**.

**AI-Роль:** **Створення інтелектуальних застосунків через Spark**.
