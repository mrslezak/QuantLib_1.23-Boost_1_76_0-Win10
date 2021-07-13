# Boost_1_76_0 + QuantLib 1.22 (latest versions at the time of this writing)
MSVC14.1 Version (Visual Studio 2017 Community), for compiling QuantLib, with some tips to get it to compile.  More needed are the steps required to get Boost to compile (yes, I used the prebuilt binaries - they aren't fully compiled...).

Documentation is severely lacking on this project (Boost) - I wouldn't touch it if I didn't need to run a POC with QuantLib.  But since I finally figured out how to build it, after a couple LONG LONG DAYS, I figured I'd summarize how to do it here.  Just in case anyone else has to go through the hassle in the future (or I have to update it later on).

1(a): Download the Boost version you need for your system, I'm only going to focus on Microsoft 10 since that's what I used.  This meant I downloaded "the precompiled binaries" which aren't actually fully compiled... makes a lot of sense, right?  
https://sourceforge.net/projects/boost/files/boost-binaries/1.76.0/
Here I used:
boost_1_76_0-msvc-14.1-64.exe	2021-04-14	194.2 MB

I was building with the 8.1 SDK (to match my Python version), so you may not care and opt to see if 2019 Visual Studio works on your installation.  No more help here, you're on your own for modifying directions.  If you want to play it safe just copy me.  I was under tight timelines so I didn't want to mess around with it.  And if you aren't using Python, I would personally try the latest SDK and Visual Studio:
boost_1_76_0-msvc-14.2-64.exe	2021-04-14	180.5 MB

1(b): My rationale (note, sleep deprived):

My goal is to link this to this Enthought/pyql project https://github.com/enthought/pyql so I kept the build at Microsoft SDK 8.1 (it's a Cython wrapper for QuantLib, to make it more compatible with Python's NumPy, Pandas, SciPy, date functions, formats, etc., and I happen to have Python 3.8.5 Ananconda installed, which uses MSVC 14.1 - I "assumed" it's on the 8.1 SDK here; it may be on 10.x, who knows).  In order to use MSVC 14.1 (Visual Studio 2017 and the 8.1 SDK), I temporarily removed the 2019 Microsoft Build Tools that were installed (as Boost's "bootstrap.bat" would always detect the latest MSVC version on the system, so I was getting MSVC 14.2 builds - not what I wanted).  The reason for not passing command line arguments being the steps that follow don't allow a lot of flexibility, not with my minimial knowledge and testing anyhow (they just don't work).

2) Okay you have Boost downloaded somewhere - install it.  Try c:\Users\Public\boost_1_76_0\ since that's normally not restricted.  Now go there and cd into tools\build\ and type: bootstrap.bat 
It will do some things for you on the programming side and get ready to build what wasn't built in the "pre-compiled" binaries.  Now copy b2.exe to c:\Users\Public\boost_1_76_0\.  Now type b2.exe and it will build the "unbuilt" libraries for you, and shove them in the bin.v2 directory.  After it completes, it may tell you what you ACTUALLY need to do next (I only had this with an accidental MSVC 14.2 compilation).  Here it is:

The Boost C++ Libraries were successfully built!

The following directory should be added to compiler include paths:

    C:\Users\Public\boost_1_76_0

The following directory should be added to linker library paths:

    C:\Users\Public\boost_1_76_0\stage\lib

Yes, finally some correct information is shown above!

3) Go download QuantLib (I used the latest version here: https://www.quantlib.org/download.shtml v1.22 which I pulled here: https://github.com/lballabio/quantlib - just click the Green code button and download a zip if you don't use Git.  Unzip it somewhere (say C:\Users\Public\QuantLib\).  It will be in a QuantLib-1.22 directory under that if the version is the same.  Here (under that directory) you will see a QuantLib.sln file you need to open in the next step.

4) Open Microsoft Visual Studio Community 2007.  Go to File, Open, Project/Solution.  Now go to the QuantLib.sln file and open it.  Right click QuantLib in the right pane (Solution Explorer).  Now set things how you want it - under Configuration (top bar), I selected Release.  Under Platform, I selected x64.  Now click General on the left tab and change the Windows SDK Version to 8.1.  Next click VC++ Directories and add: C:\Users\Public\boost_1_76_0; under Include Directories.  Now under Library Directories add: C:\Users\Public\boost_1_76_0\stage\lib;  Next click Librarian on the left panel, under Link Library Dependencies change it to Yes.  Now click OK in the bottom right.  You should be ready to build x64 Release version.

5) Ah but you probably are missing something.  Type Add or remove programs in the Search bar.  Click it when it appears.  Now find Visual Studio 2017.  Click Modify.  The Visual Studio loader will pop up - go to the second tab from the top - Individual Components.  Now type or scroll down to: Windows Universal CRT SDK.  If it's not checked, check it and let it install.  If it's checked, go back to Visual Studio 2017 and exit out of the Add or remove programs dialog.

6) Back to Visual Studio 2017.  Right click Quantlib, and select Build.  If you are extremely lucky, that's all you had to do.  I hope you don't suffer as much as I did.  When it finishes, you should have a QuantLib-x64-mt.lib with the directory listed next to it.  Go use it and play with QuantLib.

Hope that helped!  Back to work!

If you want an E-book on QuantLib, here you can purchase one from the author for as little as $5.  Of course, pay what you want.  He'll take more money if you are feeling generous.  I bought it but honestly haven't had time to look at it yet.  There are few new texts on QuantLib so I expect it's as updated as you're going to get (compared to all the guides online) 2021-01-16.  It also deals with applying the library to the world of finance, so you may find that money well spent, especially if you're also a quant.
