# Contact-App

Hello! I am Vijayavenkatesh. 

This simple Contact application is for collect the data from user and store it cloud. So, I used Fireebase Dtabase for online Cloud Pltform. The application developed by Android Studio (2011.1.1 Bumlebee) New version. 

# Softwares used

# Android Studio

Get More about Android Studio using ---> https://developer.android.com/

Get More about Firebase Database ---> https://console.firebase.google.com/

here the Androdi Studio was used to develope the application version 2011.1.1 (Bumblebee)

![image](https://user-images.githubusercontent.com/66859618/158071140-5ecd4a47-4e69-4e38-b2c6-f9301986f5cc.png)

2) Firbase Database for Store and enable ghe user authentications

![image](https://user-images.githubusercontent.com/66859618/158071220-dd8290ac-9fc3-4956-8b5c-3bc902c2eeab.png)

# Application Details

# Login Activty 

![image](https://user-images.githubusercontent.com/66859618/158071378-5879b046-b22b-489b-8757-97a8b57bf3e0.png)

                              xml design code

    <EditText
        android:id="@+id/et_login_email"
        android:layout_width="300sp"
        android:layout_height="48sp"
        android:ems="10"
        android:hint="Email"
        android:inputType="textEmailAddress"
        android:textAlignment="center"
        android:textColor="@color/black"
        android:textSize="20sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.494"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.685" />

    <EditText
        android:id="@+id/et_login_pass"
        android:layout_width="300sp"
        android:layout_height="48sp"
        android:hint="Password"
        android:textAlignment="center"
        android:textSize="20sp"
        android:textColor="@color/black"
        android:ems="10"
        android:inputType="textPassword"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.514"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.77" />

    <Button
        android:id="@+id/btn_login"
        android:layout_width="175sp"
        android:layout_height="50sp"
        android:background="@android:color/transparent"
        android:text="Login"
        android:textAlignment="center"
        android:textColor="@android:color/holo_blue_dark"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.067"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.854" />

    <TextView
        android:id="@+id/tv_goto_reg"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="New User? Click here to get Account!"
        android:textAlignment="center"
        android:textColor="@color/black"
        android:textSize="15sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.098"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.919" />

    <TextView
        android:id="@+id/tv_forgot"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Forgot Password?"
        android:textAlignment="center"
        android:textColor="@android:color/holo_red_light"
        android:textSize="15sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.814"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.842" />


Java Code

package com.example.contactapp;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import com.google.android.gms.tasks.OnCompleteListener;
import com.google.android.gms.tasks.Task;
import com.google.firebase.auth.AuthResult;
import com.google.firebase.auth.FirebaseAuth;

public class Login extends AppCompatActivity {

    private Button btn_login;
    private EditText et_login_email, et_login_pass;
    private TextView tv_forgot, tv_goto_reg;
    private FirebaseAuth mAuth;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        btn_login = findViewById(R.id.btn_login);
        et_login_email = findViewById(R.id.et_login_email);
        et_login_pass = findViewById(R.id.et_login_pass);
        tv_forgot = findViewById(R.id.tv_forgot);
        tv_goto_reg = findViewById(R.id.tv_goto_reg);


        mAuth = FirebaseAuth.getInstance();
        if (mAuth.getCurrentUser() != null){
            finish();
            return;
        }

        btn_login.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                authenticateUser();
            }
        });

        tv_goto_reg.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent goto_Reg = new Intent(Login.this,Register.class);
                startActivity(goto_Reg);
                finish();
            }
        });

        tv_forgot.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent a = new Intent(Login.this,Forgot_Data.class);
                startActivity(a);
                finish();
            }
        });

    }

    private void authenticateUser() {
        String email = et_login_email.getText().toString();
        String pass = et_login_pass.getText().toString();

        if (email.isEmpty() || pass.isEmpty()){
            Toast.makeText(this, "Please fill the all field!", Toast.LENGTH_SHORT).show();
            return;
        }

        mAuth.signInWithEmailAndPassword(email, pass)
                .addOnCompleteListener(this, new OnCompleteListener<AuthResult>() {
                    @Override
                    public void onComplete(@NonNull Task<AuthResult> task) {
                        if (task.isSuccessful()){
                            showContactPage();

                        }else {
                            Toast.makeText(Login.this, "Authentication Failed@ Try  again later!", Toast.LENGTH_SHORT).show();

                        }
                    }
                });

    }

    private void showContactPage() {
        Intent intent = new Intent(this,Contact.class);
        startActivity(intent);
        finish();
    }
}


