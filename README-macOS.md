# Baby Name App Project Instructions for Mac Users

In this project, you will use Python, Pandas, Flask, GitHub, and Visual Studio Code to complete and deploy a small web application that displays the popularity of baby names over time.

You will:

- Set up Python and Git on macOS
- Fork and clone a GitHub repository
- Create and use a Python virtual environment
- Complete portions of a Flask application
- Run and test the application locally
- Commit and push your changes to GitHub
- Deploy the completed application to PythonAnywhere

## Part 1: Install Python and Git on macOS

These instructions use the Mac Terminal app and work with Intel and Apple silicon Macs. You do not need WSL or Ubuntu.

### Install Python 3.12

The project's requirements file pins versions of NumPy and Pandas. Use **Python 3.12** for this assignment to match those packages and the deployment environment.

1. If Python 3.12 is not already installed, open the [Python 3.12.10 release page](https://www.python.org/downloads/release/python-31210/).
2. Under **Files**, download the **macOS 64-bit universal2 installer**, which supports both Intel and Apple silicon Macs.
3. Open the downloaded package and follow the installer prompts.
4. Open **Terminal** from **Applications > Utilities**, or press `Command + Space`, type `Terminal`, and press Return.
5. Check the installed version:

   ```bash
   python3.12 --version
   ```

You should see `Python 3.12.x`. Use the explicit `python3.12` command when creating your virtual environment below.

### Install Git

In Terminal, check whether Git is available:

```bash
git --version
```

If macOS asks you to install command line developer tools, accept the installation and wait for it to finish. If Git is unavailable and no prompt appears, run:

```bash
xcode-select --install
```

Follow the prompts, then run `git --version` again. If the tools are already installed and Git reports a version, continue. See [Apple's command line tools instructions](https://developer.apple.com/library/archive/technotes/tn2339/_index.html) for installation details.

## Part 2: Create a GitHub Account

If you do not already have a GitHub account, create one at [GitHub](https://github.com/).

You will use GitHub to store your project and later transfer it to PythonAnywhere.

## Part 3: Install Visual Studio Code

1. Download [Visual Studio Code for macOS](https://code.visualstudio.com/docs/setup/mac) and move the application into your **Applications** folder.
2. Open VS Code.
3. Open the **Extensions** panel by pressing `Command + Shift + X`.
4. Install the **Python** extension published by Microsoft.
5. Press `Command + Shift + P` to open the Command Palette.
6. Search for and run **Shell Command: Install 'code' command in PATH**.
7. Close and reopen Terminal so the change takes effect.

You do not need separate GitHub extensions for this project. VS Code includes the Git features needed to clone, commit, pull, and push repositories.

## Part 4: Fork the Project Repository

1. Open the [Baby-Name-App-Project-Files repository](https://github.com/Chelsea-Myers/Baby-Name-App-Project-Files).
2. Click **Fork**.
3. Create the fork in your own GitHub account.
4. On your fork, click **Code** and copy the HTTPS address for your repository.

It will look similar to:

```text
https://github.com/YOUR-USERNAME/Baby-Name-App-Project-Files.git
```

## Part 5: Clone Your Repository

Open Terminal. You installed Git and Python in Part 1.

Create a folder for your projects:

```bash
mkdir -p ~/projects
cd ~/projects
```

Clone **your fork** of the repository:

```bash
git clone https://github.com/YOUR-USERNAME/Baby-Name-App-Project-Files.git
```

Replace `YOUR-USERNAME` with your GitHub username. Then move into the project directory and open it in VS Code:

```bash
cd Baby-Name-App-Project-Files
code .
```

VS Code should open the project folder. You should see the project files in the Explorer panel. If `code .` is not recognized, repeat the PATH setup in Part 3, or use **File > Open Folder** in VS Code and select the project folder inside your home folder's `projects` directory.

## Part 6: Create a Virtual Environment

A virtual environment keeps the Python packages used by this project separate from packages used by other Python projects.

In VS Code, choose **Terminal > New Terminal**. Make sure you are inside the project directory. Create a virtual environment named `venv`:

```bash
python3.12 -m venv venv
```

Activate it:

```bash
source venv/bin/activate
```

You should now see `(venv)` at the beginning of the terminal prompt. Run `python --version` and confirm it reports Python 3.12. Each time you open a new terminal to work on this project, return to the project folder and run `source venv/bin/activate` again.

Upgrade pip and install the packages required for the project:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### Select the virtual environment in VS Code

1. Press `Command + Shift + P`.
2. Search for `Python: Select Interpreter`.
3. Select the Python interpreter located inside the project's `venv` folder.

### Do not commit the virtual environment

The repository's `.gitignore` file already tells Git to ignore the `venv` directory. Do **not** delete the virtual environment before committing, and do **not** upload it to GitHub. Git will simply ignore it.

## Part 7: Complete the Python Code

Open `app.py`. Several portions of the application have been left for you to complete.

### 1. Load the baby-names data

Find the comments describing where to load `babynames.csv`. Use Pandas to read the file:

```python
babynames = pd.read_csv(
    './data/babynames.csv',
    names=['sex', 'year', 'name', 'count']
)
```

This creates a Pandas DataFrame containing `sex`, `year`, `name`, and `count`.

### 2. Calculate each name's rank

Find the comments describing how to construct the `rank_in_year` column. Create the column using:

```python
babynames['rank_in_year'] = (
    babynames
    .groupby(['year', 'sex'])['count']
    .rank(ascending=False)
)
```

This calculates the popularity ranking of each name within each year and sex.

### 3. Select the requested name

Inside the `/submit` route, the application receives the name and sex selected by the user and stores them in `name_submitted` and `sex_submitted`.

Find the comments describing how to extract the data for the requested name. Create a subset containing the selected name and sex:

```python
name_subset = babynames[
    (babynames['name'] == name_submitted) &
    (babynames['sex'] == sex_submitted)
]
```

Then extract the years and rankings:

```python
name_years = name_subset['year'].tolist()
name_ranks = name_subset['rank_in_year'].tolist()
```

Save `app.py`.

## Part 8: Run and Test the Flask App

Make sure the virtual environment is activated. Start the Flask development server:

```bash
python -m flask --app app --debug run
```

Open [http://127.0.0.1:5000](http://127.0.0.1:5000) in your web browser. Test several baby names and make sure the visualization appears. Try names that were popular during different periods.

When you are finished testing, return to the terminal and press `Control + C` (not Command + C) to stop the server.

### If port 5000 is already in use

macOS may use port 5000 for AirPlay Receiver. If Flask reports that the address is already in use, start the app on a different port:

```bash
python -m flask --app app --debug run --port 5001
```

Then open [http://127.0.0.1:5001](http://127.0.0.1:5001). See the [Flask development server documentation](https://flask.palletsprojects.com/server/) for details.

## Part 9: Commit and Push Your Work to GitHub

After you have tested the application successfully:

1. Open the **Source Control** panel in VS Code.
2. Review the files you changed. You should **not** see the `venv` folder listed as a change.
3. Stage the files you intend to commit using the **+** button beside each file. Enter a descriptive commit message, such as `Complete baby name Flask application`.
4. Commit your changes.
5. Select **Sync Changes** or **Push** to send the commit to GitHub. If VS Code asks you to sign in to GitHub, follow the browser prompts.
6. Open your repository on GitHub and verify that your changes appear there.

### If Git asks for your name and email

In the VS Code terminal, run the following commands from the project folder, replacing the example values with your own name and an email associated with your GitHub account (or your GitHub-provided no-reply email):

```bash
git config user.name "Your Name"
git config user.email "YOUR-EMAIL"
```

Then try the commit again.

# Deploying the App to PythonAnywhere

From this point on, run commands in the **PythonAnywhere Bash console**, not in your Mac's local terminal. PythonAnywhere has its own Python environment.

## Part 10: Create a PythonAnywhere Account

**Choose the free Beginner account ($0/month). You do not need a paid plan for this assignment.**

1. Open the [PythonAnywhere pricing page](https://www.pythonanywhere.com/pricing/).
2. Scroll past the paid plans to **Explore with a limited account**.
3. Select **Create a Beginner account** and complete signup. If you already have a free account, sign in to it.

The free account includes one web app at `YOUR-USERNAME.pythonanywhere.com`. Use that included address for this project; a custom domain is not required. Your app only needs to remain live for a few days for this assignment, so there is no need to upgrade. Keep it available through grading.

After signing in, open a **Bash console**.

## Part 11: Clone Your GitHub Repository on PythonAnywhere

From the Bash console, clone your completed repository:

```bash
git clone https://github.com/YOUR-USERNAME/Baby-Name-App-Project-Files.git
```

Replace `YOUR-USERNAME` with your GitHub username. Then move into the project folder:

```bash
cd Baby-Name-App-Project-Files
```

Your project files are now stored directly on PythonAnywhere. You do **not** need to upload a ZIP file or upload your local `venv` folder.

## Part 12: Create a Virtual Environment on PythonAnywhere

For this version of the project, use **Python 3.12**.

In the PythonAnywhere Bash console, run:

```bash
mkvirtualenv --python=/usr/bin/python3.12 baby-name-env
```

Your prompt should change to show that `baby-name-env` is active. Move to your project directory if necessary and install the dependencies:

```bash
cd ~/Baby-Name-App-Project-Files
python -m pip install -r requirements.txt
```

## Part 13: Create the Web App

1. Open the **Web** tab in PythonAnywhere.
2. Select **Add a new web app**.
3. Choose **Manual Configuration**.
4. Select **Python 3.12**.

The Python version selected for the web app should match the version used for the virtual environment.

## Part 14: Connect the Virtual Environment

On the PythonAnywhere **Web** page, find the **Virtualenv** section. Enter:

```text
/home/YOUR-PYTHONANYWHERE-USERNAME/.virtualenvs/baby-name-env
```

Replace `YOUR-PYTHONANYWHERE-USERNAME` with your PythonAnywhere username.

In the **Code** section, set both **Source code** and **Working directory** to:

```text
/home/YOUR-PYTHONANYWHERE-USERNAME/Baby-Name-App-Project-Files
```

Replace the username with your PythonAnywhere username. The working directory is important because the application reads `./data/babynames.csv` using a relative path.

## Part 15: Configure the WSGI File

On the PythonAnywhere **Web** page, open the WSGI configuration file. Replace the Flask section with:

```python
import sys

path = '/home/YOUR-PYTHONANYWHERE-USERNAME/Baby-Name-App-Project-Files'

if path not in sys.path:
    sys.path.insert(0, path)

from app import app as application
```

Replace `YOUR-PYTHONANYWHERE-USERNAME` with your PythonAnywhere username and save the file.

## Part 16: Reload and Test the App

1. Return to the **Web** tab.
2. Click **Reload**.
3. Open the URL provided by PythonAnywhere.
4. Test several names to confirm that the application works.

You have now:

- Worked with a GitHub repository
- Created an isolated Python environment
- Installed project dependencies
- Modified a Python/Flask application
- Tested a web application locally
- Used version control to save your work
- Deployed the application to a web server

## Dataset

The prepared `data/babynames.csv` file contains national baby-name records for 1910–2025 from the [Social Security Administration's National Data collection](https://www.ssa.gov/oact/babynames/limits.html). The SSA omits names with fewer than five occurrences to protect privacy.
