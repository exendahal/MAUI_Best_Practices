# MAUI_Best_Practices
## Best code practices for Dot Net MAUI and Xamarin

1. Do not assign more than one page to a view model. It should be a one page to one view model
2. Do not pass view modes to methods of other pages
3. The only code in code-behind (e.g., foo.xaml.cs) should be
    * initialize in the constructor
    * create the view model
    * assign the datacontext to the view model
    * call the viewmodel’s Initialize method
    * Lifecycle methods such as OnAppearing and OnDisappearing </br>
 #### Why : We want to keep the code behind as sparse as possible. Unit testing requires reaching into code and that is infinitely easier with a viewmodel.
4. Every ViewModel should have an initialize method
#### Why : Having an initialize method keeps most of the code for the vm out of the constructor. This is recommended practice by Microsoft, and allows for async methods.
5. Do not put Xaml in templates in App.xaml. Put the Xaml in the Xaml file
#### Why : App.xaml quickly becomes bloated and hard to work with. Having the Xaml in the Xaml file is natural and helps create the troika we want: foo.xaml, foo.xaml.cs and fooViewModel.cs
6. Create viewmodel name by appending “viewmodel” to the xaml name
 <pre> 
 LigthController.xaml
   LightController.xaml.cs
      LightControllerViewModel.cs
  </pre>
#### Why : It is far easier to find the file you want if we follow a convention
7. Avoid Xaml file names ending in “view”
<pre> 
LightControllerView.xaml
   LightControllerView.xaml.cs
      LightControllerViewViewModel.cs
</pre>
#### Why : Adding view to a view file is redundant and it makes reading the name of the ViewModel more difficult.
8. Use commands rather than event handlers

```
// wrong - handled in code behind
<button Text="Divide by 2" Clicked="onClick" />

//correct - handled in viewmodel
<button Text="Divide by 2" ClickedCommand="{Binding DivideBy2Command"
```
#### Why: It is much easier to write unit tests when the event handler is in the viewmodel.
9. ListView ItemSource should bind to Observable Collections instead of Lists
#### Why: Observable Collections provide notifications when items get added, removed, or when the whole list is refreshed. Lists do not provide these notifications. Warning, there is no automatic push notification if a member of the list is changed.
10. Set BindingContext in ctor of the View CodeBehind
#### Why: This will prevent timing issues that may prevent the Content Page Title from displaying
11. Place async method calls in OnAppearing in the View CodeBehind rather than in the constructor
```
//wrong
  public PointsListView()
  {
     viewModel =
               new PointListViewModel(pointService, Device, SelectedModule, ObjectType);
     Task.Run(async()=> await viewModel.PopulateMenuOptions());
     
  }
  //correct
  public PointsListView()
  {

  }

  protected override async void OnAppearing()
  {
     viewModel = new PointListViewModel(pointService, Device, SelectedModule, ObjectType);
     await viewModel.PopulateMenuOptions();
  }
```
#### Why: The ctor should only be initializing the view and should not make calls to the controller
12. Do not end async method names with ‘Async’
#### Why: It is easy to forget


Credit: https://jesseliberty.com/