# Register Activity

![image](https://user-images.githubusercontent.com/66859618/158071555-29eaaf65-03a3-41af-9c21-4749d924096b.png)

                                xml Design Code
                               

    <EditText
        android:id="@+id/et_reg_email"
        android:layout_width="320dp"
        android:layout_height="50dp"
        android:ems="10"
        android:hint="Email"
        android:importantForAutofill="no"
        android:inputType="textEmailAddress"
        android:textAlignment="center"
        android:textColor="@color/black"
        android:textSize="20sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.494"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.591"
        tools:ignore="SpeakableTextPresentCheck" />

    <EditText
        android:id="@+id/et_reg_pass"
        android:layout_width="320dp"
        android:layout_height="50dp"
        android:ems="10"
        android:hint="Password"
        android:inputType="textPassword"
        android:textAlignment="center"
        android:textColor="@color/black"
        android:textSize="20sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.494"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.676" />

    <EditText
        android:id="@+id/et_reg_secode"
        android:layout_width="320dp"
        android:layout_height="50dp"
        android:ems="10"
        android:hint="Secret Code"
        android:inputType="textPassword"
        android:textAlignment="center"
        android:textColor="@color/black"
        android:textSize="20sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.494"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.766" />

    <Button
        android:id="@+id/btn_register"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Sign Up"
        android:textColor="@android:color/holo_blue_dark"
        android:textSize="25sp"
        android:textStyle="italic|bold"
        android:textAlignment="center"
        android:background="@android:color/transparent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.884" />

    <TextView
        android:id="@+id/tv_goto_login"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Already User? Please Click to Login!"
        android:textColor="@color/black"
        android:textSize="15sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.558"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.939" />
        ------------------------------------------------------
        
        Java Code
        
        package com.example.contactapp;

import androidx.annotation.NonNull;
import androidx.annotation.RequiresApi;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Build;
import android.os.Bundle;
import android.telephony.CellSignalStrength;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

import com.google.android.gms.tasks.OnCompleteListener;
import com.google.android.gms.tasks.Task;
import com.google.firebase.auth.AuthResult;
import com.google.firebase.auth.FirebaseAuth;
import com.google.firebase.database.FirebaseDatabase;

public class Register extends AppCompatActivity {

    private FirebaseAuth mAuth;
    private Button btn_register;
    private TextView tv_goto_login;

    private EditText et_reg_email, et_reg_pass, et_reg_secode;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_register);

        mAuth = FirebaseAuth.getInstance();
        if (mAuth.getCurrentUser() != null){
            finish();
            return;
        }

        btn_register = findViewById(R.id.btn_register);
        btn_register.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                registerUser();

            }
        });

        tv_goto_login = findViewById(R.id.tv_goto_login);
        tv_goto_login.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent goto_login = new Intent(Register.this,Login.class);
                startActivity(goto_login);
                finish();
            }
        });

    }

    @RequiresApi(api = Build.VERSION_CODES.GINGERBREAD)
    private void registerUser() {

        et_reg_email = findViewById(R.id.et_reg_email);
        et_reg_pass = findViewById(R.id.et_reg_pass);
        et_reg_secode = findViewById(R.id.et_reg_secode);

        String email = et_reg_email.getText().toString();
        String pass = et_reg_pass.getText().toString();
        String secode = et_reg_secode.getText().toString();

        if (email.isEmpty() || pass.isEmpty() || secode.isEmpty()){
            Toast.makeText(this, "Please fill all fields!", Toast.LENGTH_SHORT).show();
            return;
        }

        mAuth.createUserWithEmailAndPassword(email, pass)
                .addOnCompleteListener(this, new OnCompleteListener<AuthResult>() {
                    @Override
                    public void onComplete(@NonNull Task<AuthResult> task) {

                        if (task.isSuccessful()){
                            User user = new User(email, pass, secode);
                            FirebaseDatabase.getInstance().getReference("User")
                                    .child(FirebaseAuth.getInstance().getCurrentUser().getUid())
                                    .setValue(user).addOnCompleteListener(new OnCompleteListener<Void>() {
                                @Override
                                public void onComplete(@NonNull Task<Void> task) {
                                    showContactPage();

                                }
                            });
                        }else {
                            Toast.makeText(Register.this, "Authentication Failed! Try Later!", Toast.LENGTH_SHORT).show();

                        }

                    }

                });
    }

    private void showContactPage() {
        Intent intent = new Intent(this,Contact.class);
        startActivity(intent);
        finish();
    }
}


