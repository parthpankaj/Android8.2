# Android8.2
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textStyle="bold"
        android:layout_marginTop="10dp"
        android:hint="SharedPreference Example"
        android:ems="10" >
    </TextView>

    <EditText
        android:id="@+id/editTextEnterName"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dp"
        android:maxLength="30"
        android:hint="Enter Your Name">
        <requestFocus />
    </EditText>

    <EditText
        android:id="@+id/editTextEnterEid"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:layout_marginTop="10dp"
        android:maxLength="8"
        android:inputType ="number"
        android:hint="Enter Your Employee ID">
    </EditText>
    <EditText
        android:id="@+id/editTextEnterAge"
        android:layout_width="fill_parent"
        android:layout_marginTop="10dp"
        android:inputType ="number"
        android:maxLength="2"
        android:layout_height="wrap_content"
        android:hint="Enter Your Age">
    </EditText>

    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal"
        android:layout_marginTop="10dp"
        android:gravity="center_horizontal">

        <Button
            android:id="@+id/buttonStore"
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:onClick="onClickStore"
            android:text="Store" />
        <Button
            android:id="@+id/buttonLoad"
            android:layout_height="wrap_content"
            android:layout_width="wrap_content"
            android:layout_toRightOf="@+id/buttonStore"
            android:onClick="onClickLoad"
            android:text="Load" />
    </RelativeLayout>>

</LinearLayout>
--------
package com.example.pankaj.preference82;

import android.content.Context;
import android.content.SharedPreferences;

// all methods are static , so we can call from any where in the code
//all member variables are private, so that we can save load with our own fun only
public class SharedPrefManager {
    //this is your shared preference file name, in which we will save data
    public static final String MY_EMP_PREFS = "MySharedPref";

    //saving the context, so that we can call all
    //shared pref methods from non activity classes.
    //because getSharedPreferences required the context.
    //but in activity class we can call without this context
    private static Context 	mContext;

    // will get user input in below variables, then will store in to shared pref
    private static String 	mName 			= "";
    private static String 	mEid 			= "";
    private static int 		mAge 			= 0;

    public static void Init(Context context)
    {
        mContext 		= context;
    }
    public static void LoadFromPref()
    {
        SharedPreferences settings 	= mContext.getSharedPreferences(MY_EMP_PREFS, 0);
        // Note here the 2nd parameter 0 is the default parameter for private access,
        //Operating mode. Use 0 or MODE_PRIVATE for the default operation,
        mName 			= settings.getString("Name",""); // 1st parameter Name is the key and 2nd parameter is the default if data not found
        mEid 			= settings.getString("EmpID","");
        mAge 			= settings.getInt("Age",0);
    }
    public static void StoreToPref()
    {
        // get the existing preference file
        SharedPreferences settings = mContext.getSharedPreferences(MY_EMP_PREFS, 0);
        //need an editor to edit and save values
        SharedPreferences.Editor editor = settings.edit();
        editor.putString("Name",mName); // Name is the key and mName is holding the value
        editor.putString("EmpID",mEid);// EmpID is the key and mEid is holding the value
        editor.putInt("Age", mAge); // Age is the key and mAge is holding the value

        //final step to commit (save)the changes in to the shared pref
        editor.commit();
    }

    public static void DeleteSingleEntryFromPref(String keyName)
    {
        SharedPreferences settings = mContext.getSharedPreferences(MY_EMP_PREFS, 0);
        //need an editor to edit and save values
        SharedPreferences.Editor editor = settings.edit();
        editor.remove(keyName);
        editor.commit();
    }

    public static void DeleteAllEntriesFromPref()
    {
        SharedPreferences settings = mContext.getSharedPreferences(MY_EMP_PREFS, 0);
        //need an editor to edit and save values
        SharedPreferences.Editor editor = settings.edit();
        editor.clear();
        editor.commit();
    }

    public static void SetName(String name)
    {
        mName =name;
    }
    public static void SetEmployeeID(String empID)
    {
        mEid = empID ;
    }
    public static void SetAge(int age)
    {
        mAge = age;
    }

    public static String GetName()
    {
        return mName ;
    }
    public static String GetEmployeeID()
    {
        return mEid ;
    }
    public static int GetAge()
    {
        return mAge ;
    }
}
--------
package com.example.pankaj.preference82;

import android.os.Bundle;
import android.app.Activity;
import android.view.Menu;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;

public class MainActivity extends Activity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        SharedPrefManager.Init(this);

    }
    public void onClickStore(View v) {

        //get user input first, then store. we will use our SharedPrefManager Class functions
        EditText editTextName,editTextEmpID,editTextAge;
        editTextName=(EditText)findViewById(R.id.editTextEnterName);
        editTextEmpID=(EditText)findViewById(R.id.editTextEnterEid);
        editTextAge=(EditText)findViewById(R.id.editTextEnterAge);

        //convert EditText to string
        String srtTextName = editTextName.getText().toString();
        String	srtTextEmpID = editTextEmpID.getText().toString();
        String		strTextAge = editTextAge.getText().toString();

        if(0!= srtTextName.length())
            SharedPrefManager.SetName(srtTextName); // need string value so convert it
        if(0 !=srtTextEmpID.length())
            SharedPrefManager.SetEmployeeID(srtTextEmpID); // need string value so convert it
        if(0 != strTextAge.length())
            SharedPrefManager.SetAge(Integer.parseInt(strTextAge)); // need integer value so convert it

        //now save all to shared pref, all updated values are now available in SharedPrefManager class, as we set above
        SharedPrefManager.StoreToPref();

        //reset all fields to blank before load and update from sharedpref
        EditText tv = null;
        tv = (EditText)findViewById(R.id.editTextEnterName);
        tv.setText("");
        tv = (EditText)findViewById(R.id.editTextEnterEid);
        tv.setText("");
        tv = (EditText)findViewById(R.id.editTextEnterAge);
        tv.setText("");

        Toast.makeText(this, "Data Successfully Stored to SharedPreference", Toast.LENGTH_LONG).show();

    }
    public void onClickLoad(View v) {

        //Get all values from SharedPrefference file
        SharedPrefManager.LoadFromPref(); // all values are loaded into corresponding variables of SharedPrefManager class

        //Now get the values form SharedPrefManager class using it's static functions.
        String strTextName,strTextEmpID;
        int iTextAge;
        strTextName = SharedPrefManager.GetName();
        strTextEmpID = SharedPrefManager.GetEmployeeID();
        iTextAge =SharedPrefManager.GetAge();

        //Now we can show these persistent values on our activity (GUI)
        EditText tv = null;
        tv = (EditText)findViewById(R.id.editTextEnterName);
        tv.setText(strTextName);
        tv = (EditText)findViewById(R.id.editTextEnterEid);
        tv.setText(strTextEmpID);
        tv = (EditText)findViewById(R.id.editTextEnterAge);
        tv.setText(String.valueOf(iTextAge));

        Toast.makeText(this, "Data Successfully Loaded from SharedPreference", Toast.LENGTH_LONG).show();
    }
    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.activity_main, menu);
        return true;
    }
}
