# Hidde de Vries

Accessibility expert

---

## Terminology

### Modal vs Non-modal

**Modal:**
A modal element refers to an interactive component that captures the user's attention and restricts interaction with the rest of the page. When a modal is active, the user can only interact with the modal itself, and the remaining page content is inaccessible.

**Non-modal:**
A non-modal element, on the other hand, does not restrict interaction with the rest of the page. Users can interact with both the non-modal element and other elements on the page simultaneously.

### Light vs explicit dismiss

**Light Dismiss:**
A light dismiss behavior automatically hides an element when certain actions occur, such as clicking outside the element or scrolling the page. The element disappears without requiring any specific action from the user or a script.

**Explicit Dismiss:**
In contrast, an explicit dismiss behavior requires the user or a script to actively close or hide the element. It does not automatically disappear based on external actions.

### Z-index vs top-layer

**Z-index:**
Z-index is a CSS property that determines the stacking order of elements on a web page. By assigning different z-index values to elements, you can control which elements appear in front of or behind others. Elements with higher z-index values appear on top of elements with lower values.

**Top-layer:**
Top-layer refers to the highest layer or level in the stacking order of elements. An element placed in the top-layer will appear on top of all other elements, regardless of their z-index values. Typically, elements like full-screen content, dialogs, and popovers are placed in the top-layer.

Elements in the top-layer:

-   Full-screen content
-   Dialogs
-   Popovers
-   Backdrop

In some cases, elements in the top-layer may have a backdrop. A backdrop is a background element or overlay that provides a visual effect and can be styled according to the design requirements. Top-layer elements often come with a built-in styleable backdrop.

### Keyboard focus trap

Keyboard focus trap is a technique used to restrict the user's focus within a specific component or element. When the user navigates through the elements using the Tab key, a focus trap prevents them from moving outside of the trapped component. This technique is often employed temporarily to control the user's focus within a particular context.

## Patterns

### Dialog: what is it?

A dialog is a component that contains content that is not essential to the main content of the page. It typically appears as a modal element, meaning it restricts interaction with the rest of the page. Dialogs are often used to present information or prompt users for input in a focused and attention-grabbing manner.

Examples of dialog usage:

"Do you want to continue? YES or NO"
"Want to open a new file? What shall we do with the current file?"
Modal dialogs + non-modal dialogs: what is it?

**Modal dialogs:**
Modal dialogs are interactive components that behave as modal elements. They restrict interaction with the rest of the page while active and typically provide options for user input. Modal dialogs may close automatically when the user presses the ESC key or explicitly dismisses them.

**Non-modal dialogs:**
Non-modal dialogs, on the other hand, do not restrict interaction with the rest of the page. Users can interact with both the non-modal dialog and other elements on the page simultaneously. Non-modal dialogs may or may not have dismiss behaviors on pressing ESC or other actions.

### Popover: what is it?

A popover is a set of behaviors that can be added to any element using the popover attribute. It functions as a modal element, similar to a dialog, but with fewer built-in rules. Popovers are versatile and can take on different roles, such as dialog, menu, tooltip, or other types of content.

Examples of popover usage:

-   Datepickers
-   Tooltips
-   Teaching UI
-   Action menus
-   Popovers + non-modal dialogs

**Popovers:**
Popovers behave as modal elements, restricting interaction with the rest of the page while active. Similar to non-modal dialogs, they may or may not have dismiss behaviors on pressing ESC or other actions.

**Non-modal dialogs:**
Non-modal dialogs, as mentioned earlier, do not restrict interaction with the rest of the page. Users can interact with both the non-modal dialog and other elements on the page simultaneously. They may or may not have dismiss behaviors on pressing ESC or other actions.

### Anchor positioning

Anchor positioning is a CSS technique used to tether or position elements relative to each other. When using anchor positioning, an element is positioned in relation to another element acting as an anchor. This technique provides control over the placement and alignment of elements on a web page.

### Disclosure widget: what is it?

A disclosure widget is a UI element that allows users to show or hide certain parts of content, such as FAQ sections or expandable table rows. It provides a mechanism to reveal additional information or hide it based on user interaction, typically through toggling a button or triggering an action.

These notes summarize various concepts and patterns related to web accessibility and user interface design. They provide an overview of terminology and patterns commonly used in designing interactive components, such as dialogs, popovers, and disclosure widgets.
