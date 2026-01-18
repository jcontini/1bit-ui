# 1bit UI Roadmap

This roadmap tracks components needed for a full application migration to the 1-bit aesthetic.

## Current Status

### ✅ Done (15 components)

**Form Controls:**
- [x] Button
- [x] Input (box + underline variants)
- [x] Checkbox
- [x] Radio
- [x] Toggle (segmented on/off)
- [x] Dropdown (select with keyboard nav)
- [x] Slider

**Layout:**
- [x] Panel (with title bar)
- [x] Modal
- [x] Tabs (horizontal strip)
- [x] TabPanel (tabs + content)
- [x] List (selectable items)
- [x] TitleBar

**Feedback:**
- [x] ProgressBar

**Device:**
- [x] Screen (PDA device frame)

---

## 🚧 Needed for Full App Migration

These components are needed to migrate a full application (like AgentOS) to 1-bit UI.

### High Priority

| Component | Description | Complexity |
|-----------|-------------|------------|
| `Textarea` | Multi-line text input | Low |
| `IconButton` | Button with icon only | Low |
| `Tooltip` | Hover tooltip | Medium |
| `Toast` | Notification message | Medium |
| `Skeleton` | Loading placeholder | Low |
| `Separator` | Horizontal/vertical line | Trivial |

### Medium Priority

| Component | Description | Complexity |
|-----------|-------------|------------|
| `Card` | Content card with sections | Low |
| `Badge` | Status/count indicator | Low |
| `Avatar` | User/app icon wrapper | Low |
| `Collapsible` | Expandable section | Medium |
| `ContextMenu` | Right-click menu | Medium |
| `Popover` | Positioned floating content | Medium |

### Layout Components

| Component | Description | Complexity |
|-----------|-------------|------------|
| `Sidebar` | Navigation sidebar | Medium |
| `TopBar` | Application header | Medium |
| `Footer` | Application footer | Low |
| `SplitPane` | Resizable split view | High |

### Data Display

| Component | Description | Complexity |
|-----------|-------------|------------|
| `Table` | Data table | Medium |
| `Tree` | Hierarchical tree view | High |
| `Accordion` | Collapsible sections | Medium |

---

## Integration with AgentOS

This library was extracted from [AgentOS](https://github.com/jcontini/agentos) to enable:

1. **Reusable components** — Use across multiple projects
2. **Open source contribution** — Community can add components
3. **Framework flexibility** — Potential React port in future

### Migration Reference

See `.specs/ui/pda-full-migration.md` in AgentOS for:
- Component mapping (shadcn → 1bit)
- Icon migration (material-symbols → pixelarticons)
- View-by-view migration plan
- Design decisions

---

## Contributing

To add a new component:

1. Create `src/lib/components/ComponentName.svelte`
2. Use `bit-` prefix for CSS classes
3. Use `--1bit-` CSS variables for theming
4. Export from `src/lib/index.ts`
5. Add demo to `src/routes/+page.svelte`
6. Update this roadmap

### Design Guidelines

- **2 colors only** — Use `--1bit-fg` and `--1bit-bg`
- **No gradients** — Use dither patterns instead
- **No shadows** — Use borders for depth
- **Pixel-perfect** — Use integer values, avoid anti-aliasing
- **Invert on interaction** — Hover/active states swap fg/bg
