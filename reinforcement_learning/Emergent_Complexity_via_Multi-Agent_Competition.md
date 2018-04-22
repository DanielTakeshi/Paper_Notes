# Emergent Complexity via Multi-Agent Competition

A simple idea which involves using multi-agents for more complex environments,
based on the insight that it's really a competitive agent (e.g., an opponent in
Go of equal difficulty) and not necessarily the environment itself (e.g., Go has
very simple rules) that results in fast learning.

This naturally induces a curriculum where the curriculum is based on the
opposing agent being of the same difficulty level.

Rewards are based on random exploration, and then annealed so that it's just
based on winning or losing.

Agents use Proximal Policy Optimization.

They argue that agents are able to learn complex behaviors (kicking, running,
boxing, etc.) but obviously how much of this is due to MuJoCo itself? I dunno
...
