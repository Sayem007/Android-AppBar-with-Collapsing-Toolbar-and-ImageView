# AppBar-with-Collapsing-Toolbar-and-ImageView

A sample android application appbar which shows image as its initial position. After collapsing the image disappeared and shows a title.

## Preview

![pr2](https://user-images.githubusercontent.com/29102285/33391624-d4bde8c6-d563-11e7-8a19-057eb837bf90.gif)

## Installation & Usage
* Edit your build.gradle(Module: app) file with this

compile 'com.android.support:design:25.3.1'


* Go to your style.xml file. Remove your app theme bar and make changes like this,

<resources>

    <!-- Base application theme. -->
    <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- Customize your theme here. -->
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
    </style>

    <style name="AppTheme.PopupOverlay" parent="ThemeOverlay.AppCompat.Light"/>
    <style name="AppTheme.AppBarOverlay" parent="ThemeOverlay.AppCompat.Dark.ActionBar" />
</resources>

* In your 'values' package under resource directory, add a values resource file named 'dimens.xml' and add these codes

<resources>

    <dimen name="app_bar_height">180dp</dimen>
    <dimen name="fab_margin">16dp</dimen>
</resources>

* Put an Image in 'drawable' that you want to show before collapsing the toolbar.

* Go to your 'AndroidManifest.xml' and make the activity lable a blank space

       <activity 

            android:name=".MainActivity"
            android:label=" ">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

* Let's roll in to activity_main.xml. Here we use the Coordinate layout as parent layout of that activity.

Then we add the appbar and this will work as the parent of the toolbar.

        <android.support.design.widget.AppBarLayout
        android:id="@+id/app_bar"
        android:layout_width="match_parent"
        android:layout_height="@dimen/app_bar_height"
        android:fitsSystemWindows="true"
        android:theme="@style/AppTheme.AppBarOverlay">

Now comes the pre collapsing toolbar, image and post collapsing toolbar attributes,

          <android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/toolbar_layout"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:fitsSystemWindows="true"
            app:contentScrim="?attr/colorPrimary"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">

            <ImageView
                android:contentDescription=""
                app:layout_collapseMode="parallax"
                android:src="@drawable/banner"
                android:scaleType="centerCrop"
                android:layout_width="match_parent"
                android:layout_height="match_parent" />

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"
                app:popupTheme="@style/AppTheme.PopupOverlay" />
                
            </android.support.design.widget.CollapsingToolbarLayout>

All these included inside the AppBarLayout.

A floating action button is added after that.

      <android.support.design.widget.FloatingActionButton
        android:id="@+id/btn_abt_cntct"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="@dimen/fab_margin"
        app:layout_anchor="@id/app_bar"
        app:layout_anchorGravity="bottom|end"
        app:srcCompat="@android:drawable/ic_dialog_email" />
        
Finally include the body of the activity. For this I use another layout file called 'content_main.xml'. 
In this file NestedScrollView is used.

    <?xml version="1.0" encoding="utf-8"?>
    <android.support.v4.widget.NestedScrollView xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"
        tools:context="com.sayem.geeknot.appbarwithcollapsingtoolbarandimageview.MainActivity"
        tools:showIn="@layout/activity_main">

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical">

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_gravity="center"
                    android:text="@string/message"
                    android:textAlignment="center"
                    android:textColor="#000000"
                    android:textSize="30sp"
                    android:textStyle="bold"
                    android:padding="10dp"/>

        </LinearLayout>

    </android.support.v4.widget.NestedScrollView>

* In MainActivity.java 1st the Collapsing toolbar is declared with initial appearnce. After that, the range, post collapsing toolbar lable/title and title color is given.

        final CollapsingToolbarLayout collapsingToolbarLayout = (CollapsingToolbarLayout) findViewById(R.id.toolbar_layout);
        AppBarLayout appBarLayout = (AppBarLayout) findViewById(R.id.app_bar);
        appBarLayout.addOnOffsetChangedListener(new AppBarLayout.OnOffsetChangedListener() {
            boolean isShow = false;
            int scrollRange = -1;

            @Override
            public void onOffsetChanged(AppBarLayout appBarLayout, int verticalOffset) {
                if (scrollRange == -1) {
                    scrollRange = appBarLayout.getTotalScrollRange();
                }
                if (scrollRange + verticalOffset == 0) {
                    collapsingToolbarLayout.setTitle("Here We Go!");
                    collapsingToolbarLayout.setCollapsedTitleTextColor(Color.rgb(0, 0, 0));
                    isShow = true;
                } else if(isShow) {
                    collapsingToolbarLayout.setTitle(" ");
                    isShow = false;
                }
            }
        });
