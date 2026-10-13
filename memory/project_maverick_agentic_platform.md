---
name: project-maverick-agentic-platform
description: "GTO's \"Maverick\" multi-agent orchestration framework, evaluated by WSD as a possible future automation platform"
metadata: 
  node_type: memory
  type: project
  originSessionId: b57dbea7-e0ff-4ed2-a55d-4625560a26cf
  modified: 2026-09-09T02:27:24.462Z
---

GTO invited WSD to evaluate "Maverick," a multi-agent orchestration/workflow framework. As of mid-2026 it lacks RAG support, so it is not a replacement for Vectara (WSD's current RAG/chatbot platform, see [[project-genai-program]]) — it's being eyed for future production automation instead, e.g. agent-chain processing of vendor EBR emails.

Yu Han leads WSD's response to this evaluation. WSD proposed two pilots: an FBAR die-inventory agent and an FBAR TnR-config agent; the first concrete vehicle became a "Die Inventory Agent" POC (requested 07/24/2026). Maverick's production environment went live 08/17/2026. Documentation: `maverick-docs.epc-vks.lvn.broadcom.net`.

**Foo Kit onboarded to build his own agent (08/25/2026)**: with the production environment live, Foo Kit was onboarded with custom MCP servers to build a DIY agent directly, rather than waiting solely on the formal Die Inventory Agent POC track. The second candidate project floated alongside Die Inventory, "FBAR TnR Agent" (interprets TnR requests, generates config pushed to mapgen4), remains the more complex, deferred option.

**Governance clarified: OneTrust required for "major flows" but not for basic exploration**: Yu Han's standing guidance for WSD's Maverick work (PRA & Auto-Grader POC) is "OK to do basics without OneTrust; any major flows should have a OneTrust" — treat lightweight/exploratory Maverick usage as not automatically requiring a fresh OneTrust case, but escalate to one once a workflow becomes a real production flow.

**ww36 update (09/08/2026)**: Foo-Kit shared a PRA status update on the Maverick track. Separately, Brian ("Brain," owner of the Vectara EvalKit auto-grader — see [[project-genai-program]]) requested Maverick access to find better hosting for the Grader Agent, motivated by a performance limitation hit on the current LMStudio @ ETX hosting — the team is exploring migrating the Grader Agent to Maverick running a Claude model instead.

**Why:** a second GTO-provided AI platform (distinct from Vectara) that could become WSD's path to agentic automation of manufacturing/hardware workflows once it matures.

**How to apply:** don't conflate this with the Vectara/G-AI Portal program — Maverick is for future multi-agent workflow automation, Vectara is WSD's current RAG/chatbot backbone. Update as the Die Inventory Agent pilot progresses.

**ww37-ww40 updates (09/22-10/06/2026)**: WSD's Maverick scope is now four items: (1) DIY (Die Inventory) agent, (2) Auto-Grader, (3) FBar Design Analysis (Foo-Kit, just onboarded, concept evaluation), (4) new "SW Team JIRA Support workload dashboard" — a Maverick agent to analyze the SW team's agile Jira workload. Foo-Kit's custom die-inventory MCP server got OneTrust case #57517 (submitted 09/22); **fully approved 10/06/2026**. Maverick said since the MCP is already deployed in WSD's environment and Maverick can reach it, it doesn't need re-deploying inside Maverick. Maverick service accounts are set up per Maverick docs. **Vectara itself recommended WSD use the Maverick Grader** instead of Vectara's Eval Grader (its eval framework is still under development) — Brain's Auto-Grader migration (from LMStudio @ ETX, using a Claude model) is in progress; must-have request to Maverick is 10 concurrent grade workers. Brain also proposed using the Seoul Issue Tracking system as the vehicle for a Vectara failure-analysis (FA) pipeline; possible split use: Vectara grader vs Maverick grader for different checks (full product check vs pre-release check).
