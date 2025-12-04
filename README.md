# 📧 AI Email Assistant – n8n + OpenAI

An AI-powered email assistant built with n8n and OpenAI.  
It receives email data via webhook, classifies the message, assigns a priority, and generates a structured AI reply.

---

## 🧠 What it does

- Accepts incoming email payload via HTTP POST:
  - `from`
  - `subject`
  - `body`
- Sends the content to OpenAI (`Message a model`)
- Uses a strict JSON-style system prompt to get structured output:
  - `category` – e.g., `support`, `billing`, `complaint`, `question`, `other`
  - `priority` – `Low`, `Medium`, or `High`
  - `reply` – AI-generated email response
- Parses the raw OpenAI output into clean JSON using a JavaScript Code node
- Returns a well-formatted JSON response to the caller (e.g., Postman)

---

## 🛠 Tech stack

- **n8n** (local development – Desktop, compatible with self-hosted)
- **OpenAI API** – Message a Model (tested with gpt-4.1-mini)
- **JavaScript Code node** for JSON parsing
- **Webhooks + JSON**

---

## 🔁 Flow overview

```text
Postman (or any external client)
        ↓
n8n Webhook (POST /ai-email)
        ↓
OpenAI – Message a model
        ↓
Code (parse JSON into clean structure)
        ↓
HTTP Response: { category, priority, reply }

---

## 🚀 How to test

Follow these steps to run and verify the workflow:

1. **Start n8n**  
   Launch n8n Desktop (or your local/self-hosted instance).

2. **Create the workflow structure**  
   Build the following node chain:

3. **Configure the Webhook node**  
- Method: **POST**  
- Response mode: **When last node finishes**  
- Copy the **Test URL** provided by the node.

4. **Open Postman**  
- Method: **POST**  
- URL: `<your Webhook Test URL>`  
- Headers:  
  `Content-Type: application/json`

5. **Send a sample email payload**
Use the following JSON body:

```json
{
  "from": "customer@example.com",
  "subject": "Problem with my order",
  "body": "Hi, I ordered a product last week and it still hasn't arrived. Can you check the status of my order?"
}

6. Execute the test

In n8n, click Listen for test event on the Webhook node.

Then click Send in Postman.

7. Verify the response

You should receive a structured JSON output similar to:

{
  "category": "support",
  "priority": "High",
  "reply": "Dear Customer, thank you for reaching out. We apologize for the delay with your order. We will check the status and get back to you shortly. Thank you for your patience."
}

If you see this response, the V1 of the AI Email Assistant is working correctly.
