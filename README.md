# openmhz_delayed_uploads
Directory watch for delayed uploads to OpenMhz. Script was designed for calls from Trunk-Recorder but with slight modification can be used with almost any service. 

This was pieced together with existing scripts found on the Trunk-Recorder Dicord from SaintOlav and BinarySentinel, as well as the assistance and ideas from TheGreatCodeHolio, Jodfie, and tadscottsmith. Most modifications made with ChatGPT. All this to say this is mostly molding what others have already done to fit my own needs. 

**Known Issues:**
  - Some .json files are not moved when the rest of the call is handled. Seems to be those that are exactly on the :00 or :30 mark but I haven't found a root cause yet. 
