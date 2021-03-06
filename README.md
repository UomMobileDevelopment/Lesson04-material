# Lesson04-material


2016 - 17 Nov
2017 - 3 Nov

Στο μάθημα αυτό θα προσθέσουμε μια δεύτερη οθόνη (Activity) η οποία θα εμφανίζεται όταν πατάμε πάνω σε μια ημέρα πρόβλεψης της λίστας έτσι ώστε να παρουσιάζεται αναλυτική η πρόβλεψη για τη συγκεκριμένη ημέρα. 

###### Προσθήκη Toast όταν πατιέται ένα list item

Θα ξεκινήσουμε προσθέτοντας ένα προσωρινό [Toast (φανταστείτε το σαν Popup message box)](https://developer.android.com/guide/topics/ui/notifiers/toasts.html) για να εμφανίζεται όταν πατάμε σε κάποια ημέρα της λίστας. Θα χρειατούμε επίσης έναν ```ItemClickListener```.  
Οπως θα δείτε, τον κατασκευάζουμε ανώνυμα για απλοποίηση του κώδικα. Έτσι, με την προσωρινή λύση του Toast, θα επιβεβαιώσουμε τη σωστή λειτουργία του listener.

```
 listView.setOnItemClickListener(new AdapterView.OnItemClickListener(){
            @Override
            public void onItemClick(AdapterView<?> parent, View view, int position, long id) {
                final String item = mForecastAdapter.getItem(position);
                Intent intent = new Intent(getActivity(), DetailActivity.class);
                intent.putExtra(Intent.EXTRA_TEXT, item);
                startActivity(intent);

            }
        });

```

Ας περάσουμε στη δημιουργία της οθόνης (Activity) που θα εμφανίζεται με το πάτημα κάποιας ημέρας της λίστας. 
Φτιάχνουμε ένα νέο DetailActivity

```
package com.example.android.sunshine.app;

import android.content.Intent;
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.support.v7.app.ActionBarActivity;
import android.view.LayoutInflater;
import android.view.Menu;
import android.view.MenuItem;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

public class DetailActivity extends ActionBarActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_detail);
        if (savedInstanceState == null) {
            getSupportFragmentManager().beginTransaction()
                    .add(R.id.container, new PlaceholderFragment())
                    .commit();
        }
    }


    @Override
    public boolean onCreateOptionsMenu(Menu menu) {
        // Inflate the menu; this adds items to the action bar if it is present.
        getMenuInflater().inflate(R.menu.detail, menu);
        return true;
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        int id = item.getItemId();

        //noinspection SimplifiableIfStatement
        if (id == R.id.action_settings) {
            Intent intent = new Intent(this, SettingsActivity.class);
            startActivity(intent);
            return true;
        }

        return super.onOptionsItemSelected(item);
    }

    /**
     * A placeholder fragment containing a simple view.
     */
    public static class PlaceholderFragment extends Fragment {

        public PlaceholderFragment() {
        }

        @Override
        public View onCreateView(LayoutInflater inflater, ViewGroup container,
                                 Bundle savedInstanceState) {

            Intent intent = getActivity().getIntent();
            View rootView = inflater.inflate(R.layout.fragment_detail, container, false);

            if(intent != null && intent.hasExtra(Intent.EXTRA_TEXT)){
                String text = intent.getStringExtra(Intent.EXTRA_TEXT);
                TextView detailText = (TextView)rootView.findViewById(R.id.detail_text);
                detailText.setText(text);
            }
            return rootView;
        }
    }
}
```

Και το fragment_detail.xml, παρατηρήστε πως Hierarchical Parent είναι το MainActivity

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_detail"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.example.android.sunshine.app.DetailActivity">

<TextView
    android:id="@+id/detail_text"
    android:text=""
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />

</RelativeLayout>

```

## Σημαντικό!

Δεν ξεχνώ να προσθέσω το νέο activity στο AndroidManifest.xml

```
        <activity
            android:name=".DetailActivity"
            android:label="@string/title_activity_detail"
            android:parentActivityName=".MainActivity">
            <meta-data
                android:name="android.support.PARENT_ACTIVITY"
                android:value="com.example.android.sunshine.app.MainActivity" />
        </activity>
```

##### Intents

Τα Intents είναι ο βασικότερος τρόπος επικοινωνίας μεταξύ των Activities στο σύστημα. Χωρίζονται σε Explicit και Implicit. 

Στο σημερινό μάθημα θα δούμε μόνο τα Explicit Intents



