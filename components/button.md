---
title: "Button"
description: "Blueprint's interactive action control."
---

<Note>
  This is a **worked example** of the usage-first structure. The usage guidance below is illustrative, inferred from the spec to show the shape. In the real pipeline it should be sourced from `/describe-component` (declared intent), not hand-authored here. The developer spec in the tabs is the existing `/create-component-md` output, tucked behind progressive disclosure rather than leading the page.
</Note>

Button triggers an action. It carries a text label and up to two optional icons, and comes in four emphasis levels and three colour treatments so a single control covers everything from a page's primary call to action down to a near-link inline action.

<CardGroup cols={2}>
  <Card title="View in Storybook" icon="play" href="https://YOUR-STORYBOOK-URL/?path=/story/components-button">
    Interactive component, all variants and states.
  </Card>

  <Card title="Open in Figma" icon="figma" href="https://www.figma.com/design/QJ0f6PWjR3ERLtDVz652b5/?node-id=3062-395">
    Source component set.
  </Card>
</CardGroup>

## When to use it

Use a button when the user takes an action: submit, save, continue, download. For navigation between pages or to external destinations, use a link, not a button styled to look like one.

## Choosing an appearance

Appearance sets visual emphasis. Match it to the action's importance in the view, not to how the button looks on its own.

| Appearance | Use it for | Guidance |
| --- | --- | --- |
| **Primary** | The single most important action in a view | Aim for one per view. Two Primary buttons compete and dilute the hierarchy. |
| **Secondary** | An alternative or supporting action shown alongside a Primary | Outlined, lower emphasis. Common as the "cancel" or "back" beside a Primary. |
| **Tertiary** | Low-emphasis actions where a full button would be too heavy | Reads as a text/link-style button. Good inline or in dense layouts. |
| **Subdued** | The lowest-emphasis action, close to a plain link | Use where even Tertiary draws too much attention. |

## Choosing a colour

Colour adapts the button to its surface. It does not change emphasis or dimensions.

| Colour | Use it on |
| --- | --- |
| **Default** | Standard light surfaces |
| **Inverse** | Dark or coloured surfaces, where Default would not have enough contrast |
| **Accent** | Brand-accent moments that warrant the yellow emphasis |

## Content

Label the action, not the appearance. "Submit", "Download report", "Save changes" — never "Click here" or "Blue button". Keep labels short and lead with a verb.

Icons are optional and decorative. A leading or trailing icon can reinforce the label's meaning, but the label must stand on its own: the button has to make sense with the icon removed.

## Accessibility essentials

The detail lives in the developer spec below; these are the points a designer needs to get right.

<AccordionGroup>
  <Accordion title="The button is a single focus stop" icon="square-check">
    The visible label becomes the accessible name. Don't add a separate label element or repeat the text in an `aria-label` — that double-announces.
  </Accordion>

  <Accordion title="Icons are hidden from assistive tech" icon="eye-slash">
    Leading and trailing icons are decorative and must be hidden from screen readers. They duplicate the label's meaning; announcing them adds noise, not information.
  </Accordion>

  <Accordion title="Disabled removes the button from focus order" icon="ban">
    A disabled button is not focusable and cannot be activated. Don't rely on it as the only signal that an action is unavailable — pair it with context so the reason is clear.
  </Accordion>

  <Accordion title="Label the action, not the state" icon="tag">
    Don't bake "disabled" or "dimmed" into the label. The platform announces state via traits/role; the label stays the action.
  </Accordion>
</AccordionGroup>

---

## Developer spec

<Note>
  Everything below is the token-accurate implementation detail, sourced from `/create-component-md`. It's reference, not reading — tucked into tabs so it doesn't bury the usage guidance above.
</Note>

