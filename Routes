import sys
from typing import List, Tuple, Set, Any, Dict

# Check if you have provided enough arguements
if len(sys.argv) != 2:
    print("Usage: python3 Routes.py <inputfilename>")
    sys.exit()

"""
A helper function to open the given file and output a dictionary representation
of the tree to later build the tree with.
"""
def parsing_helper() -> dict:
    # Open the file
    f = open(sys.argv[1], "r")

    rd = f.readlines()

    # Make and return a dictionary representation of the input to later
    # build the tree with.
    my_dict = {}

    for line in rd:
        cut_point = 0

        for x in range(len(line)):
            if line[x] == ":":
                cut_point = x

                sudo = (line[x+1:].strip()).split(",")

                result = []
                for x in sudo:
                    result.append(x.strip())

        my_dict[line[:cut_point]] = result

    return my_dict

"""
Funcion responsible for building the graph representation from the given dictionary,
gotten from parsing_helper
"""
def build_graph(tree_dict: dict):

    graph = Boardgame_graph()

    # First add each vertex to the graph
    for key in tree_dict:
        graph.add_vertex(key)


    # At this point all the vertices have been initialized into the graph
    # and we are able to loop over again to add the edges between vertices
    for key in tree_dict:

        if key[:2] == "v_" or key[:2] == "c_":
            updated_key = key[2:]

        else:
            updated_key = key
        # Iterate through every neighbor of vertex and add to edge
        for value in tree_dict[key]:

            if value[:2] == "v_" or value[:2] == "c_":
                updated_value = value[2:]

            else:
                updated_value = value

            graph.add_edge(updated_key, updated_value)

    return graph


# A representation of a node in our graph.
class Graph_node(object):

    name: str
    neighborhood: Any

    def __init__(self, name, neighborhood):
        self.name = name
        self.neighborhood = []

# A representation of a Village node in our graph.
class Village(Graph_node):
    name: str
    neighborhood: Set[Graph_node]

    def __init__(self, name):
        self.neighborhood = set()
        self.name = name

# A representation of a City node in our graph.
class City(Graph_node):
    name: str
    neighborhood: Set[Graph_node]

    def __init__(self, name):
        self.neighborhood = set()
        self.name = name

# A representation of a Start node in our graph.
class Start(Graph_node):
    name: str
    neighborhood: Set[Graph_node]

    def __init__(self):
        self.name = "start"
        self.neighborhood = set()

# A representation of an End node in our graph.
class End(Graph_node):
    name: str
    neighborhood: Set[Graph_node]

    def __init__(self):
        self.name = "end"
        self.neighborhood = set()



# A representation of the graph that stores the vertices of our board game
class Boardgame_graph:

    # vertices stored as dictionaries
    # (key, value): (name, node object)
    vertices: Dict[str, Graph_node]

    def __init__(self):
        self.vertices = {}

    # Add a vertex into our graph
    def add_vertex(self, vertex: str ):

        if vertex == "start":
            new_vertex = Start()
            self.vertices["start"] = new_vertex


        elif vertex == "end":
            new_vertex = End()
            self.vertices["end"] = new_vertex


        elif vertex[0] == "v":
            new_vertex = Village(vertex[2:])
            self.vertices[vertex[2:]] = new_vertex


        elif vertex[0] == "c":
            new_vertex = City(vertex[2:])
            self.vertices[vertex[2:]] = new_vertex


    # Add an edge between two given vertices
    def add_edge(self, v1_name: str, v2_name: str):

        if v1_name in self.vertices and v2_name in self.vertices:
            # Add the edge between the two vertices
            v1 = self.vertices[v1_name]
            v2 = self.vertices[v2_name]

            v1.neighborhood.add(v2)
            v2.neighborhood.add(v1)
        else:
            # Raise a ValueError if both nodes are not in our graph
            raise ValueError



    # Find the wanted node in the graph, given the name
    def find_node(self, name):

        return self.vertices.get(name)


"""
The algorithm for the first scenario, returns an integer for the number of
possible routes using the first scenario
"""
def scenario_one(start: Start, visited: Set) -> int:

    # A counter for the amount of possible routes found
    route_counter = 0

    # Base case: For when the recursion reaches to the end
    if start.name == "end":
        return 1

    # Loop through every neighbor of the vertex we are examining and recurse
    # into every village, city, or end neighbor which we have not visited
    for n in start.neighborhood:

        if n not in visited and not isinstance(n, Start):
            new_visited = visited.copy()
            new_visited.add(n)
            # Sum to keep track of number of routes
            route_counter += scenario_one(n, new_visited)

    return route_counter

"""
The algorithm for the second scenario, returns an integer for the number of
possible routes using the second scenario
"""
def scenario_two(start: Start, visited: Set) -> int:

    # A counter for the amount of possible routes found
    route_counter = 0

    # Base case: For when the recursion reaches to the end
    if start.name == "end":
        return 1

    # Loop through every neighbor of the vertex we are examining and recurse
    # into every village, city, or end neighbor which we have not visited
    for n in start.neighborhood:

        if n not in visited:
            # If neighbor is a city, we recurse into it but do not add it to
            # visited
            if isinstance(n, City):
                new_visited = visited.copy()
                route_counter += scenario_two(n, new_visited)
            # If neighbor is village or end, recurse into it while adding to
            # visited
            elif isinstance(n, Village) or isinstance(n, End):
                new_visited = visited.copy()
                new_visited.add(n)
                route_counter += scenario_two(n, new_visited)


    return route_counter

"""
The algorithm for the third scenario, returns an integer for the number of
possible routes using the third scenario
"""
def scenario_three(start: Start, visited: Set, v_pass: bool) -> int:

    # A counter for the amount of possible routes found
    route_counter = 0

    # Base case: For when the recursion reaches to the end
    if start.name == "end":
        return 1

    for n in start.neighborhood:

        if n not in visited:
            # If neighbor is a city, we recurse into it but do not add it to
            # visited
            if isinstance(n, City):
                new_visited = visited.copy()
                route_counter += scenario_three(n, new_visited, v_pass)


            # Go to a village regularly and not use the card
            elif isinstance(n, Village) or isinstance(n, End):
                    new_visited = visited.copy()
                    new_visited.add(n)
                    route_counter += scenario_three(n, new_visited, v_pass)

        else:
            # if neighbor is a village and is in visited, we are able to recurse
            # into it if we still have v_pass
            if isinstance(n, Village) and v_pass:

                new_visited = visited.copy()
                # important to pass FALSE as v_pass value in recursive call
                # to represent using village pass
                route_counter += scenario_three(n, new_visited, False)


    return route_counter


parsed_dict = parsing_helper()
board_graph = build_graph(parsed_dict)

print("Total routes for Scenario 1: {}".format(scenario_one(board_graph.find_node("start"), set())))
print("Total routes for Scenario 2: {}".format(scenario_two(board_graph.find_node("start"), set())))
print("Total routes for Scenario 3: {}".format(scenario_three(board_graph.find_node("start"), set(), True)))
