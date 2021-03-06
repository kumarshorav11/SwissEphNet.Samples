# SwissEphNet.Samples

Samples of using the [SwissEphNet library](https://github.com/ygrenier/SwissEphNet) in
different types of .Net projects in particular the loading file
mecanism.

All this projects use the `SwissEphNet.Samples.Shared`project 
that contains the test logic based on the official [`SweWin`](https://github.com/ygrenier/SwissEphNet/tree/master/Programs/SweWin) 
project.

Each project implements the `ITestProvider` for print and
load file implementation.

The test use the `ITestProvider.LoadFile` for update the `LoadEventArgs` 
object:

```csharp
private void SwissEph_OnLoadFile(object sender, LoadFileEventArgs e)
{
    Encoding enc = e.Encoding;
    e.File = Provider.LoadFile(e.FileName, out enc);
    e.Encoding = enc;
    Provider.Debug.WriteLine($"Required file:{e.FileName} => {(e.File != null ? "OK" : "Not found")}");
}
```

## Async context

Unfortunally the SwissEphNet library is not async, and can't be it (too complex to implements).

Fortunally we have a solution: wait the result. But beause the async context can changed the
risk is to create an interlock freeze.

An explaination is available on the [`Loading files` wiki page](https://github.com/ygrenier/SwissEphNet/wiki/Loading-files#works-in-an-async-context).

## Project SwissEphNet.Samples.ConsoleNet40

Console application in .Net 4.0.

The data files are copied in a `datas` folder.

The `Net40TestProvider` read the data files from the folder.

## Project SwissEphNet.Samples.ConsoleNet46

Console application in .Net 4.6.

Works like the SwissEphNet.Samples.ConsoleNet40 project.

## Project SwissEphNet.Samples.WinForms46

WinForms application in .Net 4.6.

This sample has 3 tests:

- **Run test** : Works like the console samples for the 
files using the `WinFormsTestProvider`.
- **Run test load async** : This test use the `WinFormsTestProviderAsync`
wich simulate an async loading file, but the call is sync
- **Run test async**: this test use the `WinFormsTestProviderAsync`
wich simulate an async loading file and the test is run in a task so in 
an async mode

The `WinFormsTestProviderAsync` make a pause on each loading file to
simulate a long time running and see the blocking or non-blocking of the main 
thread.

A progress bar is displayed to show the test running. In the 2 first cases
this progress bar don't move because the test run in the main thread. For
the last case, the progress bar is animated that indicates the main thread
is not blocked.

## Project SwissEphNet.Samples.WPF46

WPF application in .Net 4.6.

Works exactly like the WinForms sample.

## Project SwissEphNet.Samples.UWP

Universal Application.

**BEWARE**: UWP use the NETStandard 1.0 version of the library, so to be
compatible the UWP application need the 
`Microsoft.NETCore.UniversalWindowsPlatform` at the version >=5.2.2.

This sample run like the WinForms or WPF samples, but it has only two 
tests because the files are read as async:

- **Run test** : Works like the **Run test load async** WPF using the `UwpTestProvider`.
- **Run test async**: this test use the `UwpTestProvider`
and is run in a task so in an async mode

## Project SwissEphNet.Samples.Universal

Windows 8.1 + WindowsPhone 8.1 applications.

Works like UWP application.

## Project SwissEphNet.Samples.Droid

Android (Monodroid) application.

Works like WinForm application.

**Beware**: the data files are added to the projet as Assets, but the
InputStream used to read an asset don't support the `get_Position()`,
so an error is raised while reading the data files. To prevent this problem
the `DroidTestProvider` copy the asset into a `MemoryStream`. So it
could be a problem to use assets has data for a real application because
all is loaded in memory.

## Project SwissEphNet.Samples.iOS

Because I don't have a Mac computer I can't make a iOS project.

