# Architecture Overview

```mermaid
flowchart TD
    User[User input<br/>pointer, click, Escape] --> Events[Browser event listeners]
    Media[Reduced-motion preference] --> Motion[Motion settings]
    Events --> State[Runtime state<br/>pointer, agents, pause, timing]
    Motion --> State

    DOM[HTML document<br/>main content and hint] --> Browser[Browser rendering]
    Styles[CSS<br/>layout, theme, cursor behavior] --> Browser
    Canvas[Canvas element<br/>#swarm] --> Browser
    State --> Loop[Animation frame loop]
    Loop --> Physics[Agent update<br/>orbit, separation, velocity, bounds]
    Physics --> State
    Physics --> Render[Canvas drawing<br/>colored arrows and lead cursor]
    Render --> Canvas

    Resize[Window resize] --> Sizing[Canvas/DPR sizing]
    Sizing --> Canvas
    Sizing --> State
    Browser --> User
```

## Runtime flow

- The HTML and CSS provide the page content, dark visual theme, and full-screen canvas.
- The script initializes pointer state and a swarm of agents; reduced-motion preference selects a smaller swarm.
- Pointer and click events update the pointer position, with clicks briefly pushing agents outward. Escape toggles the animation loop.
- Each animation frame updates agent motion (orbit attraction, separation, velocity damping, and edge wrapping), then draws the agents and active pointer on the canvas.
- Resize events update the canvas dimensions and device-pixel-ratio transform. Changes to the reduced-motion preference adjust the cursor presentation.
