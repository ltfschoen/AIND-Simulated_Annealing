## About

* Exercise implementing the Simulated Annealing algorithm in a Jupyter Notebook App
(acronym for first set of targeted languages Julia, Python, and R) that
combines code and rich text elements for data science analysis in real-time,
and includes an IPython kernel back-end.
Apply the implementation to solve the Traveling Salesman Problem (TSP) between US state capitals.

* Guide on Jupiter Notebook https://www.datacamp.com/community/tutorials/tutorial-jupyter-notebook#gs.WPbJuKE

## Setup

* Install Jupyter Notebook App with Miniconda http://jupyter.readthedocs.io/en/latest/install.html
    ```
    pip3 install --upgrade pip setuptools
    pip3 install jupyter
    pip3 install json copy numpy matplotlib sys math
    ```

* Launch Jupyter Notebook App `jupyter notebook AIND-Simulated_Annealing.ipynb`

* Run code:
    * Click any section containing a code snippet and go to menu: Cells > Run All,
    then view the outputs shown below the code snippets

## Solution

# Part I: Add to bottom of Overview code snippet

```
# show Python version being used
import sys
!pip3 install math
import math
print("{}".format(sys.version))
# show Python 2.7 and 3.6 executable directories
!which pip
!which pip3
# show jupyter config paths http://jupyter.readthedocs.io/en/latest/projects/jupyter-directories.html
!jupyter --paths
```

# Part II: Add to bottom of Part II code snippet

```
!pip3 install math
import math

min_T = 1e-10            # 0.0000000001
ct = 1                   # iteration
current_state = problem  # initial problem state
delta_E = 0              # represents change in the score E

def random_pick_for_probability(states, probabilities):
    x = random.uniform(0, 1)
    cumulative_probability = 0.0
    for item, item_probability in zip(states, probabilities):
        cumulative_probability += item_probability
        if x < cumulative_probability: break
    return item

while True:
    T = schedule(ct)

    # as temperature approaches 0 return early when temperature
    # falls below termination condition value
    # (since temperature reaching 0 gives undefined answer)
    if T < min_T:
        return current_state

    successors = problem.successors()

    # select positions randomly from neighboring region
    next_index = random.randint(0, len(successors))
    next_state = successors[next_index]
    delta_E = next_state.get_value() - current_state.get_value()

    # if new random position improves score E (up the gradient)
    # then select it (up the gradient)
    if delta_E > 0:
        current_state = next_state

    # if new random position lessons score E (down the gradient)
    # then select it based on a probability
    # reference: https://www.safaribooksonline.com/library/view/python-cookbook-2nd/0596007973/ch04s22.html
    else:
        prob = math.exp(delta_E/T)
        current_state = random_pick_for_probability( \
                        [current_state, next_state], [1-prob, prob])
    ct += 1
```

# Part III: Add to bottom of successors() function in code snippet

```
successor_problems = []
current_path = self.path
path_length = len(current_path)

for i in range(0, path_length):
    new_path = copy.deepcopy(self.path)
    new_path[i], new_path[(i + 1) % path_length] = new_path[(i + 1) % path_length], new_path[i]
    successor_problems.append(TravelingSalesmanProblem(new_path))

return successor_problems
```

# Part III: Add to bottom of get_value() function in code snippet

```
current_path = self.path
path_length = len(current_path)
cost_total = 0

for i in range(0, path_length):
    current_city_coords = np.array(current_path[i % path_length][1])
    next_city_coords = np.array(current_path[(i + 1) % path_length][1])
    cost_total += np.sqrt(np.sum(np.square(current_city_coords - next_city_coords)))

    # Alternative:
    # x1, y1 = self.coords[i]
    # x2, y2 = self.coords[(i + 1) % path_length]
    # cost_total += math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2)
return -cost_total
```

# Part IV:

* Merge all code snippets into single code snippet to overcome error