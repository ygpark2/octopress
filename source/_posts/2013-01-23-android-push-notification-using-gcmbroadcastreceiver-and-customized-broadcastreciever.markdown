---
layout: post
title: "android push notification using GCMBroadcastReceiver and customized BroadcastReciever"
date: 2013-01-23 15:40
comments: true
categories: android, GCM
---

There are two ways to receive the push message.

## one way is to use GCMBroadcastReceiver.

``` xml AndroidManifest.xml
    <receiver
		android:name="com.google.android.gcm.GCMBroadcastReceiver"
		android:permission="com.google.android.c2dm.permission.SEND" >
		<intent-filter>
			<action android:name="com.google.android.c2dm.intent.RECEIVE" />
			<action android:name="com.google.android.c2dm.intent.REGISTRATION" />

            <category android:name="com.otherlevels.dailydeals" />
		</intent-filter>
	</receiver>

	<service android:name=".GCMIntentService" />
```

As you can see the above AndroidManifest file, they filter the events by setting the action type values between intent-filter tags.
Because GCMBroadcastReceiver is the class for GCM so you do not need other actions apart from "com.google.android.c2dm.intent.RECEIVE", "com.google.android.c2dm.intent.REGISTRATION"

``` java GCMBroadcastReceiver class https://code.google.com/p/gcm/source/browse/gcm-client/src/com/google/android/gcm/GCMBroadcastReceiver.java
public class GCMBroadcastReceiver extends BroadcastReceiver {

    private static final String TAG = "GCMBroadcastReceiver";

    @Override
    public final void onReceive(Context context, Intent intent) {
        Log.v(TAG, "onReceive: " + intent.getAction());
        String receiverClassName = getClass().getName();
        if (!receiverClassName.equals(GCMBroadcastReceiver.class.getName())) {
            GCMRegistrar.setRetryReceiverClassName(receiverClassName);
        }
        String className = getGCMIntentServiceClassName(context);
        Log.v(TAG, "GCM IntentService class: " + className);
        // Delegates to the application-specific intent service.
        GCMBaseIntentService.runIntentInService(context, intent, className);
        setResult(Activity.RESULT_OK, null /* data */, null /* extra */);
    }

    ...
}
```

## The other way is to customize the BroadcastReciever class by inheriting

In this method, you first need to set the customized java class name including package name for the tag android:name.The next step is to define the broadcast intents containing an action string.

``` xml AndroidManifest.xml
	<receiver
		android:name="com.google.android.gcm.GCMBroadcastReceiver"
		android:permission="com.google.android.c2dm.permission.SEND" >
		<intent-filter>
			<action android:name="com.google.android.c2dm.intent.RECEIVE" />
			<action android:name="com.google.android.c2dm.intent.REGISTRATION" />

			<category android:name="com.otherlevels.dailydeals" />
		</intent-filter>
	</receiver>
```
The same effect we can make in java code is to use the registerReceiver() method of the Activity class

``` java
IntentFilter filter = new IntentFilter("com.pkg.ain.Broadcast");

MyReceiver receiver = new MyReceiver();
registerReceiver(receiver, filter);
```

There is two ways to filer the action type. One is to use intent-filter tag as you can see above. The other way is to write a code to distinguish the action type on GCMBroadcastReceiver class file. You can see the example for the following code.

``` java onReceive function
    @Override
    public void onReceive(Context context, Intent intent) {
        Log.d(TAG, "onReceive with action:" + intent.getAction()+"!");

        if (intent.getAction().equals("com.google.android.c2dm.intent.REGISTRATION")) {
            handleRegistration(context, intent);
        } else if (intent.getAction().equals(ACTION_SMS_RECEIVED)) {

        } else if (intent.getAction().equals(ACTION_NEW_OUTGOING_SMS)) {

        } else if (intent.getAction().equals("com.google.android.c2dm.intent.RECEIVE")) {
            ...
        }
```

## References

1. http://www.techotopia.com/index.php/Android_Broadcast_Intents_and_Broadcast_Receivers
2. http://stackoverflow.com/questions/6352281/getintent-extras-always-null
