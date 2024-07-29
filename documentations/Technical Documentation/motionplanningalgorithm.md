# Motion Planning Algorithms for the Humanoid Robot Doctor (HRD)

Motion planning algorithms are essential for enabling the Humanoid Robot Doctor (HRD) to perform complex tasks by planning and executing movements efficiently and safely. These algorithms help the robot navigate its environment, avoid obstacles, and achieve its objectives with precision. Below are detailed descriptions of several common motion planning algorithms used in robotics.

---

## 1. **Configuration Space (C-Space)**

**Overview:**
- Configuration Space (C-Space) is a fundamental concept in motion planning where the robot's physical state is represented in a multi-dimensional space. Each dimension corresponds to a degree of freedom (DOF) of the robot.

**Components:**
- **Degrees of Freedom (DOF):** Represents the number of independent parameters that define the robot's configuration.
- **Obstacle Representation:** Obstacles are mapped into the C-Space, creating regions that the robot must avoid.

**Usage:**
- Plan paths by considering the robot's DOF and the obstacles in C-Space.
- Convert the path back to the physical space for execution.

**Example:**
For a robotic arm with 3 joints, each joint's angle would be a dimension in C-Space. A path is planned in this 3D space to avoid collisions.

---

## 2. **Potential Field Method**

**Overview:**
- The Potential Field Method treats the robot as a particle moving in a field of forces. Attractive forces pull the robot towards the goal, while repulsive forces push it away from obstacles.

**Components:**
- **Attractive Potential:** Directs the robot towards the goal.
- **Repulsive Potential:** Pushes the robot away from obstacles.

**Equation:**
\[ U_{att}(q) = \frac{1}{2} k_{att} \cdot \| q - q_{goal} \|^2 \]
\[ U_{rep}(q) = \begin{cases} 
      \frac{1}{2} k_{rep} \left( \frac{1}{\| q - q_{obs} \|} - \frac{1}{d_0} \right)^2 & \text{if } \| q - q_{obs} \| \leq d_0 \\
      0 & \text{if } \| q - q_{obs} \| > d_0 
   \end{cases}
\]

Where \( U_{att} \) is the attractive potential, \( U_{rep} \) is the repulsive potential, \( q \) is the robot's configuration, \( q_{goal} \) is the goal configuration, \( q_{obs} \) is the obstacle configuration, \( k_{att} \) and \( k_{rep} \) are scaling factors, and \( d_0 \) is the influence distance of the obstacle.

**Code Example:**
```python
def potential_field(q, q_goal, obstacles):
    k_att = 1.0
    k_rep = 100.0
    d_0 = 1.0

    U_att = 0.5 * k_att * np.linalg.norm(q - q_goal)**2
    U_rep = 0
    for q_obs in obstacles:
        d = np.linalg.norm(q - q_obs)
        if d <= d_0:
            U_rep += 0.5 * k_rep * (1/d - 1/d_0)**2
    
    return U_att + U_rep
```

**Advantages:**
- Simple to implement.
- Suitable for real-time applications.

**Disadvantages:**
- Can get stuck in local minima.
- Not suitable for complex environments.

---

## 3. **Rapidly-Exploring Random Tree (RRT)**

**Overview:**
- RRT is a probabilistic algorithm that builds a tree by randomly sampling the configuration space and connecting new samples to the nearest node in the tree.

**Components:**
- **Random Sampling:** Generates random configurations in the C-Space.
- **Tree Expansion:** Adds new configurations to the tree by connecting them to the nearest existing node.
- **Goal Biasing:** Occasionally directs the sampling towards the goal to improve efficiency.

**Algorithm:**
1. Initialize the tree with the start configuration.
2. Randomly sample a new configuration.
3. Find the nearest node in the tree to the new configuration.
4. Add the new configuration to the tree.
5. Repeat until the goal is reached or a maximum number of iterations is exceeded.

**Code Example:**
```python
class Node:
    def __init__(self, q):
        self.q = q
        self.parent = None

def rrt(start, goal, obstacles, max_iter=1000):
    tree = [Node(start)]
    for _ in range(max_iter):
        q_rand = random_configuration()
        nearest_node = min(tree, key=lambda node: np.linalg.norm(node.q - q_rand))
        q_new = steer(nearest_node.q, q_rand)
        if not in_collision(q_new, obstacles):
            new_node = Node(q_new)
            new_node.parent = nearest_node
            tree.append(new_node)
            if np.linalg.norm(q_new - goal) < goal_threshold:
                return extract_path(tree, new_node)
    return None
```

