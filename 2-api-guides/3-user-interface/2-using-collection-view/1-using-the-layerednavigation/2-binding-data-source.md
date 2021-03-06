This lesson will teach you the basic of creating a model view file and binding to layered navigation items source within InAiR SDK.

__Before you begin:__ be sure you've downloaded and saved the following pictures to your **res/drawable** folder:
* [photo1](http://developer.inair.tv/upload_file/attachment/hanoi.jpg) 
* [photo2](http://developer.inair.tv/upload_file/attachment/sanfrancisco.jpg)

###Creating a model view file for one layer
Open the `FirstViewModel.java` file inside the `modelview` package, you should see the following content:

```java
package tv.inair.apptemplate.viewmodel;

import ...

public class FirstViewModel extends ViewModel implements LayeredItemViewData, ItemViewData {

  private CharSequence layeredTitle;

  // Constructor
  public FirstViewModel(CharSequence layeredTitle) {
    this.layeredTitle = layeredTitle;
  }

  @Override
  public CharSequence getTitle() {
    return layeredTitle;
  }

  @Override
  public CharSequence getTag() {
    return "firstlayer";
  }
}
```

> **Note:** 
* FirstViewModel is a view model for one layer, it need to be implement `LayeredItemViewData` interface and override `getTitle()` method to provide `title` for each layered in `UILayeredNavigation`.
* Beside, FirstViewModel is also an item view model for a collection view, it need to be implement `ItemViewData` interface and override `getTag()` method to provide `tag` data for this item view model

Now, our layout has two elements that will be diffirent between each others: the _image_, and it's _description_. Let's create two properties that shall be used to bind to our layout:

```java
  private int placeImageSrc;
  private CharSequence placeDescription;
```

And we create the Setters & Getters for them:

```java

  //Getters & Setters

  public CharSequence getPlaceDescription() {
    return placeDescription;
  }

  public void setPlaceDescription(CharSequence placeDescription) {
    this.placeDescription = placeDescription;
    notifyPropertyChanged("placeDescription");
  }

  public Drawable getPlaceImageSrc() {
    return resources.getDrawable(placeImageSrc);
  }

  public void setPlaceImageSrc(int placeImageSrc) {
    this.placeImageSrc = placeImageSrc;
    notifyPropertyChanged("placeImageSrc");
  }
```

> **Tip:** we should not store an instance of `Drawable` in model view to avoid wasting memory with bitmaps data.

Notice the function `notifyPropertyChanged("<property's name>");` that is being used on every setters? This function will tell the system that the corresponding property has changed and the layout should refresh to update with the new value.

Let's add more parameters to the class' constructor to initialize all properties we need.

```java
  // Constructor
  public FirstViewModel(CharSequence layeredTitle, int placeImageSrc, CharSequence placeDescription) {
    this.layeredTitle = layeredTitle;

    setPlaceImageSrc(placeImageSrc);
    setPlaceDescription(placeDescription);
  }
```

###Finalize the layered layout XML
Open our previous created xml `first_layout.xml`.

Since we use `first_layout.xml` as a template, content of `UIImageView` and `UITextView` will depend on the data that showing on each layer, we need to use `DataBinding`.

There're two properties inside this XML we need to change in order to bind with our Model View properties declared previously:

- `ui:src` of __`UIImageView`__
- `ui:text` of __`UITextView`__

Complete XML file should be like this:
```xml
<?xml version="1.0" encoding="utf-8"?>
<UIViewGroup xmlns:ui="http://schemas.android.com/apk/res-auto">

  <UIImageView
    ui:width="310"
    ui:height="248"
    ui:positionX="0"
    ui:positionY="100"
    ui:src="{ Binding Path='placeImageSrc' }" />

  <UITextView
    ui:width="310"
    ui:positionX="0"
    ui:positionY="400"
    ui:text="{ Binding Path='placeDescription' }"
    ui:textSize="40" />

</UIViewGroup>
```

The syntax `{Binding Path: <property name>}` will tell the layout to look for the corresponding variable inside the model view and listen for changes produced by the binding property's setter.

###Prepare content for each layers
Open the `MainViewModel.java` file inside the `modelview` package, you should see the following content:

```java
package tv.inair.apptemplate.viewmodel;

import ...;

public class MainViewModel extends ViewModel {

  public ObservableCollection<LayeredItemViewData> layeredItemViewModels = new ObservableCollection<>();

  public MainViewModel() {
    layeredItemViewModels.add(new FirstViewModel());
    layeredItemViewModels.add(new SecondViewModel());
  }
}
```

Since we use only `FirstViewModel` in this application, we should change the class's constructor to initilize view model with preparing content.

```java
  public MainViewModel() {
    layeredItemViewModels.add(new FirstViewModel(
      "San Francisco", 
      R.drawable.sanfrancisco, 
      "San Francisco, officially the City and County of San Francisco."
      )
    );

    layeredItemViewModels.add(new FirstViewModel(
      "Hanoi",
      R.drawable.hanoi,
      "Hanoi is the capital of Vietnam and the country's second largest city"
      )
    );
  }
```

__That is, you can now try to Build and Run your project.__