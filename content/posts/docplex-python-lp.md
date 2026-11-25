---
title: "在 OS X 上通过 DOcplex 用 Python 求解 LP"
date: 2018-09-10
categories: [技术]
tags: [Python, DOcplex, 线性规划]
draft: false
---

Use DOcplex Python library to write the mathematical model in Python.

**1、** Install Docplex.
Python3-m install docplex

**2、** Install CPLEX_Studio128/python
```
cd /Applications/CPLEX_Studio128/python
python3 setup.py install
```

The documentation can be found here: https://github.com/IBMDecisionOptimization/docplex-doc
The examples can be found here: https://github.com/IBMDecisionOptimization/docplex-examples

**3、** Set up the     prescriptive model

a. Create the model
All objects of the model belong to one modelinstance.
```
#first import the Model class from docplex.mp
from docplex.mp.model import Model
#create one model instance, with a name
m = Model(name='telephone_production')
```

b. Define the decision variables
```
#by default, all variables in Docplex have a lower bound of 0 and infinite upperbound
desk = m.continuous_var(name='desk')
cell = m.continuous_var(name='cell')
```

c. Set up  the     constraints
```
#write constraints # constraint #1: desk production is greater than 100 m.add_constraint(desk>= 100)
#constraint #2: cell production is greater than 100
m.add_constraint(cell >= 100)
#constraint #3: assembly time limit
ct_assembly = m.add_constraint( 0.2 * desk +0.4 * cell <= 400)
# constraint #4: paiting time limit
ct_painting = m.add_constraint( 0.5 * desk +0.4 * cell <= 490)
```

d. Express the objective

```
m.maximize(12* desk + 20 * cell)
```
all     arithmetic operations (+, *, -) are done using Python operators
comparison     operators used in writing linear constraint use Python comparison     operators too.

e. Print information about model
```
m.print_information()
```

f.
In any case, Model.solve() returns     a solution object in Python, containing the optimal values of decision     variables, if the solve succeeds, or else it returns None.
```
s = m.solve(url=url, key=key)
m.print_solution()
```
（Ifurl and key are None, the Modeling layer will look for a local runtime,otherwise will use the credentials.）

Bydefault, DOcplex variables are unbounded: their upper bound is infinite (but their lower bound is zero).

——————————————————————————————

The IBM CPLEX Optimizers to solve LPproblems in CPLEX include:
Simplex Optimizer
Dual-simplex Optimizer
Barrier Optimizer

**1、** The Simplexalgorithm
It is the basis of many optimization algorithms, it is an iterativemethod.
The CPLEXOptimizers are particularly efficient and can solve very large problemsrapidly. You can tune some CPLEX Optimizer parameters to change the algorithmicbehavior according to your needs.

**2、** The Dual simplealgorithm
The dual of a LP
The Duality is important in linear programming.Every LP problem has an associated LP problem known ad its dual. If primal problem is a minimizationproblem, then the dual problem is a maximization problem and vice versa.

Dual prices
For each constraint in the primal problem, its associateddual price indicates how much the dual objective will change with a unit changein the right hand side of the constraint.
For example, the dual price of a binding resource constraintcan be used to determine whether more of the resource should be purchased ornot.

**3、** Slack values
The slack value for a binding constraint is always zero, thatis, the constraint is met exactly.
You can determine which constraints are binding in a solutionby examining the slack values with DOcplex.
```
#now get slack value for assembly constraint: expected value is 40
print('* slack value for assembly time constraint is: {0}'.format(ct_assembly.slack_value))
# getslack value for painting time constraint, expected value is 0.
print('* slack value for painting time constraint is: {0}'.format(ct_painting.slack_value))
```

**4、** Degeneracy
It is possiblethat multiple non-optimal solutions with the same objective value exist.
As the simplex algorithm attempts to move in the direction ofan improved objective value, it might happen that the algorithm starts cyclingbetween non-optimal solutions with equivalent objective values. This is knownas degeneracy.
Modern LP solvers, such as CPLEX Simplex Optimizer, havebuilt-in mechanisms to help escape such cycling by using perturbationtechniques involving the variable bounds.

Setting a LP algorithm with Docplex
Users can changethe algorithm by editing the lpmethod parameter of the model. Wewon't go into details here, it suffices to know this parameter accepts aninteger from 0 to 6,
where
0 denotesautomatic choice of the algorithm,
1 is for primalsimplex,
2 is for dualsimplex, and
4 is forbarrier...

For example, choosing the barrieralgorithm is done by setting value 4 to this parameter. We access the parameters property of the model and from there, assignthe lpmethod parameter
```
m.parameters.lpmethod = 4
m.solve(url=url, key=key, log_output=True)
```

Barrier method
In graphicalterms, the simplex algorithm starts along the edge of the feasible region andsearches for an optimal vertex.
The barrier methodstarts somewhere inside the feasible region – in other words, it avoids the“barrier” that is created by the constraints, and burrows through the feasibleregion to find the optimal solution.

![配图1](/images/p2_1.png)

Presolve
CPLEX Optimizerprovides a presolve procedure.
Presolve evaluatesthe model formulation before solving it, and attempts to reduce the size of theproblem that is sent to the solver engine.
A reduction inproblem size typically translates to a reduction in total run time.

reference：https://ibmdecisionoptimization.github.io/tutorials/html/Linear_Programming.html