<Tabs>
  <Tab title="API">
    > State axis is decomposed: only the persistent Disabled state is exposed as `isDisabled`; Hover, Press, and Focus are transient interaction states handled at runtime and are not API properties. Leading and trailing icons are independent, optional slots, each gated by a `has*` boolean with its glyph chosen via an instance-swap of an Icon component.

    ### Properties

    | Property | Type | Values | Default | Notes |
    | --- | --- | --- | --- | --- |
    | appearance | enum | Primary \| Secondary \| Tertiary \| Subdued | Primary | Visual hierarchy / emphasis. Figma axis name: Appearance. |
    | color | enum | Default \| Inverse \| Accent | Default | Color treatment. Inverse for dark or colored surfaces; Accent for brand-accent emphasis. |
    | isDisabled | boolean | true \| false | false | Persistent disabled state. Decomposed from the Figma State axis; Hover/Press/Focus are transient and not exposed. |
    | label | string | (string) | - | Button text. Mirrored from the Label editable text property (default "Label"). |
    | hasLeadingIcon | boolean | true \| false | false | Shows a leading icon before the label. |
    | leadingIcon | instance-swap | (instance) | arrowRotateLeft | Only meaningful when hasLeadingIcon = true. Instance-swap of an Icon component. |
    | hasTrailingIcon | boolean | true \| false | false | Shows a trailing icon after the label. |
    | trailingIcon | instance-swap | (instance) | download | Only meaningful when hasTrailingIcon = true. Instance-swap of an Icon component. |

    <Accordion title="Icon slots (leading / trailing)">
      Both are instances of Icon, gated by their `has*` boolean and sized to the label (16, not independently configurable). Glyph selected via the corresponding instance-swap. See the Icon spec for internals.
    </Accordion>

    <Accordion title="Worked examples">
      - **Primary** — `appearance: primary`, `label: "Get started"`
      - **Secondary \+ leading icon** — `appearance: secondary`, `hasLeadingIcon: true`, `leadingIcon: arrowRotateLeft`, `label: "Refresh"`
      - **Tertiary \+ trailing icon** — `appearance: tertiary`, `hasTrailingIcon: true`, `trailingIcon: download`, `label: "Download"`
      - **Disabled** — `isDisabled: true`, `label: "Unavailable"`
    </Accordion>
  </Tab>
  <Tab title="Structure">
    > Button has no size axis; dimensional differences are carried by the Appearance axis. Color is visual-only. State is dimensionally constant except for two root-border changes: the Secondary outline thickens on Hover (4) and Press (5), and Tertiary / Subdued gain a 2px container-stroke focus ring on Focus.

    ### Dimensions by appearance

    | Spec | Primary | Secondary | Tertiary | Subdued | Notes |
    | --- | --- | --- | --- | --- | --- |
    | minHeight | 44 | 44 | 44 | 44 | Constant 44 tap-target height — meets WCAG 2.5.8 minimum |
    | minWidth | 88 | 88 | – | – | Primary/Secondary enforce an 88 floor; Tertiary/Subdued hug their label |
    | horizontalPadding | spacing/fluid/ml (32) | spacing/fluid/ml (32) | 0 | 0 | Zero on Tertiary/Subdued (text/link style) |
    | verticalPadding | spacing/fluid/xxxs (8) | spacing/fluid/xxxs (8) | spacing/fixed/xxxs (4) | spacing/fixed/xxxs (4) | Reduced to 4 on Tertiary/Subdued |
    | itemSpacing | spacing/fluid/xxxs (8) | spacing/fluid/xxxs (8) | spacing/fluid/xxxs (8) | spacing/fluid/xxxs (8) | Gap between icon, label, icon |
    | cornerRadius | 0 | 0 | 0 | 0 | Square corners throughout |
    | borderWidth | – | 2 | – | – | Only Secondary paints a persistent root border (thickens to 4 Hover / 5 Press). Not bound to a width token. |

    <Accordion title="State deltas & focus">
      Secondary root border strokeWeight: 2 rest → 4 hover → 5 press → 2 focus/disabled. Tertiary / Subdued gain a 2px inside container-stroke focus ring on Focus only; Primary / Secondary focus rings are a separate Focus indicator element (see Color). Content frame: itemSpacing 8, paddingTop 1 (optical), hug both axes. Label textStyle: Body/M Bold (Gotham Bold 16 / line-height 26).
    </Accordion>

    <Warning>
      Open issue from extraction: Secondary's border weights aren't bound to a width token. Consider binding them.
    </Warning>
  </Tab>
  <Tab title="Color">
    > Color and Appearance are traditional Figma variant axes, not a variable mode collection. Primary renders hover/pressed feedback via `opacity/*` state-overlay primitives. Tertiary and Subdued use a bottom-border underline; the keyboard focus ring is a container stroke (Tertiary/Subdued) or a dedicated Focus indicator (Primary/Secondary).

    ### Primary / Default

    | Element | rest | hover | press | focus | disabled |
    | --- | --- | --- | --- | --- | --- |
    | Container fill | button/primary/surface (#25273A) | opacity/white-20 | opacity/white-40 | button/primary/surface | surface/disabled (#CACACA) |
    | Focus indicator | none | none | none | button/primary/surface | none |
    | Label | text/inverse (#FFFFFF) | text/inverse | text/inverse | text/inverse | text/subtler (#666666) |
    | Icons | icon/inverse (#FFFFFF) | icon/inverse | icon/inverse | icon/inverse | icon/subtler (#666666) |

    <Note>
      For the demo I've shown Primary / Default only. In the real page all twelve Appearance × Color combinations render here in full (Primary/Secondary/Tertiary/Subdued × Default/Inverse/Accent), each as its own table.
    </Note>
  </Tab>
  <Tab title="Voice / Screen reader">
    > Button is a single focus stop. The visible Label merges into the accessible name and is never a separate element. Icons are decorative and hidden. The State axis decomposes to one persistent API state, `isDisabled`. Disabled removes the button from the focus order entirely. Web: prefer native `<button>` over `role='button'` so Enter/Space activation and disabled handling come for free.

    ### Rest (enabled) — ARIA

    | Property | Value | Notes |
    | --- | --- | --- |
    | element | `<button>` | Native element; role and keyboard activation implicit. |
    | textContent | "Label" | Supplies the accessible name; no `aria-label` needed when text is present. |
    | Do NOT | repeat text in aria-label | Overrides the visible name and double-announces. |
    | Do NOT | announce icons | Set `aria-hidden="true"` on the icon SVG. |

    ### Disabled — ARIA

    | Property | Value | Notes |
    | --- | --- | --- |
    | element | `<button disabled>` | Native disabled removes it from tab order and prevents activation. |
    | aria-disabled | true | Only if the design must keep it focusable for discoverability; native `disabled` is preferred. |

    <Accordion title="iOS (VoiceOver) & Android (TalkBack)">
      **VoiceOver** — rest: "Label, button" via `.isButton`; disabled: "Label, dimmed, button" via `.notEnabled`. Don't bake "dimmed" into the label. **TalkBack** — rest: merge the label via `mergeDescendants`, role `Role.Button`; disabled: apply `disabled()`, remove `onClick`. In both, hide the decorative icon (`accessibilityElementsHidden` / `importantForAccessibility = no`).
    </Accordion>
  </Tab>
</Tabs>

---

<Card title="Live component" icon="play" href="https://YOUR-STORYBOOK-URL/?path=/story/components-button">
  Replace the URL with the Storybook story. If the URLs are stable we can embed the iframe inline here instead of linking out.
</Card>