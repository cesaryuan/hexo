---
title: 在MVVM设计模式中如何设置Esc关闭窗
date: 2022-01-16 21:21:05
tags:
---
## 背景

想要设置窗口按Esc关闭，在普通的设计模式中实现非常简单，如下所示

```C#
public MainWindow()
{
    InitializeComponent();

    this.PreviewKeyDown += new KeyEventHandler(HandleEsc);
}

private void HandleEsc(object sender, KeyEventArgs e)
{
    if (e.Key == Key.Escape)
        Close();
}
```


然而在MVVM模式中，我倾向于使用Command来实现，我首先考虑的是WPF内置的`ApplicationCommands.Close`，然而`Window`本身并没有实现`Close`对应`CommadBinding`，所以只能自己实现，而如果自己实现Binding，势必又会遇到这个问题：[ApplicationCommands这类内置的Command如何与ViewModel中的方法绑定？MVVM](https://www.wolai.com/24Fi2UaeH6uuvfdZtLgS7V)。

所幸HandyControl里有内置的`hc:ControlCommands.CloseWindow`命令，这个命令可以关闭`CommandParameter`传入的Control所属的Window。因此，我自然而然的想到以下代码

```XML
<Window.InputBindings>
    <KeyBinding
        Key="Esc"
        Command="hc:ControlCommands.CloseWindow"
        CommandParameter="{Binding RelativeSource={RelativeSource Self}}" />
</Window.InputBindings>
```


然而，怎么运行都不行。

## 解决方法

问题的症结在于`{Binding RelativeSource={RelativeSource Self}}`将CommandParameter设置为了KeyBinding，而HandyControl的内部的逻辑如下

```C#
namespace HandyControl.Interactivity
{
  public class CloseWindowCommand : ICommand
  {
    public bool CanExecute(object parameter) => true;

    public void Execute(object parameter)
    {
      if (!(parameter is DependencyObject dependencyObject))
        return;
      **Window.GetWindow(dependencyObject)**?.Close();
    }

    public event EventHandler CanExecuteChanged;
  }
}
```


当**dependencyObject**为KeyBinding时，**Window.GetWindow(dependencyObject)** 返回值为null。

因此，猜测将CommandParameter Bind到一个Control而不是KeyBinding可能会解决问题，修改如下

```XML
<Window.InputBindings>
    <KeyBinding
        Key="Esc"
        Command="hc:ControlCommands.CloseWindow"
        CommandParameter="**{Binding RelativeSource={RelativeSource Mode=FindAncestor, AncestorType=Window}}**" />
</Window.InputBindings>
```


![哈哈](images/image.png)
