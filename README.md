# Blueprint Physics Simulator

Four small 2D physics simulations in Python, with a Tkinter launcher and a ChatGPT parser for word problems.

Built for Blueprint (MIT).

## What it does

You pick a scenario from a dropdown and press Calculate. A Pygame window opens and runs that simulation in real time.

| Scenario | What you see |
|---|---|
| Free fall | 300 balls drop into a bowl made of three static walls and pile up. |
| Orbit | A planet circles a star. Gravity uses Newton's law with real constants (G, solar and Earth mass, 1 AU) scaled to pixels. The planet leaves a fading trail. |
| Pendulum | A rigid rod and bob swing from a fixed pivot, released at 40 degrees. |
| Projectile motion | A ball launches with an initial velocity. Press Space to fire five more from random spots. |

The launcher also has a text box for a physics word problem. `chatgpt_api_call.py` sends that text to the OpenAI API and asks for the problem's parameters (mass, initial velocity, start and end time, gravity) as JSON. That call is written but not yet wired into the launcher. The line that calls it is commented out.

## How it works

- **Physics:** [Pymunk](https://www.pymunk.org/) handles bodies, shapes, collisions and joints. Each sim builds a `pymunk.Space`, adds bodies, and steps it every frame.
- **Rendering:** [Pygame](https://www.pygame.org/) draws each frame and handles the window and keyboard.
- **Orbit math:** Pymunk gravity is off for the orbit sim. `calculate_gravity()` computes the star's pull on the planet each frame and applies it as a force. Each step covers 4 hours of simulated time (14,400 s).
- **Pendulum:** The rod and bob are one body pinned to a static pivot with a `PinJoint`.
- **Launcher:** Tkinter dropdown plus a Calculate button. `app.py` routes the choice to each sim's `main()`.

## Run it

Requires Python 3 with Tkinter (included with most Python installs).

```bash
pip install pygame pymunk openai
python app.py
```

Each simulation also runs on its own:

```bash
python free_fall_sim.py
python orbit_sim.py
python pend_sim.py
python projectile_motion_sim.py
```

To try the ChatGPT parser, set your key first. The OpenAI client reads it from the environment.

```bash
export OPENAI_API_KEY=your_key_here
python -c "import chatgpt_api_call as c; c.parse('A 2 kg ball is dropped from rest and falls for 3 seconds.')"
```

Close a simulation window to exit.

## Repo layout

| File | Purpose |
|---|---|
| `app.py` | Main launcher. Dropdown for all four sims. |
| `GUI.py` | Earlier version of the launcher. Always runs free fall. |
| `free_fall_sim.py` | Falling balls in a bowl. Also has an unused `slope()` scene. |
| `orbit_sim.py` | Star and planet with Newtonian gravity and a trail. |
| `pend_sim.py` | Rod and bob pendulum. |
| `projectile_motion_sim.py` | Launched projectiles. Space adds more. |
| `chatgpt_api_call.py` | Sends a word problem to `gpt-3.5-turbo` and prints the parameters it returns as JSON. |
| `calculate.py` | Placeholder stubs for the closed-form calculations. Not used yet. |

## Status

This was a competition prototype. The simulations run. The word problem input does not change the simulations yet, and `calculate.py` is empty. The next step would be passing the parsed JSON values into each sim's starting conditions.
