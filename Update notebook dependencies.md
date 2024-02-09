# Updating CASES modules requirements.txt
First install git and [pipenv](https://pipenv.pypa.io/en/latest/installation.html) if you don't have them.

First clone the git repository to your local drive:
```
$ git clone git@github.com:cases-umd/Japanese-American-WWII.git
```
This will copy the files to a new local folder called "Japanese-American-WWII", or you can specify the folder name by adding it to the clone command.

Next use pipenv to create a new Python virtual environment for your project. Your Python version should be up to date, version 3.9 or later. From within the project directory execute this:
```
$ cd Japanese-American-WWII
$ pipenv --python 3.10
```

Pipenv will add a file "Pipfile" in your project folder. It will copy the relevant dependencies and their versions from the old "requirements.txt" file, if one exists in the cloned repository. Nothing has been installed yet in your virtual environment.

The next step is to scrub the old version numbers from the Pipfile. Open the Pipfile in an editor and under `[packages]` replace all the specific version numbers and other notation within double quotes with an asterisk, indicating "any version". You will also add a development dependency on Jupyter, so that you can run Jupyter Lab from within this virtual environment. The file will look something like this:
```
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
matplotlib = "*"
numpy = "*"
pandas = "*"

[dev-packages]
jupyter = "*"

[requires]
python_version = "3.10"
python_full_version = "3.10.6"
```
Because the last two lines are mutually exclusive according to Binder, we will remove the last line, which specifies the "python_full_version". So the `[requires]` block should only have the "python_version" line.

Now we will ask pipenv to find and install the dependencies specified in Pipfile, including the development dependency on Jupyter.
```
$ pipenv install --dev
```
Pipenv now computes the most recently set of mutually compatible dependencies, that is compatible with respect to each other. Pipenv does not compute that the recent versions are compatible with your own code. Pipenv places the specific versions that it installed and their digital signatures into the "Pipfile.lock" file.

In order to check if newer packages will break your code, you will need to run Jupyter Lab and run the notebook cells. Use pipenv to activate the virtual environment, where it installed your packages, and then run jupyter:
```
$ pipenv run jupyter lab
```

When you have addressed any issues found in your code cells, such as code that uses outdated function calls from third party packages, then you are ready to save everything and proceed to publish your updated notebook files and your new dependencies.

The first step is to export the dependency versions that Pipenv has configured into an updated requirements.txt file that will work in the Binder environment. There is an easy pipenv command for this. First delete your old requirements file and then "freeze" your package versions into a new one:
```
$ rm requirements.txt
$ pipenv requirements >requirements.txt
```

Now your edited files and requirements details need to be commited to your local git repository for this project, which is stored inside the .git folder, then that new commit needs to be pushed back into the Github repository. The steps for commiting your changes are as follows:

```
$ git status
```
This tells you which files have been changed or are not yet tracked in git. Use the git add command to add them to your next commit.
```
$ git add Pipfile Pipfile.lock notebook.ipynb
```
When you have added all of the new or changed files, then you make a commit from all of those changes like so:
```
$ git commit -m "updating python dependencies and adding pipenv support files"
```
The quoted string after -m indicates a comment that is attached to this commit in your repository's history.

Finally, you are ready to push the new commits to Github.
```
$ git push origin main
```
Note, your main branch may still be named "master". If so, it is a good idea to remove this old style branch name, in favor of the less loaded term "main", see the link or proceed to push the existing master branch and then let Greg know which repository needs this treatment.

[Using Git to Rename Master Branch to Main](https://www.git-tower.com/learn/git/faq/git-rename-master-to-main/)


