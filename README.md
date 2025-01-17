# Day 1 registeration and login
package com.example.firbaseregisteration

import android.content.Intent
import android.os.Bundle
import android.provider.ContactsContract.CommonDataKinds.Email
import android.view.View
import android.widget.Button
import android.widget.EditText
import android.widget.GridView
import android.widget.Toast
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.google.firebase.auth.FirebaseAuth

class RegisterActivity : AppCompatActivity() {

    private lateinit var auth: FirebaseAuth

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_register)

        title = "Register"

        auth = FirebaseAuth.getInstance()

        var editTextEmail = findViewById<EditText>(R.id.email)
        var editTextPwd = findViewById<EditText>(R.id.pwd)
        var buttonLogin = findViewById<Button>(R.id.button)

        buttonLogin.setOnClickListener {
            var email = editTextEmail.text.toString()
            var pwd1 = editTextPwd.text.toString()

            register(email, pwd1)
        }


    }

    fun register(email: String, pwd1:String){
        val email = email
        val password = pwd1

        auth.createUserWithEmailAndPassword(email, password).addOnCompleteListener { task ->
            if (task.isSuccessful){
                val intent = Intent(this, LoginActivity::class.java)
                startActivity(intent)
                finish()
            }
        }.addOnFailureListener { exception ->
            // Handle failure (e.g., network issues)
            Toast.makeText(applicationContext, exception.localizedMessage, Toast.LENGTH_LONG)
                .show()

        }
    }

    fun  goToLogin(view: View){
        val intent = Intent(this, LoginActivity::class.java)
        startActivity(intent)
    }
}

# login activity
package com.example.firbaseregisteration

import android.content.Intent
import android.os.Bundle
import android.provider.ContactsContract.CommonDataKinds.Email
import android.view.View
import android.widget.Button
import android.widget.EditText
import android.widget.Toast
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.google.firebase.auth.FirebaseAuth
import kotlin.math.log

class LoginActivity : AppCompatActivity() {

    private  lateinit var  auth: FirebaseAuth

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        setContentView(R.layout.activity_login)

        title = "Login"
        auth = FirebaseAuth.getInstance()
        var editTextEmailAdress = findViewById<EditText>(R.id.emailAdress)
        var editTextPass = findViewById<EditText>(R.id.password)
        var buttonLogin = findViewById<Button>(R.id.buttonLogin)

        buttonLogin.setOnClickListener {
            var email = editTextEmailAdress.text.toString()
            var pwd1: String = editTextPass.text.toString()

            login(email, pwd1)
        }
    }

        fun login(email1: String, pwd1: String){
            val email = email1
            val password = pwd1

            auth.signInWithEmailAndPassword(email, password).addOnCompleteListener { task->
                if (task.isSuccessful){
                    val intent = Intent(this, MainActivity::class.java)
                    startActivity(intent)
                    finish()
                }
            }.addOnFailureListener { exception ->
                Toast.makeText(applicationContext,exception.localizedMessage, Toast.LENGTH_LONG)
                    .show()
            }

        }

        fun goToRegister(view: View){
            val intent =Intent(this, RegisterActivity::class.java)
            startActivity(intent)
            finish()
        }

}
