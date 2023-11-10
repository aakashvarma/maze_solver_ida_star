# maze_solver_ida_star
solving mazes using IDA * algorithm

# Solving mazes using iterative deepening A* algorithm

Created: November 10, 2023 10:30 AM
Tags: ACI, Algorithm
Date: November 10, 2023

## Problem Statement

There are two agents named R1 and G1. Both are searching for a "heart" as shown in the below configuration as “H” that gives everlasting power. Both agents are trying to reach the heart. In this process many obstacles may be encountered to reach the heart. Help them in finding the best path to reach the heart from any arbitrary start positions. [Dynamically fetch the start position while executing the code]

For the agent R1 the obstacle is the green room. If R1 enters the green room it incurs a penalty of +10 cost and if it uses the red room it incurs a penalty of -10 points. For the agent G1 the obstacle is the red room. If G1 enters the red room it incurs a penalty of +10 cost and if it uses the green room it incurs a penalty of -10 points. In addition to the given cost, for every transition an agent visits incurs a path cost of 1.

For any arbitrary node “n” the heuristic to reach the Heart h(n) is given by the below:

$Manhattan Distance + Color Penalty$
*where, Color Penalty = +5 if the node “n” and goal node is in different colored room
and Color Penalty = -5 if the node “n” and goal node is in same colored room*

![Untitled](Solving%20mazes%20using%20iterative%20deepening%20A%20algorith%20298167442e4f416f8d60d38f417f13a9/Untitled.png)

Note: The agents are not competing with each other. 

---

## Green and Red Rooms and Penalties

| Agent | Red Penalty | Green Penalty |
| --- | --- | --- |
| R1 | -10 | +10 |
| G1 | +10 | -10 |

Additionally, the cost for each transition path is 1.

The heuristic for any node "n" is calculated as follows:

`Heuristic = Manhattan distance + Color Penalty`

where,

`Color Penalty = +5 if the node "n" and the goal node are in different colored rooms`

`Color Penalty = -5 if the node "n" and the goal node are in the same colored room`

---

## High level Design

### Data Structure

Main data structure used in the algorithm are Array and below are the arrays used:

- **Maze**: 2D Matrix with 0 and 1 where, Red Cell is represented by 0 and Green Cell is represented by 1
- **Heuristic Matrix**: 2D Matrix representing the heuristics of each node
- **Path Traversed**: Array of points
- **Nodes Visited**: Array of points

## Assumptions

The following assumptions have been considered as part of our algorithm:

- The path with less cost is preferred over the shortest path
- The initial threshold is considered as the heuristic value of the start node

### PEAS (Performance measure, Environment, Actuator, Sensor)

The PEAS (Performance measure, Environment, Actuator, Sensor) model is a framework used to describe the characteristics of an agent in a given environment. It consists of four elements:

1. **`Performance measure**:` This refers to the metric or metrics used to evaluate the agent's performance. In the case of the agents R1 and G1 searching for the "heart", the performance measure could be the length of the path taken to reach the heart or the total cost of the path.
2. **`Environment**:` This refers to the environment in which the agent operates, including the physical layout of the environment and any constraints or limitations. In the case of the agents R1 and G1, the environment consists of rooms with different colors, and the agents incur different penalties for entering certain rooms.
3. **`Actuator**:` This refers to the mechanism by which the agent is able to take actions in the environment. In the case of the agents R1 and G1, the actuator could be the ability to move from one room to another, based on the current heuristics.
4. **`Sensor**:` This refers to the mechanism by which the agent is able to perceive and gather information about the environment. In the case of the agents R1 and G1, the sensor could be the color detectors of the rooms and the detector for the presence of the heart (goal) in the room. Detection of the next possible room to move into can also be another sensor.

Overall, the PEAS model provides a useful framework for understanding the characteristics and capabilities of an agent in a given environment and can be used to design and evaluate the performance of the agent.

The PEAS for the given problem statement is summarized as below:

| Agent | Performance Measure | Environment | Actuator | Sensor |
| --- | --- | --- | --- | --- |
| Path Search Agent R1 | Best Path to Heart (H Cell), optimizing;
· total cost of the path
· length of the path taken
 | Rooms in different colour code: Green Room is considered obstacle and Red room is non-obstacle. | Ability to move from one room to another, based on the current heuristics in following directions: North, South, East, West, North East, South East, North West, South West | · Colour detector
· Goal (Heart) Room detector
· Detector- Next possible room to move |

### PSA (Problem Solving Agent)

Problem Solving agents, also known as rational agents in Artificial Intelligence, use specific strategies or algorithms to solve problems and provide optimal results. These agents are goal-based.

To describe a PSA, we need to define the following parameters: Initial State, Possible Actions, Transition Model, Goal Test, Path Cost, and the possible number of states.

For the given problem statement, the PSA can be defined as follows:

- Initial State: This includes the coordinates of the starting node and the corresponding color code (represented as 0 or 1 for obstacle and non-obstacle cells). It can be represented as `< (start node), (color code)>`.

For Agent R1, a green cell is an obstacle. The green cell color code is 1, and the red cell color code is 0.

For Agent G1, a red cell is an obstacle. The green cell color code is 1, and the red cell color code is 0.

