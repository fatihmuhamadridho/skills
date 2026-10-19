# Component Level Examples

Use this reference when you need concrete examples from this repo to decide whether a component belongs in `atoms`, `molecules`, `organisms`, `templates`, or `pages`.

## Atoms

Atoms are the smallest reusable UI building blocks. They expose styling and small behavior, but they should not own feature workflows or domain logic.

- `Box`
  - Path: `src/commons-ui/atoms/Box/index.tsx`
  - Why: layout primitive with spacing and background props only
- `Flex`
  - Path: `src/commons-ui/atoms/Flex/index.tsx`
  - Why: low-level layout wrapper
- `Image`
  - Path: `src/commons-ui/atoms/Image/index.tsx`
  - Why: reusable media primitive
- `Modal`
  - Path: `src/commons-ui/atoms/Modal/index.tsx`
  - Why: primitive dialog shell, still reusable across many features
- `Select`
  - Path: `src/commons-ui/atoms/Select/index.tsx`
  - Why: single control input, even though it has internal UI behavior

## Molecules

Molecules combine atoms into a small reusable interaction unit. They can own limited UI behavior, but not screen-level orchestration.

- `TextSection`
  - Path: `src/commons-ui/molecules/TextSection/index.tsx`
  - Why: combines title, subtitle, and styling into one small reusable block
- `Loader`
  - Path: `src/commons-ui/molecules/Loader/index.tsx`
  - Why: composed loading feedback unit reused in multiple places
- `EditableTitle`
  - Path: `src/commons-ui/molecules/EditableTitle/index.tsx`
  - Why: focused inline-edit interaction, still smaller than a section-level component
- `UploadingField`
  - Path: `src/commons-ui/molecules/UploadingField/index.tsx`
  - Why: richer than an atom, but still a single focused UI control

## Organisms

Organisms are larger composed sections. They usually assemble atoms and molecules into a feature-ready block or a complex shared form element.

- `Combobox`
  - Path: `src/commons-ui/organisms/Combobox/index.tsx`
  - Why: complex composed control with search, dropdown, and portal behavior
- `AuthFlowForm`
  - Path: `src/commons-ui/organisms/AuthFlowForm/index.tsx`
  - Why: a full auth form section made from smaller building blocks
- `ModalConfirmation`
  - Path: `src/commons-ui/organisms/Modals/ModalConfirmation/index.tsx`
  - Why: reusable confirmation flow, larger than a primitive modal shell
- `LoginBox`
  - Path: `src/features/Login/presentation/organisms/LoginBox/index.tsx`
  - Why: feature-specific composed section that should stay inside the feature
- `CampaignHeader`
  - Path: `src/features/Campaign/presentation/organisms/CampaignHeader/index.tsx`
  - Why: screen section tied to campaign-specific context

## Templates

Templates assemble organisms into a page layout or screen structure. They can own orchestration-level UI concerns and data wiring, but they are still one level below the route page.

- `Layout`
  - Path: `src/commons-ui/templates/Layout/index.tsx`
  - Why: shared app shell with sidebar, header, auth data, and layout orchestration
- `ErrorTemplate`
  - Path: `src/commons-ui/templates/ErrorTemplate/ErrorTemplate.tsx`
  - Why: shared screen structure for error states
- `MobileLayout`
  - Path: `src/commons-ui/templates/MobileLayout/index.tsx`
  - Why: shared mobile page structure
- `Campaign.template`
  - Path: `src/features/Campaign/presentation/templates/Campaign.template.tsx`
  - Why: feature screen composition and orchestration for the campaign list flow
- `MobileDeviceForm.template`
  - Path: `src/features/MobileDevice/presentation/templates/MobileDeviceForm.template.tsx`
  - Why: feature form layout and binding surface
- `Gallery.template`
  - Path: `src/features/Gallery/presentation/templates/Gallery.template.tsx`
  - Why: feature-level screen structure and UI orchestration

## Pages

Pages are route entrypoints. They should stay as thin as possible and delegate most work to templates, hooks, or feature logic.

- `Login.page`
  - Path: `src/features/Login/presentation/pages/Login.page.tsx`
  - Why: route-facing entrypoint for the login screen
- `Campaign.page`
  - Path: `src/features/Campaign/presentation/pages/Campaign.page.tsx`
  - Why: route-level feature entrypoint that delegates to feature UI structure
- `DashboardV2.page`
  - Path: `src/features/DashboardV2/presentation/pages/DashboardV2.page.tsx`
  - Why: page-level entrypoint for a full feature screen

## Quick Heuristics

- If it is a primitive reused everywhere, it is probably an atom.
- If it is one focused control or small content block built from atoms, it is probably a molecule.
- If it is a larger section or complex shared block, it is probably an organism.
- If it lays out a whole screen or coordinates major screen sections, it is probably a template.
- If it is the route entrypoint, it is a page.
