# system_alert_window

A flutter plugin to show Truecaller like overlay window, over all other apps along with callback events.

## Android

### Application Class

      public class Application extends FlutterApplication implements PluginRegistry.PluginRegistrantCallback {

          @Override
          public void onCreate() {
              super.onCreate();
              //This is required as we are using background channel for dispatching click events
              SystemAlertWindowPlugin.setPluginRegistrant(this);
          }

          @Override
          public void registerWith(PluginRegistry pluginRegistry) {
              GeneratedPluginRegistrant.registerWith(pluginRegistry);
          }

      }

### Manifest

      //Permissions
      <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />
      <uses-permission android:name="android.permission.FOREGROUND_SERVICE " />
      <uses-permission android:name="android.permission.WAKE_LOCK" />


          <application
              //Linking the previously added application class
              android:name=".Application"
              android:label="system_alert_window_example"
              android:icon="@mipmap/ic_launcher">

#### Android 10 and below

Uses &#x27;draw on top&#x27; permission and displays it as a overlay window

#### Android 11 and above

Uses Android Bubble APIs to show the overlay window.


## IOS

Displays as a notification in the notification center [Help Needed]


## Example

### Show the overlay
          
      SystemWindowHeader header = SystemWindowHeader(
          title: SystemWindowText(text: "Incoming Call", fontSize: 10, textColor: Colors.black45),
          padding: SystemWindowPadding.setSymmetricPadding(12, 12),
          subTitle: SystemWindowText(text: "9898989899", fontSize: 14, fontWeight: FontWeight.BOLD, textColor: Colors.black87),
          decoration: SystemWindowDecoration(startColor: Colors.grey[100]),
          button: SystemWindowButton(text: SystemWindowText(text: "Personal", fontSize: 10, textColor: Colors.black45), tag: "personal_btn"),
          buttonPosition: ButtonPosition.TRAILING);
            );
            
      SystemWindowFooter footer = SystemWindowFooter(
          buttons: [
            SystemWindowButton(
              text: SystemWindowText(text: "Simple button", fontSize: 12, textColor: Color.fromRGBO(250, 139, 97, 1)),
              tag: "simple_button", //useful to identify button click event
              padding: SystemWindowPadding(left: 10, right: 10, bottom: 10, top: 10),
              width: 0,
              height: SystemWindowButton.WRAP_CONTENT,
              decoration: SystemWindowDecoration(
              startColor: Colors.white, endColor: Colors.white, borderWidth: 0, borderRadius: 0.0),
             ),
            SystemWindowButton(
              text: SystemWindowText(text: "Focus button", fontSize: 12, textColor: Colors.white),
              tag: "focus_button",
              width: 0,
              padding: SystemWindowPadding(left: 10, right: 10, bottom: 10, top: 10),
              height: SystemWindowButton.WRAP_CONTENT,
              decoration: SystemWindowDecoration(
              startColor: Color.fromRGBO(250, 139, 97, 1), endColor: Color.fromRGBO(247, 28, 88, 1), borderWidth: 0, borderRadius: 30.0),
             )
          ],
          padding: SystemWindowPadding(left: 16, right: 16, bottom: 12),
          decoration: SystemWindowDecoration(startColor: Colors.white),
          buttonsPosition: ButtonPosition.CENTER);
          
      SystemWindowBody body = SystemWindowBody(
              rows: [
                EachRow(
                  columns: [
                    EachColumn(
                      text: SystemWindowText(text: "Some body", fontSize: 12, textColor: Colors.black45),
                    ),
                  ],
                  gravity: ContentGravity.CENTER,
                ),
              ],
              padding: SystemWindowPadding(left: 16, right: 16, bottom: 12, top: 12),
            );

      SystemAlertWindow.showSystemWindow(
          height: 230,
          header: header,
          body: body,
          footer: footer,
          margin: SystemWindowMargin(left: 8, right: 8, top: 100, bottom: 0),
          gravity: SystemWindowGravity.TOP);
          
### Register for onClick events (button click)

      SystemAlertWindow.registerOnClickListener(callBackFunction);

      ///
      /// As this callback function is called from background, it should be declared on the parent level
      /// Whenever a button is clicked, this method will be invoked with a tag (As tag is unique for every button, it helps in identifying the button).
      /// You can check for the tag value and perform the relevant action for the button click
      ///
      void callBackFunction(String tag) {
        switch(tag){
          case "simple_button":
            print("Simple button has been clicked");
            break;
          case "focus_button":
            print("Focus button has been clicked");
            break;
          case "personal_btn":
            print("Personal button has been clicked");
            break;
          default:
            print("OnClick event of $tag");
        }
      }
          
### Close the overlay

      SystemAlertWindow.closeSystemWindow();