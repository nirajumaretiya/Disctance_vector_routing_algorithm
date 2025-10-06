# Distance Vector Routing Visualization

A small Python app that shows how the Distance Vector (DV) routing algorithm converges on a 4‑node network. It visualizes how nodes exchange their current best-known path costs (min‑cost vectors), update distance tables, and eventually settle on the shortest paths between all pairs of nodes.

## Modes
- Step-by-step (`mainGUI.py`): click Advance to process one packet/update at a time. This makes it easy to follow which packet was delivered, which node updated, and which table entries changed.
- Real-time (`mainGUITime.py`): animated colored packets move along links continuously. When a packet reaches its destination node, that node updates its table and may send new packets if it discovers shorter paths.

## Requirements
- Python 3.8+
- Pygame

Install:
```bash
pip install pygame
```

## Run
Step-by-step:
```bash
python mainGUI.py
```
Real-time:
```bash
python mainGUITime.py
```

## Controls
- Click buttons (Advance/Start)
- Close window to quit
- Watch the node circles and link labels: nodes are drawn at four corners; link labels show weights.

## Files
- `mainGUI.py` – step-by-step UI
- `mainGUITime.py` – real-time animation
- `distanceTableGUI.py` – distance table rendering
- `drawText.py` – text helpers
- `node0.py` … `node3.py` – node logic and packet queues
- `visualPacket.py` – animated packet objects

## How it works (brief)
1) Initialization: each node sets its direct link costs in its distance table and places an initial min‑cost vector in its send queue (rtinit).
2) Communication: packets carry the sender’s current min‑costs to all destinations. In step‑by‑step mode, one packet is delivered per click; in real‑time, packets animate toward the destination node.
3) Update rule: upon delivery, the destination node copies the sender’s row, then checks if going “via sender” offers a cheaper path to any destination. If so, it updates its own min‑costs and enqueues fresh packets to notify its neighbors (rtupdate).
4) Convergence: this repeats until no node can improve any entry. Queues empty out, and the visualization shows stable distance tables (algorithm complete).

### What you see
- Nodes (0–3) placed in a square; links drawn between connected nodes with weights (e.g., 0–1:1, 0–2:3, 0–3:7, 1–2:1, 2–3:2).
- Distance tables next to each node. Newly changed entries are highlighted so you can track exactly what improved.
- In real‑time mode, colored packets move along links; when they arrive, the corresponding node updates and may emit new packets.

### Assumptions
- Static, symmetric link costs; no link failures or dynamic changes during a run.
- Four fixed nodes for clarity; the logic generalizes to more nodes.

### Quick troubleshooting
- If the window doesn’t open, ensure Pygame installed correctly and you’re using Python 3.8+.
- If nothing seems to happen in step‑by‑step mode, click Advance; in real‑time, click Start.


