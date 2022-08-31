# The OlMEGA Framework
Version 0.1, Author and Contact Joerg.Bitzer(AT)jade-hs.de

OlMEGA is an open-source framework for acoustical ecological momentary assessment (EMA). It consits of many (in)-dependent tools. These tools are written in different programming languages and in many cases are under-maintained (due to the needed resources for full maintance). It is likely that you have to get around quirks or to report an issue. However, it worked for many studies even with its limitations.

## Sources:
The toools are available here: 

https://tgm.jade-hs.de/media/olMEGA/

and here:

https://github.com/ol-MEGA

## Overall structure

The OlMEGA System has several parts and you have to decide which parts you really need.

* Hardware: The transmission device with MEMS Microphones and bluetooth transmitter (only necessary for head-related acoustic monitoring of the environment). You can buy fully build devices or build them yourself (not recommended)
* Android-Software: The questionaire and acoustic monitoring software on the mobile device, JAVA
* Extraction-Software: Copies data from the mobile device to storage device and checks the data integrety, Matlab
* Analysis-Software: Some tools to display and analyse data, Matlab
* Database-server: A database to store all data and get access to it with search functions for specific features of the data, SQL, Python
* Database-client: Some tools to get CLI access to the database via a Python interface. Helpful to build intermediate csv files for further statistical analysis, Python

Near Future: Change the extraction and analysis tools to Python, to elimnate the Matlab dependency

## Typical workflow (overview)

### Do your homework 
* define your study
* define your questionaire
* get your device (Android mobile phone and OlMEGA Hardware)
* get a Matlab licence
* get a modern python version > 3.8 with TCL/TK GUI support
* read the manual for the mobile software 
* if you want to use and share your data in a (or in our) database, please contact us. This is necessary for shared unique IDs for questions in the questionaire.

