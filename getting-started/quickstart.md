# Installation

### **Add Marrow to your project**

In your `TeamCode/build.gradle`, add the following inside the `dependencies` block:

```gradle
dependencies {
   implementation 'io.github.skeleton-army.marrow:core:VERSION'
   
   // Additionally, add one of the following for your command-based library:
   implementation 'io.github.skeleton-army.marrow:nextftc:VERSION' // For NextFTC
   implementation 'io.github.skeleton-army.marrow:ftclib:VERSION' // For FTCLib
   
   // SolversLib users already have RetryCommand natively, no extra dependency needed.
}
```

Replace `VERSION` with the latest version below:

[![Latest Release](https://img.shields.io/github/v/tag/Skeleton-Army/Marrow)](https://github.com/Skeleton-Army/Marrow/releases)

{% hint style="info" %}
If you intend to use Marrow’s command features, you also need to install the Marrow module for your chosen command library, as written above.
{% endhint %}



### **Install a Command-Based library (Optional, but highly** **recommended)**

Using a command-based system makes it much easier to build complex robot behaviors from simple, reusable actions. Marrow is designed to complement such library and includes features that integrate directly with them.

We recommend using [SolversLib](https://docs.seattlesolvers.com/) for its simplicity and additional built-in features. That said, you’re free to use any other command library—such as [NextFTC](https://nextftc.dev/) or [Mercurial](https://docs.dairy.foundation/Mercurial/overview).

{% hint style="success" %}
Installed Marrow? Head to the [Core Features](/broken/pages/pwsFrVRPAqGab5rdlVeK) section to learn more about the features.
{% endhint %}
