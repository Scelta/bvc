# About BVC

Bioinformatics Version Control (BVC) is a solution designed to manage **modern** projects with **iterated scripts** and **huge data**, especially for sequencing data. Another challenge comes from interdisciplinary projects, which requires **multiple teams** with experts in various fields. How to collaborate with team members to understand codes and reproduce (validate) results from each other are becoming a kind of new bottle-neck during practicing.

From individual to team, efficient and convenient methodology and toolbox are needed to manage our project well. This repository shares my ideology about how to organize such projects with public excellent tools to make ourselves more efficient and professional.

## Core concepts

- File types

- Codes

- Data

- Version Control

- Collaborate

- Workflow

- Coding style


## Getting started

To make it easy for you to understand the idea of bvc, here's some basic steps for practice.

## Installation
Git is necessary for code version control. To install it independently, please refer to the [offical website](https://git-scm.com/downloads).
Though dvc is not necessary, it helps saving your life, so I also recommend to install it.

As a user of linux, macOS and windows, I suffered painful experiences to install packages among different OSs. Finally I found conda partially solved this issue, providing unified working environment in multiple locations and different OSs.
As this repository is originally written for MEER project, here I provided a conda environment including git and dvc as well as other commonly used packages.


```bash
conda env create -f bvc/MEER_env.yaml
# This may take a while ...

conda activate meer
```

## Initialize git and dvc
For your first work repository:
```bash
mkdir psudo-project
cd psudo-project
git init
dvc init
```
Now some hidden dvc config files are added into your git. You can see it by typing:
```bash
git status
```
with following information:
```
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   .dvc/.gitignore
	new file:   .dvc/config
	new file:   .dvcignore
```

Commit those git-added files:
```bash
git commit -am 'init git and dvc'
```
### track a code and its result


## Support
Tell people where they can go to for help. It can be any combination of an issue tracker, a chat room, an email address, etc.

## Roadmap
If you have ideas for releases in the future, it is a good idea to list them in the README.

## Contributing
State if you are open to contributions and what your requirements are for accepting them.

For people who want to make changes to your project, it's helpful to have some documentation on how to get started. Perhaps there is a script that they should run or some environment variables that they need to set. Make these steps explicit. These instructions could also be useful to your future self.

You can also document commands to lint the code or run tests. These steps help to ensure high code quality and reduce the likelihood that the changes inadvertently break something. Having instructions for running tests is especially helpful if it requires external setup, such as starting a Selenium server for testing in a browser.

## Authors and acknowledgment
Show your appreciation to those who have contributed to the project.

## License
For open source projects, say how it is licensed.

## Project status
If you have run out of energy or time for your project, put a note at the top of the README saying that development has slowed down or stopped completely. Someone may choose to fork your project or volunteer to step in as a maintainer or owner, allowing your project to keep going. You can also make an explicit request for maintainers.
