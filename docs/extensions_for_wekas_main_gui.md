
# Description
The main GUI (= `weka.gui.Main`) contains a plugin mechanism to add functionality to the main menu without having to modify the code of that class (the GUIChooser in the developer version as well). Thanks to the automatic class discovery, Weka will display all components that are found in packages listed in the [GenericPropertiesCreator.props](weka_gui_genericpropertiescreator.props.md) file.

# Version
>3.5.5 (or [snapshot](snapshots.md)/[Subversion](subversion.md) after 25/05/2007)

# Requirements
The are only *two* requirements for components to be listed in the main menu (under the *Extensions* menu):

* they have to implement the `weka.gui.MainMenuExtension` interface
* the packages they reside in must be listed in the [GenericPropertiesCreator.props](weka_gui_genericpropertiescreator.props.md) under the `weka.gui.MainMenuExtension` entry

# Examples
In the following, I'll present two really simple examples of how to add *stuff* to the main menu. An item that gets added to the main menu either handles everything itself, i.e., creating frame and displaying it, or it needs a frame to place its GUI components in. In the first case, one only needs to let the `getActionListener(JFrame)` method return an `ActionListener` and implement the `fillFrame(Component)` method with an empty body. In the other case, one lets the `getActionListener(JFrame)` method return `null` and uses the `fillFrame(Component)` method to fill the frame with life.

## Launching a browser
Launching a browser with the Weka homepage is a really example, since one only needs to use the `weka.gui.BrowserHelper` class to launch a browser with a specific URL. Since we don't need a frame for this, we don't add any functionality to the `fillFrame(Component)`, but only let the `getActionListener(JFrame)` method return an `ActionListener` that launches the browser with the Weka homepage. The [StartBrowser.java](files/StartBrowser.java) extension will be listed in the sub-menu *Internet* as *Start browser*.
```java
  public String getSubmenuTitle() {
    return "Internet";
  }
 
  public String getMenuTitle() {
    return "Start browser";
  }
 
  public ActionListener getActionListener(JFrame owner) {
    final JFrame finalOwner = owner;
    
    ActionListener result = new ActionListener() {
      public void actionPerformed(ActionEvent evt) {
	BrowserHelper.openURL(finalOwner, "http://www.cs.waikato.ac.nz/~ml/weka/");
      }
    };
    
    return result;
  }
 
  public void fillFrame(Component frame) {
  }
```
Since the class is part of the `weka.gui.extensions` package, we must add this package to the `weka.gui.MainMenuExtension` entry of the [GenericPropertiesCreator.props](weka_gui_genericpropertiescreator.props.md) file, e.g.:

```ini
 weka.gui.MainMenuExtension=\
  weka.gui.extensions
```

## SQL Worksheet
The [SqlWorksheet.java](files/SqlWorksheet.java) mode, one needs to take care of to check for `JFrame` and `JInternalFrame` as ancestor of the frame that is being passed through. The method only instantiates an `SqlViewer` panel, places it in the center of the frame, resizes the frame to 800x600 and moves it into the center of the screen.
```java
  public String getSubmenuTitle() {
    return null;
  }
 
  public String getMenuTitle() {
    return "SQL Worksheet";
  }
 
  public ActionListener getActionListener(JFrame owner) {
    return null;
  }
 
  public void fillFrame(Component frame) {
    SqlViewer sql = new SqlViewer(null);
 
    * add sql viewer component
    if (frame instanceof JFrame) {
      JFrame f = (JFrame) frame;
      f.setLayout(new BorderLayout());
      f.add(sql, BorderLayout.CENTER);
      f.pack();
    }
    else if (frame instanceof JInternalFrame) {
      JInternalFrame f = (JInternalFrame) frame;
      f.setLayout(new BorderLayout());
      f.add(sql, BorderLayout.CENTER);
      f.pack();
    }
 
    * size + location (= centered)
    frame.setSize(800, 600);
    frame.validate();
    int screenHeight = frame.getGraphicsConfiguration().getBounds().height;
    int screenWidth  = frame.getGraphicsConfiguration().getBounds().width;
    frame.setLocation(
	  (screenWidth - frame.getBounds().width) / 2,
	  (screenHeight - frame.getBounds().height) / 2);
  }
```
This class is part of the `weka.gui.extensions` package and we therefore must add this package to the `weka.gui.MainMenuExtension` entry of the [GenericPropertiesCreator.props](weka_gui_genericpropertiescreator.props.md) file:

```ini
 weka.gui.MainMenuExtension=\
  weka.gui.extensions
```

# Downloads
* [StartBrowser.java](files/StartBrowser.java)
* [SqlWorksheet.java](files/SqlWorksheet.java)
