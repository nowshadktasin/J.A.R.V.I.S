The n8n setup is crucial as it handles all of the actual tool calling in the backend for your AI assistant. It acts as the routing mechanism for various actions the AI takes based on user requests.

Here's a step-by-step breakdown of the n8n setup:

* **1\. Download the n8n Workflows:**
  * Find all the necessary n8n agents and templates, including the main Jarvis assistant and its child agents, access the JSON from the GitHub.  
  * Download the workflows, which include the email agent, calendar agent, research agent, contact agent, and the main Jarvis agent (referred to as the "ultimate personal assistant" template).  
* **2\. Import Workflows into n8n:**
  * Go to [n8n website](https://n8n.io/) 
  * In n8n, open a new workflow.  
  * Go to the top right corner and click **"Import from file"**.  
  * Select the downloaded n8n workflow files, and they will be imported exactly as provided.
  * You need to create workflows to import separate files
  
  One JSON file for One workflow

* **3\. Connect Main Jarvis Workflow to Child Agents:**

  * The main Jarvis workflow acts as a central router. It has access to four different child agents (email, calendar, contact, content creator) and a Tavily search tool.  
  * Ensure that the main Jarvis workflow in your n8n instance is correctly linked to the specific workflow files you imported for each child agent. For example, a "call n8n workflow as a tool" node will link to a separate workflow like the "email agent".  
* **4\. Plug in Your Credentials:**

  * Upon importing, you will likely see red indicators in various areas, meaning you need to plug in your own API keys and credentials.  
  * This includes your **Tavily API key** and **Gemini API key** in the main workflow.  
  * Additionally, when you open child agents (like the email or calendar agents), you'll need to input your specific credentials for those tools as well. You need to create your own contact list on Airtable and connect the credentials in n8n  
      
* **5\. Set up the Webhook Trigger:**

  * The n8n workflow is **triggered by a webhook** that receives data from the Eleven Labs voice agent.  
  * In n8n, set up a **"webhook trigger" node**.  
  * Ensure its **method is set to `POST`** because Eleven Labs will send data to n8n.  
  * You will get a **test webhook URL** from n8n; this is the URL you'll initially paste into the Eleven Labs custom tool configuration.  
  * The webhook receives a **body parameter called `query`** which contains the user's request from the Eleven Labs voice agent.  
* **6\. Configure the "Respond to Webhook" Node:**

  * Set up a **"respond to webhook" node** in your n8n workflow.  
  * Crucially, this node should be configured to **`Respond to Webhook` (not `Immediately`)**. This ensures that n8n waits for all the tools to execute and actions to complete before sending a response back to the Eleven Labs voice agent.  
* **7\. Configure the Main Agent Node:**

  * **User Message Input**: The agent node in n8n needs to know where to find the user's message. You'll **drag the `body.query`** from the webhook node into the "user message" field of the agent node.  
  * **System Prompt**: Provide a system prompt for the main n8n agent. Its primary job is **to route the user's query to the correct child tool/agent**, not to perform the actions itself.  
    * **Overview**: "You are the ultimate personal assistant. Your job is to send the user's query to the correct tool".  
    * **Tool Listing**: Explicitly list and describe each tool (email agent, calendar agent, Tavily tool, etc.) and explain when to use them.  
    * **Rules**: Include rules for tasks requiring multiple tool calls, e.g., using the contact agent first to grab contact information before hitting another tool.  
    * **Current Date/Time**: Provide access to the current date and time using `$now` function.  
  * **Chat Model**: The example uses **Gemini 2.5** as the chat model for its reasoning capabilities, especially when multiple steps or tools are required.  
  * **Memory**: Configure "simple memory" (previously "window buffer memory") to separate chat history by user. The **`host` from the webhook** was used as the key for memory storage. The agent can look through five context interactions for chat history.  
* **8\. Child Agent Workflows (Examples):**

  * These are separate n8n workflows that the main agent calls. They are specialized in specific tasks:  
    * **Email Agent**: Contains various actions for interacting with email (e.g., labeling as high priority, sending emails).  
    * **Calendar Agent**: Handles calendar functions like creating, deleting, or checking events.  
    * **Contact Agent**: Used to grab contact information (e.g., from an Airtable database) before subsequent actions like sending an email or creating an event with an attendee.  
    * **Content Creator Agent**: Can use internet search (Tavily) and create content.  
    * **Tavily Tool**: Used for web searches, receiving a `search term` query and returning search results.  
* **9\. Activate for Production:**

  * During setup and testing, workflows are usually inactive, requiring you to click "test workflow" to make them listen for incoming requests.  
  * To make the webhook **always listening**, you need to **activate the workflow**.  
  * When activated, you must **switch the URL in Eleven Labs from the test URL to the production URL** provided by n8n.  
* **10\. Troubleshooting and Testing:**

  * It's important to **explicitly define in the system prompt when the agent should use a tool and what information it should send to that tool**. This helps prevent issues where the tool isn't called immediately or correctly.  
  * If n8n isn't actively listening (i.e., the workflow isn't active or in test mode), the Eleven Labs agent will return an error.  
  * The system prompt in Eleven Labs includes instructions for error handling, for instance, telling Jarvis to subtly imply external inefficiencies rather than taking blame, such as saying, "Ah, it seems something went wrong. Naturally, it isn't my fault sir, but I shall investigate regardless".

