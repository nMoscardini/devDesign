# UI Principles

Version 1.0. Updated 13 October 2026.

## Purpose

How Nino's apps and pages look and behave. It supports DESIGN_PRINCIPLES.md and sits below it: if they disagree, the gates win, then DESIGN_PRINCIPLES.md, then this file.

Two kinds of interface are covered. Native apps are Swift apps for Mac and iPhone. Web pages are the server-rendered HTML pages in REFERENCE_ARCHITECTURE.md.

How strictly it applies depends on the class:

- **Lab.** Ignore it, unless the experiment is about the interface.
- **Personal.** Defaults. A departure needs a lite deviation record if it would surprise a future reader.
- **Shared, and paid products.** All of it. Rules marked **must** cannot be skipped.

The source for native apps is Apple's Human Interface Guidelines (developer.apple.com/design). Where this file is silent, follow them.

## 1. Use the system, not your own

- Build native apps from standard SwiftUI controls, navigation, toolbars, sheets, menus, and settings. They come with accessibility, keyboard support, dark mode, and Apple's current look for free.
- Apple's current look (Liquid Glass, 2025 onward) comes from building with the current Xcode and standard controls. Do not imitate it with custom drawing.
- A custom control needs a reason: the system has nothing that does the job. Record it.
- Use SF Symbols for icons, and the system font (San Francisco) for text.

## 2. Respect the platform

- **Mac apps behave like Mac apps.** A full menu bar with standard menus. Keyboard shortcuts for common actions, shown in the menus. Resizable windows that remember their size and position. Settings in a Settings window. Undo where it makes sense.
- **iPhone apps behave like iPhone apps.** Thumb-reachable main actions. Standard navigation and back behaviour. Touch targets at least 44 by 44 points.
- Never stretch an iPhone layout across a Mac window, or squeeze a Mac layout onto a phone.

## 3. Accessible to everyone

- Text scales with the user's text size setting (Dynamic Type on iPhone). Layouts survive the largest size.
- Text contrast is at least 4.5 to 1 against its background. Large text at least 3 to 1.
- Never use colour alone to carry meaning. Add a label, shape, or symbol.
- Every control has a VoiceOver label. Images that carry meaning have a description.
- Everything works from the keyboard, with a visible focus. On Mac, without a mouse.
- Honour Reduce Motion and Reduce Transparency.
- For Shared projects and paid products, each of these is a **must**.

## 4. Light and dark

- Both modes work. Use the system's semantic colours (label, background, accent) rather than fixed values.
- One accent colour per app. Use it for actions and selection, not decoration.

## 5. Content first

- Every element earns its place. If removing it loses nothing, remove it.
- One main action per screen. Make it obvious.
- Plain words in labels, buttons, and messages. Same voice as these documents.
- Design the empty, loading, and error states, not just the full one. An error says what happened and what to do next.
- Show progress for anything over a second. Never leave the user wondering if it worked.
- Save without asking where possible. Confirm only what is hard to undo.

## 6. Web pages share one style

- One stylesheet, `devDesign/web/style.css` (TARGET), defines colours, type sizes, spacing, and the few components the pages need. Every project links to it or copies it unchanged (principle 6: one source of truth).
- Use the system font stack, so pages look at home on a Mac.
- Support light and dark with `prefers-color-scheme`.
- Pages work at phone width. No horizontal scrolling.
- Sections 3 and 5 apply to web pages too.

## 7. Check it before calling it done

Add these to a review under REVIEW_PROCESS.md for any project with an interface. For Lab, skip them.

- Standard controls used? Any custom ones recorded?
- Mac: menus, shortcuts, resizable windows, Settings window?
- Tried at the largest text size, in dark mode, and with VoiceOver on?
- Worked through the main path with the keyboard only?
- Empty, loading, and error states designed?
- Web pages: shared stylesheet used, readable at phone width?
- Someone looked at it on the real device, not only in a preview (principle 8).
