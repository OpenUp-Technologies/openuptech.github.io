## Trouble shooting MacOS editor

This article has a few know errors and ways to fix them.

### Project won't open, crashes

Remove the AudioSpatializer bundle van Oculus.

### Hands do not animate

The `.fbx` animation files have import error due to incorrect settings. The animation settings should be
`Generic` and `No Avatar`. `Convert Units` should be enabled in the main import settings
to convert cm to metres. 

It is possible that Unity refuses to accept these settings and reverts them after clicking
`Apply`. Current best guess is that the `.meta` files are not interpreted correctly.
The best way to fix this is to remove all the `.meta` files and to let Unity manually
regenerated them. 

This does however cause the AnimatorControllers to lose the animations because the GUIDs
of the files have chagned. You will need to manually edit those files and set them back
to the old GUID values. The fileIDs must all be set to 7400000.

### App is stuck loading on startup on headset

Enable `XRPluginManagement` and select the `Oculus` option.

### EditorUtils.cs in Vuplex plugin errors

That part can be commented out.

### Item missing from project

Vuplex directory is not in the project due to size and must be manually downloaded.

Elastic credentials are currently needed to run the app (should only be needed for server logging).