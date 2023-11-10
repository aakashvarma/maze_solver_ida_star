
# Solving mazes using iterative deepening A* algorithm

## Problem Statement

There are two agents named R1 and G1. Both are searching for a "heart" as shown in the below configuration as “H” that gives everlasting power. Both agents are trying to reach the heart. In this process many obstacles may be encountered to reach the heart. Help them in finding the best path to reach the heart from any arbitrary start positions. [Dynamically fetch the start position while executing the code]

For the agent R1 the obstacle is the green room. If R1 enters the green room it incurs a penalty of +10 cost and if it uses the red room it incurs a penalty of -10 points. For the agent G1 the obstacle is the red room. If G1 enters the red room it incurs a penalty of +10 cost and if it uses the green room it incurs a penalty of -10 points. In addition to the given cost, for every transition an agent visits incurs a path cost of 1.

For any arbitrary node “n” the heuristic to reach the Heart h(n) is given by the below:

$Manhattan Distance + Color Penalty$
*where, Color Penalty = +5 if the node “n” and goal node is in different colored room
and Color Penalty = -5 if the node “n” and goal node is in same colored room*