### Setup mobile device and OlMEGA hardware
* install ADB (Android Debug Bridge) on your PC/notebook and be sure it is in the system path.
* download latest version of OlMEGA mobile software (apk file) (see above)
* have your questionaire file ready (xml file) (see manual)
* transfer apk and xml file to your mobile phone (https://github.com/ol-MEGA/olMEGA_Installation)

### Test everything
* be your first participant. Record at least two days of data, 
* start with 15 minutes and try to get the results displayed in matlab

### Do the study
* start with only a few particpants
* test everything again

### Analysis
* develop your own analysis tools

### Use database if applicable
* for usage of the database, you can setup your own server
* we recommend to contact us and use our server.

## Step by Step guide for a mini study to get used to the system

### do your homework
* study design and questions:
	* This study will monitor the background noise level without any speech of the test participant (no own voice)
	* 2 questions will be asked every 30 minutes:
		* What is the nature of the background noise? Answer is a list
	* How do you feel? Answer is a simple scale
* hardware:
	* have my mobile phone ready and I have one OlMEGA transmitter

* matlab and python are ready as well

* no database usage

### Setup mobile device and OlMEGA hardware (following the procedure in the handbook)

more info at 2.2 Installation steps in the handbook (https://raw.githubusercontent.com/ol-MEGA/olMEGA_Handbook/master/olMEGA_Handbook.pdf).

1. Check if adb is installed on your computer. Start a console and type adb (adb is a command line interface (CLI) tool)
2. If nothing happens (e.g. the console returns adb is not a command). Download and install the sdk tools (adb must be in your systems path to be found as a command)
https://developer.android.com/studio/releases/platform-tools

3. check 1. again, if a long help message pops up, everythings fine
4. connect your device and allow for debugging from the computer. (perhaps you have to put your mobile phone into developer mode first and you have to unlock the screen, if you have any protection installed)
5. test the connection with `adb usb` and aftwerwards `adb devices`
A list of connected devices should be printed. Perhapas is just a strange number, but this is your mobile phone

6. transfer the latest application file (https://tgm.jade-hs.de/media/olMEGA/latestVersion.php) to the mobile phone (see handbook) e.g. `adb install app-release_2.0beta43.apk`
7. (optional for kiosk mode, will work on vanilla devices without an users only) make device owner e.g `/adb shell dpm set-device-owner com.iha.olmega_mobilesoftware_v2/.AdminReceiver`
8. prepare youre questionaire file (starting example is https://tgm.jade-hs.de/media/olMEGA/quest_templates/tmp.xml . This is an example for this survey, saved as myquestionaire.xml
``` xml
<?xml version="1.0" encoding="utf-8"?>
<mobiquest>
<survey>
    /*This is where the timer values are modified. Mean and Deviation create an interval (in s) in which
    random times are generated. eg half hour with 5 min jitter*/
    <timer mean="1800" deviation="300"></timer>

        <title>
            <text>Mini acoustic survey</text>
        </title>
		
    	<question id="10099" type="infoscreen">
		<label>
		<text>PLease be quiet!</text>
		</label>
		<option>
			<text>Please be quiet (no talking) for the time you fill out the questionaire</text>
		</option>
		</question>

		<question id="10100" type="radio" forceAnswer="true">
		<label>
		<text>What is the nature of your acoustical surroundings?</text>
		</label>
		<option id="10101_01">
		<text>nature</text>
		</option>
		<option id="10101_02">
		<text>city noise</text>
		</option>
		<option id="10101_03">
		<text>cafetaria / bar</text>
		</option>
		<option id="10101_03">
		<text>at home / chores</text>
		</option>
		<option id="10101_03">
		<text>transport (own car, public)</text>
		</option>
		</question>
		
		<question id="10103" type="emoji">
		<label>
		<text>How are you feeling?</text>
		</label>
		<option id="10103_01">
                <text>emoji_happy2</text>
            </option>
            <option id="10103_02">
                <text>emoji_happy1</text>
            </option>
            <option id="10103_03">
                <text>emoji_neutral</text>
            </option>
            <option id="10103_04">
                <text>emoji_sad1</text>
            </option>
            <option id="10103_05">
                <text>emoji_sad2</text>
            </option>
		</question>

        <finish>
            <text>Thank you!</text>
        </finish>
    </survey>
</mobiquest>
```
9. transfer the xml file to your mobile phone (e,g Android 10 device, for more information e.g. for android 11 RTFM)
`adb push myquestionaire.xml sdcard/olMEGA/quest`

10. Configure audio input in the app (if desired)

### Test everything

1. do your survey for 15 minutes and start 2-3 questionaires during this time. Recommandation try different situation (in your office and on the street)
2. transfer the data to your computer:
    1. clone (git command) the latest version of the matlab tools (https://github.com/ol-MEGA/olMEGA_DataExtraction) the CLI command is `git clone git@github.com:ol-MEGA/olMEGA_DataExtraction.git`
    2. start matlab and change directory
    3. start the extractor (GUI) `olMEGA_DataExtraction()`
    4. use the given buttons to transfer the data
	
	If this is not working, check if adb is available to matlab by typing `system("adb devices")`
	if not: check the PATH variable in Matlab. `getenv('PATH')`

3. display the data to see, if this test works.
    You have 2 kind of data now (in subdirectories) in a directory that is coded PSEUDONAME_DATE_EXPERIMENTERNAME and a log.txt file
	
	The data of the log file is displayed in the extraction tool and gives you some insight of the 
	transfered data.

	1. display the information stored in the questionaires:
		* Questionaire data are tricky. All questionaires are different and therefore, there is no easy display option. Furthermore, the answer files contain the answer IDs not the actual text. There is no easy interpretation.
		* Unfortunately, you cannot use all analysis tools for questionaires in the OlMEGA_DataExtractor. They are all hard-coded for one special questionaire (its all legacy code). 
		* Recommendation: Use the python script in https://github.com/ol-MEGA/Quest2CVS
		This tool (GUI based, very easy to use) combines the original xml questionaire and the answers from one participant to one comma seperated file (csv). This file can be used as input for many other programs (Excel, SPSS, matlab or python with pandas) for further analysis.

    2. display the information stored in the feature data: 
		* The most important data is the PSD (Power Spectral Density), which is stored as three vectors (Auto PSD left and right and the cross PSD, which allows to compute the coherence). We will concentrate on this, since RMS (broadband) and Zero Crossing rate are much easier (single channel signals)
		* To load feature files and get the feature file information, you have 2 functions in matlab. `GetFeatureFileInfo` and `LoadFeatureFileDroidAlloc`, both can be found in the directory `/olMEGA_DataExtraction/functions_dataHandling`.
		* Lets assume you have a directory `data` with your subjects data. Get the information from one feature file. e.g.  `stInfo = GetFeatureFileInfo("data/jo123456_220829_bi/jo123456_AkuData/PSD_20220829_164630945.feat")`. The returning struct gives you a lot of information about the structure of the data and some information about the used systems.

		```
		         FrameSizeInSamples: 2000
           HopSizeInSamples: 2000
                         fs: 16000
                 SystemTime: [2022 8 29 16 54 52.7120]
            calibrationInDb: [120 120]
                  AndroidID: `1e848880fd4d7d4c`
            BluetoothTransmitterMAC: `20:FA:BB:0E:9A:48`
               nBytesHeader: 97
            ProtokollVersion: 4
                nDimensions: 1030
                    nBlocks: 1
                  StartTime: [2022 8 29 16 46 30.9450]
            nFramesPerBlock: 480
                    nFrames: 480
                    vFrames: 480
            BlockSizeInSamples: 960000
                 mBlockTime: [2022 8 29 16 46 30.9450]
        ```
		* The AndroidID is the internal ID of your smartphone, The BuetoothTransmitterMAC is an unique ID for the OlMEGA hardware. Therefore, it is possible to track the data to the used devices, e.g. to find systematic errors. Furthermore, the internal calibration coefficients stored in the bluetooth device are transmitted (not in this example, 120dB is the default value, usually it is around 90dB).
		* From the data you can determine, that the samplingrate is 16000 Hz, so the maximum frequency that is measured is 8000 Hz. And the feature samplingrate is 8 Hz (16000 Samples with a HopSize of 2000 samples equals 8 results per second). 
		* The data itself is a matrix of 480 (60s * 8 samples/s) vectors with 1030 entries (2*257 (Auto PSD)+ 257 (real CPSD) + 257 (imag CPSD) + 2 timing information values).
		* Attention: The system time and the StartTime and BlockTime deviate by several seconds. This happens if the system run for a long time, since the sampling rate is not exactly 16000 Hz, but can jitter up to 5 Hz (usually much smaller) for battery powered devices. 
		* load the data with: `[data, timeinfo, stInfo] = LoadFeatureFileDroidAlloc("data/jo123456_220829_bi/jo123456_AkuData/PSD_20220829_164630945.feat");`
		* stInfo contains exact the same info as before (so you can skip `GetFeatureFileInfo` if you want to). timeinfo is the start and endtime of each data vector. The data is provided as the internal `datenum` of matlab. To get a human readible output use `datestr(timeinfo(1,1))`.
		* data contains the PSD data. convert this data to usable spectra with `[pxy, pxx, pyy] = get_psd(data,4);`
		* plot a spectrogram of the max of the Auto PSD to get an idea what happens: The 90 is the assumed calibration constant (change to your real number given by the OLMEGA Hardware).
		```matlab
		maxPxxPyy = max(pxx,pyy);
		freq_vec = linspace(0,stInfo.fs/2,size(maxPxxPyy,2));
		time_vec = linspace(0,60-0.125,size(a,1));
		figure;imagesc(time_vec, freq_vec,10*log10(maxPxxPyy')+90); axis xy; colorbar
		xlabel('Time in s');
		ylabel('Frequency in Hz');
		```

4. repeat 1. and 2, with your own two-day recordings and many questionaires

5. Test out some other analysis tools, e.g.
	* to show, which dates and times are available
	`showAvailableFeatureDataOneTestSubject(obj,'PSD')`
	* to analyse larger parts of the data use `getObjectiveData`. 
	example for test recordings of one day (a few minutes)
	```
	obj = olMEGA_DataExtraction()
	```
	use the GUI to load (open button) the data of one subject
	```
	[Data,TimeVec,stInfoFile]=getObjectiveData(obj,"PSD");
	[pxyAll,pxxAll, pyyAll] = get_psd(Data,4);
	figure;imagesc(10*log10(pxxAll')+90); axis xy; colorbar
	```

### Do the study (not necessary in this example)
* It takes time to copy the data from the smartphone to the computer, be patiant.

### Analysis
* develop your own analysis tools and statistics


## What is missing?

* A simple Editor to generate questionaires
	* Idea browser-based Questionaire editor that is connected to our database
	* This allows to use all the standard questions we already have and use the correct genuine IDs

* A sinple Python based GUI for Mobile Phone --> PC data transfer

* Analysis toolbox in Python