# Forgot User 

![image](https://user-images.githubusercontent.com/66859618/158071645-87d61326-c00b-4c2d-b9ab-c7474bdbace3.png)

xml Design Code

    <ImageView
        android:id="@+id/imageView2"
        android:layout_width="240dp"
        android:layout_height="287dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.072"
        app:srcCompat="@drawable/contact" />

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Forgot Your Password"
        android:textAlignment="center"
        android:textColor="@color/black"
        android:textSize="30sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.471" />

    <EditText
        android:id="@+id/et_forgot_email"
        android:layout_width="300sp"
        android:layout_height="48sp"
        android:hint="Recovery Email"
        android:textAlignment="center"
        android:textSize="20sp"
        android:textColor="@color/black"
        android:ems="10"
        android:inputType="textEmailAddress"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.497"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.631" />

    <Button
        android:id="@+id/btn_forgot"
        android:layout_width="300sp"
        android:layout_height="50sp"
        android:text="Reset Password"
        android:textStyle="italic|bold"
        android:textSize="20sp"
        android:textColor="@android:color/holo_blue_dark"
        android:background="@android:color/transparent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.742" />

    <Button
        android:id="@+id/btn_back"
        android:layout_width="150sp"
        android:layout_height="48sp"
        android:background="@android:color/transparent"
        android:text="Back"
        android:textColor="@android:color/holo_red_light"
        android:textSize="20sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.061"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.9" />
        
        
        
        
Java Code

package com.example.contactapp;

import androidx.annotation.NonNull;
import androidx.appcompat.app.AppCompatActivity;

import android.content.Intent;
import android.os.Bundle;
import android.util.Patterns;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;

import com.google.android.gms.tasks.OnCompleteListener;
import com.google.android.gms.tasks.Task;
import com.google.firebase.auth.FirebaseAuth;

public class Forgot_Data extends AppCompatActivity {

    private Button btn_forgot, btn_back;

    private EditText et_forgot_email;
    FirebaseAuth auth;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_forgot_data);

        btn_forgot = findViewById(R.id.btn_forgot);
        btn_back = findViewById(R.id.btn_back);
        et_forgot_email = findViewById(R.id.et_forgot_email);

        auth = FirebaseAuth.getInstance();

        btn_back.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Intent getback = new Intent(Forgot_Data.this,Login.class);
                startActivity(getback);
                finish();
            }
        });

        btn_forgot.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                resetPassword();
            }
        });

    }

    private void resetPassword() {
        String email = et_forgot_email.getText().toString();

        if(email.isEmpty()){
            et_forgot_email.setError("Email is Required");
            et_forgot_email.requestFocus();
            return;
        }

        if(!Patterns.EMAIL_ADDRESS.matcher(email).matches()){
            et_forgot_email.setError("Please Enter a Valid Email ID!");
            et_forgot_email.requestFocus();
            return;
        }

        auth.sendPasswordResetEmail(email).addOnCompleteListener(new OnCompleteListener<Void>() {
            @Override
            public void onComplete(@NonNull Task<Void> task) {

                if (task.isSuccessful()){
                    Toast.makeText(Forgot_Data.this, "Check Your Email Inbox to Reset your Password!", Toast.LENGTH_LONG).show();
                }else {
                    Toast.makeText(Forgot_Data.this, "Something went Wrong! Please try again later!", Toast.LENGTH_LONG).show();
                }
            }
        });

    }
}