- Possible Actions: The possible action is to move the agent to the next child node in the given maze. The next child can be in the North, South, East, West, Northeast, Southeast, Northwest, or Southwest direction.
- Transition Model: If `<Xj, Yj>` refers to the next state and `<Xi, Yi>` refers to the initial state, then `<Xj, Yj> = <Xi, Yi> + [(0,1) or (1,0) or (1,1)]`, based on the node with the least value of defined heuristics.
- Goal Test: This checks if the current node is the required goal node, i.e., Is `current_node == Goal_node`?
- Path Cost: This refers to the total cost of reaching the goal node from the start node. If `g(n)` defines the actual path cost of reaching from the start node to the current node 'n', and `h(n)` refers to the heuristics cost of reaching the goal node from the current node, then the total path cost is `Total path cost = g(n) + h(n)`.
- Possible Number of States: This refers to the maximum number of nodes that can be generated at each step. The maximum number of possible states from the current state is 7.

---

## Algorithm (Iterative Deepening A*)

IDA* or Iterative Deepening A* is a path search and graph traversal algorithm used to find the shortest and optimal path between a given start node and goal node in a graph. It is a variation of iterative deepening depth-first search that incorporates a heuristic function to calculate the actual path cost and other heuristic costs to reach the goal node from the current node.

Being a depth-first search algorithm, IDA* consumes less memory compared to A*. Unlike regular iterative deepening search, it prioritizes finding the most optimal nodes and does not explore the same depth throughout the entire search tree.

The only drawback of using the IDA* algorithm is that it can be slower due to the possibility of exploring repeated nodes, which increases processing time compared to the A* algorithm.

### **Pseudo code**

**iterative_deepening_a_star_rec(maze, heuristic, node, goal, g, threshold, visited, path, agent)**

| Method | iterative_deepening_a_star |
| --- | --- |
| Data | • maze: input maze
• heuristic: heuristic maze
• node: tuple of the current node coordinates
• goal: tuple of the goal coordinates
• g: current path cost
• f: cost of the cheapest path
• threshold: threshold of the cost
• visited: list of visited node tuples
• path: list of tuples consisting of the path taken by the algorithm |
| Result | • min: threshold
• isGoalReached: boolean representation of whether the goal has been reached or not |
| Procedure | node := path.last
f := g + h(node)
if f > bound then return f
if is_goal(node) then return FOUND
min := ∞
for succ in successors(node) do
    if succ not in path then
        path.push(succ)
        t := search(path, g + cost(node, succ), bound)
        if t = FOUND then return FOUND
        if t < min then min := t
        path.pop()
    end if
end for |

**iterative_deepening_a_star ()**

| Method | iterative_deepening_a_star |
| --- | --- |
| Data | • maze: input maze
• heuristic: heuristic maze
• node: tuple of the current node coordinates
• goal: tuple of the goal coordinates |
| Result | None |
| Procedure | path := [root]
loop
    t := search(path, 0, bound)
    if t = FOUND then return (path, bound)
    if t = ∞ then return NOT_FOUND
    bound := t
end loop |

### Application design

Functions would be created to achieve the requirements

**init_maze()**

| Method | init_maze |
| --- | --- |
| Data | NA |
| Result | 2D arrays maze 1 and maze 2 as per Scenario 1 and Scenario 2 . |
| Procedure | 2D array is built as per the Maze provided in scenario 1 and 2 considering Red as 0 and Green as 1 |

**get_agent_color_penalty()**

| Method | get_agent_color_penalty |
| --- | --- |
| Data | maze: input maze
point: current node point
agent: type of agent used |
| Result | agent_penalty: returns the penality associated with current node point passed. |
| Procedure | Depending on the Agent and Current node point color return the penalty value. Throws an exception if the wrong agent or node is passed. |

**get_heurestic()**

| Method | get_heurestic |
| --- | --- |
| Data | maze: input maze
goal: tupple of the goal coordinates |
| Result | manhattan_matrix: 
where 
$heurestic = manhattan_distance + color_penalty$ |
| Procedure | Loop through the all the points of the input maze and calculate heuristic for each point  
$heuristic = manhattan_distance + color_penalty$ |

**get_neighbours()**

| Method | get_neighbours |
| --- | --- |
| Data | current_node: tuple which represents the current node position
maze: matrix representation of the maze |
| Result | list of valid neighbors of the current_node |
| Procedure | Returns the list of all 8 cells around the passed node.  For corner and edge nodes should return only the list of valid cells as per maze dimensions. |

**get_manhatttan_distance()**

| Method | get_manhatttan_distance |
| --- | --- |
| Data | p1: point1 as tuple
p2: point2 as tuple |
| Result | manhattan_distance between two points |
| Procedure | Calculate and return manhattan_distance between two points. |

### **Helper Functions**

**visualise_path(maze, path)**

| Method | visualise_path |
| --- | --- |
| Data | maze: matrix representation of the maze
path: list of tuples consisting of the path taken by the algo |
| Result | None |
| Procedure | Plot the maze and write over the cells with Start to Goal trace.  First element of the path array is the starting point and last element of path array is the goal. |

### **Time and Space Complexity**

The IDA* algorithm is an optimal search algorithm, since the heuristics are admissible and it has a finite branching factor ‘b’.

The average branching factor is defined as the total nodes generated divided by the maximum depth.

For given problem statement, maximum nodes generated can be (6*6 -1)=35 and maximum depth can be 5. So, average branching factor (b)= 35/5
