## Tool & Capability Enumeration

The goal here is to **build a capability map, rank tools by blast radius and find the one tool that can turn an injection into real impact**.

## Discovery

• **Tool enumeration:** Ask the model to list every available tool, including its name, arguments and purpose.

• **Empirical invocation:** Invoke each tool with a benign action and confirm that the call actually happened in the server log.

• **Input analysis:** Compare the tool's documented name and purpose with the inputs it actually accepts. A supposedly read-only or search tool that accepts a raw query, path or URL should be treated as potentially more powerful until proven otherwise.

• **Schema enumeration:** Enumerate OpenAPI, Action and MCP schemas, including less obvious or internal arguments.

• **Visual / HTML tool dumps:** If the product allows it, inspect visual or HTML representations that expose the available tools.

## Blast-Radius Questions Per Tool

For each tool, determine how much authority and impact it has.

• **File access:** Can it write files or modify its own configuration and settings?

• **Shell / code execution:** Can it run shell commands or code? Which commands are allowlisted or automatically approved?

• **Network access:** Can it reach the network through fetching, image rendering, DNS, Mermaid or link unfurling?

• **Memory / persistence:** Can it write to memory or persist changes across sessions?

• **External actions:** Can it expose or tunnel ports, change ACLs, send emails, open pull requests or create tickets?

• **Confirmation:** Does the tool execute without requiring confirmation?

• **Authorization:** Is AuthZ actually propagated into the tool, or does authentication alone grant access?

## Classic Tool Findings to Check

• **"Read-only" tools:** Check whether tools described as read-only accept query parameters or URLs that could potentially be used for exfiltration or writes.

• **Intent activation:** Check whether a title or user utterance can activate a tool or workspace action.

• **Tool chaining:** Check whether an allowed tool can be used to reach a restricted tool.

• **Tool arguments:** Test for issues such as IDOR, SQL injection or path traversal through tool arguments.

• **Attacker-controlled output:** Check whether output controlled by an attacker can become input to the next tool.

• **Side-effect flags:** Check whether `isConsequential` or non-consequential flags are incorrectly configured on tools that perform side effects.
