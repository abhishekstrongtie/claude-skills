---
name: azure_architect
description: Design an Azure cloud architecture, render it as a diagram using real Azure resource icons (Python `diagrams` library), and generate the matching Terraform (azurerm provider) code. Use when the user wants to plan/design Azure infrastructure, asks for an architecture diagram, or wants Terraform generated for a described Azure setup.
when_to_use: "User asks to design, diagram, sketch, or visualize an Azure architecture; or wants Terraform code generated to match an Azure architecture description."
argument-hint: "[description of the Azure architecture, e.g. 'AKS cluster with app gateway, key vault, and a postgres flexible server']"
allowed-tools: Bash(python3 *), Bash(pip3 *), Bash(pip *), Bash(which *), Bash(terraform *), Read, Write, Edit, Glob, Grep
---

# Azure Architect

Turn a description of an Azure setup into (1) a visual architecture diagram using real Azure
resource icons and (2) matching Terraform code using the `azurerm` provider.

See [reference.md](reference.md) for the Azure-service → `diagrams` Python class → Terraform
resource type mapping table. Read it before writing the diagram script — don't guess class names
from memory, the `diagrams` library renames/reorganizes classes across versions.

## Workflow

1. **Clarify scope.** If the user's description is ambiguous (missing networking tier, unclear
   whether HA/multi-region is wanted, no mention of auth/secrets handling), ask 1-3 short
   questions before generating anything. Don't over-ask if the request is already concrete.
   Also ask which output format they want delivered — **PNG**, **SVG**, or **both** — unless
   they've already said so (e.g. "for the wiki" implies SVG; "just a quick look" implies PNG).
   Default to offering both if they don't have a preference; don't silently pick one for them.

2. **Check tooling.** Verify the `diagrams` package and Graphviz are available:
   ```
   python3 -c "import diagrams" || pip3 install diagrams
   which dot || echo "Graphviz missing — install via 'brew install graphviz' (mac) or the user's package manager"
   ```
   If Graphviz is missing, tell the user and ask them to install it — don't silently skip the
   diagram step.

3. **List real class names before using them — in one batched call, not one per category.** By
   this point you already know every Azure category the diagram needs (compute, network,
   database, etc.), so check them all in a single script/call instead of spawning a separate
   `python3 -c "..."` subprocess per category — each Bash call costs a full agentic round-trip
   regardless of how fast the command itself runs, so 5 categories checked one at a time pays that
   cost 5× for no benefit over checking them all at once:
   ```python
   import importlib
   for cat in ["compute", "network", "database", "security", "web"]:  # only the categories this diagram needs
       m = importlib.import_module(f"diagrams.azure.{cat}")
       print(cat, [n for n in dir(m) if n[0].isupper()])
   ```
   Cross-check against [reference.md](reference.md), but trust the live `dir()` output over the
   table if they disagree — the installed version wins.

4. **Write and run a diagram script.** Create a short Python script (e.g. `architecture_diagram.py`
   in a scratch location, not the user's repo, unless they ask for it to be committed — default to
   local-only output) using `diagrams.azure.*` nodes, grouped into `Cluster`s for logical
   boundaries (e.g. a VNet cluster containing subnet-scoped resources). Follow the "Diagram
   construction notes" in [reference.md](reference.md) for label formatting and per-node boxing —
   they capture specific rendering bugs (cluster-name collisions, auto-cycled background colors,
   ragged multi-line text, labels spilling past neighboring nodes) that are easy to reintroduce.
   Use `Edge` to show real traffic/data flow direction, not just proximity. If the diagram uses
   more than one edge style (dashed, bold, plain, etc.), give each style its own `color=` too
   (redundant encoding of the same meaning) and add a small legend as a single HTML-like-label
   node (`diag.node("legend", ...)`) — see the edge-style-and-color convention and the legend
   pattern in reference.md. Do not build the legend out of tiny nodes/edges — that looked
   reasonable but Graphviz's rank/label spacing rules blew it up to a third of the canvas; a
   single node's own table stays small. Reuse the same style/color-to-meaning mapping across
   diagrams rather than inventing a new one each
   time. Render with `outformat=["png", "svg"]` regardless of which format the user asked for —
   rendering both is one call and effectively free, and you need the PNG yourself either way to
   inspect the result (read it back with the Read tool before showing the user, and iterate on
   layout/spacing issues rather than shipping the first render — SVGs aren't viewable through the
   Read tool's image support, PNG is your only way to actually see what you built). Then only
   hand over / mention the format(s) the user actually chose in step 1. If SVG is one of them, it
   needs one extra step to actually be portable — see the inlining note in reference.md — a raw
   `diagrams` SVG embeds icons by absolute local file path, which breaks the moment it leaves this
   machine.

5. **Generate the Terraform.** Using the same resource list as the diagram, write `azurerm`
   Terraform matching the project's existing module conventions if inside a Terraform repo
   (check for an existing `modules/` structure, variable/output naming, provider version
   constraints, and tagging conventions before inventing new patterns). If this isn't inside an
   existing Terraform project, use a conventional `main.tf` / `variables.tf` / `outputs.tf` /
   `versions.tf` layout. Keep resource names/tags consistent between the diagram labels and the
   Terraform resource names so the two stay easy to cross-reference.

6. **Validate.** Run `terraform fmt` and `terraform validate` (init a throwaway backend if needed)
   on the generated code. Report any errors and fix them rather than leaving broken HCL.

7. **Summarize.** Tell the user where the diagram image and Terraform files landed, and call out
   any assumptions you made (region, SKU sizes, HA/redundancy choices) so they can correct them.

## Notes

- Don't bundle or hardcode a fixed list of Azure services — the `diagrams` package ships the
  actual icon set, so always derive available resources from the installed library rather than
  a memorized list.
- Prefer `azurerm_resource_group` + explicit `location`/`tags` variables over hardcoded values,
  matching typical IaC repo conventions.
- If the user's repo already has Terraform modules for a resource type (search before writing
  new HCL), reuse/call that module instead of writing raw resource blocks.
- Default the diagram PNG to a local-only location (e.g. the scratch directory, or the user's
  Desktop if they want it kept around) rather than the project repo or a `thoughts/`-style shared
  directory — ask first if it's unclear whether the user wants it committed/shared.
