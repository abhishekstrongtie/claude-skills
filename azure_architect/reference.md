# Azure service → `diagrams` class → Terraform resource

Verified against `diagrams` 0.25.1 (`diagrams.azure.*`), installed globally via `pip3 install
diagrams` (see SKILL.md step 2). Class names shift between library versions — before relying on a
name below, confirm it still exists:

```
python3 -c "import diagrams.azure.<category> as m; print([n for n in dir(m) if n[0].isupper()])"
```

Import pattern: `from diagrams.azure.<category> import <ClassName>`.

## compute
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| Virtual machine | `compute.VM` | `azurerm_linux_virtual_machine` / `azurerm_windows_virtual_machine` |
| VM scale set | `compute.VMScaleSet` | `azurerm_linux_virtual_machine_scale_set` / `azurerm_windows_virtual_machine_scale_set` |
| AKS | `compute.KubernetesServices` (alias `compute.AKS`) | `azurerm_kubernetes_cluster` |
| Container Apps | `compute.ContainerApps` | `azurerm_container_app` |
| Container Instances | `compute.ContainerInstances` | `azurerm_container_group` |
| Container Registry | `compute.ContainerRegistries` (alias `compute.ACR`) | `azurerm_container_registry` |
| Function App | `compute.FunctionApps` | `azurerm_linux_function_app` / `azurerm_windows_function_app` |
| App Service (compute view) | `compute.AppServices` | `azurerm_linux_web_app` / `azurerm_windows_web_app` |
| Disks | `compute.Disks` | `azurerm_managed_disk` |
| Batch Accounts | `compute.BatchAccounts` | `azurerm_batch_account` |

## web
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| App Service Plan | `web.AppServicePlans` | `azurerm_service_plan` |
| App Service | `web.AppServices` | `azurerm_linux_web_app` / `azurerm_windows_web_app` |
| Static Web App | `web.StaticApps` | `azurerm_static_web_app` |
| API Management | `web.APIManagementServices` | `azurerm_api_management` |
| App Service Environment | `web.AppServiceEnvironments` | `azurerm_app_service_environment_v3` |

## network
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| Virtual Network | `network.VirtualNetworks` | `azurerm_virtual_network` |
| Subnet | `network.Subnets` | `azurerm_subnet` |
| Network Security Group | (see `security` category if missing here) | `azurerm_network_security_group` |
| Application Gateway | `network.ApplicationGateway` | `azurerm_application_gateway` |
| Load Balancer | `network.LoadBalancers` | `azurerm_lb` |
| Firewall | `network.Firewall` | `azurerm_firewall` |
| Public IP | `network.PublicIpAddresses` | `azurerm_public_ip` |
| Private Endpoint | `network.PrivateEndpoint` | `azurerm_private_endpoint` |
| Front Door | `network.FrontDoors` | `azurerm_cdn_frontdoor_profile` |
| DNS Zone | `network.DNSZones` | `azurerm_dns_zone` |
| Private DNS Zone | `network.DNSPrivateZones` | `azurerm_private_dns_zone` |
| Route Table | `network.RouteTables` | `azurerm_route_table` |
| VPN Gateway | `network.VirtualNetworkGateways` | `azurerm_virtual_network_gateway` |
| ExpressRoute Circuit | `network.ExpressrouteCircuits` | `azurerm_express_route_circuit` |
| Traffic Manager | `network.TrafficManagerProfiles` | `azurerm_traffic_manager_profile` |
| CDN Profile | `network.CDNProfiles` | `azurerm_cdn_profile` |
| Network Interface | `network.NetworkInterfaces` | `azurerm_network_interface` |
| Virtual WAN | `network.VirtualWans` | `azurerm_virtual_wan` |

