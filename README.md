# -ShopEase-Complaints
# ShopEase AI Support Desk (n8n Workflow)

An automated customer support triage pipeline built in **n8n** that ingests customer complaints, enriches them with live order details, uses an LLM to classify category and urgency, logs all tickets to Google Sheets, and dispatches instant Telegram alerts for high-priority issues[cite: 1, 2].

## Architecture & Pipeline
1. **Form Trigger:** Captures customer details (`Full Name`, `Email`, `Order ID`, and `Complaint`) via an n8n web form[cite: 2, 3].
2. **Order Lookup (`HTTP Request`):** Queries the ShopEase Order API (`GET /carts/{orderId}`) to retrieve cart totals, item counts, and product details[cite: 2, 3].
3. **Data Sanitization (`Edit Fields`):** Filters the payload down to essential customer and order fields (`customer_name`, `order_total`, `number_of_items`, `first_product`)[cite: 2, 3].
4. **AI Classification (`LLM Chain`):** Analyzes the complaint text and outputs strict JSON containing `category` (`refund`, `delivery`, `other`), `urgency` (`high`, `low`), and a one-sentence `summary`[cite: 2].
5. **JSON Parsing (`Code`):** Strips markdown formatting and parses the raw LLM response into structured JSON keys[cite: 2, 4].
6. **Database Logging (`Google Sheets`):** Appends a timestamped row with full customer, order, and triage metadata to the `ShopEase Complaints` tracker[cite: 2, 3].
7. **Urgent Alerting (`IF` + `Telegram`):** Evaluates whether `urgency === "high"` and immediately notifies the support team via Telegram[cite: 2, 4].

## Tech Stack
* **Orchestration:** n8n[cite: 1]
* **AI / LLM:** OpenAI / Groq (`llama-3.3-70b-versatile`)[cite: 1, 4]
* **Integrations:** REST API (DummyJSON), Google Sheets OAuth2, Telegram Bot API[cite: 1, 3]
