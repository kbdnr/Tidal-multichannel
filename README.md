# Tidal-Multichannel

Here is a quick overview of setting up an 8-channel tidalcycles environment using Dirt.

![alt text](https://raw.githubusercontent.com/kbdnr/Tidal-multichannel/master/Screenshot%20from%202018-07-28%2011-02-09.png "Jack Screenshot")

### Dirt
https://github.com/tidalcycles/Dirt

First you will want to compile Dirt to use 8 output channels.  You will find this flag exposed in the project's config.h as:

```c
#define DEFAULT_CHANNELS 8
```

### Tidal

Attached are functions created for 8 channels panning.

There is an off-by-one issue when using multichannel output through dirt, so instead of pan functions scaling from 0 - 1 (which will wrap entirely to channel one output) you will need to calculate your highest value as n-1/n where n is the total number of desired outputs.  In the example of 8 channels, your final pan value will simply be 7/8 or 

```haskell
#pan 0.875
```

A second issue encountered is with the use of #cut functions.  a pan list specified as:

```haskell
#pan "[0, 0.125]"
#cut "1"
```

Will only play correctly on one of the two expected output channels.  This is addressed by sending a pan value between the values.

```haskell
#pan "0.0625"
#cut "1"
```

For reference, here was the arrangement of the 8 speakers labeled by their output channel from an interface.

![alt text](https://raw.githubusercontent.com/kbdnr/Tidal-multichannel/master/speakerarrangement.png "Speaker Arrangement")

### Jack & Jackmixer

After compiling Dirt for 8 channels of output, you will see the appropriate number of exposed outs.  I recommend using a graphical mixing tool in order to work through output expectations such as those mentioned in the above section.  For this I opted to use jackmixer which provides a basic interface with channel levels.

I chose to setup d1-d8 for dirt output and e1-e8 (external) for handling 8 channel output of synthesizers.  The XML configuration for jackmixer is included in this repo for reference.  An extra bonus of using jackmixer is the ability to midi map channel pan/levels.  These values were defined for manually mixing synth inputs to target speakers during live performance.
