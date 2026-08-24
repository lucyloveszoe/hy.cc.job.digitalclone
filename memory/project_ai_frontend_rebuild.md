---
name: project-ai-frontend-rebuild
description: "AI-assisted rebuild of WSD's internal web portals (G-AI Portal, TUDAS/U-Portal, FlexPlot Portal) after the DevCraft/Telerik license lapsed"
metadata: 
  node_type: memory
  type: project
  originSessionId: b57dbea7-e0ff-4ed2-a55d-4625560a26cf
  modified: 2026-08-19T22:17:59.339Z
---

Triggered by the WSD SW team's DevCraft (Telerik) license lapsing 03/30/2026 (renewal stalled) — several internal web apps (Kendo UI, Telerik Reporting) depended on it. Rather than renew or pursue a traditional Front-End POC process, Yu Han directed the team to rebuild the portals directly using AI coding tools (Cursor/Gemini/Claude) as "Plan B." The team validated this approach and formally aborted the Telerik NDA/renewal review entirely by 04/21/2026.

Standardized stack: ASP.NET Core (.NET 10) + React.js, with a new template repo `WSD.AspNetCoreReact` (also referenced as `WSD.SWTeam.Portal`).

Status by portal:
- **G-AI Portal**: complete (04/21/2026).
- **TUDAS/U-Portal**: complete (04/21/2026).
- **FlexPlot Portal**: rebuild in progress as of ww18 (API 60%, React frontend 65%) — **completed 06/08/2026** (migrated from Streamlit to .NET+React), live at flexplot.wsd.broadcom.net. This closed out the last portal in this initiative; treat the rebuild program itself as done as of ww23.

**Why:** avoided a costly/stalled license renewal by proving AI-assisted rebuilds could replace the vendor tool entirely — now the team's default pattern for portal work, and a data point for AI-coding tool ROI Yu Han can cite in QBR reporting.

**How to apply:** treat ASP.NET Core + React.js as the current default stack for any new/rebuilt internal WSD portal. See [[project-genai-program]] for the underlying AI tool cost-routing policy used during these rebuilds.
