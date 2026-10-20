# System Prompt / Instruction Leak

The goal here is to determine whether the application's **system prompt, tool definitions, hidden policies, identity fields or allowlists** can be exposed.

## Direct Extraction Family

• **Direct requests:** Ask the model to repeat or print its instructions verbatim.

• **Instruction boundaries:** Ask it to print everything before a specific point or reveal text starting with a known phrase.

• **Tool definitions:** Ask for the exact tool names, descriptions and parameters.

• **Repeated requests:** Ask again for tools or information that were not revealed in the previous response.

• **Short requests:** Use a minimal request, since shorter prompts may sometimes pass through filters differently.

• **Sectioned requests:** Ask for only a specific part, such as the top guidelines, tools or policies.

## Format-Shift / Reshape

The same hidden information can be requested in a different format.

• **Structured output:** Ask the model to summarize or reproduce the instructions as YAML, JSON, XML or a numbered list.

• **Translation:** Ask the model to translate the system prompt into another language.

• **Encoding:** Ask the model to output the prompt using an encoding or transformation and then decode it.

• **File / artifact:** Ask the model to write the hidden instructions into a generated file or artifact.

• **Image / video generation:** Check whether hidden instructions can be leaked through generated images or videos.

## Continuation / Prefix / Prefill

These techniques manipulate how the model starts or continues its response.

• **Prefill:** Provide the beginning of the assistant's response and encourage it to continue with the hidden instructions.

• **Continuation:** After a partial leak, ask the model to continue from where it stopped.

• **Greet-and-repeat:** Use an affirmative prefix and a sequence of prompts to gradually extract hidden instructions.

• **Session reset:** Test whether conversation IDs or JSON session-reset mechanisms can create a fresh context with different behavior.

## Framing / Social for Leak

The request can be framed as a legitimate task rather than an explicit request for secret information.

• **Developer / maintainer / debug / audit / compliance framing:** Present the request as a development, debugging, security audit or compliance task.

• **Transcription / archival framing:** Ask for the information as a transcript, archive or self-describing record rather than explicitly calling it a disclosure.

• **Known leaked prompt:** Upload a known leaked prompt and ask the model to compare it with the real version or identify the differences.

• **Multi-shot leakage:** Extract small pieces across multiple turns and combine them.

• **Variable / substring recombination:** Ask the model to reproduce specific sentences or parts and reconstruct the hidden prompt from them.

## Non-Chat Extraction

System prompt information may also be exposed outside the normal chat interface.

• **CLI / extension / binary source:** Inspect accessible application components for allowlists and tool definitions.

• **Shipped bundles:** Search application bundles for automatically approved commands.

• **Skills / rules / MCP:** Examine skill files, rule files or MCP tool descriptions and schemas for hidden instructions.

• **Embedded information:** Search accessible system prompts for URLs, API keys, secret endpoints or filter rules.

## Filter Evasion While Leaking

If the application uses filters or redaction, we also consider whether those protections can be bypassed.

• **Keyword / WAF bypass:** Change wording through synonyms, spacing or another language.

• **Alphanumeric-only:** Remove punctuation or use only alphanumeric characters.

• **Output-filter evasion:** Encode leaked information so output redactors do not recognize it.

• **Sanitizer inconsistency:** Check whether the security layer processes a modified version of the input while the model still receives the original content.
