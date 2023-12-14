# WinTAK SDK Install

[<img src="img/18.png" width="800"/>](img/18.png)

Here I demonstrate a step by step process to get a basic WinTAK development environment setup.

### Prerequisites
- Visual Studio 2022 installed
- At least .NetFramework 4.8.0 installed
- WinTAK SDK installed `WinTAK-4.5.0.251-civ-sdk-installer-x64.exe` as an example
- PC with 8GB - 16GB of RAM recommended
- Windows 10 (11 works to)
- GPU that supports **D3D9** or **D3D11**.

### Other
- WinTAK SDK documentation `WinTAK-4.5.0.245-SDK-Documentation` as an example
this helps give a breif overview and displays some method names etc.

However the doc is generated with doxgen and is a bit limited in my experience.

---

I start the steps below with `Visual Studio 2022` and `.NetFramework 4.8.0` already pre-installed.

I also made sure that all software is updated.

Check out how to install `Visual Studio 2022` and how to install `.NetFramework 4.8.0` if you don't know how BEFORE you install the SDK.

My test laptop basic specs are
- windows 10
- Intel(R) Core(TM) i5-4200H CPU
- 8,00 GB
- Windows 10 Home Single Language
- 1080p screen

# STEPS

## Installing WinTAK SDK Binary

### Step 1

Install the SDK as an admin user

Double click on `WinTAK-4.10.0.170-civ-sdk-installer-x64.exe`

You may get a security warning. Confirm to start the setup wizard.

[<img src="img/1.png" width="600"/>](img/1.png)

Next you will see the Welcom wizard form.

Click next to continue.

[<img src="img/2.png" width="600"/>](img/2.png)

You will see the `Path\to\install\folder` displayed.

Leave as default or choose a path you will remember for later.

[<img src="img/3.png" width="600"/>](img/3.png)

The optional plugins included in the WinTAK SDK binary will now be visable.

In the past it could cause an issue if you loaded them using this form.

Now they seem to work well but that may change in future.

In this case I DO NOT select any since this is a DEV environmet, and click next.

[<img src="img/4.png" width="600"/>](img/4.png)

Finally you will see the ready to install page.

Click install and the SDK will install.

Wait until it's finished.

Now double click the WinTAK icon on your desktop to check that the SDK loads up

## Setting Up Visual Studio 2022

### Step 2

After making sure Visual Studio 2022 is up to date, start VS2022

Click on **Create a new project**

[<img src="img/6.png" width="600"/>](img/6.png)

Search for **WPF User Control Library (.Net Framework)**

Make sure to select the .Net Framework version as WinTAK uses this it.

[<img src="img/7.png" width="600"/>](img/7.png)

Now that you have selected the correct project give your project plugin a **name**.

Also make sure tha ``.Net Framework 4.8`` is selected.

The .Net Framework will change in future as the SDK is updated.

Click create and your plugin project will load up.

[<img src="img/8.png" width="600"/>](img/8.png)


The first thing we need to do now is add the WinTAK NuGet Package Dependencies.

Right click on your project name and go to *NuGet Package Manager* > *Manage NuGet Packages for Solution*.

[<img src="img/9.png" width="600"/>](img/9.png)

You will now see the NuGet Package Manager Dash.

Click on the Gear icon next to Package Source.

This will open the Options dialog.

Click on the + ico.

Rename *Name* to WinTAK and click on ... to find the offline NuGet Packages.

In my case they are located in `C:\Program Files\WinTAK\NuGet`.


[<img src="img/10.png" width="600"/>](img/10.png)

Since the packages are added you can now click on WinTAK in the dropdown box next to the Package Source label.

You will now see the WinTAK NuGet Packages listed.

You only need to install `WinTak-Dependencies` and `Prism.Mef` for this example.

[<img src="img/11.png" width="600"/>](img/11.png)

With the NuGet Packages installed we still need to add references to some libs.

Right click on your project name and go to *Add* > *Reference*.

Click on *Assemblies* and make sure `System.ComponentModel.Composition` has a check mark.

