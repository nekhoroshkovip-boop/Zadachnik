#!/bin/bash
set -e
P=proj
mkdir -p $P/app/src/main/java/com/example/zadachnik $P/app/src/main/assets $P/app/src/main/res/values $P/app/src/main/res/drawable
cp index.html $P/app/src/main/assets/index.html
cd $P
cat > settings.gradle <<'EOF'
pluginManagement { repositories { google(); mavenCentral(); gradlePluginPortal() } }
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories { google(); mavenCentral() }
}
rootProject.name = "Zadachnik"
include ':app'
EOF
cat > build.gradle <<'EOF'
plugins { id 'com.android.application' version '8.5.2' apply false }
EOF
cat > gradle.properties <<'EOF'
org.gradle.jvmargs=-Xmx2048m -Dfile.encoding=UTF-8
android.useAndroidX=true
android.nonTransitiveRClass=true
EOF
cat > app/build.gradle <<'EOF'
plugins { id 'com.android.application' }
android {
    namespace 'com.example.zadachnik'
    compileSdk 34
    defaultConfig {
        applicationId 'com.example.zadachnik'
        minSdk 24
        targetSdk 34
        versionCode 1
        versionName '1.0'
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_17
        targetCompatibility JavaVersion.VERSION_17
    }
}
dependencies { implementation 'androidx.webkit:webkit:1.11.0' }
EOF
cat > app/src/main/AndroidManifest.xml <<'EOF'
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:configChanges="orientation|screenSize|screenLayout|keyboardHidden|smallestScreenSize"
            android:windowSoftInputMode="adjustResize">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>
</manifest>
EOF
cat > app/src/main/java/com/example/zadachnik/MainActivity.java <<'EOF'
package com.example.zadachnik;

import android.app.Activity;
import android.os.Bundle;
import android.view.WindowManager;
import android.webkit.WebResourceRequest;
import android.webkit.WebResourceResponse;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import androidx.webkit.WebViewAssetLoader;

public class MainActivity extends Activity {
    private WebView web;

    @Override
    protected void onCreate(Bundle state) {
        super.onCreate(state);
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON);
        final WebViewAssetLoader loader = new WebViewAssetLoader.Builder()
                .addPathHandler("/assets/", new WebViewAssetLoader.AssetsPathHandler(this))
                .build();
        web = new WebView(this);
        setContentView(web);
        WebSettings s = web.getSettings();
        s.setJavaScriptEnabled(true);
        s.setDomStorageEnabled(true);
        s.setAllowFileAccess(false);
        web.setWebViewClient(new WebViewClient() {
            @Override
            public WebResourceResponse shouldInterceptRequest(WebView v, WebResourceRequest r) {
                return loader.shouldInterceptRequest(r.getUrl());
            }
        });
        if (state == null) {
            web.loadUrl("https://appassets.androidplatform.net/assets/index.html");
        } else {
            web.restoreState(state);
        }
    }

    @Override
    protected void onSaveInstanceState(Bundle out) {
        super.onSaveInstanceState(out);
        web.saveState(out);
    }
}
EOF
cat > app/src/main/res/values/strings.xml <<'EOF'
<resources><string name="app_name">Задачник</string></resources>
EOF
cat > app/src/main/res/values/themes.xml <<'EOF'
<resources>
    <style name="AppTheme" parent="android:Theme.Material.Light.NoActionBar">
        <item name="android:statusBarColor">#3D6B5E</item>
        <item name="android:navigationBarColor">#3D6B5E</item>
    </style>
</resources>
EOF
cat > app/src/main/res/drawable/ic_launcher.xml <<'EOF'
<vector xmlns:android="http://schemas.android.com/apk/res/android"
    android:width="96dp" android:height="96dp" android:viewportWidth="96" android:viewportHeight="96">
    <path android:fillColor="#3D6B5E" android:pathData="M0,0h96v96h-96z"/>
    <path android:fillColor="#FFFEFB" android:pathData="M24,14h48v68h-48z"/>
    <path android:fillColor="#3D6B5E" android:pathData="M24,14h8v68h-8z"/>
    <path android:strokeColor="#8A877C" android:strokeWidth="2" android:pathData="M38,30h28M38,42h28M38,54h28M38,66h28"/>
    <path android:strokeColor="#C0543A" android:strokeWidth="3" android:strokeLineCap="round" android:pathData="M40,30l6,0"/>
</vector>
EOF