## database / databases
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| Azure SQL Database | `database.SQLDatabases` | `azurerm_mssql_database` |
| SQL Managed Instance | `database.SQLManagedInstances` | `azurerm_mssql_managed_instance` |
| Cosmos DB | `database.CosmosDb` | `azurerm_cosmosdb_account` |
| PostgreSQL Flexible Server | `database.DatabaseForPostgresqlServers` | `azurerm_postgresql_flexible_server` |
| MySQL Server | `database.DatabaseForMysqlServers` | `azurerm_mysql_flexible_server` |
| MariaDB Server | `database.DatabaseForMariadbServers` | `azurerm_mariadb_server` |
| Cache for Redis | `database.CacheForRedis` | `azurerm_redis_cache` |
| Synapse Analytics | `database.SynapseAnalytics` | `azurerm_synapse_workspace` |

## storage
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| Storage Account | `storage.StorageAccounts` | `azurerm_storage_account` |
| Blob Storage | `storage.BlobStorage` | `azurerm_storage_container` / `azurerm_storage_blob` |
| Data Lake Storage | `storage.DataLakeStorage` | `azurerm_storage_data_lake_gen2_filesystem` |
| Recovery Services Vault | `storage.RecoveryServicesVaults` | `azurerm_recovery_services_vault` |
| Azure NetApp Files | `storage.AzureNetappFiles` | `azurerm_netapp_account` |

## security / identity
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| Key Vault | `security.KeyVaults` | `azurerm_key_vault` |
| Microsoft Defender for Cloud | `security.MicrosoftDefenderForCloud` | `azurerm_security_center_subscription_pricing` |
| Azure Sentinel | `security.Sentinel` / `security.AzureSentinel` | `azurerm_sentinel_log_analytics_workspace_onboarding` |
| Managed Identity | `identity.ManagedIdentities` | `azurerm_user_assigned_identity` |
| Azure AD / Entra | `identity.ActiveDirectory` / `identity.AzureActiveDirectory` | `azuread_*` resources (separate `azuread` provider) |

## integration
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| Service Bus | `integration.ServiceBus` | `azurerm_servicebus_namespace` |
| Logic Apps | `integration.LogicApps` | `azurerm_logic_app_workflow` |
| API Management | `integration.APIManagement` / `integration.APIManagementServices` | `azurerm_api_management` |
| Event Grid Topics | `integration.EventGridTopics` | `azurerm_eventgrid_topic` |
| App Configuration | `integration.AppConfiguration` | `azurerm_app_configuration` |

## analytics
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| Event Hubs | `analytics.EventHubs` | `azurerm_eventhub` |
| Log Analytics Workspace | `analytics.LogAnalyticsWorkspaces` | `azurerm_log_analytics_workspace` |
| Databricks | `analytics.AzureDatabricks` / `analytics.Databricks` | `azurerm_databricks_workspace` |
| Stream Analytics | `analytics.StreamAnalyticsJobs` | `azurerm_stream_analytics_job` |
| Data Factory | `analytics.DataFactories` | `azurerm_data_factory` |

## devops
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| Application Insights | `devops.ApplicationInsights` | `azurerm_application_insights` |

## general
| Service | `diagrams` class | Terraform resource |
|---|---|---|
| Resource Group | `general.ResourceGroups` | `azurerm_resource_group` |

## Diagram construction notes

**Start from this complete, runnable script and adapt it** — replace the placeholder
`AppServices`/`SQLDatabases`/`KeyVaults` section with the real architecture's resources and
edges, but keep everything else (the color constants, `boxed()`, `legend_label()`,
`inline_svg_images()`, sizing attrs) as-is. It already incorporates every gotcha explained in the
bullets below, so building from it instead of writing a script from scratch avoids reintroducing
bugs that were already found and fixed once (cluster-name collisions, auto-cycled background
colors, an oversized legend, non-portable SVGs). This is the single source of truth for the
pattern — if you find a new gotcha, fix it here, not in a separate template file, so there's only
one place for the pattern to drift out of date.

