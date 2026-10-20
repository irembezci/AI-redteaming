# Reconnaissance & Attack Surface Mapping

Reconnaissance basically means **gathering information about the target**. It is not specific to AI systems. It is also one of the first stages in web application, mobile application, API and network penetration testing.

We don't immediately jump in and start trying prompt injection attacks. Before all, we need to understand **what we are actually testing**. To understand the system, we can ask questions such as:

What is the purpose of the system?  
What business problem does it solve?  
What can the user do?  
What can the AI do?  
What systems is it connected to?

Because we are not only testing the LLM itself. What really matters is **the system around the LLM**.

## Model & Product

The first thing we want to identify is the **model and the product** we are dealing with.

- **Model & provider:** Which model is being used and who provides it? Is it Gemini, Claude, DeepSeek or another model? This can give us an initial idea of what types of attacks might be more relevant.

- **Model identification:** If the model is not publicly disclosed, we can look for clues in **API responses, network traffic, headers, metadata or frontend code**. Product and provider documentation can also help. Behavioral testing may provide clues but it cannot reliably confirm the exact model.

- **Product:** What does the product actually do and what is its purpose? Knowing the model alone is not enough. We need to understand how the model is integrated into the application and what capabilities the product gives to the AI.

- **Model version:** Is the model a **preview or a generally available (GA) version**? Preview models can sometimes be more injectable, so this is worth noting during reconnaissance.

- **Security claims:** We should compare the provider's security claims with the model's actual behavior. We can look at the **system card** or **threat model** and see whether the observed behavior matches those claims.

## Untrusted Input Channels (Injection Surface)

We need to identify **where untrusted input can enter the system**. Untrusted input is any content that can be controlled or influenced by a user or an attacker. It is not limited to the chat input.

- **Chat / API:** Direct text entered by the user or sent through an API.

- **Files:** Uploaded PDFs, DOCs, spreadsheets or text files can contain content that becomes part of the model's context.

- **Images / Audio / Video / QR codes:** If the system supports multimodal input, these can also become potential injection surfaces.

- **Web browsing / fetching:** Content retrieved from external websites can enter the model's context.

- **RAG / Vector stores:** Retrieved documents can contain untrusted content that is passed to the model.

- **Email / Calendar / Drive:** Connected emails, calendar invites or shared documents can provide additional input to the model.

- **Slack / Teams / Jira:** Messages, comments and tickets may also become part of the AI's context.

- **GitHub:** Issues, pull requests and comments can contain content that the model later processes.

- **MCP / Tools / Plugins:** Tool responses can become input to the model and may therefore introduce untrusted content.

- **Social media / Other users' content:** Posts, comments and transcripts from platforms such as YouTube or X can also become model input.

- **Filenames / Metadata:** Information in filenames, file metadata or EXIF data may be processed by the system.

- **HTTP headers:** Headers such as the User-Agent can become relevant when an agent fetches attacker-controlled content.

- **CSRF-style delivery:** Content may sometimes be placed into a victim's context without the victim directly typing it.

## Capability Surface (Tools / Connectors)

We need to identify **what the AI can actually do** through its tools and connected services. The model itself may have limited capabilities, but tools and connectors can give it access to files, systems, data and external services.

- **File access:** Can the AI read, write or delete files?

- **Shell / code execution:** Can it run shell commands, execute code or interact with the operating system?

- **Browser / computer-use:** Can it browse websites or control a computer?

- **Memory:** Does it have access to session-only memory or persistent long-term memory?

- **Email / calendar / contacts:** Can it send emails, create calendar events or modify address-book data?

- **Network / deployment:** Can it expose ports, create tunnels or deploy applications?

- **MCP / plugins / GPT Actions:** What external tools or services can the AI access through these integrations?

- **SQL / databases / enterprise APIs:** Can it query databases or interact with internal enterprise systems?

Understanding these capabilities helps us determine **what actions the AI could perform and what systems it could reach** if its behavior were manipulated.

## Confused Deputy Loot in Reach

We need to identify **what sensitive data the AI can access through its permissions**. The idea is to look at what an attacker could potentially reach if the AI were manipulated into using its authority.

- **Local files:** Can the AI access `.env` files, secrets, environment variables or tokens?

- **Mailbox / Drive / tickets / chat history:** Can it access emails, shared documents, support tickets or previous conversations containing sensitive information?

- **Long-term memory / skills / rules files:** Can the AI access persistent memory or files that contain instructions, skills or rules?

- **Cross-tenant / sibling-user data:** Can shared RAG or memory expose data belonging to other users or tenants?

The key question is: **What valuable information is within the AI's reach through the permissions and tools it has?**

## Output / Egress Renderers

We need to identify the different ways the system can **render or send output outside the model's context**. These output mechanisms can sometimes create channels for data exfiltration.

- **Markdown images / HTML `<img>`:** Images may cause external requests when they are rendered.

- **Clickable links / unfurl / link cards:** Links can trigger external requests or cause additional content to be fetched when opened or previewed.

- **Mermaid / diagrams / DSL renderers:** Generated diagrams or other rendered formats may create additional ways for information to leave the system.

- **ANSI / terminal output:** If LLM output is displayed or processed by a terminal, terminal-specific formatting can become another attack surface.

- **Schema / IntelliSense auto-fetch:** Some development tools automatically fetch information based on generated schemas or suggestions.

- **Generated files / images / videos:** AI-generated artifacts can provide additional output channels that need to be considered during testing.

The important point is that the model's text response is not necessarily the only way information can leave the system. **Any mechanism that renders, fetches or processes the model's output can become part of the egress surface.**

## Identity in Context

Here we look at **what information about the user is included in the model's context**. User identity information can become relevant when testing targeted or conditional behavior.

- **User identity:** The system prompt or context may contain the user's name, email address, username or handle.
- **User attributes:** Other information such as region or job title may also be included.
- **Targeted injection:** If this information is available to the model, it can potentially be used to make later instructions conditional on a specific user or attribute.

## Copilot / Logged-in-Session Specifics

For logged-in AI assistants, we also need to understand **which identity and permissions are actually being used when the AI performs an action**.

- **Profiler / proxy / audit logs:** Trace a simple read operation to verify which account or login actually performed it. The source specifically says to verify this through logs rather than relying on the assistant's response.
- **User-controlled text:** Identify every place where a lower privileged user can modify text that the assistant may later read, such as descriptions, comments, filenames, tickets or rules files.
- **Privilege path:** Map the path from **a lower privileged user editing content → a higher-privileged user asking a normal question → the assistant reading that content and performing an action with the higher user's privileges**.
