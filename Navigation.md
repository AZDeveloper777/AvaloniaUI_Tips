I'm using CommunityToolkit.Mvvm, not ReactiveUI.  
There are a few ways to handle Navigation in Avalonia.    
One of them is to load your views into a ContentControl on your MainWindow.axaml  
```
<ContentControl Content="{Binding CurrentViewModel}" />
```
You'll need your MainWindowViewModel to look like:
```
    public partial class MainWindowViewModel : ObservableObject
    {
        [ObservableProperty]
        private ObservableObject currentViewModel;

        public MainWindowViewModel()
        {
            // Default View
            currentViewModel = new WelcomeViewModel();
        }
        

        [RelayCommand]
        private void ShowWelcome() => CurrentViewModel = new WelcomeViewModel();
    }
```
AND, you must change your ViewLocator.cs to handle the various Base classes you will use on your VMs's i.e.
```
        public bool Match(object? data)
        {
            return data is ViewModelBase || data is ObservableObject;
        }
```
Now, whenever you change CurrentViewModel on MainWindowViewModel, you'll get the corresponding View loaded inside the ContentControl

Also, take a look at similar approach https://github.com/stevemonaco/AvaloniaNavigationDemo