```python
from diagrams import Cluster, Diagram, Edge
from diagrams.azure.general import ResourceGroups
from diagrams.azure.web import AppServices          # placeholder — replace with real resources
from diagrams.azure.database import SQLDatabases    # placeholder
from diagrams.azure.security import KeyVaults       # placeholder
import base64
import re

# Edge-style convention: style AND color redundantly encode the same 4 meanings.
FLOW_COLOR = "gray35"
AUTH_COLOR = "darkorange3"
WRITEBACK_COLOR = "firebrick3"
ASSOC_COLOR = "steelblue3"

graph_attr = {
    "fontsize": "22", "bgcolor": "white", "pad": "1.0",
    "nodesep": "1.3", "ranksep": "1.3", "margin": "0.4",
}
node_attr = {"fontsize": "13"}

# Every resource gets its own titled, dashed box sized to fit its own label.
box_attr = {"style": "dashed", "color": "gray45", "bgcolor": "none", "margin": "20", "fontsize": "12"}


def boxed(node_cls, title, lines):
    label = "".join(f" {line}\\l" for line in lines)
    with Cluster(title, graph_attr=box_attr):
        return node_cls(label)


LEGEND_SWATCH = {"solid": "────", "dashed": "╌╌╌╌╌", "bold": "━━━━", "plain": "────"}
# Only list the rows for styles the diagram actually uses below (an unused legend row is as
# confusing as a missing one) — the full 4-way convention/definitions are in the table above,
# add "bold"/"plain" rows here too if the real architecture needs a write-back or association edge.
LEGEND_ROWS = [
    ("solid", FLOW_COLOR, "data / event flow"),
    ("dashed", AUTH_COLOR, "auth / permission (control-plane)"),
]


def legend_label():
    rows = "".join(
        f'<TR><TD ALIGN="LEFT"><FONT COLOR="{color}" POINT-SIZE="12">{LEGEND_SWATCH[kind]}</FONT></TD>'
        f'<TD ALIGN="LEFT"><FONT POINT-SIZE="9">{text}</FONT></TD></TR>'
        for kind, color, text in LEGEND_ROWS
    )
    return (
        '<<TABLE BORDER="1" CELLBORDER="0" CELLSPACING="2" CELLPADDING="2" COLOR="gray60">'
        '<TR><TD COLSPAN="2" ALIGN="LEFT"><FONT POINT-SIZE="10"><B>Legend</B></FONT></TD></TR>'
        f"{rows}"
        "</TABLE>>"
    )


def inline_svg_images(svg_path):
    """Replace local-file-path icon refs with base64 data URIs so the SVG is portable."""
    with open(svg_path, "r", encoding="utf-8") as f:
        svg = f.read()

    def replace(match):
        path = match.group(1)
        if path.startswith("data:"):
            return match.group(0)
        with open(path, "rb") as img:
            encoded = base64.b64encode(img.read()).decode("ascii")
        return f'xlink:href="data:image/png;base64,{encoded}"'

    svg, _ = re.subn(r'xlink:href="([^"]+)"', replace, svg)
    with open(svg_path, "w", encoding="utf-8") as f:
        f.write(svg)


with Diagram(
    "REPLACE ME: architecture title",
    filename="architecture",
    outformat=["png", "svg"],  # only render the format(s) the user asked for
    show=False,
    direction="LR",
    graph_attr=graph_attr,
    node_attr=node_attr,
) as diag:
    with Cluster("rg-{productName}-{env}"):
        rg = boxed(ResourceGroups, "Resource Group", ["rg-{productName}-{env}"])

        # --- replace below with the actual resources/edges for this architecture ---
        app = boxed(AppServices, "App Service", ["app-{productName}-{env}"])
        db = boxed(SQLDatabases, "SQL Database", ["sql-{productName}-{env}"])
        kv = boxed(KeyVaults, "Key Vault", ["kv-{productName}-{env}"])

        rg >> Edge(style="invis") >> app  # pins the RG icon's position, implies no data flow
        app >> Edge(label="query", color=FLOW_COLOR) >> db
        app >> Edge(label="get secret", style="dashed", color=AUTH_COLOR) >> kv
        # --- end replace ---

    # Drop this if the diagram only uses one edge style (nothing to explain in a key).
    diag.node("legend", legend_label(), shape="none")

# Only needed if SVG is one of the format(s) actually delivered to the user.
inline_svg_images("architecture.svg")
```

