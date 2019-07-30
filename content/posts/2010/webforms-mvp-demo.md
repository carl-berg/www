---
title: "Creating a basic form with ASP.NET Webforms MVP"
description: "I create a simple Webforms MVP based form and also go through some of the advantages when using the MVP pattern compared to regular webforms."
date: 2010-10-31
tags: ["MVP", "Webforms"]
---

This post describes some of the problems I have experienced when using regular webforms and the benefits of using an MVP framework.

## Why I decided to try webforms MVP
The main reason I started looking at webforms MVP is that I've felt some pain points in working with standard webforms. The best idea in my mind would be to jump ship and use MVC instead, but sometimes you may find yourself stuck with legacy code and the switch to MVC would just be to big/costly leap.

A few problems i see with standard webforms:
- Hard to test.
- Hard to introduce IoC.
- Mixes presentation logic with application logic.
- Has a complicated page life cycle of events that you can get entangled in when your view starts to get complicated.

So when I was given a task of creating some new functionality in a webforms legacy application I decided to take a look at webforms MVP. I started with checking out the main webforms MVP site to download the framework and to look for examples, but when I started out, most of the documentation was noted "to be done" (this is starting to change now as documentation is starting to increase). So since there weren't many examples to be found I decided to write a blog post with a basic example.

I won't go into much detail on the MVP Pattern itself, there are already lots of sources to read about that. Instead I will dive into how to build a basic form using webforms MVP. To create this form I will use a user control which will implement my view, a presenter class to hold my application logic, and a model to pass data from my presenter to my view. The demo project I've created lists used cars in a table which can be filtered by a make and model dropdown.

## The model

```cs
public class FormModel
{
    public string Heading { get; set; }
    public IEnumerable<SelectItem> MakesList { get; set; }
    public IEnumerable<SelectItem> ModelList { get; set; }
    public IEnumerable<Car> SelectedCars { get; set; }
    public int SelectedMakeId { get; set; }
    public int SelectedModelId { get; set; }
    public bool EnableModelSelection 
    { 
        get { return SelectedMakeId > 0; } 
    }
}
```

The model holds a make list, a model list, and a list of filtered cars. It also holds the selected make/model id's.

## The View interface and a custom event argument
```cs
public interface IFormView : IView<FormModel>
{
    event EventHandler<MakeSelectedEventArgs> UpdateModel;
}

public class MakeSelectedEventArgs : EventArgs
{
    public int SelectedMakeId { get; set; }
    public int SelectedModelId { get; set; }
}
```

The view interface inherits a webforms MVP interface called IView which contains the generic typed model. The view interface also contains an EventHandler for a custom event containing our selected id's.

## The Presenter
```cs
public class FormPresenter : Presenter<IFormView>
{
    private readonly ICarRepository _carRepository;
    public FormPresenter(IFormView view, ICarRepository carRepository) : base(view)
    {
        _carRepository = carRepository;
        View.Load += View_Load;
        View.UpdateModel += View_UpdateModel;
    }
    public override void ReleaseView()
    {
        View.Load -= View_Load;
        View.UpdateModel -= View_UpdateModel;
    }

    void View_Load(object sender, EventArgs e)
    {
        View.Model.Heading = "Webforms MVP Demo Form";
        View.Model.MakesList = //Get makes from repository
        View.Model.ModelList = new[] 
        {
            new SelectItem 
            {
                Name = "-- Select model --", 
                Value = 0
            }
        };
    }
    private void View_UpdateModel(object sender, MakeSelectedEventArgs e)
    {
        if(e.SelectedMakeId > 0)
        {
            View.Model.ModelList = //Get models for selected make from repository
        }
        View.Model.SelectedCars = //Get cars from repository based on event argument parameters
        View.Model.SelectedMakeId = e.SelectedMakeId;
        View.Model.SelectedModelId = e.SelectedModelId;
    }
}
```

Our presenter class inherits from a webforms MVP base class with our View as generic type. When inheriting from this class we also need to pass in the generic typed view into the constructor. I also like to use dependency injection which is why i have my repository interface as a constructor parameter (Webforms MVP allows you to create a PresenterFactory to inject dependencies into your presenters, more on that later). In the constructor I also wire up my view events to my presenter events, which is where I update the model.

## The implemented View

