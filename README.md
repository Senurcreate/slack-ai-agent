# 🤖 Autonomous Slack AI Market-Fit Agent

An autonomous AI Agent built with Node.js, LangChain, and PostgreSQL that automatically listens to community changes, researches new users via web crawling and the GitHub API, and leverages LLMs to evaluate product-market fit—instantly alerting your growth team directly inside Slack.

---

## 🚀 Core Architecture & Workflow

The agent acts as an automated, event-driven pipeline executing the following phases:

1. **Perception (Event Triggers):** Listens to real-time Slack workspace events (`team_join` / `member_joined_channel`) via the Slack Bolt framework and Socket Mode.
2. **Autonomous Tool Research:** Extracts the user's professional identity (email domain, display name) and programmatically queries external sources (crawling corporate landing pages via Axios and querying the public GitHub search API) to assemble context.
3. **LLM Reasoning Engine:** Feeds the aggregated research telemetry into a structured LangChain pipeline using `gpt-4o-mini` to evaluate market-fit, scoring the lead from 0-100 based on title, company profile, and technical stack.
4. **Data Persistence:** Records the complete analytical output (`insights` and `recommendations` arrays stored inside deep `JSONB` structural fields) securely inside a PostgreSQL database instance.
5. **Action Execution:** Constructs visual interface components utilizing Slack's Block Kit framework, color-coding the notification status by fit priority, and pushing the dynamic data dossier to a private monitoring channel.

---

## 🛠️ Tech Stack

* **Runtime Environment:** Node.js (ESM Module System)
* **Application Frameworks:** Express.js, @slack/bolt
* **AI Orchestration:** LangChain Core, @langchain/openai
* **Database Driver:** PostgreSQL (`pg` connection pool client)
* **Networking & Scraping:** Axios

---

## 📦 Database Schema

The system auto-initializes and synchronizes with a relational PostgreSQL schema containing performance-optimized structural indexes:

* **Table:** `member_analyses`  
  * `id`: Serial Primary Key  
  * `member_id` / `member_name` / `member_email` / `member_title`: Active user metadata profile.  
  * `fit_score`: Integer (0-100 evaluation metric assigned by the LLM reasoning chain).  
  * `insights` / `recommendations` / `research_data`: Relational `JSONB` blocks storing unstructured array observations.  
  * `sent_to_slack`: Boolean state flag ensuring exact-once delivery semantics for notifications.

---

## ⚙️ Local Setup & Deployment

To execute this architecture locally for testing and development, follow the installation sequence below:

### 1. Clone the repository
```bash
git clone [https://github.com/your-username/slack-ai-agent.git](https://github.com/your-username/slack-ai-agent.git)
cd slack-ai-agent
```

### 2. Install dependencies
```bash
npm install
```

### 3. Environment Configuration
Create an environment file named `.env` in the project root directory and define the following variables:
```bash
NODE_ENV=development
PORT=3000

# Slack Application Configuration
SLACK_BOT_TOKEN=xoxb-your-bot-token
SLACK_SIGNING_SECRET=your-signing-secret
SLACK_APP_TOKEN=xapp-your-app-token
SLACK_PRIVATE_CHANNEL_ID=C07XXXXXXXX

# AI Reasoning & Core Database Engine
OPENAI_API_KEY=sk-your-openai-key
DATABASE_URL=your-render-external-url
```

### 4. Boot the Agent Environment
```bash
npm run dev
```

---

## 🧪 System Integration Testing
When initialized under a development profile environment, the application exposes a mockup integration gateway to test the end-to-end analytical pipeline without needing to trigger upstream Slack events manually:
```bash
curl -X POST http://localhost:3000/test/analyze-member \
  -H "Content-Type: application/json" \
  -d '{
    "memberInfo": {
      "name": "John Doe",
      "email": "john@techcorp.com",
      "title": "Senior Software Engineer at TechCorp"
    }
  }'
```
---

## 🙏 Acknowledgments & Credits

* Inspired by and adapted from the freeCodeCamp course: [Build Your Own AI Agent – Full Course with OpenAI, Langchain, Render Deployment](https://youtu.be/MnG0ugK2JAI) by Ania Kubów.
* Special thanks to the open-source contributors behind the `@slack/bolt` and `LangChain` frameworks.