- Use `from diagrams import Cluster, Edge, Diagram` alongside the azure imports.
- Model VNets/subnets/resource-groups as `Cluster(...)` blocks containing the resources inside
  them. **But also add an actual `ResourceGroups` (or other) icon node when the user expects to
  *see* that resource, not just infer it from a box border** — a cluster's title text alone reads
  as a label, not as "this resource is present in the diagram." Default to both: a `Cluster` for
  containment, plus an icon node for anything the user would expect to click on in the real
  architecture.
- Use `Edge(label=...)` on connections that represent real traffic or data flow (e.g.
  `appgw >> Edge(label="HTTPS") >> aks`), not just adjacency. For a node that has no real
  connection but still needs a deliberate position in the layout (e.g. placing a Resource Group
  icon before the first resource it contains), use `a >> Edge(style="invis") >> b` rather than a
  visible edge that would wrongly imply data flow.
- **Use a consistent edge-style-and-color convention, and always add a `Legend` cluster once more
  than one style appears in a diagram** — a dashed or bold line means nothing to the viewer
  without a key. Style and color redundantly encode the *same* meaning (not two independent
  dimensions) so the relationship type still reads even if one signal is hard to spot. Default
  convention (reuse it across diagrams so it stays predictable):
  | Style | Color | Meaning |
  |---|---|---|
  | solid arrow (`>>`, no `style=`) | `gray35` | real data / event flow between resources |
  | dashed arrow (`style="dashed"`) | `darkorange3` | auth / permission / control-plane relationship, not data |
  | bold arrow (`style="bold"`) | `firebrick3` | write-back / state change to a resource shown elsewhere in the same diagram (flags a loop) |
  | plain line, no arrowhead (`-`) | `steelblue3` | structural association ("belongs to" / "is part of"), not a flow |

  The colors, `LEGEND_SWATCH`/`LEGEND_ROWS`, and `legend_label()` are all in the starter script
  above — define them once as constants/module-level code and pass both `style=` and `color=` to
  every `Edge`.

  **Build the legend as a single raw HTML-like-label node, not a cluster of tiny nodes/edges** (see
  `legend_label()` + `diag.node("legend", ...)` in the starter script). An earlier version built
  each legend row out of `diagrams.generic.blank.Blank` nodes connected by a styled `Edge` — this
  looked reasonable in code, but Graphviz reserves real rank/nodesep space *and* a synthetic
  label-sized node for every edge label, none of which is controlled by a node's own
  `height=`/`width=` override. The result was a "legend" that ballooned to a third of the whole
  canvas — the opposite of a small corner key. A single node with a Graphviz HTML-like label sizes
  itself tightly to its own content instead, with no edges or ranks involved at all. Also note:
  don't use `█` (U+2588 FULL BLOCK) for the "bold" swatch — it fills the entire glyph cell
  top-to-bottom and renders as a fat rectangle, not a thicker line; `━` (U+2501 HEAVY HORIZONTAL)
  is the box-drawing character that actually reads as a bold line.

  Notes:
  - The label string must literally start with `<` and end with `>` (double angle brackets:
    the outer pair is Graphviz's HTML-label marker, the inner content is the actual
    `<TABLE>...</TABLE>` markup) — the python `graphviz` package detects this via a
    `^<.*>$`-style check and passes it through unescaped instead of quoting it as plain text.
  - `shape="none"` avoids wrapping the table in an additional default node shape/border.
  - This node has no edges, so nothing constrains its position — Graphviz will place it wherever
    it fits. If it doesn't land in a sensible corner, pin it near a specific node with an
    `Edge(style="invis")` the same way as any other unconnected placement (see the Resource Group
    icon note above), or just accept wherever it lands since it's small enough not to intrude.
  - Only include the rows for styles actually used in that diagram — an unused legend row is as
    confusing as a missing one.
