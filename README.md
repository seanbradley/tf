# SETTING UP A WINDOWS MACHINE WITH WSL2 (WINDOWS SUBSYSTEM FOR LINUX) FOR A.I. AND DEEP LEARNING

	The utilities for AI and ML are constantly evolving and changing.
	
	Versioning alignment between dependencies--i.e., between tools and libraries 
	that rely on one another--is extremely important, and things fall out of sync 
	easily. Things are even more non-standard / experimental with regard to leveraging 
	these tools on WSL.

	These instructions are for anyone interested in playing with the various
	libraries and frameworks for AI, ML, and Deep Learning on a consumer box
	with only one GPU running Windows as the underlying OS. If you're training
	large models, you'll likely be somewhat hamstrung if this is the only way 
	you go about it. Leveraging Google Colab or Hugging Face would probably be
	a smarter way to go about it.

	Interestingly, however, everyday, newer pre-trained models with smaller 
	memory footprints are being released so having the option of playing with and
	tweaking these on your localhost seems useful.
	
	Where salient, I've included links for the installation of each respective tool, 
	but I expect instructions for installing any particular library or framework
	will likely inadvertently break in relatively short order due to the velocity
	of change in this domain of tech. Hence, these instructions should not be followed 
	blindly. Your ingenuity will likely be required to see your way through to the
	end...to a functional development environment.
	
	DISCLAIMER: Since WSL2 is rather new and still somewhat experimental, and not
	generally considered as straightforward or robust for AI / ML development as 
	a native, truly bare metal, non-virtualized Ubuntu environment, I cannot guarantee 
	the following will work exactly as you might hope. Things herein worked as of
    of May 2023.
	
	ASSUMPTIONS: A high-performance gaming laptop with a single Nvidia GeForce 
	RTX GPU chip with Windows as the sole operating system. 
	
	REASON: Dual booting set-ups are a bit of a pain. WSL seems like a convenient
	compromise. Remains to be seen.
	
	The gear I used...
	
		Windows 11
		Processor: 12th Gen Intel(R) Core(TM) i7-12700H   2.70 GHz
		Memory: 1TB plus 16.0 GB RAM
		
	The following were successfully installed via these instructions...
	
		Python version = 3.10.6
		CUDA version = 12.1.66
		RAPIDS
		nvidia-tensorrt = 8.6.1
		Tensorflow version = 2.12.0
		
		
		
