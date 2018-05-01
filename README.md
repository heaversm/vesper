# VESPER

### WHAT IS VESPER?

[Vesper](https://github.com/HaroldMills/Vesper) is open source software that will enable researchers and enthusiasts to collaboratively collect, view, and analyze nocturnal flight calls (NFCs) of migratory birds at spatial scales ranging from local to continental. It runs on Python, through Anaconda.

### WHAT IS PYTHON?

[Python](https://www.python.org/) is a programming language. It is built in to MacOS, so there is no need to install it. 

### WHAT IS ANACONDA?

[Anaconda](https://www.anaconda.com/download/#macos) is a data science platform utilizing Python. It allows you to set up Python environments with all the stuff you need to run your Python project. Vesper is easiest to install when you are using Anaconda. You can install the Anaconda Navigator, which is a graphical user interface for Anaconda that helps you avoid running commands in your system terminal. 

## INSTALLING VESPER

* Install [conda](https://conda.io/docs/user-guide/install/macos.html) or [Anaconda Navigator](https://www.anaconda.com/download/#macos) version 3.6 or greater.
* In Anaconda Navigator, go to `environments > create > vesper (Python 3.6)`
* Click the green arrow on Vesper, and choose `Open Terminal`
* Install Vesper: `conda install -c defaults -c conda-forge -c haroldmills vesper`. Say `y` to all installs

## VIEWING EXAMPLE ARCHIVE

* [Download the starter archive](https://github.com/HaroldMills/Vesper-Example-Archive/archive/v0.3.0.zip)
* Extract somewhere on your hard drive
* In terminal, change to the directory with this archive, and run `vesper_admin runserver`
* Open a web browser to `http://localhost:8000/`
* Choose a station, choose a classification, and view the bird calls by date.

## CREATING A NEW ARCHIVE

* create a directory for your database, and switch to it
* copy the `Presets` subdirectory and `preferences.yaml` file into your new directory.
* run `vesper_admin migrate` in your new directory to create the database. This should create the file `Archive Database.sqlite` in the archive directory.
* Run `vesper_admin createsuperuser`, and enter a username and password (`vesper` / `VesperAdmin`)
* Run `vesper_admin runserver` to start your server, and open your browser to `localhost:8000` to view it
* Import Archive Data YAML file. Make sure to edit this file to appropriately reflect your recording devices, locations, etc.
* Import Recordings - Vesper expects a specific file format, e.g: `Station 1_2018-04-26_11.29.47_Z.wav` where:
	* `Station 1` is the name of your station from the YAML file
	* `2018-04-26` is your date in the format of `YYYY-MM-DD`
	* `11.29.47_Z.wav` is the time of the recording in UTC time (+7 hours later than PDT) in the format of `HH.MM.SS_Z` where Z stands for Zulu.
* Place recordings in a `Recordings` directory
* The path to the recordings is relative to the root directory of your project (where the `Preferences.yaml` file lives). So if all of your recordings are in a directory called `Recordings`, you could just specify the path as `Recordings`. Check `recursive` if you have subfolders within that folder.
* View clip calendar, and click on a date with recordings.
* Detect:
	* Select detectors used
	* Select station
	* Select start and end date, e.g: `2018-04-26`
	* Press run, and wait until the recordings are processed. The job should run at about 100-200x speed of the original recording durations.

## CLASSIFYING YOUR CLIPS

* Clip calendar should now show unclassified clips by selecting `Unclassified` from the classification dropdown in `View > Clip Calendar`.
* Then Classify, select your Classifier, Detectors, Station/Mics, Start and End Date.
* Now view clip calendar, pick a date, and look at recordings to identify calls.
* Select a call, and key bindings will associate a call with classifications.
* Key bindings are listed in the presets directory YAML files. A few are:
	* `,` key moves back
	* `n` classifies as noise
	* `c` classifies as a call
	* `>` and `<` navigate pages of clips

## NOTES

* Vesper has two processors - **detectors** and **classifiers**. 
	* Detectors are integrated into the hardware used to make recordings and generally look for any noise (energy) occurring in specific frequency bands.
	* Classifiers attempt to identify the type of bird and type of call. This is usually done through a combination of machine-learning and manual classification.
* Recordings are generally all-night recordings. These get processed automatically into **clips**: statistically significant segments of audio.
* Vesper uses [TensorFlow](https://www.tensorflow.org/) to try and distinguish calls from noise. 
* Training data is from Montana, so depending on your region, classification might not be as effective.
* Vesper utilizes an SQLite database and Django Dev server in order to make it easy to get up and running, but neither are very robust for production use.
	* SQLite doesn't support concurrency well, so you shouldn't run more than 1 classification at a time
	* Don't browse while classifying

## LINKS

* [Bird Vox](https://wp.nyu.edu/birdvox/resources/) - collab between Cornell and NYU to utilize machine learning for automatic classification of bird species from their vocalizations to determine migration. Vesper is hoping to utilize some of their technology.
* [Sound Classification with TensorFlow](https://www.iotforall.com/tensorflow-sound-classification-machine-learning-applications/) - High level overview of setting up Tensorflow to classify sounds.
* [Recognizing Biological Sounds with Machine Learning](https://www.researchgate.net/publication/2302452_Recognising_Biological_Sounds_Using_Machine_Learning) - research article from 