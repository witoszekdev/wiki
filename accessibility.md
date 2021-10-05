## Guides

[WAI-ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices/)
[Inclusive Components](https://inclusive-components.design/)
[The A11Y Project](https://www.a11yproject.com/)

## Buttons

### Disabled

> Whenever possible, don’t use disabled buttons. Let people click it at any time and, if necessary, show an error message as feedback.
> [Source](https://css-tricks.com/making-disabled-buttons-more-inclusive/)

## Tooltips

[Inclusive Components](https://inclusive-components.design/tooltips-toggletips/)
[Tooltips in the time of WCAG 2.1](https://sarahmhigley.com/writing/tooltips-in-wcag-21/)

## Aria-live

[Using aria-live](https://bitsofco.de/using-aria-live/)
[More Accessible Skeletons](https://adrianroselli.com/2020/11/more-accessible-skeletons.html)

## React

[Usefull resources](https://github.com/t12t/a11y-components)

### Libraries

Component libraries that include many usefull components without styling (menu / dropdown, listbox, modals, popvers, etc)

- [List of libraries - Digital a11y](https://www.digitala11y.com/accessible-ui-component-libraries-roundup)
- [Reakit](https://github.com/reakit/reakit)
- [Headless UI](https://headlessui.dev/)
- [React Spectrum](react-spectrum.adobe.com/react-aria) - A11Y hooks by Adobe
- [Downshift](https://www.downshift-js.com/) - Autocomplete and Select component by PayPal
- [Frend](https://frend.co/)
- [Lion](https://lion-web.netlify.app/)
- [accessible_componnets](https://scottaohara.github.io/accessible_components/)
- [Cody House Components](https://codyhouse.co/ds/components)
- [Auditum](https://github.com/oslabs-beta/aditum)
- [axe-core](https://github.com/dequelabs/axe-core) - Accessibility testing engine
  - [cypress-axe](https://www.npmjs.com/package/cypress-axe) - Testing accessibility with Cypress

### Utilities

#### Browser Extensions

- [Accessibility Insights](https://accessibilityinsights.io/en/downloads/)
- [Web Disability Simulator](https://chrome.google.com/webstore/detail/web-disability-simulator/olioanlbgbpmdlgjnnampnnlohigkjla?hl=en)
- [ChromeLens](https://chrome.google.com/webstore/detail/chromelens/idikgljglpfilbhaboonnpnnincjhjkd?hl=en#:~:text=ChromeLens%20is%20a%20Google%20Chrome,blind%20or%20a%20colorblind%20person.%20%2D)
- [Color Contract Analyzer](https://chrome.google.com/webstore/detail/color-contrast-analyzer/dagdlcijhfbmgkjokkjicnnfimlebcll?hl=en)
- [NoCoffe](https://addons.mozilla.org/en-US/firefox/addon/nocoffee/)

#### SaaS

- [Assistiv Labs](https://assistivlabs.com) - Browserstack for screen readers

#### Resources

- [HTML5 Doctor](http://html5doctor.com/)
- [WAI-ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices-1.1)
- [Inclusive Components](inclusive-components.design)

#### Figma plugins

- [Stark](https://www.figma.com/community/plugin/732603254453395948/Stark)
- [A11y Annotation Kit](https://www.figma.com/community/file/953682768192596304)

#### A11y testing groups

- [Access Works](https://access-works.com/)
- [Fable](https://makeitfable.com/)

## Screen readers

- NVDA
- JAWS
- VoiceOver (macOS only)

## Articles

- [How A Screen Reader User Accesses The Web](https://www.smashingmagazine.com/2019/02/accessibility-webinar)
- [No Mouse Days](https://www.a11yproject.com/posts/2020-10-15-no-mouse-days)
- [Screen reader modes](https://tink.uk/understanding-screen-reader-interaction-modes)
- [Prefers Reduced Motion - React](https://www.joshwcomeau.com/react/prefers-reduced-motion/)
- [Accessibility for Vestibular Disorders](https://alistapart.com/article/accessibility-for-vestibular/)
-

## Notes

### First rule of ARIA

Don't use ARIA

> If you can use a native HTML element or attribute with the semantics and behavior you require already built in, instead of repurposing an element and adding an ARIA role, state or property to make it accessible, then do so.

[From W3C Docs](https://www.w3.org/TR/using-aria#rule1)

It's unfortunately easy to use ARIA wrong and it can really screw up the accessibility of a page if used incorrectly.

### Keyboard interactions

Not every element needs to be keyboard-interactive. Screen readers have dedicated commands for headings and landmarks (among other things), so tabindex isn't necessary for those items.

Tabindex

- `0` - make focusable (order by DOM)
- `-1` - make unfocusable (still focusable by JS)
- `1+` - DO NOT USE (it will change the order)

There are some established conventions with keyboard access, such as binding the Escape key to close a dialog and return to a triggering button element. Before trying to invent patterns for keyboard access based on a new design that was passed to you, do some research on common conventions using the [W3C's ARIA Authoring Practices Guide](https://www.w3.org/TR/wai-aria-practices-1.1/?ck_subscriber_id=1450767998) and/or [Inclusive Components](https://inclusive-components.design/?ck_subscriber_id=1450767998) from Heydon Pickering

Accommodating users' needs with optional settings is one thing, but allowing enough time is another entirely. Don't assume all users will see something flashing across the screen before going away automatically, such as a Toast or other notification. Keep dialogs and other temporary layers on screen until the user has time to navigate to them with the keyboard and close by activating a button to confirm, close, or some other intentional action.

### Accessibility is impacted by UX and design

There are a couple of comparisons that could help to understand how design and accessibility relate to one another. First, accessibility is like blueberry muffins: you wouldn't make them by shoving the berries in after baking (paraphrased from Cordelia McGee-Tubb). You also wouldn't "take a condemned home and put new siding on it to successfully pass building code" (Shell Little).

The most inclusive websites and web apps consider accessibility as part of the design process (and earlier, such as in requirements gathering). This is often referred to as "shifting left", where the responsibility for accessibility isn't left to developers or QAs to identify and fix alone. Keeping this ideal process knowledge in mind can be helpful when working with your colleagues in different disciplines. As a developer, you shouldn't be on your own to implement accessibility–even though it happens quite often.

Design tips

- Anything a mouse user can do, a keyboard user should be able to accomplish as well (sometimes with an alternative function, or "affordance"). Keyboard support has the added benefit of supporting assistive technologies quite often, making it a base requirement for any interactivity.
- Visual treatments should have enough contrast and not rely on color alone. This applies to links and text over backgrounds, data visualizations and legends, and product color pickers in e-commerce. Use underlines, text labels, and patterns along with colors that meet WCAG contrast requirements.
- Designs could annotate accessibility information for non-visual mediums like screen readers, such as suggestions for semantics/structure and alternative text for images. This could note intent for markup like headings, links, or buttons; ARIA roles and widget patterns; focus/tab order; and responsive/flow details.
- Motion, animation, and autoplay can make people sick and should be employed responsibly. [Have conversations](https://www.tatianamac.com/posts/prefers-reduced-motion) about necessary movement in designs to determine the most accessible ways forward that respect humans. You can also use the prefers-reduced-motion CSS media query.

### Motivating stakeholders

To convince people with different mindsets to adopt accessibility, sometimes you have to employ different strategies. By approaching with curiosity, you could learn more about what would motivate someone the most.

Some will respond to a cool factor, like dangling a carrot, because they are inspired and encouraged to do the right thing with more information. "I can make a beautifully-designed component that works with the keyboard and screen readers? COOL!" These personalities are examples of when accessibility education and awareness shared with a spirit of collaboration and humility can accomplish wonderful things. Carrots. Accessibility that goes beyond compliance to instill it as a core value can make your product more usable and competitive.

Others less motivated by altruism or innovation for accessibility may require more of a stick approach, such as a legal requirement or financial risk. They may respond to competitive pressure, potential missed sales opportunities, or the cost of design and technical debt. You could stress with them the impact on budgets and timelines to redo a project or defend it in court for accessibility, which are significant. There is also the issue of missing revenue from people with disabilities who are excluded from using an inaccessible website or app. Share this with folks concerned with finance: the total after-tax disposable income for working-age people with disabilities in the US is about $490 billion, which is similar to that of other significant market segments, such as African Americans ($501 billion) and Hispanics ($582 billion) according to the [American Institutes for Research (AIR)](https://www.air.org/search?search=/resource%20hidden%20market%20purchasing%20power%20working%20age%20adults%20disabilities).

[List of legal policies](https://www.w3.org/WAI/policies/)
[Voluntary Product Accessibility Template (VPAT)](https://www.section508.gov/sell/vpat/)

#### Estimation

Estimation in software development is hard. It can still play a useful role for accessibility in your projects. By estimating the time necessary to make components accessible from the start, you can quantify what it will cost to deliver quality work on time. You can also compare your up-front estimates to the impact of a total rewrite post-launch: the dreaded "what would it take to make this app accessible?" Because accessibility has to be considered throughout the software development lifecycle, rewrites can inflate budgets into something much more costly than what it would have taken to build it right once.

Enterprise teams recommend padding estimates by up to 30% to give accessibility the time and attention it requires. It's possible your organization could be more lean and efficient at achieving accessibility. However, this could be a good first metric to make sure your project meets expectations with varying accessibility expertise among team members. Aim to underpromise and overdeliver. You can always improve your estimates as an organization over time.
