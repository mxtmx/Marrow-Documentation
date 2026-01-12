# Installation

### **Add Marrow to your project**

In your `TeamCode/build.gradle`, add the following inside the `dependencies` block:

```groovy
dependencies {
   implementation 'com.skeletonarmyftc.marrow:core:<VERSION>'
}
```

Replace `<VERSION>` with the latest version (only the number): [![Latest Release](https://img.shields.io/github/v/tag/Skeleton-Army/Marrow)](https://github.com/Skeleton-Army/Marrow/releases)



### **Install a Command-Based library (Optional, but highly** **recommended)**

Using a command-based system makes it much easier to build complex robot behaviors from simple, reusable actions. Marrow is designed to complement such library and includes features that integrate directly with them.

We recommend using [SolversLib](https://docs.seattlesolvers.com/) for its simplicity and additional built-in features. That said, you’re free to use any other command library - such as [NextFTC](https://nextftc.dev/) or [Mercurial](https://docs.dairy.foundation/Mercurial/overview).
