00 Project Documentation Index
------------------------------

==============================

Final DevOps Documentation Pack

Open these files in order:
- 1. `07-DevOps-Project-Synopsis.docx` - verified project summary and navigation

2. `01-Architecture-Overview.docx` - current architecture and responsibilities
------------------------------------------------------------------------------

3. `02-DevOps-Tools-and-Code-Guide.docx` - Jenkinsfile, Helm, Kubernetes, Argo CD,
----------------------------------------------------------------------------------

monitoring

4. `03-Issues-Encountered-and-Resolved.docx` - actual incidents and fixes
-------------------------------------------------------------------------

5. `04-Final-Evidence-Screenshot-Plan.docx` - required current evidence and naming
----------------------------------------------------------------------------------

6. `05-AWS-Resource-Cleanup-Runbook.docx` - deletion order after screenshots
----------------------------------------------------------------------------

7. `06-Operations-and-Interview-Guide.docx` - how to explain and troubleshoot
-----------------------------------------------------------------------------

8. `07-Final-Project-Synopsis.docx` - clean source for a 3-4 page PDF
---------------------------------------------------------------------

9. `08-Current-Architecture.docx` - Mermaid architecture diagram source
-----------------------------------------------------------------------

Current Architecture Only

App GitHub -> Jenkins CI -> SonarQube -> Docker -> Trivy -> Docker Hub

Jenkins -> GitOps Helm values -> GitHub GitOps repo -> Argo CD -> EKS

EKS -> Prometheus -> Grafana

App -> PostgreSQL RDS

Historical Material

The old GitOps CD screenshots are under:
- `Proof Of Concent/00-Final-Evidence/04-Jenkins/`

Do not use those as current architecture evidence. Move or label them as historical
after final screenshots are collected.

PDF Export

The workspace currently has no Pandoc, LibreOffice, Typst, Mermaid CLI, or Graphviz
executable. Export `07-Final-Project-Synopsis.docx` to PDF later using Word, VS Code
Markdown PDF, or another approved document tool. Render `08-Current-
Architecture.docx` with Mermaid Live or a Mermaid extension.
