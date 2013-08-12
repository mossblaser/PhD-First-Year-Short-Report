Short Report Plan
=================

1000 Words to explain.

Introduction/Motivation
-----------------------

Brain complex, we model it to understand better. Spaun is a pretty cool model
which exhibits realistic behaviour which intuatively will be useful to
researchers but is super slow to run on a computer, 2.5 hours per second.

Machines have been built to simulate these networks which diverge from
traditional super computers due to the unsuitability for the task. In
particular, these machines differ greatly in their interconnection network
topology. This work hopes to develop an improved topology for use in a future
simulator.

Artificial Neural Networks
--------------------------

ANNs essentially simulate large sparsely connected graphs of nodes (neurons)
which send signals (spikes) along their edges. Fast simulation means getting
these spikes around fast. Spikes don't need any associated information except
that they've happened and their timing is important. As a result, transmission
should be low-latency.

Super-Computers
---------------

Conventional supercomputers give impressive levels of computation but limit the
size of hetworks as they cannot handle the communication requirements. Super
comuters typically optimise for bandwidth of large transfers rather than latency
and bandwidth of many tiny packets. The current Top500 list of leading super
computers all feature such networks but with latencies for packets in the tens
of thousands of CPU cycles.

Topologies typically have high powered compute nodes with tree/torus networks
simple (figures). Trees and tori are good for locality, which is good for
neurons because they tend to fire locally, though since the brain is essentially
a 2D sheet, a torus/mesh fits the problem better than a tree.

Blue Brain attempts to use super computers and can simulate very accurately very
small systems (requiring little communication) but with great detail in the
neuron behaviour (making use of computational resources). Due to the limit in
the network size, these systems can't be used for high-level simulations like
Spaun.

Brain Simulators
----------------

Others have tried to produce machines optimised for the job. Many inspired by
analogue nature of the brain replace conventional computational nodes with
analogue electronics which power-efficiently implement popular models. Many of
these have been experimental and only supported relatively small numbers of
neurons [cite some early examples] and thus have been able to simply connect
all-to-all. This really doesn't scale.

Current topologies have been developed with fairly limited scalability.

### Neurogrid

Neurogrid features chips which can simulate 65,025 neurons simultaneously but
serialises their communication. As a result of this, the time taken to send a
message to $N$ neurons is $O(N)$. Chips are combined in a tree to build larger
systems. [Why is this limited?]

### BrainScaleS

BrainScaleS uses a silicon wafer worth of chips in a mesh network. This network
is limited in size by silicon manufacturing process constraints and must be
extended using off-wafer connections to other wafers (proposed using 10 gbit/s
Ethernet) currently in a star network. This has obvious scaling limits.

### SpiNNaker

Finally, the focus of my work so far: SpiNNaker has many small processors
connected via a torus network whose size is only limited by the address space
available to neurons. Network optimised for small (maybe zero-data) packets.
Unlike the first two it runs simulations in software making it far more
flexible, easier with current understanding of electronics but less power
efficient.

Here's how it looks in implemented hardware: cores, chips and boards.

Preliminary Work
----------------

Current work has focused on SpiNNaker and possible extensions to it.

### SpiNNaker Topology

To make things practical, board-to-board links use different technology to
within-board links as hundreds of wires is impractical between boards but fine
on-board. Built a simulator which simulates a model of the network to examine
how these links affect performance. Found that these links do introduce a
significant penalty but one which is probably not an issue for current
simulation needs but may be later when latency becomes more sensitive.

### SpiNNaker Wiring

Wiring up big machines is tough. Boards must be placed into cabinets and wires
run by hand between them. Three major constraints: cost of the cables, length to
allow tech to work fast and complexity to make it possible to wire up by hand.
Work was done to propose wiring schemes for large SpiNNaker machines of 1,200
boards.

Cost constraint solved by using the high speed links tested previously.

In SpiNNaker you have a torus meaning you get links which span one end of the
system to another. This violates the length requirement. To solve this the
system is 'folded'. This is illustrated in a ring network (figure) and easily
generalises to a torus. This removes the long wrap-around link eliminating long
wires from the system.

A folded system, when mapped into cabinets and racks, becomes very non-uniform.
Luckily it isn't as bad as it looks, especially for larger systems (see figure
of full machine) but listing every connection and expecting a human to do it
isn't viable. Regularity can be found by dividing the connections up by logical
direction and whether they leave the cabinet they're in or not. For a large
machine the number of instructions therefore drops from 3,600 (one per
connection) to 53 (with each rule being used to connect hundreds of wires).

### Small-World Super-Computers

The connections in the brain, along with many graphs observed in nature exhibits
the 'small world' property, first described by Frigyes Karinthy in 1929, which
states that the maximum length of a path between any two nodes in a small-world
graph is very short. This idea is popularly known as the theory of six degrees
of seperation and is the basis of theoretical work carried out by Watts and
Strogatz who proposed an algorithm for generating random graphs with the
small-world property.

This work extends this model to generate torus-like networks (as in SpiNNaker)
augmented with random connections and found that an great reduction in the
number of links messages must traverse in the network was achieved for a modest
increase in the number of connections in the network.

Based on the work on wiring practicality, this system was extended to generate
networks which met the wiring practicality criteria outlined above. Despite
severe restrictions on allowed connections the new topology still resulted in
improved performance.

Research Plan
-------------

[Gantt Chart]

Initial work will focus on the development of a simulator for SpiNNaker's
interconnect topology to contribute to work comparing actual prototype SpiNNaker
hardware against various software models of its behaviour. This work is hoped
for Journal publication later this year. This work is expected to take 

With the development of a suitable simulator, this will be extended to perform
more detailed experiments on the semi-random small-world networks examined in
the preliminary work.

With the process for extending the simulator and performing comparisons proven
for the small-world network case, other less conventional topologies such as
express cubes, which offer latency advantages over standard torus networks will
be examined and compared.

Work will continue looking at the impact of 'place and route', the task of
allocating parts of a neural simulation to spinnaker's processor resources and
routing spikes between them. This will provide a further comparison to be drawn
between the topologies examined as place and route can be extremely
computationally expensive. In addition, this will consider the effect of
multicast traffic.

The penultimate stage of the project will be to compare available link
technologies. The tech used in SpiNNaker is no longer available and a
replacement is needed.

The final goal of the project will be to bring together the work to suggest a
new architecture for a next-generation SpiNNaker.