Now click on browse then click on the button *Browse*

Go to the folder where you installed WinTAK.

Now select the `TAK.Engine.dll` lib and add it.

In my case the path was  `C:\Program Files\WinTAK\TAK.Engine.dll`

[<img src="img/12.png" width="600"/>](img/12.png)

[<img src="img/13.png" width="600"/>](img/13.png)

[<img src="img/14.png" width="600"/>](img/14.png)

Since whe finished The initial setup we can now continue to build events

Right click on your project name and select *properties*

Click on *Build* and under the *Platform target* dropdown box select `x64`

[<img src="img/15.png" width="600"/>](img/15.png)

Next click on Build Events.

In the `Post-build event command line:` text area paste:
```
xcopy "$(TargetDir)$(TargetName).dll" "%appdata%\wintak\plugins\$(TargetName)\" /y
xcopy "$(TargetDir)$(TargetName).pdb" "%appdata%\wintak\plugins\$(TargetName)\" /y
```

This automatically copies yout plugin files whenever you rebuild your plugin

[<img src="img/16.png" width="600"/>](img/16.png)


Click on Debug

Under Start Action select the Start external program radio button.

Click on Browse and select the `WinTAK.exe`` file.

In my case it was located at `C:\Program Files\WinTAK\WinTAK.exe`

Under Start options click Browse next to the Working directory.

Select the directory where the WinTAK plugins are is installed.

In my case it was located at `C:\Program Files\WinTAK\Plugins\`

[<img src="img/17.png" width="600"/>](img/17.png)



Congratulations we have set up a basic Dev environment for WinTAK!!!


Now We can start adding code

Below I have made some easy to add copy paste snippets

Button snippet
```C#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;


namespace WinTAK_Simple_Usage_Plugin
{
    [WinTak.Framework.Tools.Attributes.Button("Super_Simple_Plugin_Button", "Super Simple Plugin",
    LargeImage = "pack://application:,,,/WinTAK_Simple_Usage_Plugin;component/Img/tech.png",
    SmallImage = "pack://application:,,,/WinTAK_Simple_Usage_Plugin;component/Img/tech.ico")]

    internal class Plugin_Button : WinTak.Framework.Tools.Button
    {
        private WinTak.Framework.Docking.IDockingManager _dockingManager;

        [System.ComponentModel.Composition.ImportingConstructor]
        public Plugin_Button(WinTak.Framework.Docking.IDockingManager dockingManager)
        {
            _dockingManager = dockingManager;
        }

        protected override void OnClick()
        {
            base.OnClick();

            var pane = _dockingManager.GetDockPane(Plugin_DockPane.ID);
            pane?.Activate();
        }
    }
}

```

DockPane snippet
```C#
using System;
using System.Windows.Input;

namespace WinTAK_Simple_Usage_Plugin
{
    [WinTak.Framework.Docking.Attributes.DockPane(ID, "Super_Simple_Plugin_Example", Content = typeof(UserControl1))]
    internal class Example_DockPane : WinTak.Framework.Docking.DockPane
    {
        private int _counter;
        internal const string ID = "Super_Simple_Plugin_Example_DockPane";
        private double _lon;
        private double _lat;
        private bool _isActive;

        public Example_DockPane()
        {
            var command = new ExecutedCommand();
            command.Executed += OnCommandExecuted;
            NotifyCommand = command;
        }

        public new bool IsActive
        {
            get { return _isActive; }
            set
            {
                SetProperty<bool>(ref _isActive, value, "IsActive");
                
                if (value)
                {
                    WinTak.Display.MapViewControl.MapMouseMove += MapViewControl_MapMouseMove;
                }
                else
                {
                    WinTak.Display.MapViewControl.MapMouseMove -= MapViewControl_MapMouseMove;
                }
            }
        }

        public double Lon
        {
            get { return _lon; }
            set { SetProperty(ref _lon, value); }
        }

        public double Lat
        {
            get { return _lat; }
            set { SetProperty(ref _lat, value); }
        }

