# Installation

### **Add Marrow to your project**

In your `TeamCode/build.gradle`, add the following inside the `dependencies` block:

```groovy
dependencies {
   implementation 'com.skeletonarmyftc.marrow:core:<VERSION>'
}
```

Replace `<VERSION>` with the latest release, without the leading "v" (e.g. `1.0.0`): ![](data:image/svg+xml;utf8,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%2294%22%20height%3D%2220%22%20role%3D%22img%22%20aria-label%3D%22release%3A%20v1.0.0%22%3E%3Ctitle%3Erelease%3A%20v1.0.0%3C%2Ftitle%3E%3ClinearGradient%20id%3D%22s%22%20x2%3D%220%22%20y2%3D%22100%25%22%3E%3Cstop%20offset%3D%220%22%20stop-color%3D%22%23bbb%22%20stop-opacity%3D%22.1%22%2F%3E%3Cstop%20offset%3D%221%22%20stop-opacity%3D%22.1%22%2F%3E%3C%2FlinearGradient%3E%3CclipPath%20id%3D%22r%22%3E%3Crect%20width%3D%2294%22%20height%3D%2220%22%20rx%3D%223%22%20fill%3D%22%23fff%22%2F%3E%3C%2FclipPath%3E%3Cg%20clip-path%3D%22url\(%23r\)%22%3E%3Crect%20width%3D%2249%22%20height%3D%2220%22%20fill%3D%22%23555%22%2F%3E%3Crect%20x%3D%2249%22%20width%3D%2245%22%20height%3D%2220%22%20fill%3D%22%23007ec6%22%2F%3E%3Crect%20width%3D%2294%22%20height%3D%2220%22%20fill%3D%22url\(%23s\)%22%2F%3E%3C%2Fg%3E%3Cg%20fill%3D%22%23fff%22%20text-anchor%3D%22middle%22%20font-family%3D%22Verdana%2CGeneva%2CDejaVu%20Sans%2Csans-serif%22%20text-rendering%3D%22geometricPrecision%22%20font-size%3D%22110%22%3E%3Ctext%20aria-hidden%3D%22true%22%20x%3D%22255%22%20y%3D%22150%22%20fill%3D%22%23010101%22%20fill-opacity%3D%22.3%22%20transform%3D%22scale\(.1\)%22%20textLength%3D%22390%22%3Erelease%3C%2Ftext%3E%3Ctext%20x%3D%22255%22%20y%3D%22140%22%20transform%3D%22scale\(.1\)%22%20fill%3D%22%23fff%22%20textLength%3D%22390%22%3Erelease%3C%2Ftext%3E%3Ctext%20aria-hidden%3D%22true%22%20x%3D%22705%22%20y%3D%22150%22%20fill%3D%22%23010101%22%20fill-opacity%3D%22.3%22%20transform%3D%22scale\(.1\)%22%20textLength%3D%22350%22%3Ev1.0.0%3C%2Ftext%3E%3Ctext%20x%3D%22705%22%20y%3D%22140%22%20transform%3D%22scale\(.1\)%22%20fill%3D%22%23fff%22%20textLength%3D%22350%22%3Ev1.0.0%3C%2Ftext%3E%3C%2Fg%3E%3C%2Fsvg%3E)



### **Install a Command-Based library (Optional, but highly** **recommended)**

Using a command-based system makes it much easier to build complex robot behaviors from simple, reusable actions. Marrow is designed to complement such library and includes features that integrate directly with them.

We recommend using [SolversLib](https://docs.seattlesolvers.com/) for its simplicity and additional built-in features. That said, you’re free to use any other command library - such as [NextFTC](https://nextftc.dev/) or [Mercurial](https://docs.dairy.foundation/Mercurial/overview).
