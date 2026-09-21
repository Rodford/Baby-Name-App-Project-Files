# Baby Name App Project Instructions

**Mac users:** Follow the [macOS project instructions](README-macOS.md).

In this project, you will use Python, Pandas, Flask, GitHub, and Visual Studio Code to complete and deploy a small web application that displays the popularity of baby names over time.

You will:

- Set up a Linux development environment using Windows Subsystem for Linux (WSL)
- Fork and clone a GitHub repository
- Create and use a Python virtual environment
- Complete portions of a Flask application
- Run and test the application locally
- Commit and push your changes to GitHub
- Deploy the completed application to PythonAnywhere

## Part 1: Install Windows Subsystem for Linux (WSL)

> If you already have WSL and Ubuntu installed and working, you can skip this section.

1. Open **PowerShell as Administrator**.
2. Run:

   ```powershell
   wsl --install
   ```

3. Restart your computer if prompted.
4. After restarting, open **Ubuntu** from the Start menu.
5. The first time Ubuntu opens, create a Linux username and password when prompted.

**Important:** Nothing appears on the screen while you type a Linux password. This is normal. Type the password and press Enter.

## Part 2: Create a GitHub Account

If you do not already have a GitHub account, create one at [GitHub](https://github.com/).

You will use GitHub to store your project and later transfer it to PythonAnywhere.

## Part 3: Install Visual Studio Code

1. Download and install [Visual Studio Code](https://code.visualstudio.com/) on Windows.
2. Open VS Code.
3. Open the **Extensions** panel by pressing `Ctrl + Shift + X`.
4. Install these Microsoft extensions:

   - **Python**
   - **WSL**

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

Open Ubuntu. First, make sure Git and Python are installed:

```bash
sudo apt update
sudo apt install git python3 python3-pip python3-venv
```

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

VS Code should open a window connected to WSL. You should see the project files in the Explorer panel.

Check that the bottom-left corner shows **WSL: Ubuntu** (or your Ubuntu distribution name). In this WSL-connected window, open **Extensions** with `Ctrl + Shift + X` and find **Python** by Microsoft (`ms-python.python`). If you see **Install in WSL: Ubuntu**, click it and wait for installation to finish. The Python extension must be installed and enabled in WSL, even if you already installed it locally on Windows.

## Part 6: Create a Virtual Environment

A virtual environment keeps the Python packages used by this project separate from packages used by other Python projects.

In the VS Code terminal, make sure you are inside the project directory. Create a virtual environment named `venv`:

```bash
python3 -m venv venv
```

Activate it:

```bash
source venv/bin/activate
```

You should now see `(venv)` at the beginning of the terminal prompt.

Upgrade pip and install the packages required for the project:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### Select the virtual environment in VS Code

1. Press `Ctrl + Shift + P`.
2. Search for `Python: Select Interpreter`.
3. Select the Python interpreter located inside the project's `venv` folder.

### If `Python: Select Interpreter` does not appear

This command comes from Microsoft's Python extension. A common cause is that the extension is installed on Windows but is not installed or enabled in the WSL-connected VS Code window. See [Microsoft's VS Code and WSL setup guide](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-vscode).

1. Use the **VS Code desktop application** for these steps. GitHub's repository page in a browser is where you store and review your code.
2. Check the bottom-left corner of VS Code for **WSL: Ubuntu** (or your distribution name). Opening an Ubuntu terminal inside a Windows VS Code window alone does not connect that window to WSL. If the WSL indicator is missing, open Ubuntu and run:

   ```bash
   cd ~/projects/Baby-Name-App-Project-Files
   code .
   ```

3. In the WSL-connected window, press `Ctrl + Shift + X`. Search for `ms-python.python` and select **Python** by **Microsoft**. Click **Install in WSL: Ubuntu** if offered. If the extension is disabled, enable it for this workspace.
4. Open `app.py` and allow the extension to finish loading. If VS Code offers a reload, accept it. Otherwise, open the Command Palette and run `Developer: Reload Window`.
5. Press `Ctrl + Shift + P` again and search for `Python: Select Interpreter`.

If the command appears but your virtual environment is not listed, choose **Enter interpreter path...** and browse to the project's `venv/bin/python`. You can find its full path by running these commands from the project directory in the WSL terminal:

```bash
source venv/bin/activate
python -c "import sys; print(sys.executable)"
```

The path should end with `Baby-Name-App-Project-Files/venv/bin/python`. Selecting an interpreter in VS Code and activating the virtual environment in a terminal are separate steps; keep using `source venv/bin/activate` when you open a new terminal to run the app.

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

When you are finished testing, return to the terminal and press `Ctrl + C` to stop the server.

## Part 9: Commit and Push Your Work to GitHub

After you have tested the application successfully:

1. Open the **Source Control** panel in VS Code.
2. Review the files you changed. You should **not** see the `venv` folder listed as a change.
3. Enter a descriptive commit message, such as `Complete baby name Flask application`.
4. Commit your changes.
5. Select **Sync Changes** or **Push** to send the commit to GitHub. If VS Code asks you to sign in to GitHub, follow the browser prompts.
6. Open your repository on GitHub and verify that your changes appear there.

# Deploying the App to PythonAnywhere

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
