# 🎮 Pac-Man AI Planning

A university project implementing AI planning techniques for a Pac-Man environment using **Q-Learning** (Reinforcement Learning).  
Developed as part of the **AI Planning for Robot Systems** course at the **British University in Egypt (BUE)**.

---

## Project Overview

This project explores how a Pac-Man agent can learn to navigate a maze, collect pellets, and avoid obstacles using reinforcement learning. The agent is trained through repeated episodes in a custom-built Pygame environment, progressively improving its decision-making via a Q-table.

The project consists of two phases:
1. **Environment Design** — Building a Pac-Man maze with walls, pellets, and multiple obstacle types.
2. **AI Agent Implementation** — Training a Q-Learning agent to autonomously play the game.

---

## Methodology

### Environment

The maze is represented as a 20×10 grid using a string-based layout, rendered with **Pygame**:

| Symbol | Element | Description |
|--------|---------|-------------|
| `W` | Wall | Impassable barrier |
| `.` | Pellet | Collectible item (+10 score) |
| `O` | Red Obstacle | 5-second time penalty |
| `P` | Purple Obstacle | 10-second time penalty |
| `S` | Orange Obstacle | Score penalty (−50 points) |
| ` ` | Empty | Traversable space |

```
WWWWWWWWWWWWWWWWWWWW
W.. ...W.....P.. ..W
W.WWWW.W.WWW.WWWWW.W
W WWWW W.WWW.WWWWW WW
W.WWWW.W.WWW.WWWWW.W
W... .... O.. .....W
W.WWWW.W.WWW.WWWWW.W
W.WWWW.W.WWW.WWWWW.W
W. ....W.... .S....W
WWWWWWWWWWWWWWWWWWWW
```

### Q-Learning Algorithm

The agent uses **Q-Learning**, an off-policy temporal-difference reinforcement learning algorithm, to learn an optimal action-selection policy.

#### Hyperparameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| α (alpha) | `0.1` | Learning rate |
| γ (gamma) | `0.05` | Discount factor |
| ε (epsilon) | `0.9` → `0.01` | Exploration rate (decays with factor `0.65`) |
| Timer | `20s` | Episode duration |
| FPS | `30` | Frames per second |

#### Q-Table Update Rule

The Q-value is updated after each action using the Bellman equation:

```
Q(s, a) ← Q(s, a) + α · [r + γ · max Q(s', a') − Q(s, a)]
```

#### Action Space

The agent has **4 possible actions** corresponding to cardinal directions:

| Action | Direction |
|--------|-----------|
| 0 | Up |
| 1 | Down |
| 2 | Left |
| 3 | Right |

#### State Representation

The state encodes three features into a single integer:
1. **Pac-Man's grid position** — `y × maze_width + x`
2. **Elapsed time** — scaled to `[0, 1000]` and appended
3. **Obstacle presence** — binary flag indicating if any `O` obstacle remains

#### Reward Structure

| Event | Reward |
|-------|--------|
| Collecting a pellet (`.`) | **+100** |
| Hitting a red obstacle (`O`) | **−500** |
| Hitting a purple obstacle (`P`) | **−100** |
| Hitting an orange obstacle (`S`) | **−500** |

#### Exploration Strategy

The agent uses an **epsilon-greedy** policy:
- With probability **ε**, choose a **random action** (exploration)
- With probability **1 − ε**, choose the **best known action** from the Q-table (exploitation)
- Epsilon decays from `1.0` toward `0.01` using a decay factor of `0.65`

---

## 📊 Results

### Training Run Scores

The agent was trained over multiple consecutive episodes (each lasting 20 seconds). Below are the scores recorded from a sample training session:

| Episode | Score |
|---------|-------|
| 1 | 320 |
| 2 | 250 |
| 3 | 390 |
| 4 | 200 |
| 5 | 200 |
| 6 | 380 |
| 7 | 340 |
| 8 | 260 |
| 9 | 230 |
| 10 | 360 |
| 11 | 280 |
| 12 | **520** |
| 13 | 340 |

### Key Observations

- **Average score**: ~313 points across 13 episodes
- **Best score**: **520** (Episode 12) — demonstrating the agent's ability to improve over time
- **Score variance**: Scores ranged from 200 to 520, reflecting the stochastic nature of epsilon-greedy exploration
- The agent successfully learned to collect pellets while progressively avoiding obstacles as epsilon decayed
- Higher scores in later episodes suggest the Q-table was converging toward a more effective policy

---

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|---------|
| **Python 3.11** | Core programming language |
| **NumPy** | Q-table initialization and manipulation |
| **Pygame 2.5** | Game environment rendering and input handling |

---

## How to Run

### Prerequisites

```bash
pip install numpy pygame
```

### Run the Environment (Manual Play)

```bash
jupyter notebook pacman_env_obstacles.ipynb
```
Use **arrow keys** to move Pac-Man manually through the maze.

### Run the AI Agent (Q-Learning)

```bash
jupyter notebook pacman_final_implementation.ipynb
```
The agent will automatically play and learn across multiple episodes. Scores are printed to the console after each episode.

---

## Project Files

| File | Description |
|------|-------------|
| `pacman_env_obstacles.ipynb` | Pac-Man environment with manual keyboard control and obstacle mechanics |
| `pacman_final_implementation.ipynb` | Q-Learning AI agent that autonomously navigates the maze |
| `proposal.docx` | Project proposal document |
| `documentation.docx` | Full project documentation and analysis |
| `presentation.pptx` | Final presentation slides |
| `pacman_gui.png` | GUI screenshot of the game in action |
| `submission.zip` | Complete project submission archive |

---

## 👥 Team

- **Ahmed ElKeshawy**
- **Mohamed Hussein**

---

## Course Info

- **Course**: AI Planning for Robot Systems
- **University**: British University in Egypt (BUE)
- **Year**: Year 3, Semester 1

---

## License

This project was developed for academic purposes as part of the BUE curriculum.