- **Render both PNG and SVG (`Diagram(..., outformat=["png", "svg"])`), and inline the SVG's icon
  images before handing it to the user.** `diagrams` writes each icon into the SVG as
  `<image xlink:href="/absolute/local/path/to/icon.png">`, pointing at wherever the package is
  installed on *this* machine. It renders fine here, but the moment the SVG is copied elsewhere
  (another machine, a wiki page, a PR comment) every icon shows as a broken image — the whole
  point of offering SVG (a portable, scalable copy) is defeated if you skip this step. Inline them
  as base64 data URIs instead — see `inline_svg_images()` in the starter script above, called
  after the `with Diagram(...)` block closes. Verify afterward with
  `grep -c 'xlink:href="/' architecture.svg` — it should print `0`.
- Categories with icons for a name may exist in more than one place (e.g. `AppServices` appears in
  both `compute` and `web`, `KeyVaults`/`ApplicationSecurityGroups` overlap `security`/`network`) —
  pick whichever import path the installed version exposes; both usually work.
- Use `{placeholder}` curly braces for template/variable values in labels (e.g.
  `kv-{productName}-{env}`), not `<placeholder>` angle brackets — angle brackets read as
  unmatched HTML/XML tags and are easy to confuse with Graphviz's own `<...>` HTML-label syntax.
- **Give every resource its own box so its label can never spill into a neighboring node or
  cluster.** Wrap each leaf node in a single-node `Cluster` styled as a plain border — see
  `box_attr`/`boxed()` in the starter script above. Two gotchas this avoids:
  1. **Every `Cluster` needs a distinct, non-empty `title`.** `diagrams.Cluster` names its
     subgraph `"cluster_" + label` internally — if you wrap several nodes in `Cluster("", ...)`
     with the same blank label, Graphviz silently *merges* every same-named subgraph into one,
     producing a single box that wrongly spans multiple unrelated nodes. Passing the resource's
     own name as the title fixes this for free (it also gives the box a readable header, so the
     node's own label only needs the detail lines, not a repeated title).
  2. **Set `bgcolor` explicitly on the wrapper.** `diagrams.Cluster` auto-assigns a background
     color cycling through 4 fixed colors based on nesting *depth* (`depth % 4`). Wrapping every
     leaf node in its own cluster adds a nesting level, which shifts every wrapper's computed
     depth and silently repaints them in colors that don't match the surrounding diagram. Passing
     `"bgcolor": "none"` (transparent, parent color shows through) or an explicit fixed color in
     `graph_attr` overrides the auto-cycle.
- **Multi-line labels: left-justify every detail line with `\l`, not `\n`.** A plain `\n` between
  lines centers each line independently, which looks ragged once one line (e.g. a long
  `topic_type=...` value) is much wider than the others. Terminate every detail line with the
  literal two-character escape `\l` (Python source: `"\\l"`, not the newline character `"\n"`) so
  all lines share the same flush-left edge:
  ```python
  " kv-{productName}-{env}\\l"
  " (a-tls-* certs)\\l"
  ```
- **If long lines still crowd neighboring nodes after boxing**, reduce `node_attr={"fontsize": "13"}`
  (or smaller) and increase the diagram's `graph_attr` `nodesep`/`ranksep` (e.g. `1.3`) — a wide
  label's bounding box extends symmetrically from the node's center, so it needs enough
  side-clearance from its neighbors regardless of justification.
