# JsonSettings
[![Nuget downloads](https://img.shields.io/nuget/v/Nucs.JsonSettings.svg)](https://www.nuget.org/packages/nucs.JsonSettings/)
[![NuGet](https://img.shields.io/nuget/dt/Nucs.JsonSettings.svg)](https://github.com/Nucs/JsonSettings)
[![GitHub license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/Nucs/JsonSettings/blob/master/LICENSE)

Easiest way you'll ever write settings for your app.
* Modular
* One Liner
* Abstract
### Install
```
PM> Install-Package nucs.JsonSettings
```
### Getting Started
See https://github.com/Nucs/JsonSettings/wiki/<br>
or for all features goto test project: https://github.com/Nucs/JsonSettings/tree/master/tests/nucs.JsonSettings.xTests

JsonSettings is the base abstract class that inherits ISavable.
Here is a self explanatory quicky of to how and what:

* **I want a hardcoded settings file**
```C#
//Step 1: create a class and inherit JsonSettings
class MySettings : JsonSettings {
    //Step 2: override a default FileName or keep it empty. Just make sure to specify it when calling Load!
    //This is used for default saving and loading so you won't have to specify the filename/path every time.
    //Putting just a filename without folder will put it inside the executing file's directory.
    public override string FileName { get; set; } = "TheDefaultFilename.extension"; //for loading and saving.

    #region Settings

    public string SomeProperty { get; set; }
    public int SomeNumberWithDefaultValue { get; set; } = 1;

    #endregion
    //Step 3: Override parent's constructors
    public MySettings() { }
    public MySettings(string fileName) : base(fileName) { }
}
//Step 4: Load
public MySettings Settings = JsonSettings.Load<MySettings>("config.json"); //relative path to executing file.
//Step 5: Pwn.
Settings.SomeProperty = "ok";
SomeProperty.Save();
```

* **I want a dynamic settings**
```C#
//Step 1: Just load it, it'll be created if doesn't exist.
public SettingsBag Settings = JsonSettings.Load<SettingsBag>("config.json");
//Step 2: use!
Settings["key"]  = "dat value tho";
Settings["key2"] = 123; //dat number tho
dynamic dyn = Settings.AsDynamic();
if ((int?)dyn.key2==123)
    Console.WriteLine("explode");
dyn.Save(); /* or */ Settings.Save();
```
* **I want a encrypted settings file**
```C#
MySettings Settings = JsonSettings.Load<MySettings>("config.json", q=>q.WithEncryption("mysupersecretpassword"));
SettingsBag Settings = JsonSettings.Load<SettingsBag>("config.json", q=>q.WithEncryption("mysupersecretpassword"));
//or
MySettings Settings = JsonSettings.Configure<MySettings>("config.json")
                     .WithEncryption("mysupersecretpassword")
               //or: .WithModule<RijndaelModule>("pass");
                     .LoadNow();

SettingsBag Settings = JsonSettings.Configure<SettingsBag>("config.json")
                     .WithEncryption("mysupersecretpassword")
               //or: .WithModule<RijndaelModule>("pass");
                     .LoadNow();

```
* **I want settings to auto save when changed**
```C#
//Step 1:
SettingsBag Settings = JsonSettings.Load<SettingsBag>("config.json").EnableAutosave();
//Unavailable for hardcoded settings yet! (ty netstandard2.0 for not being awesome on proxies)
//Step 2:
Settings.AsDynamic().key = "wow"; //BOOM! SAVED!
Settings["key"] = "wow two"; //BOOM! SAVED!
```
