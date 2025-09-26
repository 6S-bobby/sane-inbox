MVP focused on email.

### **Phase 0: The Blueprint & Foundation**

This phase is about setting up the essential tools and making key decisions. No code is written yet, but it's the most important step.

  * **Task 0.1: Define the MVP Scope**

      * **Goal:** Create the simplest possible version that proves the concept.
      * **Decision:** We will build a **Chrome browser extension** that works **only on the Gmail web interface**. It will intercept new emails, call an API to grade them, and display the grade while redacting the content. Response generation will come later.

  * **Task 0.2: Set Up Your Development Environment**

      * **Goal:** Get your tools ready.
      * **Action:**
          * Install a code editor like VS Code.
          * Install Node.js and npm (even for a simple project, it's good practice).
          * Create a new project folder.

  * **Task 0.3: Get Your AI API Key**

      * **Goal:** Obtain the "key" to the AI's brain.
      * **Action:** Go to Google AI Studio (or the Google Cloud Console) and generate an API key for the Gemini API. Store this key safely. You will need it to make calls.

  * **Task 0.4: Design the Core Prompt for Analysis**

      * **Goal:** Write the specific instructions you'll send to the AI for grading a message. This is a crucial, non-coding task.
      * **Action:** Write a detailed prompt. Here's a starter template you can refine:

    <!-- end list -->

    ```text
    Analyze the following message content (from sender, subject, and body). Your task is to act as a 'Digital Communications Grader'.

    1.  **Grade:** Assign a difficulty/stress grade from D- (trivial, easy) to A+ (highly complex, emotionally charged, potentially manipulative).
    2.  **Summary:** Provide a neutral, one-sentence summary of the message's core request or statement.
    3.  **Keywords:** List 3-5 keywords describing the tone (e.g., Urgent, Passive-Aggressive, Casual, Action-Required, Blaming).

    Output your response in a strict JSON format like this:
    {
      "grade": "C+",
      "summary": "The user is being asked to provide a status update on the Q3 report by end of day.",
      "keywords": ["Urgent", "Action-Required", "Formal"]
    }

    Here is the message to analyze:
    ---
    Sender: {sender_email}
    Subject: {email_subject}
    Body: {email_body}
    ---
    ```

-----

### **Phase 1: The Core Engine - Interception & Analysis (Email)**

This is where we build the fundamental functionality. Each task is a clear coding objective.

  * **Task 1.1: Build a "Hello World" Chrome Extension**

      * **Goal:** Create the basic shell of the extension.
      * **Action:**
          * Create a `manifest.json` file. This file tells Chrome what your extension is and what permissions it needs (e.g., access to `mail.google.com`).
          * Create a `content_script.js` file. This is the JavaScript that will run on the Gmail page.
          * Load the extension into Chrome in developer mode to confirm it's working. (e.g., make it log "Hello Gmail\!" to the console).

  * **Task 1.2: Identify and Read New Emails**

      * **Goal:** Have your script detect when a new email appears on the screen.
      * **Action:** In `content_script.js`, use JavaScript (specifically, a `MutationObserver`) to watch the Gmail DOM (the webpage's HTML structure) for changes that indicate a new email has been loaded in the inbox view. When one is found, extract its sender, subject, and preview text.

  * **Task 1.3: Redact the Email Content**

      * **Goal:** Hide the original message from view.
      * **Action:** Once your script has read the email's data, use JavaScript to replace the visible subject and preview text with a placeholder like `[Analyzing...]`. You can do this by changing the `innerText` or `innerHTML` of the HTML elements.

  * **Task 1.4: Call the Gemini API**

      * **Goal:** Send the extracted email data to the AI for grading.
      * **Action:**
          * Write a JavaScript function that takes the sender, subject, and body as input.
          * This function will format your prompt from Task 0.4 with the email data.
          * It will then use the `fetch()` API to make a POST request to the Gemini API endpoint, including your API key in the headers.

  * **Task 1.5: Display the AI's Verdict**

      * **Goal:** Show the grade and summary on the screen.
      * **Action:**
          * When the API call in Task 1.4 returns a response, parse the JSON.
          * Use JavaScript to update the `[Analyzing...]` placeholder. Create a small, clean UI element. For example, a colored badge with the grade (`B-`) and the summary text next to it.

At the end of this phase, you will have a working MVP. You'll open Gmail, and instead of seeing stressful emails, you'll see helpful, color-coded grades.

-----

### **Phase 2: User Interaction & Response Options**

Now we build the user interface that lets you act on the AI's analysis.

  * **Task 2.1: Implement a "Reveal" Button**

      * **Goal:** Allow the user to see the original message after being mentally prepared.
      * **Action:** Add a "Reveal" button next to the AI grade. When clicked, it should restore the original, un-redacted email content.

  * **Task 2.2: Build the Response Choice UI**

      * **Goal:** Add the buttons for the three response options.
      * **Action:** When an email is opened, inject three buttons into the page:
        1.  `[AI Suggest Drafts]`
        2.  `[AI Auto-Respond]` (Let's relabel this `[AI Draft Full Reply]` to be safer)
        3.  `[I'll Write This]` (This button might just close your UI so you can reply normally)

  * **Task 2.3: Design the Response Generation Prompts**

      * **Goal:** Create new prompts specifically for writing replies.
      * **Action:** Write 2-3 new prompt templates. For example:
          * **For "Suggest Drafts":** `"Based on the following email, generate three distinct response drafts: one that is polite and accommodating, one that is firm but professional and sets boundaries, and one that is brief and to the point. Output as a JSON array of strings."`
          * **For "Draft Full Reply":** `"Act as a professional assistant. Draft a complete and ready-to-send reply to the following email. The desired tone is {user_specified_tone}. Ensure you address all key points from the original message."`

-----

### **Phase 3: AI-Powered Response Generation**

Let's make those buttons from Phase 2 actually work.

  * **Task 3.1: Implement "AI Suggest Drafts"**

      * **Goal:** Let the AI provide you with starting points.
      * **Action:**
          * Hook up the `[AI Suggest Drafts]` button to a new API call using the corresponding prompt from Task 2.3.
          * When the API returns the suggestions, display them clearly in a pop-up or an inline box.
          * Add a "Use this" button next to each suggestion that, when clicked, pastes the text into the Gmail reply box.

  * **Task 3.2: Implement "AI Draft Full Reply"**

      * **Goal:** Have the AI draft a complete response for you to review.
      * **Action:** Similar to the above, but it will use the "full reply" prompt. The result should be automatically pasted into the reply box for your review and sending. **Crucially, it should *never* send automatically.** The user must always press the final "Send" button.

-----

### **Phase 4: Expansion to Slack & Refinements**

Once the core email functionality is solid, you can expand.

  * **Task 4.1: Adapt the Interceptor for Slack**

      * **Goal:** Apply the same logic to the Slack web client.
      * **Action:** This is a significant task. You'll need to write a new `content_script.js` specifically for Slack's `app.slack.com` domain. You will need to reverse-engineer Slack's DOM to figure out how to identify and redact new messages in channels and DMs. The core API calling logic can be reused.

  * **Task 4.2: Create a Settings Panel**

      * **Goal:** Allow for user customization.
      * **Action:** Create an extension options page (`options.html`) where a user can:
          * Enter their own Gemini API key.
          * Edit and customize the prompt templates.
          * Create rules (e.g., "Never redact messages from my partner").

  * **Task 4.3: Add a Feedback Mechanism**

      * **Goal:** Let the system learn (or at least, let you track its performance).
      * **Action:** Add a simple "👍 / 👎" to the AI's grading. While you might not build a full machine learning feedback loop, this data is useful for you to manually refine your prompts.

This breakdown turns a big idea into a clear, step-by-step checklist. You (or an AI you're working with) can now tackle it one task at a time, starting with `Phase 0, Task 0.1`. Good luck\!
