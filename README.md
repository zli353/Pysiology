# Pysiology
Python package to analyze Physyological signals.
With pysiology you can easily analyze:
- Electromyography signals
- Electrocardiography signals
- Electrodermal activity signals

# Requirements
- Numpy
- Scipy
- peakutils

# Example
```python
import os #used for reading files and directories
import pickle #used to open pickle files.
import pysiology


datafolder = "path/to/data/" #path to the data folder
fileECG = "ECGSignal.pkl" #filename of the ECG signal
fileEDA = "EDASignal.pkl" #filename of the EDA signal
fileEMG = "EMGSignal.pkl" #filename of the EMG signal

if(__name__ == "__main__"):
    samplerate = 1000 #samplerate of the signals
    eventDuration = 8 #event duration in seconds 
    
    #First we load our data
    with open(datafolder+fileECG,"rb") as f:
        rawECGSignal = pickle.load(f)
    with open(datafolder+fileEDA,"rb") as f:
        rawEDASignal = pickle.load(f)
    with open(datafolder+fileEMG,"rb") as f:
        rawEMGSignal = pickle.load(f)
        
    #Here we create some fake events
    events = [["A",30],["B",60],["C",90]] #id, starttime in seconds
    results = {}
    for event in events:
        startSample = event[1] * samplerate #First sample of the event
        endSample = eventDuration*samplerate + startSample #Final sample of the event
        results[event[0]] = {} #create a dict for this event results
        results[event[0]]["ECG"] = pysiology.heartrate.analyzeECG(rawECGSignal[startSample:endSample],samplerate) #analyze the ECG signal
        results[event[0]]["EDA"] = pysiology.electrodermalactivity.analyzeGSR(rawEDASignal[startSample:endSample],samplerate) #analyze the GSR signal
        results[event[0]]["EMG"] = pysiology.electromiography.analyzeEMG(rawEMGSignal[startSample:endSample],samplerate) #analyze the EMG signal

```
