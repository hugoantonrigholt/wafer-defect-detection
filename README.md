# Wafer cell

S1 Challenge 2, (A)I, Robot. A robot cell that sorts wafers into pass, review and scrap,
based on the defect pattern a neural network recognises on each wafer map.

## Setup
1. Python 3.12 virtual environment
   `python -m venv C:\Users\<you>\venvs\wafer-cell`
2. Install PyTorch with CUDA 12.8, then `pip install -r requirements.txt`
3. Download the dataset, see `data/README.md`
4. In PyCharm, set the project interpreter to the wafer-cell environment, then open `notebooks/01_explore_data.ipynb`

## Steps
1. `01_explore_data` - load the maps, look at them, count the classes
2. baseline model (to do)
3. CNN (to do)
4. evaluation and Grad-CAM (to do)
5. robot cell in MuJoCo (to do)