## ADD'L DOCUMENTATION
	
	End to end, the following will get you part of the way there...

		[How To Create the Perfect Machine Learning Environment]

		(https://towardsdatascience.com/how-to-create-perfect-machine-learning-development-environment-with-wsl2-on-windows-10-11-2c80f8ea1f31)
		
	Some good notes here...

		[A Useful Gist on Github]
	
		(https://gist.github.com/Ayke/5f37ebdb84c758f57d7a3c8b847648bb)
	
	See here also...
	
		[A Detailed Rubric of Requirements from Nvidia]

		(https://docs.nvidia.com/deeplearning/cudnn/support-matrix/index.html#cudnn-cuda-hardware-versions)

	And...

		[A Table of Compatible Dependencies for TensorFlow from Nvidia]
		
		(https://www.tensorflow.org/install/source#linux)

	
	
## CONFIRMING VERSIONS AND INSTALLATIONS


	The following bullets suggest HOW to check versioning or to confirm installation 
	of the respective resources and/or dependencies as you proceed...
		
	* For Windows version:
	
		About your PC
		
	* For Ubuntu version (on WSL):
	
		cat /proc/version
		
	* For Enviornment Variables via a BASH terminal:
	
		printenv PATH
		
	* For Python version:
	
		python --version

		...and/or...

		ls /usr/bin/python*

	* Nvidia drivers are located in:
	
		C:\Windows\System32\lxss\lib
		
	* For Nvidia driver and CUDA version:
	
		nvidia-smi
		
	* For CUDA Toolkit:
	
		nvcc -V
		
	* For CUDA drivers:
	
		echo $LD_LIBRARY_PATH
		
	* For RAPIDS
	
		conda info --envs
		
		...and...
	
		python3 -c "import cudf; print(cudf.Series([1, 2, 3]))"
		
	* For TensorRT (optional):
	
	    python3 -c "import tensorrt; print(tensorrt.__version__); assert tensorrt.Builder(tensorrt.Logger())"	
		
	* For Tensorflow:
					

		The following commands can be executed in the Python interpretor or 
		a Juptyer Notebook only after activating the virtualenv into which 
		TensorFlow was installed...
	
		Verify CPU set-up...
		
			python3 -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
			
		Verify GPU set-up...
		
			python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
				
		Or you return a more simple confirmation via running the following Python 
		if / else statement...
			
			import tensorflow as tf
			
			if tf.config.list_physical_devices('GPU'):    
				print("Tensorflow is using a GPU")
			else: 
				 print("Tensorflow is not using a GPU")



## BE MINDFUL OF THE OS IN WHICH YOU ARE WORKING

	The following instructions essentially guide you through the installation of 
	two different versions of Python, two different versions of Pip, and two different 
	versions of Virtualenv...each being set-up in the two different operating systems 
	(Windows, and Ubuntu on WSL) respectively. To the degree possible, you should 
	seek to keep versions on each operating system up to date and in sync with one 
	another. Even so: best practice dictates one develops on / in the same environment 
	into which they intend to deploy any solution. The most common way to normalize 
	environs across different machines and different devs is: containerization. That 
	being articulated: the instructions herein are primarily focused on and prioritize 
	getting things set-up on Ubuntu in WSL.
	
	Only the directory for virtualenv artifacts remains the same across both operating 
	systems. (I'm not certain if that's actually ideal. Probably not.)
	
	The workon command used via virtualenvwrapper will only work in Ubuntu (WSL) 
	~after~ the first environment is created. That is: in Ubuntu, if memory serves 
	correctly, you must first evoke the mkvirtualenv command before workon will 
	return anything at all.
	
	If you create a virtualenv in Ubuntu, evoke it only from Ubuntu. If you try 
	to evoke a virtualenv created in Ubuntu via the workon command from within 
	PowerShell, it'll complain "doesn't contain a virtualenv (yet)."

	To avoid confusion, I highly recommend including some identifier when naming 
	your environments just to doubly make sure you're clear about which OS is 
	being used...
	
	Example:
	
		mkvirtualenv test-ubuntu
	
	Not simply the more generic name:
	
		mkvirtualenv test
	
	
	
## INSTALL UBUNTU ON WSL

	...on Windows (via the Windows Store)
	
		First: go to "Turn Windows features on or off".
		
		Enable WSL, Hyper V, Virtual Machine Platform. 
		
		You may have to toggle your CFG setting under Exploit Protection. 
		
		Not yet recommended: installing Ubuntu from a bootable thumb drive.
		
		Upon configuring WSL via any prompts, set the user and password for UNIX 
		(i.e., for Ubuntu within the WSL).

	Note: in Windows, the default path when firing up an Ubuntu terminal within 
	WSL (via VSCode, for example) will be...
	
		/mnt/c/Users/<username>
		
	Not...
	
		/home/<username> 
		
	...as it would be naturally in a native Ubuntu instance.
		
	You can try to reconfigure this in settings.json for the cwd settings--so that 
	launching an Ubuntu terminal will automatically start in your preferred working 
	directory within Ubuntu. That may work when connecting to the Ubuntu instance 
	remotely (via the gear icon in the lower right of VSCode), but it will basically 
	break your ability to reopen Powershell from the dropdown of terminal options 
	in the terminal selector on the right side of VSCode. 
	
	You can try including or adjusting the cwd param in your Ubuntu specific profile 
	definition in the windows profile settings as follows. However, YMMV...
	
		"terminal.integrated.profiles.windows": {
			"PowerShell": {
				"source": "PowerShell",
				"icon": "terminal-powershell"
			},
			"Command Prompt": {
				"path": [
					"${env:windir}\\Sysnative\\cmd.exe",
					"${env:windir}\\System32\\cmd.exe"
				],
				"args": [],
				"icon": "terminal-cmd"
			},
			"Git Bash": {
				"source": "Git Bash"
			},
			"Ubuntu-22.04 (WSL)": {
				"path": "wsl.exe",
				"args": ["-e", "bash"],
				"cwd": "/home/<your_username>"
			}
		
	
	
	
## VIRTUALIZATION (Windows)...
	
	Install Docker on Windows desktop...
	
	NOTE: We are not leveraging Docker for Tensorflow in these instructions. Including 
	the following link for ancillary purposes. You'll need it eventually.
	
	
		https://www.docker.com/products/docker-desktop/ 
		
		AND 
		
		https://docs.docker.com/desktop/windows/wsl/)



## CONFIGURE VSCODE (Windows; possibly optional)...
	
	[Install VSCode Remote Development Pack]
	
	(https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)



## INSTALL AND CONFIGURE GIT

	...on Windows
	
		[Install Git] 
		
		(https://git-scm.com/download/win) 
			
		You may need to update your PATH. Confirm where the executable installed 
		first:
		
			PATH = C:\Users\<username>\AppData\Local\GitHub\PortableGit_<guid>\cmd\git.exe
			
		Optional: [Posh-Git] 
		
			(https://github.com/dahlbyk/posh-git)
			
			scoop bucket add extras
			scoop install posh-git
			Add-PoshGitToProfile -AllUsers -AllHosts	
			
	...on Ubuntu

			sudo apt install git-all
			
	Once installed, you'll need configure Git via the git config command, as well
	as set up .ssh between your remote origin and your local repo.
	
	This is relatively standard practice, and easy enough to learn how to do via 
	Google and/or StackOverflow. For purposes of concision, not including those 
	instructions here.)

	If you're truly high speed, you'll want to install Git Flow...
	
		sudo apt install git-flow
		


## INSTALL PANDAS, NUMPY, SCIPY, MATPLOTLIB, JUPYTERLAB


	Pandas: Data manipulation and analysis, powerful for working with tabular data 
	and time series data.

	NumPy: A Python library for numerical computations, powerful for numerical 
	operations on large data sets.

	SciPy: Another Python library for scientific computing and technical computing, 
	powerful for optimization, signal processing, and image processing.

	Matplotlib: Data visualization, powerful for plotting graphs, charts, and histograms 
	in AI and machine learning.

	JupyterLab: An interactive development environment using "notebooks" in the 
	browser for data exploration, prototyping, collaboration, and documentation.
	
	(You might also want to install Scikit-Learn. Happens in these docs via the 
	RAPIDS installation.)
		
	...on Ubuntu
	
		sudo apt install python3 python3-pip python3-venv ipython3
		cd ~
		nano .bashrc
		
			export PATH="$PATH:/home/<username>/.local/bin/"
		
		pip install pandas numpy scipy matplotlib jupyterlab
		jupyter server --generate-config
		pip install jupyterlab-spellchecker
		pip install jupyterlab-code-formatter
		pip install black isort
		
		(FWIW: Python3 comes out of the box / batteries included on Ubuntu.)
		
	...on Windows
	
		Install Python on Ubuntu first--in the usual way. Once that's done, 
		check the version on Ubuntu...
		
			python3 --version
		
		...then use the installer for the same version from the python.org website.
		
		Adjust the PATH env var in Windows accordingly.
		
			<full_path>/PythonXX
			<full_path>/PythonXX/Scripts
		
		...etc.
		
		Install Numpy etc. only if req'd for your use case.
		
		
	
## FYI: THE PYTHON PATH

	...on Windows

		C:\Users\<username>\AppData\Local\Programs\Python\Python310
	
	As mentioned above, adjust the PATH env var in Windows according.

	Also: ensure this is the default interpreter path under the "Python" settings 
	in VSCode.

	You may need to reboot VSCode and/or your actual machine to see evoke Python 
	correctly from the command line of any IDE.

	I have the PATH set-up so that Python 3 will be invoked at the command line 
	simply by typing "python".

	If want to be able to switch between Python 3.x and Python 2.7, you may need 
	to adjust your PATH values accordingly.

	There are several utils and/or ways of managing things when you have multiple 
	versions of Python installed on your system. One such solution is here:

	[Switching Between Python 2 and 3 on Ubuntu]

	(https://www.fosslinux.com/39384/switching-between-python-2-and-3-versions-on-ubuntu-20-04.htm)



## SET UP VIRTUALENV AND VIRTUALENVWRAPPER

	VSCode's setting for virtual environment directories out of the box is the 
	project directory: ${workspaceFolder}/.env
	
	We're going to set the same centralized directory for all envs--i.e., on directory 
	for all envs regardless of whether or not we're in Windows or Ubuntu.

	In other words, all artifacts fors virtualenv will be stored under the 
	C:\Users\<username>\Envs directory in Windows and the mounted path to this
	 directory (/mnt/c/Users/<username>/Envs) in Ubuntu.
	
	Caveat: creating a new env from within Ubuntu will be very slow.
	
	Advantage: no need to .gitignore envs; simply pip freeze requirements.

    ...on Windows
    
		pip install virtualenv virtualenvwrapper-win
	
	If your PATH in Windows env vars is set up correctly, after installation, you'll 
	be able to create a new virtualenv simply with "mkvirtualenv".
	
	If you upgraded Python after installing virtualenvwrapper-win, you may need 
	to reinstall virtualenvwrapper-win for the "workon" command to function properly.
	
		Set workon in PowerShell (optional; may not be req'd)...

			get-executionpolicy --> should be signed

			function workon ($env) {
					& $env:WORKON_HOME\$env\Scripts\activate.ps1
			}

	...on Ubuntu

		[About Virtualenvwrapper]
	
		(https://opensource.com/article/21/2/python-virtualenvwrapper)

		
		[Installing Virtualenvwrapper on Windows]

		(https://sachinjose31.medium.com/virtual-environments-with-virtualenvwrapper-for-windows-c535c2a0de8c)

			pip install virtualenvwrapper
			cd ~
			nano .bashrc

			export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
			export VIRTUALENVWRAPPER_VIRTUALENV=/usr/local/bin/virtualenv
			export WORKON_HOME = /mnt/c/Users/<username>/Envs
			export PROJECTS_HOME = $HOME/projects
			source /usr/local/bin/virtualenvwrapper.sh

    Again, after you source the ,bashrc, when creating a virtualenv from within 
	Ubuntu, the artifacts for the virtualenv will be stored under the /mnt/c/Users/<yourname>/Envs 
	directory (i.e., in Windows--essentially the same as the C:\Users\<username>\Envs directory).
    
	If all goes well, environment creation may be evoked via virtualenvwrapper's 
	mkvirtualenv command. (Again, takes awhile. Be patient.) After you clone a repo, 
	and create the associated virtualenv, then pin the env to the repo's directory 
	via virtualenvwrapper's setvitualenvproject command.



## INSTALL CUDA ON WSL

	See the following...

	[Nvidia CUDA on WSL User Guide]

	(https://docs.nvidia.com/cuda/wsl-user-guide/index)

		
### IMPORTANT:
	
	Read this carefully...
	
	[Nvidia CUDA Support for WSL]
	
	{https://docs.nvidia.com/cuda/wsl-user-guide/index.html#cuda-support-for-wsl2}
	
	Nvidia stresses...
	
	"To compile new CUDA applications, a CUDA Toolkit for Linux x86 is needed. 
	CUDA Toolkit support for WSL is still in preview stage as developer tools such 
	as profilers are not available yet. However, CUDA application development is 
	fully supported in the WSL2 environment, as a result, users should be able to 
	compile new CUDA Linux applications with the latest CUDA Toolkit for x86 Linux."
	
	So--basically--you can develop but not compile CUDA apps in WSL...presently. 
	If you attempt to install the toolkit, you'll overwrite the the correct GPU 
	driver. In other words...
	
	Users must not install any other NVIDIA GPU Linux driver within WSL 2. 
	
	One has to be very careful here as the default CUDA Toolkit comes packaged 
	with a driver, and it is easy to overwrite the WSL 2 NVIDIA driver with the 
	default installation.
		
	The RAPIDS installation instructions confusingly suggest:
	
	"It’s important to execute sudo apt-get -y install cuda-toolkit instead of 
	sudo apt-get -y install cuda to avoid installing a GPU driver into WSL2. The 
	Windows host system provides the driver to WSL2."
	
	So--your mileage may vary.
	
	I did as as follows...
	
	Download and install the correct driver...

		[Find the correct Nvidia driver for your system here]
	
		(https://www.nvidia.com/Download/index.aspx#)
		
	Install CUDA on WSL...
	
		[Confirm the correct CUDA version here]

		(https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=WSL-Ubuntu&target_version=2.0&target_type=deb_local)
		
		wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
		sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
		wget https://developer.download.nvidia.com/compute/cuda/12.1.0/local_installers/cuda-repo-wsl-ubuntu-12-1-local_12.1.0-1_amd64.deb
		sudo dpkg -i cuda-repo-wsl-ubuntu-12-1-local_12.1.0-1_amd64.deb
		sudo cp /var/cuda-repo-wsl-ubuntu-12-1-local/cuda-*-keyring.gpg /usr/share/keyrings/
		sudo apt-get update
		sudo apt-get -y install cuda
		
	If installed correctly, the "nividia-msi" command will work as expected. 
	
	The "nvcc --version" command may not work as expected, in which case you may 
	need to update your .bashrc file with the 
	following export statements...
		
		export PATH=/usr/local/cuda/bin:$PATH
		export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

	

## INSTALL MINICONDA

	I prefer using Virtualenv for standard Python projects.
	
	Nvidia recommends at least Miniconda to use their RAPIDs interface for ML / AI.

	[Understanding the difference between Pip, Virtualenv, Conda, and Miniconda...]

		(https://deeplearning.lipingyang.org/2018/12/23/anaconda-vs-miniconda-vs-virtualenv/)
		
	Install Miniconda (in Ubuntu)...
	
		wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
		bash Miniconda3-latest-Linux-x86_64.sh
	
	Customize Conda and Run the Install. Use the terminal window to finish installation. 
	Nvidia recommends enabling conda-init.

	If you'd prefer that conda's base environment not be activated on startup, set 
	the auto_activate_base parameter to false:

		conda config --set auto_activate_base false
		
	Env artifacts stored in...
	
		/home/<username>/miniconda3/bin/conda-env

	List Conda envs via...
	
		conda env list
		
	In WSL, activate a Conda env via...
	
		conda activate <env_name>
		
	Deactivate via...
	
		conda deactivate
		
	Update the resolver if you want to install RAPIDS...
		
		conda update -n base conda
		conda install -n base conda-libmamba-solver
		conda config --set solver libmamba
		
		

## INSTALL RAPIDS (optional)

	NVIDIA RAPIDS is a suite of open-source software libraries that allows data 
	scientists to accelerate data processing and machine learning workloads on GPUs. 
	The suite includes a collection of GPU-accelerated data processing and machine 
	learning algorithms that are designed to take advantage of the massively parallel 
	architecture of NVIDIA GPUs.
		
	Make sure to read the docs carefully... 

	[Official Nvidia Docs for RAPIDS on WSL]

	(https://docs.rapids.ai/install#WSL2)

	The following RAPIDS packages are not compatible with Tensorflow packages on Pip. 
	
	Including here in the event one uses NGC containers or Conda packages instead.
	
	May not be compatible with some versions of Jupyter.

	RAPIDS also has a few additional dependencies (some of which we installed in 
	an earlier step), including...
	
		scipy
		matplotlib!=3.6.1,>=3.1
		pytz>=2020.1
		packaging
		protobuf==4.21
		numpy<1.24,>=1.18
		numba
		
	If something is missing, when you try to confirm your RAPIDS install, it'll 
	tell you.
		
	Provided needed dependencies are installed...

		pip install cudf-cu11 dask-cudf-cu11 --extra-index-url=https://pypi.nvidia.com
		pip install cuml-cu11 --extra-index-url=https://pypi.nvidia.com
		pip install cugraph-cu11 --extra-index-url=https://pypi.nvidia.com
		
	cuDF is a Python library that provides a pandas-like DataFrame API on top of 
	Apache Arrow on the GPU. It provides a way to manipulate and analyze large datasets 
	using the power of NVIDIA GPUs.
		
	Then...
	
		conda create -n rapids-23.04 -c rapidsai -c conda-forge -c nvidia rapids=23.04
	
	The standard docs articulate how to triage potential error messages.
	
	[Standard docs are here]
	
	(https://docs.rapids.ai/user-guide)
		


## INSTALL cuDNN


	CAUTION: DO NOT INSTALL REGULAR DRIVERS...
	
	[Review these docs about installing TensorFlow on Windows]
	
	(https://towardsdatascience.com/how-to-finally-install-tensorflow-gpu-on-windows-10-63527910f255)

	[Nvidia docs for installing cuDNN]

	(https://developer.nvidia.com/cudnn)
		
	Installation...
	
		pip install nvidia-cudnn-cu11==8.6.0.163
		
		sudo apt-key del 7fa2af80
		
			
	Configure the system paths. You can do it with following command everytime your 
	start a new terminal after activating your conda environment.

		CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))
		export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/:$CUDNN_PATH/lib
			
	For your convenience it is recommended that you automate it with the following 
	commands. The system paths will be automatically configured when you activate 
	this conda environment.

		mkdir -p $CONDA_PREFIX/etc/conda/activate.d
		echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
		echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/:$CUDNN_PATH/lib' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
		
	The activate.d directory--and corollary deactivate.d directory--contain shell 
	scripts that are evoked when environment is activated or deactivated, respectively. 
	The above commands essentially create a shell script within the activate.d directory 
	for the specific Conda environment in which the commands are run...and subsequently 
	permanently set the LD_LIBRARY_PATH env var for the environment.
		
	Even more conveniently, you can set this up globally for ALL Conda environs. 
	(I only use Conda for ML / AI work. Pure Python projects and web dev, I'm more 
	inclined to use Pip and Virtualenv / Virtualenvwrapper.) To make sure these 
	env vars are set with the instantiation of each new Conda environment...
	
	Navigate to the miniconda directory...
	
	Create a /conda/activate.d subdirectory if it does not exist.
	
		~/miniconda3/etc/conda/activate.d
			
	You can stuff shell scripts in here that will impact every Conda environment, 
	globally--i.e., the "base" Conda env.
	
	Create a global shell script within it...
	
		sudo nano global_env_vars.sh
		
	...and, within it, add the following commands:
	
		CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))
		export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/:$CUDNN_PATH/lib

	...save the file. Now the necessary env vars for Tensorflow will be set with 
	the creation of every Conda env.
	


## INSTALL TensorRT (optional)

	TensorRT is a high-performance deep learning inference library developed by 
	NVIDIA. It is designed to optimize and accelerate deep learning inference on 
	NVIDIA GPUs, by providing a framework to optimize deep learning models for 
	deployment in production environments. TensorRT uses a combination of graph 
	optimizations, kernel fusion, and precision calibration to maximize the performance 
	of the deep learning models running on the GPU. It supports popular deep learning 
	frameworks such as TensorFlow, PyTorch, and ONNX, and allows developers to deploy 
	their trained models to edge devices, data centers, and the cloud.

	[Nvidia docs for installing TensorRT]

	(https://docs.nvidia.com/deeplearning/tensorrt/archives/tensorrt-723/install-guide/index.html#installing-pip)
	
	Installation...
	
		pip install --upgrade setuptools pip
		pip install nvidia-pyindex
		
	Check for desired specific tensorrt-versions -> pip install nvidia-tensorrt==
	
		py3.10 -> available from 8.4.0.+
		py3.9 -> available from 8.0.+
		py3.8 -> available from 7.2.2.+
		
	Install your favorite version e.g. -> pip install nvidia-tensorrt==7.2.3.4
	
	Verify inmstallation...
	
		python3 -c "import tensorrt; print(tensorrt.__version__); assert tensorrt.Builder(tensorrt.Logger())"
		
	Configure the system paths once again as before to contain tensorrt path:
	
		export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/python3.8/site-packages/tensorrt/
	
	...or with recommended automation:
	
		echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/python3.8/site-packages/tensorrt/' \
		>> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
		export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/python3.8/site-packages/tensorrt/


## INSTALL TensorFlow

	At this point, let [Google's install instructions] take the lead.
	
	(https://www.tensorflow.org/install/pip)

	Note: Do NOT install into your environ via Pip. Use Conda! Create a new environ 
	leveraging the Python and CUDA versions you've installed globally thus far, 
	then install correspondingly fresh Jupyter, Tensorflow, and cuDNN libs into 
	the specific environ.

	Doublecheck versions of required dependencies, then...
	
		pip install tensorflow==2.12.0

	Confirm installation via 
	
	
		
## TROUBLESHOOTING

	Check to see if the appropriate Nvidia drivers are installed...
	
	In Windows check under C:\Windows\System32\lxss\lib if the CUDA libs are 
	missing:

    Directory: C:\Windows\System32\lxss\lib

		Mode                 LastWriteTime         Length Name
		----                 -------------         ------ ----
		-a---          11/23/2022  1:11 AM         149912 libcuda.so
		-a---          11/23/2022  1:11 AM         149912 libcuda.so.1
		-a---          11/23/2022  1:11 AM         149912 libcuda.so.1.1
		-a---           9/15/2021  7:33 AM         828840 libd3d12.so
		-a---           9/15/2021  7:33 AM        4834848 libd3d12core.so
		-a---           9/15/2021  7:33 AM         878768 libdxcore.so
		-a---          11/23/2022  1:11 AM        8989896 libnvcuvid.so
		-a---          11/23/2022  1:11 AM        8989896 libnvcuvid.so.1
		-a---          11/23/2022  1:11 AM       14551728 libnvdxdlkernels.so
		-a---          11/23/2022  1:11 AM         514664 libnvidia-encode.so
		-a---          11/23/2022  1:11 AM         514664 libnvidia-encode.so.1
		-a---          11/23/2022  1:11 AM         222944 libnvidia-ml.so.1
		-a---          11/23/2022  1:11 AM         358864 libnvidia-opticalflow.so
		-a---          11/23/2022  1:11 AM         358864 libnvidia-opticalflow.so.1
		-a---          11/23/2022  1:11 AM          68560 libnvoptix.so.1
		-a---          11/23/2022  1:11 AM       60186056 libnvwgf2umx.so
		-a---          11/23/2022  1:11 AM         630224 nvidia-smi


## TODO

	The Troubleshooting section could use some more tips. Feel free to contribute.

	Some laptop manufacturers create machines expressly for AI and ML and have
	their own images and/or software stacks pre-installed. For example: [Lambda
	Stack], makers of the Tensorbook laptops.

	(https://lambdalabs.com/lambda-stack-deep-learning-software)

	Similarly, I'd love to have a single Makefile that manages all the libraries 
	and frameworks outlined in this README for easy custom install on a brand 
	spanking new box... Or a Cloudformaton or Terraform script that 
	does likewise for an applicance in the cloud. 
	
	Of course, a container is the easier and saner solution--particularly for
	production workflows--but I think the exercise of installing, configuring,
	and working through the gotchas inherent in so many dependencies when getting
	up and running on bare metal--there's some value to that.

	Feel free to point me to other Docker images with all the above ready to rock.