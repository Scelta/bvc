# About BVC

Bioinformatics Version Control (BVC) is a solution designed to manage **modern** projects with **iterated scripts** and **huge data**, especially for sequencing data. Another challenge comes from interdisciplinary projects, which requires **multiple teams** with experts in various fields. How to collaborate with team members to understand codes and reproduce (validate) results from each other are becoming a kind of new bottle-neck during practicing.

From individual to team, efficient and convenient methodology and toolbox are needed to manage our project well. This repository shares my ideology about how to organize such projects with public excellent tools to make ourselves more efficient and professional.

## Core concepts & ideology

- File types
  - Documents/Metadata
  - Codes
  - Data


- Codes
  - Tools
  - Running scripts
  - Visualizations


- Data
  - Lv1 - Raw data (generally huge)
  - Lv2 - Intermedia data
  - Lv3 - Output (profile/annotation/table)


- Version Control
  - For code: git
  - For data: dvc


- Collaborate
  - For yourself
  - For your team
  - For cooperating teams

- Workflow
  - For project files organization
  - For Bioinformatics analysis


- Coding style


## Getting started

To make it easy for you to understand the idea of bvc, here's some basic steps for practice.

## Installation
Git is necessary for code version control. To install it independently, please refer to the [offical website](https://git-scm.com/downloads).
Though [dvc](dvc.org) is not necessary, it helps saving your life, so I also recommend to install it.

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
## Track a code and its result

Assume we have a task to compute
```bash
mkdir -p {Assay,Results}
# Assay for analysis running scripts
# Results for scripts generated outputs

cd Assay

echo '#!/usr/bin/env bash
export a=0
for i in $(seq 1 2 2000)
do
  a=$(($a + $i))
  echo $a
done > ../Results/out.calculate.tsv' > run.calculate.sh

sh run.calculate.sh

cd ..
# IT's very important to always run the script at where it locate so the
# working directory and the script directory are the same. Otherwise
# it may mistakenly output its results by using relative path.
```
By checking the content of `Results/out.calculate.tsv`, we now track it by dvc:
```bash
dvc add ../Results/out.calculate.tsv

```
By entering `git status`, the following content will shown:
```
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   run.calculate.sh
        new file:   ../Results/.gitignore
        new file:   ../Results/out.calculate.tsv.dvc
```
Here:
- `run.calculate.sh` is the run script we used to record and execute during analysis
- `../Results/.gitignore` prevent the data itself `out.calculate.tsv` from tracing by git.
- `../Results/out.calculate.tsv.dvc` record the metadata(fingerprint) of `out.calculate.tsv`

The real data of `out.calculate.tsv` was moved into dvc cache and a link pointed back to `../Results/out.calculate.tsv`. By this way, only the data's link and its corresponding metadata will be tracked by git, thus git storage volume saved and efficient ensured.

> More detail for dvc commands: [DVC Get Started](https://dvc.org/doc/start)

Now commit those changes to git:
```bash
git commit -m 'record process of bash calculate' run.calculate.sh ../Results/.gitignore ../Results/out.calculate.tsv.dvc
```

## Set shared DVC cache location
By default, all real data are stored under `/.dvc/cache`. For some team, whose workshop deposited in High Performance Cluters (HPC), may feel happy to have a central share space to store generated data to avod massive copies and duplicates with mixture versions.
```bash
mkdir -p /mnt/d/repo/dvc-cache #change this path to anywhere you prefer
mv .dvc/cache/* /mnt/d/repo/dvc-cache # If you still want a copy of local cache, `cp` is also fine.

# change permission to aloow group members read/write:
find /mnt/d/repo/dvc-cache -type d -exec chmod 0775 {} \;
find /mnt/d/repo/dvc-cache -type f -exec chmod 0444 {} \;
chown -R myuser:ourgroup /mnt/d/repo/dvc-cache # replace myuser:ourgroup to the name of your real account and group name.
```
Then configure the shared cache:
```bash
dvc cache dir --local /mnt/d/repo/dvc-cachedvc-cache

dvc config --local cache.shared group
dvc config --local cache.type symlink
```
::warning:: It's very important to config the shared cache by `--local` to avoid conflicts especially when you have multiple shared cache locations (Like workshop deposited in different HPCs, laptops, PCs).
Each cloned repo should be configured the shared cache according its actual location.
> More detail: [How to Share a DVC Cache](https://dvc.org/doc/user-guide/how-to/share-a-dvc-cache)

## Set remote DVC storage location
Assume we receving a backup of all project data managed by dvc. This disk then mounted to the path `/mnt2/MEER-dvc`:
```bash
dvc remote add hdd /mnt2/MEER-dvc
dvc pull -r hdd
```

For preparing such backup:
```bash
dvc remote add hdd /mnt2/MEER-dvc
dvc push -r hdd
```
> More detail: [External Dependencies](https://dvc.org/doc/user-guide/external-dependencies)

## Now sync all your work in a git manner

For push:
```bash
git remote add origin /path/to/your/remote/repo
git push origin --all
```
For pull:
```bash
git pull origin --all
```

## Contributing
This repository was created by [Chao](https://github.com/Scelta). I very much encourage contributions by users practicing this workflow. If anyone would like to add an implementation or provide suggestions, please feel free to raise an issue.

## Reference
Following I list some books and papers sharing excellent ideas and inspired me.

[*Bioinformatics Data Skills*](https://www.oreilly.com/library/view/bioinformatics-data-skills/9781449367480/) by Vince Buffalo, 2015

[*Git for Teams*](https://www.oreilly.com/library/view/git-for-teams/9781491911204/) by Emma Jane Hogbin Westby, 2015

[*Pro Git*](https://git-scm.com/book/en/v2) by Scott Chacon and Ben Straub, 2014

[DVC](https://dvc.org/) Documentation

## License
For open source projects, say how it is licensed.
