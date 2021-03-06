This lesson will teach you the basic of creating a model view file and binding to list view items source within InAiR SDK.

###Creating a model view file for item in list view
We need to create `ColorViewModel.java` file and save it to the `modelview` package. Since this class is view model, it need to be inherited from ViewModel class. Otherwise, the `ColorViewModel` is also an item of collection view, it need to be implement `ItemViewData` interface and override `getTag()` method to provide `tag` data for this item view.

```java
package tv.inair.apptemplate.viewmodel;

import ...

public class ColorViewModel extends ViewModel implements ItemViewData {

  public ColorViewModel() {
  }

  @Override
  public CharSequence getTag() {
    return null;
  }
}
```

In this example, we have two `tag` for data: `right` and `left`. Following application design, we can see the odd items using `left` template and even items using `right` template, we will add an property to store it initialize index and used it to return the corresponding `tag`

```java
	private int index;

	@Override
	public CharSequence getTag() {
		return index % 2 == 1 ? "left" : "right";
	}

```

Now, our layout has two elements that will be diffirent between each others: the _color_, and it's _description_. Let's create two properties that shall be used to bind to our layout:

```java
  private int color;
  private CharSequence description;
```

And we create the Setters & Getters for them:

```java

  //Getters & Setters

  public CharSequence getDescription() {
    return description;
  }

  public void setDescription(CharSequence description) {
    this.description = description;
    notifyPropertyChanged("description");
  }

  public int getColor() {
    return color;
  }

  public void setColor(int color) {
    this.color = color;
    notifyPropertyChanged("color");
  }
```

Notice the function `notifyPropertyChanged("<property's name>");` that is being used on every setters? This function will tell the system that the corresponding property has changed and the layout should refresh to update with the new value.

Let's add more parameters to the class' constructor to initialize all properties we need.

```java
  // Constructor
  public ColorViewModel(int index, int color, CharSequence description) {
    this.index = index;

    setColor(color);
    setDescription(description);
  }
```

###Finalize the layered layout XML
Open our previous created xml `right_layout.xml`.

There're two properties inside this XML we need to change in order to bind with our Model View properties declared previously:

- `ui:color` of __`UIRectView`__
- `ui:text` of __`UITextView`__

Complete XML file should be like this:
```xml
<?xml version="1.0" encoding="utf-8"?>
<UIViewGroup
	xmlns:ui="http://schemas.android.com/apk/res-auto">
	
	<UIRectView
	  ui:height="100"
	  ui:width="100"
	  ui:color="{ Binding Path='color' }" />

	<UITextView
    ui:width="250"
    ui:positionX="105"
    ui:positionY="20"
    ui:text="{ Binding Path='description' }"
    ui:textSize="40" />

</UIViewGroup>
```

Do the same with `left_layout.xml` file, we will have the finalize content like this
```xml
<?xml version="1.0" encoding="utf-8"?>
<UIViewGroup
	xmlns:ui="http://schemas.android.com/apk/res-auto">

	<UITextView
    ui:width="250"
    ui:positionY="20"
    ui:text="{ Binding Path='description' }"
    ui:textSize="40" />
	
	<UIRectView
		ui:positionX="255"
	  ui:height="100"
	  ui:width="100"
	  ui:color="{ Binding Path='color' }" />

</UIViewGroup>
```

The syntax `{Binding Path: <property name>}` will tell the layout to look for the corresponding variable inside the model view and listen for changes produced by the binding property's setter.

###Prepare content for each layers
Open the `MainViewModel.java` file inside the `modelview` package, you should see the following content:

```java
package tv.inair.apptemplate.viewmodel;

import ...;

public class MainViewModel extends ViewModel {

	// Constructor
  public MainViewModel() {
  }
}
```

We will use this `MainViewModel` as data context of `main_layout.xml` which contain an `UIListView`, we must create an `ObservableCollection` of `ItemViewData` and change the class's constructor to initilize view model with preparing content.

```java
	public ObservableCollection<ItemViewData> itemViewModels = new ObservableCollection<ItemViewData>();

  public MainViewModel() {
  	itemViewModels.add(new ColorViewModel(1, Color.BLACK, "Black"));
  	itemViewModels.add(new ColorViewModel(2, Color.CYAN, "Cyan"));
  	itemViewModels.add(new ColorViewModel(3, Color.DKGRAY, "Dark gray"));
  	itemViewModels.add(new ColorViewModel(4, Color.GRAY, "Gray"));
  	itemViewModels.add(new ColorViewModel(5, Color.GREEN, "Green"));
  	itemViewModels.add(new ColorViewModel(6, Color.LTGRAY, "Light gray"));
  	itemViewModels.add(new ColorViewModel(7, Color.MAGENTA, "Magenta"));
  	itemViewModels.add(new ColorViewModel(8, Color.RED, "Red"));
  	itemViewModels.add(new ColorViewModel(9, Color.TRANSPARENT, "Transparent"));
  	itemViewModels.add(new ColorViewModel(10, Color.WHITE, "White"));
  	itemViewModels.add(new ColorViewModel(11, Color.YELLOW, "Yellow"));
  	itemViewModels.add(new ColorViewModel(12, Color.BLUE, "Blue"));
  }
```

###Final work
We done with data right now, now is the time for grab it together.
Open the `main_layout.xml` file from `res/layout` directory. We need to add binding code for itemsSource:

```xml
<?xml version="1.0" encoding="utf-8"?>

<UIListView
  xmlns:ui="http://schemas.android.com/apk/res-auto"
  ui:positionX="1280.0"
  ui:width="400.0"
  ui:height="1080.0"
  ui:itemTemplate="@layout/listview_datatemplate"
  ui:itemsSource="{ Binding Path='itemViewModels' }"/>

```

__That is, you can now try to Build and Run your project.__