The GUIChooser starts, but the Explorer and Experimenter do not start and output an Exception like this in the terminal:

```text
 /usr/share/themes/Mist/gtk-2.0/gtkrc:48: Engine "mist" is unsupported, ignoring
 ---Registering Weka Editors---
 java.lang.NullPointerException
        at weka.gui.explorer.PreprocessPanel.addPropertyChangeListener(PreprocessPanel.java:519)
        at javax.swing.plaf.synth.SynthPanelUI.installListeners(SynthPanelUI.java:49)
        at javax.swing.plaf.synth.SynthPanelUI.installUI(SynthPanelUI.java:38)
        at javax.swing.JComponent.setUI(JComponent.java:652)
        at javax.swing.JPanel.setUI(JPanel.java:131) 
        ...
```
This behavior happens only under Java 5/6 and Gnome/Linux, KDE doesn't produce this error. The reason for this is, that  Weka tries to look more "native" and therefore sets a platform-specific Swing theme. Unfortunately, this doesn't seem to be working correctly in Java 5/6 together with Gnome. A workaround for this is to set the cross-platform *Metal* theme. 

In order to use another theme one only has to create the following properties file:

```text
 LookAndFeel.props
```
with this content:

```ini
 Theme=javax.swing.plaf.metal.MetalLookAndFeel
```