        private void MapViewControl_MapMouseMove(object sender, WinTak.Display.MapMouseEventArgs e)
        {
            Lat = e.WorldLocation.Latitude;
            Lon = e.WorldLocation.Longitude;
        }

        public ICommand NotifyCommand { get; private set; }
        public int Counter
        {
            get { return _counter; }
            set { SetProperty(ref _counter, value); }
        }


        private void OnCommandExecuted(object sender, EventArgs e)
        {
            Counter++;
        }
        

        private class ExecutedCommand : ICommand
        {
            public event EventHandler CanExecuteChanged;

            public event EventHandler Executed;

            public bool CanExecute(object parameter)
            {
                return true;
            }

            public void Execute(object parameter)
            {
                var handler = Executed;
                if (handler != null)
                {
                    handler(this, EventArgs.Empty);
                }
            }
        }
    }
}

```

Module snippet
```C#
namespace WinTAK_Simple_Usage_Plugin
{
    [Prism.Mef.Modularity.ModuleExport(typeof(Example_Module), InitializationMode = Prism.Modularity.InitializationMode.WhenAvailable)]
    internal class Example_Module : Prism.Modularity.IModule
    {
        private readonly Prism.Events.IEventAggregator _eventAggregator;

        [System.ComponentModel.Composition.ImportingConstructor]
        public Example_Module(Prism.Events.IEventAggregator eventAggregator)
        {
            _eventAggregator = eventAggregator;
        }
        // Modules will be initialized during startup. Any work that needs to be done at startup can
        // be initiated from here.
        public void Initialize()
        {

        }
    }
}
```

UserControl xaml snippet
```xml
<UserControl x:Class="WinTAK_Example_WpfControlLibrary1.UserControl1"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006" 
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008" 
             xmlns:local="clr-namespace:WinTAK_Example_WpfControlLibrary1" d:DataContext="{d:DesignInstance Type=local:Example_DockPane}"
             mc:Ignorable="d" 
             d:DesignHeight="450" d:DesignWidth="800">
    <Grid>
        <StackPanel>
            <Button Content="Increase counter " Command="{Binding NotifyCommand}" />
            <ToggleButton Content="(de)activate" IsChecked="{Binding IsActive}"/>
            <TextBlock>
        <Run Text="{Binding Lat}"/>
        <Run Text=","/>
        <Run Text="{Binding Lon}"/>
            </TextBlock>
        </StackPanel>
    </Grid>
</UserControl>

```

AssemblyInfo snippet
```C#
[assembly: AssemblyTitle("WinTAK_Example_WpfControlLibrary1")]
[assembly: AssemblyDescription("An example plugin by C.A Torino")]
[assembly: AssemblyConfiguration("")]
[assembly: AssemblyCompany("IQ-Blue Integrated Systems")]
[assembly: AssemblyProduct("WinTak Example Plugin Product")]
[assembly: AssemblyCopyright("Copyright ©  2022")]
[assembly: AssemblyTrademark("IQ-Blue Integrated Systems Pty ltd")]
[assembly: AssemblyCulture("")]//don't fill in the culture otherwise plugin can't be found

[assembly: AssemblyVersion("1.0.0.0")]
[assembly: AssemblyFileVersion("1.0.0.0")]
[assembly: WinTak.Framework.TakSdkVersion("4.5.0.251")]
[assembly: WinTak.Framework.PluginDescription("WinTak Example Plugin Product By C.A Torino")]
[assembly: WinTak.Framework.PluginIcon(@"C:\Users\User\source\repos\WinTAK_Example_WpfControlLibrary1\WinTAK_Example_WpfControlLibrary1\img\Gear.ico")]
[assembly: WinTak.Framework.PluginName("WinTAK_Example_WpfControlLibrary1")]
```


For the WinTAK Simple Usage Plugin go to:

https://github.com/Cale-Torino/WinTAK_Simple_Usage_Plugin

A simple plugin showing a few of the basic components which can be utilized when you develop a plugin for WinTAK and is a little more complicated.

