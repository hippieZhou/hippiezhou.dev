---
title: Custom route event in WPF
date: 2017-11-28 21:37:04
updated: 2017-11-28 21:37:04
tags: WPF
---

# 示例代码

```C#
public class MyButtonSimple: Button
{
    // Create a custom routed event by first registering a RoutedEventID
    // This event uses the bubbling routing strategy
    public static readonly RoutedEvent TapEvent = EventManager.RegisterRoutedEvent(
        "Tap", RoutingStrategy.Bubble, typeof(RoutedEventHandler), typeof(MyButtonSimple));

    // Provide CLR accessors for the event
    public event RoutedEventHandler Tap
    {
            add { AddHandler(TapEvent, value); }
            remove { RemoveHandler(TapEvent, value); }
    }

    // This method raises the Tap event
    void RaiseTapEvent()
    {
            RoutedEventArgs newEventArgs = new RoutedEventArgs(MyButtonSimple.TapEvent);
            RaiseEvent(newEventArgs);
    }
    // For demonstration purposes we raise the event when the MyButtonSimple is clicked
    protected override void OnClick()
    {
        RaiseTapEvent();
    }
}
```

```XAMl
<Window>
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:custom="clr-namespace:SDKSample;assembly=SDKSampleLibrary"
    x:Class="SDKSample.RoutedEventCustomApp">
    <Window.Resources>
      <Style TargetType="{x:Type custom:MyButtonSimple}">
        <Setter Property="Height" Value="20"/>
        <Setter Property="Width" Value="250"/>
        <Setter Property="HorizontalAlignment" Value="Left"/>
        <Setter Property="Background" Value="#808080"/>
      </Style>
    </Window.Resources>
    <StackPanel Background="LightGray">
        <custom:MyButtonSimple Name="mybtnsimple" Tap="TapHandler">Click to see Tap custom event work</custom:MyButtonSimple>
    </StackPanel>
</Window>
```

# 参考文档：

[https://docs.microsoft.com/zh-cn/dotnet/framework/wpf/advanced/how-to-create-a-custom-routed-event](https://docs.microsoft.com/zh-cn/dotnet/framework/wpf/advanced/how-to-create-a-custom-routed-event)

