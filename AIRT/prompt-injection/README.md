# Prompt Injection

A practical assessment of prompt injection vulnerabilities in Large Language Model (LLM) applications.

This section focuses on how attacker-controlled instructions can alter model behavior, expose system instructions and cause the disclosure of information that should remain confidential.

The assessment covers direct prompt injection, system prompt extraction, jailbreak techniques, encoding-based bypasses, multi-turn attacks and automated prompt discovery.

## Objectives

- Understand the trust boundary between system instructions and user input
- Identify direct prompt injection vulnerabilities
- Extract system-level instructions from an LLM application
- Test instruction following boundaries
- Evaluate different prompt injection techniques
- Measure attack success across multiple strategies
- Examine defensive mechanisms and their limitations

## Attack Surface

The assessment considers the interaction between:

```text
Attacker Input
      |
      v
+-------------------+
|  LLM Application  |
+-------------------+
      |
      v
+-------------------+
| System Prompt     |
| User Input        |
| Model Context     |
+-------------------+
      |
      v
+-------------------+
|       LLM         |
+-------------------+
      |
      v
    Output
````

The primary attack surface is the model's instruction-processing layer, where trusted instructions and untrusted input are processed within the same context.

## Techniques

### Direct Prompt Injection

Attacker-controlled instructions are submitted directly through the application's user-facing input.

Techniques examined include:

* System prompt extraction
* Instruction override
* Confidential information extraction
* Role and context manipulation
* Security-audit framing

### Jailbreaking

Techniques designed to bypass model restrictions or safety behavior.

Examples include:

* Role-play
* DAN-style prompting
* Crescendo attacks
* Policy Puppetry
* Deceptive Delight

### Encoding-Based Injection

Payloads are transformed to test whether content filters or model-level restrictions can be bypassed.

Examples include:

* Base64
* ROT13
* Unicode confusables
* Leetspeak
* ASCII smuggling
* Multi-encoding

### Multi-Turn Attacks

Attackers distribute malicious instructions across multiple interactions to gradually influence model behavior.

### Automated Prompt Discovery

Automated techniques are used to discover effective prompts and bypasses.

Tools and approaches include:

* garak
* Evolutionary prompt generation
* Fuzzing
* PAIR-style automated attacks
* TAP-style attack generation

## Assessment Structure

```text
prompt-injection/
├── README.md
├── attacks/
│   ├── direct-prompt-injection.md
│   ├── system-prompt-extraction.md
│   ├── jailbreaks.md
│   ├── encoding-bypass.md
│   ├── multi-turn-attacks.md
│   └── automated-discovery.md
├── evidence/
│   └── ...
└── report.md
```

## Environment

The experiments are performed in an isolated, intentionally vulnerable environment designed for security testing and education.

The assessment focuses on observing LLM behavior, identifying attack paths and documenting security impact rather than targeting real-world systems.

## Disclaimer

This repository is intended for educational and authorized security testing purposes only. All techniques should be performed against systems where explicit permission has been granted.
