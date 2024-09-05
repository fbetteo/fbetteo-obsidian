
This happened since I have python installed in my secondary drive (D:) using Anaconda. This is probably not the idea poetry usage since I'm running it inside a conda environment but this way I'm using my python installations and not messing with C:. I had an awful time trying to do a clean install of poetry as if I haven't Anaconda because it tried to use my Python installation in C: and etc.

ChatGPT ftw.

Create a conda environment with desired python version. If the minimum python version required is defined in the poetry toml, use at least that one.

`conda create --name myenv python=3.11.8`

1. **Activate your Anaconda environment:** Open Anaconda Prompt and activate the environment you want to use:
    

    `conda activate your_environment_name`
    
2. **Install Poetry via Pip:** Poetry can be installed using pip within your activated environment:
    

    `pip install poetry`
    
3. **Verify Installation:** Check if Poetry is installed correctly by running:

    
    `poetry --version`
    
4. **Configure Poetry to use the current environment:** By default, Poetry might try to create a new virtual environment. To configure Poetry to use the current Anaconda environment, you can set this in your Poetry configuration:

    `poetry config virtualenvs.create false`
    
5. **Initialize or Install Dependencies:** If you need to initialize a new Poetry project, navigate to your project directory and run:

    
    `poetry init`
    
    Or, if you are working with an existing project, you can install its dependencies by navigating to the project directory and running:

    `poetry install`
    

By following these steps, you'll have Poetry installed and configured to work with your existing Anaconda environment on your D: drive.