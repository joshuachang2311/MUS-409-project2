# MUS 409 Project 2

## Choosing samples
I chose `viola_bow_overpressure.aiff` and `threaded_rod_scrape.aiff` as my two sources. This is not much of a musical choice, though; I merely wanted two sound files that have similar length, so that when I am to generate sound elements based on a relationship between the two sounds, I can consider them as equals without having to trim a lot from either side. While most of the eventual sound elements are just them processed individually, the idea still came in handy for the "AM" tracks.

## Creating sound elements

### Python housekeeping

For this project, as a challenge for myself, I generated all the sound elements with Python. It's of course not just vanilla Python, of course, since the nice thing about Python is its rich array (pun intended) of packages, which I am highly aware of because of my background as one of the contributors. **I would also like to pick these Python packages for topic presentation if they qualify.**

Of all those libraries, almost all media processing (audio, image, video, etc.) are based on NumPy, a package that enables way more efficient array computations than vanilla Python's data structures. In order to convert back and forth between sound files and NumPy arrays, I found [PySoundFile](https://pysoundfile.readthedocs.io/en/latest/) very useful.

### 1D experiments

The intuitive way of generating sound elements is through the manipulation of 1D arrays, since that is the means of presentation of uncompressed sound files. The all experiments I had in mind are something like this:

$\text{NewElement}_i = f(\text{Source1}_i, \text{Source2}_i)$

while trying out different designs for $f$. The result turns out to not be that fruitful, though, since most of them are either too similar to the original sources or too random that they sound like heavily distorted version of the sources or unpleasant noises. This was quite a pity since I spent almost a week trying to figure out some way before giving up, but fortunately, an extra dimension came to rescue.

### 2D experiments with spectrogram

There were demos on softwares that deals with spectrograms with functions of shifting, shrinking, and crop with polygons. I am, however, not satisfied with just this. A spectrogram is basically an image, and with all the photoshops and filters we see every day on the internet, it is evident that images have a almost limitless possibility.

After getting the spectrogram in the form of NumPy arrays with the help of a package called [librosa](https://librosa.org/doc/latest/index.html), I then took inspiration from [image processing techniques from scikit-image](https://scikit-image.org/docs/stable/auto_examples/index.html). Of the examples, I chose a few to experiment with, and the elements that are ultimately used are explained as follows.

 - grid: Shrink the image and restore the image to the original size by padding many of the shrinked images together. Different shrinking rates and padding patterns (wrapped, reflected, and symmetric) are experimented with.
 - padded: Same as grid, but the shrink only happens vertically, meaning the rate (and resolution) of the padded spectrogram is higher than the grid counterpart.
 - AM: Apply element-wise multiplication and then normalize.
 - canny: Detect and preserve only the edges as binary values, making the new spectrogram's power level of each frequency uniform, thus creating timbres even more different than other transformations. Different sigma values are experimented with.
 - swirl: Create a swirled transformation with the center of the image as origin. Different swirling strengths are experimented with.
- unsharp: A linear combination of the original image with the blurred version, often used for sharpening the image.

## Assembling sound elements

Since the highlight of this project for me is how the sounds came to existence in the first place (and also because of word count), I will touch on just a few things in the DAW session with bullet points.
 - The time interval between AM tracks' attack times form a Fibonacci sequence.
 - The symmetric grid tracks' notes sound a bit arbitrary in terms of pitch... or is it? In fact, you just got Rickrolled! The notes are the melodies of the chorus from Rick Astley's "Never Gonna Give You Up" without consecutive notes that have the same pitch.
 - An unsharped track is muted with the audio source splitted into 11 chunks. Those are the elements for the unmuted track, with the index of used segment and time interval between segments decided at uniform random until all segments of the original audios are used for at least once.
