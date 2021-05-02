# Reporting task's progress

Example:

```csharp
public partial class MainWindow : Window
{
    private async void Button_Click(object sender, RoutedEventArgs e)
    {
        var progress = new Progress<int>(x => MyProgressBar.Value = x);

        await Task.Run(() => DoWork(progress));
    }

    private async Task DoWork(IProgress<int> progress)
    {
        for (int i = 0; i < 10; i++)
        {
            // Reporting using Dispatcher.Invoke
            Dispatcher.Invoke( () => MyLabel.Content = i * 10);

            // Reporting using progress.Report
            progress.Report(i * 10);
            
            await Task.Delay(1000);
        }
    }
}
```

## Links

[IProgress\<T> Interface ↑](https://docs.microsoft.com/en-us/dotnet/api/system.iprogress-1)