**Advantages:**
- Can handle high-dimensional spaces.
- Efficiently explores the configuration space.

**Disadvantages:**
- Path quality can be suboptimal.
- Requires post-processing to smooth paths.

---

## 4. **Probabilistic Roadmap (PRM)**

**Overview:**
- PRM is a two-phase algorithm: a learning phase that builds a roadmap by randomly sampling the configuration space and connecting the samples, and a query phase that searches the roadmap for a path from the start to the goal.

**Components:**
- **Random Sampling:** Generates random configurations in the C-Space.
- **Roadmap Construction:** Connects nearby configurations to form a graph.
- **Path Search:** Uses graph search algorithms (e.g., Dijkstra's, A*) to find a path in the roadmap.

**Algorithm:**
1. **Learning Phase:**
   - Randomly sample configurations and add them as nodes in the roadmap.
   - Connect each node to its nearest neighbors if the path between them is collision-free.
2. **Query Phase:**
   - Add the start and goal configurations to the roadmap.
   - Use a graph search algorithm to find a path between the start and goal nodes.

**Code Example:**
```python
def prm(start, goal, obstacles, num_samples=100):
    roadmap = []
    for _ in range(num_samples):
        q_rand = random_configuration()
        if not in_collision(q_rand, obstacles):
            roadmap.append(q_rand)
    
    graph = build_graph(roadmap, obstacles)
    path = graph_search(graph, start, goal)
    return path

def build_graph(roadmap, obstacles):
    graph = {}
    for q in roadmap:
        graph[q] = []
        for q_near in roadmap:
            if q != q_near and not in_collision_path(q, q_near, obstacles):
                graph[q].append(q_near)
    return graph
```

**Advantages:**
- Efficient for multiple queries in static environments.
- Can handle complex environments.

**Disadvantages:**
- Computationally expensive to build the roadmap.
- Not suitable for dynamic environments.

---

## 5. **Dynamic Window Approach (DWA)**

**Overview:**
- DWA is a local motion planning algorithm that considers the robot's kinematic and dynamic constraints. It evaluates possible velocities within a dynamic window and selects the best one based on an objective function.

**Components:**
- **Dynamic Window:** A set of admissible velocities considering the robot's acceleration limits.
- **Objective Function:** Evaluates the quality of each velocity based on criteria such as distance to the goal, obstacle avoidance, and speed.

**Algorithm:**
1. Generate a set of possible velocities within the dynamic window.
2. Simulate the robot's motion for each velocity over a short time horizon.
3. Evaluate each simulated trajectory using the objective function.
4. Select the velocity with the highest score and apply it.

**Code Example:**
```python
def dwa(current_state, goal, obstacles, dt=0.1):
    velocities = generate_dynamic_window(current_state)
    best_velocity = None
    best_score = -float('inf')

    for v in velocities:
        trajectory = simulate_trajectory(current_state, v, dt)
        score = evaluate_trajectory(trajectory, goal, obstacles)
        if score > best_score:
            best_score = score
            best_velocity = v
    
    return best_velocity

def generate_dynamic_window(current_state):
    # Generate a set of admissible velocities
    pass

def simulate_trajectory(current_state, velocity, dt):
    # Simulate the robot's motion with the given velocity
    pass

def evaluate_trajectory(trajectory, goal, obstacles):
    # Evaluate the trajectory based on the objective function
    pass
```

**Advantages:**
- Considers the robot's dynamics.
- Suitable for real-time applications.

**Disadvantages:**
- May get stuck in local minima.
- Requires tuning of the objective function.

---

## 6. **A* Search Algorithm**

**Overview:**
- A* is a graph search algorithm that finds the shortest path from a start node to a goal node. It uses a heuristic to guide the search and improve efficiency.

**Components:**
- **Heuristic Function:** Estimates the cost to reach the goal from a given node.
- **Cost Function:** Represents the cost to move from one node to another.

**Algorithm:**
1. Initialize the open list with the start node.
2. Repeat until the open list is empty:
   - Remove the node with the lowest cost from the open list.
   -

   ### A* Search Algorithm (continued)

**Algorithm (continued):**
2. Repeat until the open list is empty:
   - Remove the node with the lowest cost from the open list.
   - If the node is the goal, reconstruct the path and return it.
   - Otherwise, expand the node by generating its neighbors.
   - For each neighbor, calculate the cost and add it to the open list if it is not already in the closed list with a lower cost.

**Code Example:**
```python
import heapq

def a_star(start, goal, obstacles):
    open_list = []
    heapq.heappush(open_list, (0, start))
    came_from = {}
    cost_so_far = {start: 0}

    while open_list:
        current = heapq.heappop(open_list)[1]

        if current == goal:
            return reconstruct_path(came_from, start, goal)

        for neighbor in get_neighbors(current):
            if in_collision(neighbor, obstacles):
                continue
            new_cost = cost_so_far[current] + cost(current, neighbor)
            if neighbor not in cost_so_far or new_cost < cost_so_far[neighbor]:
                cost_so_far[neighbor] = new_cost
                priority = new_cost + heuristic(neighbor, goal)
                heapq.heappush(open_list, (priority, neighbor))
                came_from[neighbor] = current

    return None

def reconstruct_path(came_from, start, goal):
    path = []
    current = goal
    while current != start:
        path.append(current)
        current = came_from[current]
    path.reverse()
    return path

def heuristic(a, b):
    return abs(a[0] - b[0]) + abs(a[1] - b[1])

def cost(a, b):
    return np.linalg.norm(np.array(a) - np.array(b))
```

**Advantages:**
- Finds the shortest path if the heuristic is admissible.
- Efficient for pathfinding in static environments.

**Disadvantages:**
- Computationally intensive for large or complex environments.
- Requires a good heuristic for optimal performance.

---

### 7. **Dijkstra’s Algorithm**

**Overview:**
- Dijkstra’s Algorithm is a graph search algorithm that finds the shortest path from a start node to all other nodes in the graph. It is similar to A* but does not use a heuristic function.

**Components:**
- **Cost Function:** Represents the cost to move from one node to another.

**Algorithm:**
1. Initialize the open list with the start node.
2. Repeat until the open list is empty:
   - Remove the node with the lowest cost from the open list.
   - Expand the node by generating its neighbors.
   - For each neighbor, calculate the cost and add it to the open list if it is not already in the closed list with a lower cost.

**Code Example:**
```python
import heapq

def dijkstra(start, goal, obstacles):
    open_list = []
    heapq.heappush(open_list, (0, start))
    came_from = {}
    cost_so_far = {start: 0}

    while open_list:
        current = heapq.heappop(open_list)[1]

        if current == goal:
            return reconstruct_path(came_from, start, goal)

        for neighbor in get_neighbors(current):
            if in_collision(neighbor, obstacles):
                continue
            new_cost = cost_so_far[current] + cost(current, neighbor)
            if neighbor not in cost_so_far or new_cost < cost_so_far[neighbor]:
                cost_so_far[neighbor] = new_cost
                heapq.heappush(open_list, (new_cost, neighbor))
                came_from[neighbor] = current

    return None

def reconstruct_path(came_from, start, goal):
    path = []
    current = goal
    while current != start:
        path.append(current)
        current = came_from[current]
    path.reverse()
    return path

def cost(a, b):
    return np.linalg.norm(np.array(a) - np.array(b))
```

**Advantages:**
- Finds the shortest path in a weighted graph.
- Does not require a heuristic function.

**Disadvantages:**
- Computationally intensive for large graphs.
- Less efficient than A* when a good heuristic is available.

---

### 8. **Conclusion**

Motion planning algorithms are crucial for enabling the Humanoid Robot Doctor (HRD) to navigate and perform tasks efficiently and safely. Each algorithm has its strengths and weaknesses, and the choice of algorithm depends on the specific requirements and constraints of the application. By understanding and implementing these algorithms, the HRD can achieve precise and reliable motion planning, ensuring successful operation in various environments.

---

**Signatures:**

**Project Owner:** ______________________  
**Lead Engineer:** ______________________  
**Date:** ______________________  

---

This documentation serves as a comprehensive guide to the motion planning algorithms used in the HRD, ensuring that developers can effectively implement and optimize these algorithms for reliable and efficient robot operation.