```html
<h1><%#Model.Heading %></h1>
<fieldset class="form">
    <legend>Used car search</legend>
    <p><label for="<%#MakeList.ClientID %>">Make</label>
        <asp:DropDownList 
            ID="MakeList" 
            DataSource='<%#Model.MakesList%>' 
            DataValueField ="Value" DataTextField="Name" 
            SelectedValue='<%#Model.SelectedMakeId %>' 
            AutoPostBack="true" 
            runat="server" />
    </p>  
    <p><label for="<%#ModelList.ClientID %>">Model</label>
        <asp:DropDownList ID="ModelList" 
            DataSource='<%#Model.ModelList%>' 
            DataValueField="Value" 
            DataTextField="Name" 
            SelectedValue='<%#Model.SelectedModelId %>' 
            AutoPostBack="true" 
            runat="server" 
            Enabled='<%#Model.EnableModelSelection %>' />
    </p>
</fieldset>
<asp:Repeater ID="Repeater1" 
    DataSource='<%#Model.SelectedCars%>' 
    runat="server">
<HeaderTemplate>
<table id="cars">
    <tr class="table-header">
        <th>ID</th>
        <th>Brand</th>
        <th>Model</th>
        <th>Description</th>
        <th>Manufactured</th>
    </tr>
</HeaderTemplate>
<ItemTemplate>
    <tr>
        <td><%#Eval("Id") %></td>
        <td><%#Eval("Make.Name") %></td>
        <td><%#Eval("Model.Name") %></td>
        <td><%#Eval("Description") %></td>
        <td><%#Eval("Manufactured","{0:yyyy}") %></td>
    </tr>
</ItemTemplate>
<FooterTemplate></table></FooterTemplate>
</asp:Repeater>
```

The view is fairly uncomplicated and more or less just contains databound values from the model (not too different from an MVC view).

## The implemented View codebehind
```cs
[PresenterBinding(typeof(FormPresenter))]
public partial class FormControl : MvpUserControl<FormModel>, IFormView
{
    public event EventHandler<MakeSelectedEventArgs> UpdateModel;
    protected void Page_Load(object sender, EventArgs e)
    {
        MakeList.SelectedIndexChanged += View_UpdateModel;
        ModelList.SelectedIndexChanged += View_UpdateModel;
    }
    private void View_UpdateModel(object sender, EventArgs e) 
    {
        if(UpdateModel == null) return;
        UpdateModel(this, new MakeSelectedEventArgs
        {
            SelectedMakeId = Convert.ToInt32(MakeList.SelectedValue),
            SelectedModelId = Convert.ToInt32(ModelList.SelectedValue)
        });
    }
}
```

The ideal codebehind for a view is literally codeless, but when you use custom events, you might need to put a little bit of code in your codebehind to wire up events and values togeather. At least the codebeind only deals with view logic. Something you need to handle is to add the PresenterBinding attribute to your class and specify which presenter to use for this view. Here’s one of the differences between MVC and MVP. In MVC the controller gets called first and it selects it’s view but in MVP it’s the view that gets called first and it picks it’s presenter.

## Dependency Injection
```cs
protected void Application_Start(object sender, EventArgs e)
{
    PresenterBinder.Factory = new MyPresenterFactoryOfChoice();
}
```
Webforms MVP also makes it easy to use dependency injection in your Presenters. You can create a presenter factory yourself or use the built in support for Castle and Unity or use Ninject, Structuremap or Autofac factories from the contrib project (link below).

## The end result
![demo-result](/img/mvp-ui-medium.png)

The main reasons I prefer the MVP framework is that you can split application logic and presentation logic. I like having "dumb" views that just display model data, no more, no less. Another important aspect of MVP is that you get testable presentation classes. I have never bothered with testing webform pages or controls just because of the complexity, but now you have all the logic abstracted away in class you can actually test and mock properly.

## Resources & Examples:
- [Project Website](http://webformsmvp.com/)
- [Source Code (also contains some example controls)](http://webformsmvp.codeplex.com/)
- [Contrib project, contains some extra Presenter factories for IoC](http://webformsmvpcontrib.codeplex.com/)
- [Using MVP with Episerver, also shows a bit of testing](http://joelabrahamsson.com/entry/introducing-epimvp-a-framework-for-using-web-forms-mvp-with-episerver-cms)