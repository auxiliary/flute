Flute
===

A simple tool for running serial/parallel task pipelines. 

## Motivation

The benefits of a data processing pipeline are:

- Being able to easily feed in inputs and running tasks in a data processing environment that otherwise you would have to type in the command line everytime. 
- Forcing data analysts to write what they run on the command line in a configuration file so that others (or themselves) will be able to easily follow, remember and redo at a later time. 
- Keep track of everything that happened to a data input and be able to easily rerun it all if needed.

There are a lot of tools out there for running data processing pipelines. However I couldn't find a lightweight tool with the following features:

- Would not make you change the code you already have
- Support any language/executable by supporting files 
- Providing the ability to run a task in parallel for its inputs

Note: Flute is currently in a very early stage (a very simple script really). A lot of decent ideas can be added to it. If you thought of a feature, feel free to contribute to the code or add the feature request in the [issues](https://github.com/auxiliary/flute/issues). 

## Installation

`sudo python setup.py install` or if you want to install locally: `python setup.py install --user`

## Usage

Create a configuration file called `config.ini` and write the configuration of your pipeline there. 
Then run `flute <pipeline name>` to run the pipeline. 

The configuration file may contain several tasks, pipelines and comments with in the `ini` format. Here's an example:

```ini
[task sample_task]
file = somefile.py
input = input1.csv input2.csv input3.csv
parallel = true

[task sample_task2]
file = anotherfile.sh
input = stdin

[pipeline pipe1]
tasks = sample_task sample_task2
```

This pipeline (`pipe1`) runs the first task (`sample_task`) first and then runs sample_task2. For the first task, 3 copies of the file `somefile.py` are executed with the inputs of input1.csv, input2.csv and input3.csv respectively. The output of this task is then fed into sample_task2 that's run serially. The keyword `stdin` tells Flute to take the input from the pipe (meaning from the first task in this case). 

For running this sample pipeline, you would run:

`flute pipe1`

Variables can be used in the configuration files as they're normal ini files. So for example, we can have:

```ini
[task task1]
file = variable_extraction.py
input = input_file.nc input_file2.nc

[task task2]
file = quantization.py
input = ${task task1:input}
```
