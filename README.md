# wiki cheatsheets


1. [Terraform](terraform/terraform.md)
2. [Aws](cloud/aws.md)
4. [Ansible](ansible/ansible.md)
5. [RedHat9](linux/RedHat9.md)
6. [SSL](linux/SSL.md)
7. [PasswdPolicies](linux/passwd.md)
8. [Vagrant Boxes](tools/vagrant-box.md)
9. [Oracle](oracle/oracle.md)
10. [Azure](cloud/azure/azure.md)
12. [FinOPs](finops/finops.md)





## Más temas del vault

- [Conceptos de IA, modelos y agentes](ai/ai-definitions.md)
- [OpenCode — Plantillas y guía de uso](ai/OpenCode/README.md)
- [OpenCode — Chuleta para macOS](ai/OpenCode/OpenCode-CheatSheet.md)
- [OpenCode — Plantilla AGENTS.md](ai/OpenCode/templates/AGENTS.md)
- [OpenCode — Plantilla MEMORY.md](ai/OpenCode/templates/MEMORY.md)
- [OpenCode — Plantilla de Skill](ai/OpenCode/templates/skills/SKILL.md)
- [IA responsable (AI-900)](ai901/azure-concepts.md)
- [macOS — entorno con Multipass](mac/multipass.md)
- [Zabbix](zabbix/readme.md)
- [Opciones financieras](finance/options.md)
- [Git](tools/git.md)

## MCP (Model Context Protocol)

MCP permite conectar OpenCode con servidores que ofrecen herramientas y datos externos, como documentación, navegador o servicios. Se configuran en `opencode.json`/`opencode.jsonc` y se gestionan desde OpenCode con `/mcps`. Consulta la [guía de OpenCode, con detalles de MCP y ejemplos de configuración](ai/OpenCode/README.md#mcp-model-context-protocol).

## Estructura de un proyecto OpenCode con SDD

Al usar agentes, skills, comandos y SDD, el proyecto puede tener esta estructura. Crea solo las carpetas que necesite:

```text
project/
├── AGENTS.md
├── MEMORY.md                         # opcional: contexto entre sesiones
├── opencode.json                     # configuración MCP (o opencode.jsonc)
├── docs/
│   └── constitution.md               # principios del proyecto
├── specs/
│   └── 001-nombre/
│       ├── spec.md
│       ├── plan.md
│       └── tasks.md
├── tests/
├── .agents/
│   └── skills/
│       └── sdd/
│           └── SKILL.md
├── .opencode/
│   ├── agents/
│   │   ├── coordinator.md
│   │   ├── planner.md
│   │   ├── implementer.md
│   │   └── reviewer.md
│   └── commands/
│       ├── sdd-constitution.md
│       └── sdd-*.md
└── <código del proyecto>
```

La guía de [plantillas y uso de OpenCode](ai/OpenCode/README.md) explica cómo instalar y adaptar estos archivos.

## Cheat Sheets

- [Ansible Cheat Sheet](cheat-sheets/Ansible_Cheat_Sheet.pdf)
- [Kubernetes Cheat Sheet](cheat-sheets/Kubernetes_Cheat_Sheet.pdf)
