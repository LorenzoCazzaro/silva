# Silva
Silva (**S**ilvarum **I**nterpretatione **L**ator **V**alens **A**nalysis) is an abstract interpretation based tool for proving properties of tree ensembles classifiers, in particural we aim at proving stability-related properties of classifiers.

Given a point *x* and a perturbation *P*, silva symbolically computes an overapproximation of *P(x)*, the region of (possibly infinite) points which corresponds to perturbations of *x*, and runs an abstract version of the forest classifier on it, returning a superset of the labels associated to points in *P(x)*. Whenever such set yelds the same output as the classification ona single point in *P(x)*, the concrete classifier is definitively stable on point *x* for perturbation *P*.

When silva returns more labels, it may happen due to the classifier really being not stable, or because of a loss of precision induced by the abstract process. For forests consisting of univariate hard splits only (i.e. *x_i < k*) and *l_\inf* perturbation silva becomes *complete*: every single label in the output set is guaranteed to be associated to at least one point in *P(x)*, thus no false results can be returned.

More information can be found in [Abstract interpretation of decision tree ensemble classifiers](http://www.math.unipd.it/~ranzato/papers/aaai20.pdf).

## Requirements ##

 - Any C99-compatible C compiler

## Installation
To install silva you need to clone or download the source code files from this repository and compile them. There are no additional requirements nor dependencies:

    git clone https://github.com/abstract-machine-learning/silva
or:

    wget https://github.com/abstract-machine-learning/silva/archive/master.zip
    unzip master.zip
then:
    cd silva/src
    make
    make install
The executable file will be available under `silva/bin/silva`.

Every piece of code is documented using [Doxygen](http://www.doxygen.nl/). If you have Doxygen installed and wish to generate the documentation pages (HTML), run:

    cd silva/src
    make doc
Documentation will be available under `silva/doc/html/index.html`.

## Usage
Run `silva` without arguments for a quick online help message. Full syntax is

    bin/silva <classifier> <dataset> [options]
Mandatory arguments:

 - classifier       Path to classifier file, in silva format
 - dataset          Path to dataset file (CSV or binary)

Optional arguments:
 - --max-print-length VALUE         Maximum number of characters to print for long strings, -1 to disable limit (deafult: 32)
 - --voting {max | average | softargmax} Voting scheme to use for forests (default: max)
 - --abstraction {interval | hyperrectangle} Abstract domain to use (default: hyperrectangle)
 - --perturbation {l\_inf} [DATA]    Perturbation to analyse, followed by perturbation-specific options (default: l\_inf 0)
 - --sample-timeout VALUE           Maximum allowed execution time for each sample analysis, in seconds (default: 1)
 - --seed VALUE                     Seed to use for random number generation, reserved for future use (default: 42)
 - --index-of-instance              Index of the instance in the dataset to verify (default: -1)

Perturbation-specific options:
 - l\_inf
   - magnitude	Radius of the L\_inf ball giving the perturbation region

## Example
    silva my_classifier.silva my_dataset.csv --abstraction hyperrectangle --perturbation l_inf 64
Analyses classifier "my\_classifier.silva" using "my\_dataset.csv", adversarial region is generated by an L_\inf ball with radius 64, analysis is performed using hyperrectangles.

## Data set format
See [dedicated section on our data-collection repository](https://github.com/abstract-machine-learning/data-collection#dataset-format), from which you can also download some ready-to-use [datasets](https://github.com/svm-abstract-verifier/data-collection/tree/master/datasets) and [models](https://github.com/abstract-machine-learning/data-collection/tree/master/models). The python scripts in the folder *trainers* of the data-collection repository are also contained in the *src/trainers* folder of this repository, with some small modifications.