# Contact Activity

![image](https://user-images.githubusercontent.com/66859618/158071793-5ea9268f-0a97-4ce3-bca9-e102738ceb64.png)


    <ImageView
        android:id="@+id/imageView"
        android:layout_width="295dp"
        android:layout_height="300dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.176"
        app:srcCompat="@drawable/one" />

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Contact app"
        android:textColor="@android:color/holo_red_dark"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.21"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.06" />

    <EditText
        android:id="@+id/et_main_name"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:inputType="textPersonName"
        android:hint="Name"
        android:textColor="@color/black"
        android:textSize="20sp"
        android:textAlignment="center"
        android:textStyle="normal"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.716"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.619" />

    <EditText
        android:id="@+id/et_main_phone"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="Phone No"
        android:textAlignment="center"
        android:textSize="20sp"
        android:textColor="@color/black"
        android:inputType="phone"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.716"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.717" />

    <EditText
        android:id="@+id/et_main_email"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:inputType="textEmailAddress"
        android:hint="Email ID"
        android:textStyle="normal"
        android:textAlignment="center"
        android:textSize="20sp"
        android:textColor="@color/black"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.711"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.82" />

    <TextView
        android:id="@+id/textView3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Name: "
        android:textColor="@android:color/black"
        android:textStyle="normal"
        android:textSize="20sp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.113"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.613" />

    <TextView
        android:id="@+id/textView4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Phone: "
        android:textColor="@android:color/black"
        android:textSize="20sp"
        android:textStyle="normal"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.113"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.708" />

    <TextView
        android:id="@+id/textView5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Email:"
        android:textColor="@android:color/black"
        android:textSize="20sp"
        android:textStyle="normal"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.109"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.799" />

    <Button
        android:id="@+id/btn_save"
        android:layout_width="280dp"
        android:layout_height="50sp"
        android:text="Save Contact"
        android:textSize="25sp"
        android:textAlignment="center"
        android:textStyle="bold|italic"
        android:background="@android:color/transparent"
        android:textColor="@android:color/holo_blue_dark"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.91" />

    <TextView
        android:id="@+id/tv_logout"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Logout"
        android:textAlignment="center"
        android:textColor="@android:color/holo_red_light"
        android:textSize="20sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.832"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.069" />
        

# Firebase User Data Details

# importent Note!

Enable this Email Authentication is must for user authentication

![image](https://user-images.githubusercontent.com/66859618/158072316-acafbf72-5002-48db-8f10-3d7103e1159d.png)

Re-Write the this Real-Time Database rlues like this. 

Defult Condition is "Read --> false" | "Write --> false" and You Must change into "Read --> true" | "Write --> true"

![image](https://user-images.githubusercontent.com/66859618/158072365-12345a22-309d-441e-a3e6-22fc4bb6fef2.png)


Realtime Database

![image](https://user-images.githubusercontent.com/66859618/158071876-4e8f6c65-c439-4ca8-8a53-70771cb73820.png)

User Data

![image](https://user-images.githubusercontent.com/66859618/158071934-ebe84f51-48e8-4371-9477-cd665071f37a.png)


Authentication

![image](https://user-images.githubusercontent.com/66859618/158071948-593c7e48-8633-442d-8f1c-24740dc0546a.png)

# Conclusion 

This app is developed by using java for backend and xml for frontent process. This application is working still now and the database also working properly

