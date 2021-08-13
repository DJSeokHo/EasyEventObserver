# EasyEventObserver
easy event observer, same as notification center in iOS, easy to use.

# Step 1. Add it in your root build.gradle at the end of repositories:
```
allprojects {
  repositories {
    ...
    maven { url 'https://jitpack.io' }
  }
}
```

# Step 2. Add the dependency:
```
dependencies {
  implementation 'com.github.DJSeokHo:EasyEventObserver:1.0.6'
}
```

# How to use:

For example, send data from 2nd activity to 1st activity and update UI.

1. Create a Event Constants Class like this:
```
object EventConstants {

    const val UPDATE_ACTIVITY_2 = "UPDATE_ACTIVITY_1"
    
    // add your event here
    .
    .
    .
}
```

2. Add observer in onCreate method of 1st activity:
```
...
...

// add observer
EventCenter.addEventObserver(EventConstants.UPDATE_ACTIVITY_1, this, object : EventCenter.EventRunnable {
    override fun run(arrow: String, poster: Any, data: MutableMap<String, Any>?) {
        data?.let {

            val content = data["youMapKey"] as String

            yourTextView.text = content
        }
    }
})

...
...

```

3. Don't forget remove observer in 1st activity:
```
override fun onDestroy() {
    // don't forget this
    EventCenter.removeAllObserver(this)
    super.onDestroy()
}
```

4. Send data from your source activity, such as 2nd activity:
```
// for example, click button to send data
yourButton.setOnClickListener {

    val dictionary = mutableMapOf<String, Any>()
    
    // the key of value must same as the key in target activity
    dictionary["content"] = text

    EventCenter.sendEvent(EventConstants.UPDATE_ACTIVITY_1, this, dictionary)
}
```

5. Done! You can add observer in activity, fragment, custom view, just remember, remove observer when activity destroyed, fragment destroyed, that's means if you don't need observer any more in activity, fragment, customview, remove it~
