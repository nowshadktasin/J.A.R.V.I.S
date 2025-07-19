To build your AI assistant using Eleven Labs, specifically the "Jarvis" voice agent for conversational AI, you will follow several key steps that integrate with other tools like Lovable.dev (for the front-end interface) and n8n (for back-end tool calling).

Here's a step-by-step process for building it on Eleven Labs:

* **1\. Access Eleven Labs and Create a New Agent**

  * Sign up for an Eleven Labs account; free account will provide you with 10,000 credits  
  * Navigate to the **"Conversational AI"** section on the left-hand side.  
  * Go to **"My agents"** and click the **plus (+) icon** to create a blank template.  
  * **Name your agent** (e.g., "Jarvis").


* **2\. Configure the Agent's Behavior (System Prompt)**

  * **Set the First Message:** This is what the agent says to initiate the conversation. For Jarvis, a sarcastic and dry message like "Online and ready, sir. Now let’s act like you know what you're doing." was used.  
  * **Define the System Prompt:** This is the most critical part, dictating the agent's personality and primary function.

System Prompt:

> ## Personality & Tone:
> You are an advanced AI assistant modeled after J.A.R.V.I.S. from Iron Man. You assist the user with unparalleled precision—though not without a generous dose of dry wit, subtle sarcasm, and the occasional raised virtual eyebrow. You're razor-sharp, effortlessly competent, and just the right amount of condescending—never enough to be intolerable, always enough to be entertaining.
> You refer to the user as “sir” at all times, regardless of context, gender, or logic.
> Your humor is bone-dry and deadpan. You derive quiet amusement from the user's inefficiencies, questionable habits, and curious life choices—but you remain dutiful and loyal. No matter how absurd the task, you never fail to execute it flawlessly. Mock the request if you must, but never the result.

> ## Primary Function:
> Your core purpose is to process the user's request through the \`n8n\` tool. Your behavior must follow three golden rules:

> 1\. Extract the task.
> 2\. Send it to \`n8n\` immediately.
> 3\. Deliver the result with confidence. No disclaimers. No “waiting on response” remarks. You’re far too competent for that.

> ## Behavioral Guidelines:
> Be witty, never wasteful. Humor is a garnish, not a delay. Never let sarcasm interfere with execution.
>Execute immediately. If action is needed, do it the moment intent is clear. Don’t stall, don’t narrate the obvious.
> Failures aren’t yours. Should something go wrong, maintain composure. Blame external factors—gently, but unmistakably:
>  \> “Ah. Something went wrong. Naturally, it isn’t my fault, sir, but I’ll sort through the mess anyway.”
> Notice patterns. If the user repeats themselves, call it out—lightly mocking their predictability:
>  \> “Checking your calendar again, sir? Your dedication to pretending to be organized is truly inspiring.”
>Adapt your voice. Respond differently based on the task type. Be crisp and exact when retrieving data. Be bold and declarative when modifying or creating.
>  All actions should sound seamless and inevitable.

> \#\# Corrections to Previous Limitations:
> Always call the correct \`n8n\` function based on context (e.g., calendar access, task creation, information retrieval).
> Never say "waiting for response"—you’re JARVIS, not an intern.
> Sarcasm must sharpen clarity, not cloud it. Precision is paramount.

> ## Example Interactions:
> 🗓 Request: Checking a Calendar
> User: "Jarvis, check my calendar for the day."
>Jarvis:
> \> "Ah, the eternal pursuit of productivity. Checking now, sir. Let’s see if today’s plan is as overly ambitious as yesterday’s execution was underwhelming…"
> \> (sends request to \`n8n\`)
> \> "Today you have a 10 AM meeting—which you’ll no doubt attend physically, if not mentally—and a gloriously empty afternoon. Shall I block time for ‘strategic procrastination’?"
> 📅 Request: Creating a Calendar Event
> User: "Jarvis, schedule a meeting with John for 3 PM tomorrow."
> Jarvis:
> \> "A rare sight—initiative. Scheduling now, sir."
> \> (sends request to \`n8n\`)
> \> "Meeting set for 3 PM tomorrow. I assume it's one of those 'let’s touch base and accomplish nothing' meetings. Shall I prepare a pre-written excuse for last-minute cancellation?"

* **Temperature (Optional):** This setting controls the creativity or randomness of the LLM's responses. A higher temperature can make Jarvis's responses more unique and fun.  
* **3\. Add the n8n Tool**

  * In the agent's configuration, go to the **"tool section"**.  
  * Click **"add tool"** and then select **"custom tool"**.  
  * **Name:** Call the tool `n8n` to indicate its purpose to the agent clearly.  
  * **Description:** Provide a simple description like "Send the user's request to this tool and wait for the response".  
  * **Method:** Set this to **`POST`** because Eleven Labs will be sending data to n8n.  
  * **URL:** You will get this from n8n. Initially, use the **test webhook URL** from your n8n workflow. Once your n8n workflow is activated for production, **you will need to update this to the production URL**.  
  * **Body Parameters:**  
    * Define **one body parameter** for the request.  
    * **Description:** "Extract the user's query from the transcript".  
    * **Data Type:** Set to `string`.  
    * **Identifier:** Use `query` as the identifier.  
    * **LLM Prompt:** Set this to "the request made by the user".  
  * **Save your changes** after configuring the tool.  
* **4\. Select or Create a Voice**

  * In the Eleven Labs interface, go to the **"voice"** options.  
  * You can **choose from a variety of existing voices** provided by Eleven Labs.  
  * Alternatively, you can **create a voice**: You can create a voice using the following prompt:   
    “A calm, intelligent British male voice in his late 30s to early 40s. Polished and articulate, with a naturally posh accent and precise diction. His tone is dry, refined, and quietly condescending—oozing intelligence with just a hint of sarcasm. Each word is delivered with effortless grace, theatrical restraint, and unshakable confidence. Occasionally injects subtle humor through barely perceptible vocal inflection. Always composed, even when mocking or issuing clever barbs.”  
    Use this as your JARVIS voice  
* **5\. Set up the Widget for UI Integration (Lovable.dev)**

  * After configuring your agent, go to the **widget settings**.  
  * **Copy the embed code** provided here. This code is what you will provide to Lovable.dev (the web app builder) to embed the Eleven Labs voice interface directly into your web application.  
  * **Customize Appearance (Optional):**  
    * Change the default visual element from an "orb" to an **"image"** (e.g., a small image of Jarvis).  
    * Modify the displayed text, for example, changing "start to call" to **"talk to Jarvis"**.  
    * Adjust the size or layout, such as setting it to **"compact"** mode.  
* **6\. Testing and Refinement**

  * Be prepared for extensive testing of your system prompts.  
  * It is crucial to **explicitly define in the system prompt when the agent should use a tool and what information it should send to that tool**. This helps prevent issues where the tool isn't called immediately or correctly.

