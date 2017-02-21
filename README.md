# UVIC-SLAM-HW3

## Installation

### Gtsam

Lines starting with `#` are software installations that are possibly not needed, depending on your current state of Ubuntu.

```
$ cd ~
$ wget https://github.com/indoor-reality/libgtsam/archive/master.zip
#$ sudo apt-get install unzip
$ unzip master.zip
$ cd libgtsam-master
$ mkdir build
$ cd build
#$ sudo apt-get install g++
#$ sudo apt-get install cmake
#$ sudo apt-get install libboost-all-dev
$ cmake ..
$ sudo make install
```

### HW3

Install graphviz if not already installed. Graphviz is a command-line tool for visualizing graphs.

```
$ sudo apt-get install graphviz
```

## Related documentation

  - The GTSAM tutorial should be of great help for this HW:

  https://pdfs.semanticscholar.org/5f27/7ad6aff683be07bed616897a159464354333.pdf

  - For the part II of the HW, you may want to consult the format of the SLAM datasets:

  http://www.lucacarlone.com/index.php/resources/datasets

  We are using the dataset named M3500, with file format 'g2o', which is a Manhattan-type map with 3500 poses or 'vertices', and more than 4500 factors or 'edges'.

## Homework

This HW3 has two parts: the first part is a small exercise to get in contact with GTSAM. The second part is a full SLAM problem.

See 'Compiling and executing' below for further instructions.

### Part I: build a simple SLAM problem and solve it. 

To make this part, you need to get inspiration from Listing 1 and Listing 2 in the GTSAM tutorial text.

  - Build a prior for the first state:
    - prior: `[x,y,th] = [0,0,0], sigmas = [0.2,0.2,0.1]`
  - Build a chain of odometry steps between consecutive states:
    - step 1: `[dx,dy,dth] = [1.1, 0.02, 0.0 ]; sigmas = [0.3,0.3,0.1]`
    - step 2: `[dx,dy,dth] = [0.9, 0.04, 1.6]`
    - step 3: `[dx,dy,dth] = [1.05, -0.08, 1.6]`
    - step 4: `[dx,dy,dth] = [1.01, 0.01, 1.6]`
  - Solve the resulting graph.
  - Visualize it.
  - Compute the marginal covariances of each node
  - Add a loop closure to the graph, between nodes 2 and 5:
    - loop: `[dx,dy,dth] = [0.98, -0.06, 1,6]; sigmas = [0.3,0.3,0.1]`
  - Solve it again.
  - Visualize it. 
  - Compute the marginal covariances of each node
  - Compare the covariances with the case of no loop closure.
  
### Part II: build a complete SLAM problem

To make this part, you need to extend the work on part I by reading a large dataset containing information about the nodes and edges of a factor graph.

  - Read the dataset
  - Parse it and build the graph:
    - For each node, initialize the state
    - For each edge, create a odometry factor
  - save the graph of the problem prior to solving: file `<input_name>.dot`
  - visualize the problem prior to solving, 
    - In the linux terminal, type 
    ```
    neato -x -Gorientation=landscape -Nfontsize=30 -Tpdf <input_name>.dot > <output_name>.pdf
    ``` 
    and visualize the document. 
  - Compare and comment the results.

### Compiling and executing

#### Part I
```
$ cd ~/SLAM-HW3/part_1
$ mkdir build
$ cd build
$ cmake ..
$ make
$ ./PoseSLAM
$ neato -x -Nfontsize=30 -Tpdf NonOptimizedGraph.dot > non_optimized_graph_part_1.pdf
$ neato -x -Nfontsize=30 -Tpdf OptimizedGraph.dot > optimized_graph_part_1.pdf
```

#### Part II
```
$ cd ~/SLAM-HW3/part_2
$ mkdir build
$ cd build
$ cmake ..
$ make
$ ./PoseSLAM
$ neato -x -Gorientation=landscape -Nfontsize=30 -Tpdf NonOptimizedGraph.dot > non_optimized_graph_part_2.pdf
$ neato -x -Gorientation=landscape -Nfontsize=30 -Tpdf OptimizedGraph.dot > optimized_graph_part_2.pdf
```
